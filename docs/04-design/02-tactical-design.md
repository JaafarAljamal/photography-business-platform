# Tactical Design

**Document:** `docs/04-design/02-tactical-design.md`  
**Version:** 1.0.0  
**Status:** Approved — Tactical Baseline  
**Project:** Photography Business Platform  
**Sprint:** Sprint 2 — System Design

---

## 1. Purpose

This document defines the tactical domain model of the Photography Business Platform, translating the approved strategic design into implementable domain structures and business behavior.

It establishes:

- Aggregate Roots and consistency boundaries.
- Entities and Value Objects.
- Aggregate responsibilities and invariants.
- Domain events and cross-context contracts.
- Application and domain service responsibilities.
- Persistence and transaction principles.
- Reliability and testing requirements.

The decisions in this document are authoritative for subsequent architecture, database, API, validation, and implementation design.

The platform remains a modular Laravel monolith. Bounded Contexts are logical boundaries within the application and are not independently deployed services.

## 2. Tactical Design Principles

The tactical model follows these principles:

1. **Explicit ownership:** Each Bounded Context owns its business data and rules.
2. **Aggregate consistency:** Each Aggregate Root protects the invariants within its own consistency boundary.
3. **Controlled access:** External code interacts with an Aggregate through its Root.
4. **Identity-based references:** Aggregates reference other Aggregates by stable identifiers.
5. **Independent domain logic:** Domain code does not depend on Laravel, HTTP, database implementations, or external services.
6. **Application orchestration:** Application services coordinate use cases without taking ownership of domain rules.
7. **Explicit integration:** Cross-context communication uses defined contracts and events.
8. **Reliable processing:** Event publication and consumption must support retries and idempotency.
9. **No artificial abstractions:** Aggregates and domain services are introduced only when justified by business rules and consistency requirements.
10. **Testability:** Domain behavior must be testable independently of infrastructure.

## 3. Tactical Building Blocks

### 3.1 Entity

An Entity is an object with a stable identity and a lifecycle.

Its state may change, but its identity remains stable.

Examples include:

- Booking
- Photography Session
- Client
- Payment
- Refund
- Invoice
- Inquiry
- Album
- Notification

### 3.2 Value Object

A Value Object represents a concept defined by its values rather than by an independent identity.

Value Objects should be immutable where practical.

Candidate Value Objects include:

- `Money`
- `BookingTerms`
- `BookingPeriod`
- `ContactInformation`
- `PostalAddress`
- `PaymentReference`
- `InvoiceNumber`
- `FinancialPosition`
- `SessionSchedule`

Their exact fields, validation rules, and equality semantics will be defined during detailed implementation design.

### 3.3 Aggregate and Aggregate Root

An Aggregate is a consistency boundary containing domain objects that must preserve their invariants together.

The Aggregate Root is the only entry point for modifying the Aggregate's internal state.

Other Aggregates must be referenced by identity rather than by directly modifying their internal objects.

### 3.4 Domain Service

A Domain Service represents domain behavior that does not naturally belong to a single Entity, Aggregate, or Value Object.

It must not depend on infrastructure.

### 3.5 Application Service

An Application Service coordinates a use case.

It loads the required Aggregates, invokes their domain behavior, persists changes, and coordinates communication with other contexts.

It must not become a container for business rules that belong inside the domain model.

### 3.6 Domain Event

A Domain Event represents a business fact that has occurred.

Events are owned by the context responsible for the business operation that produced them.

Events handled only inside their owning context are internal domain events. Events published for consumption by another context are integration events.

The distinction concerns their purpose and scope, not necessarily their names.

---

## 4. Bounded Contexts and Aggregate Roots

The approved tactical boundaries are:

| Bounded Context | Aggregate Root | Consistency boundary |
|---|---|---|
| Booking Management | `Booking` | Booking lifecycle, agreed terms, and photography sessions |
| Client Management | `Client` | Client identity and contact information |
| Services & Packages | `PhotographyService` | Individual service definition |
| Services & Packages | `Package` | Package definition and included service references |
| Payment Management | `Payment` | Individual payment and its refund records |
| Invoice Management | `Invoice` | One invoice associated with one confirmed payment |
| Inquiry Management | `Inquiry` | Inquiry lifecycle |
| Portfolio Management | `Album` | Portfolio album and its managed content |
| Business Information | `BusinessProfile` | Public business information |
| Notification Management | `Notification` | Individual notification and its delivery lifecycle |

**Important ownership decision:** Payment records are owned exclusively by Payment Management. They are not part of the Booking Aggregate.

The financial position maintained by Payment Management is a calculated model or projection, not an additional Aggregate Root.

---

## 5. Booking Management

### 5.1 Booking Aggregate

**Aggregate Root:** `Booking`

**Purpose:** Protect the booking agreement and manage its lifecycle, including the photography sessions associated with it.

**Responsibilities:**

- Manage booking requests and their progression.
- Maintain the booking agreement.
- Preserve agreed services, packages, prices, and relevant terms.
- Manage booking approval and rejection.
- Manage booking confirmation and termination.
- Manage photography sessions.
- Record session start and completion.
- Determine whether the booking satisfies its completion conditions.
- Maintain the financial snapshot required for booking lifecycle decisions.
- Raise booking-related domain events.

### 5.2 Internal Entities

`Booking` contains the following internal Entity:

**PhotographySession**

A Photography Session represents one scheduled photography activity within a booking.

A booking may contain one or more sessions, including sessions scheduled on different days.

A Photography Session has its own identity and lifecycle, but it is not an independent Aggregate Root.

All session changes must be performed through the `Booking` Aggregate Root.

This design protects booking-level rules that depend on the state of all required sessions.

### 5.3 Value Objects

Candidate Value Objects include:

- `BookingTerms`
- `BookingPeriod`
- `Money`
- `SessionSchedule`
- `BookingStatus`
- `BookingFinancialSnapshot`

`BookingTerms` represents the agreed terms between the photographer and the client. It may contain references and snapshots of the selected services, package details, prices, sessions, and other agreed conditions.

`BookingFinancialSnapshot` represents the financial information received from Payment Management and retained by Booking Management for its own business rules.

### 5.4 Booking Invariants

The Booking Aggregate must enforce the following rules:

1. A booking must be associated with a valid Client identity.
2. A booking request must contain the information required by its business rules.
3. Booking request transitions must follow the permitted lifecycle.
4. A booking cannot be confirmed unless the required initial-payment condition has been satisfied.
5. A payment confirmation event does not directly change the booking state; Booking Management must apply its own confirmation rule.
6. A session cannot be completed unless its completion conditions are satisfied.
7. A booking cannot be completed while any required session remains incomplete.
8. A booking cannot be completed while the remaining balance is greater than zero.
9. Changes to agreed booking terms must follow the authorized change process.
10. Changes to the current service catalog must not silently modify an existing booking agreement.
11. Session lifecycle changes must be performed through the Booking Aggregate Root.

### 5.5 Booking Confirmation

Booking confirmation is a decision owned by Booking Management.

The process is:

1. Payment Management confirms a payment.
2. Payment Management publishes `PaymentConfirmed`.
3. Booking Management receives the financial facts.
4. Booking Management evaluates the initial-payment requirement.
5. If the requirement is satisfied and the booking is eligible for confirmation, Booking Management changes the booking state and raises `BookingConfirmed`.

The initial-payment requirement is a business rule, not a fixed percentage introduced by the technical design. Its exact amount or calculation must follow the agreed booking terms.

### 5.6 Booking Completion

A booking is completed only when both conditions are true:

- All required photography sessions are completed.
- The remaining balance is zero.

The Booking Aggregate evaluates these conditions using its own session state and the latest valid financial information received from Payment Management.

`PhotographySessionCompleted` does not automatically produce `BookingCompleted`.

### 5.7 Financial Snapshot

Booking Management does not own payment records or payment-confirmation rules.

It maintains only the financial information necessary to evaluate booking lifecycle conditions.

The snapshot may include:

- Agreed total.
- Cumulative confirmed payments.
- Remaining balance.
- Initial-payment requirement status.
- Financial revision or source version, where required.

This snapshot is not a ledger and must not be used as the authoritative source for payment history.

Payment Management remains the authority for payment records and financial calculations.

### 5.8 Booking Domain Events

Booking Management owns:

- `BookingRequestSubmitted`
- `BookingRequestUpdated`
- `BookingRequestCancelled`
- `AdditionalBookingInformationRequested`
- `AdditionalBookingInformationProvided`
- `BookingRequestApproved`
- `BookingRequestRejected`
- `BookingConfirmed`
- `PhotographySessionStarted`
- `PhotographySessionCompleted`
- `BookingCompleted`
- `BookingPriceChanged`
- `BookingAgreementTerminated`

`PhotographySessionStarted` and `PhotographySessionCompleted` are internal domain events within Booking Management.

The events `BookingPriceChanged` and `BookingAgreementTerminated` are approved events and must be reflected in the canonical domain-events document.

---

## 6. Client Management

### 6.1 Client Aggregate

**Aggregate Root:** `Client`

**Responsibilities:**

- Maintain client identity.
- Maintain contact information.
- Support authorized client information updates.
- Provide a stable Client identifier to other contexts.

### 6.2 Invariants

1. A Client has a stable identity.
2. Contact information must satisfy the applicable validation rules.
3. Client identification must avoid accidental duplicate records.
4. An inquiry alone must not create a Client.
5. A visitor submitting a Booking Request must be identified as a new or existing Client according to the client-identification rules.

### 6.3 Context Relationships

Other contexts reference a Client using its stable identifier.

Booking Management does not own or directly modify the Client Aggregate.

Inquiry Management may associate an inquiry with an existing Client, but an inquiry submitted by a visitor remains independent of Client creation.

Invoice Management references the relevant client identity without owning the client record.

---

## 7. Services & Packages

### 7.1 PhotographyService Aggregate

**Aggregate Root:** `PhotographyService`

**Responsibilities:**

- Maintain an individual service definition.
- Maintain current service descriptions and offering details.
- Maintain the current service price.
- Validate service updates.

The current service catalog is owned by Services & Packages.

### 7.2 Package Aggregate

**Aggregate Root:** `Package`

**Responsibilities:**

- Maintain a package definition.
- Maintain the services included in the package.
- Maintain the current package description and price.
- Validate package composition.

A Package references its included Photography Services by identity. It does not own their Aggregates.

### 7.3 Booking Relationship

Booking Management obtains the relevant offering information when preparing the booking agreement.

Once agreed, the necessary service and package details are preserved in the booking terms.

Subsequent catalog changes must not silently alter an existing booking.

The exact snapshot structure and persistence mapping will be defined in database design.

---

## 8. Payment Management

Payment Management is an independent Bounded Context and owns the complete lifecycle of payment records and payment confirmation.

### 8.1 Payment Aggregate

**Aggregate Root:** `Payment`

Each Payment Aggregate represents one individual payment associated with a booking.

**Responsibilities:**

- Record an individual payment.
- Maintain its amount and currency.
- Maintain its payment channel and relevant reference information.
- Track its status.
- Confirm or reject the payment.
- Maintain its confirmation details.
- Manage associated refund records.
- Raise payment-related domain events.

### 8.2 Refund Entity

A `Refund` is an internal Entity within the `Payment` Aggregate.

A refund is associated with one payment and is managed by Payment Management.

This design keeps payment and refund records within the same consistency boundary.

The model must support the possibility of partial refunds if required by the approved business rules. The exact limits, number of refunds, and cumulative refund constraints must be defined before implementation.

A refund is not considered confirmed merely because it was requested or because the associated booking agreement was terminated.

### 8.3 Payment Invariants

1. A payment must reference a valid booking identity.
2. Its amount must be greater than zero.
3. Its currency must be valid for the agreed transaction.
4. A payment cannot be confirmed without satisfying the required verification rules.
5. A confirmed payment must preserve its financial history.
6. Repeated confirmation requests must not create duplicate confirmation outcomes.
7. Rejected payments must not contribute to confirmed-payment totals.
8. Refund operations must follow their own eligibility and confirmation rules.
9. Booking termination alone does not confirm a refund.
10. Refund records must remain traceable to their associated payment.

### 8.4 Booking Financial Position

Payment Management is responsible for calculating and providing the financial position of a booking.

The financial position is a calculated model or projection based on:

- The current agreed booking total received from Booking Management.
- Confirmed payment records.
- Confirmed refund records.
- The approved financial calculation rules.

It is not an independent Aggregate Root.

The financial position may contain:

- Agreed total.
- Cumulative confirmed payments.
- Cumulative confirmed refunds.
- Net amount paid.
- Remaining balance.
- Initial-payment requirement status.
- Financial revision or source version.

The basic calculation is:

`Net Amount Paid = Confirmed Payments − Confirmed Refunds`

`Remaining Balance = Agreed Total − Net Amount Paid`

The calculation must follow the final rules for refund accounting and must not count pending or rejected transactions as confirmed financial facts.

Payment Management must preserve confirmed payment history when the agreed booking price changes.

### 8.5 Financial Position and Booking Management

Booking Management receives financial facts from Payment Management and maintains its own snapshot.

The two contexts have different responsibilities:

- Payment Management calculates and owns authoritative financial information.
- Booking Management decides whether the received information satisfies booking lifecycle rules.

The Booking Aggregate must not query or modify Payment Aggregates directly.

### 8.6 Payment Domain Events

Payment Management owns:

- `PaymentRecorded`
- `PaymentConfirmed`
- `RefundConfirmed`

`PaymentConfirmed` communicates the confirmed payment and the relevant financial position.

`RefundConfirmed` communicates a confirmed refund and the relevant updated financial facts.

Their final payloads must provide sufficient information to support Booking Management and Invoice Management without exposing unnecessary internal payment data.

---

## 9. Invoice Management

### 9.1 Invoice Aggregate

**Aggregate Root:** `Invoice`

Each Invoice represents one individually confirmed payment.

**Responsibilities:**

- Generate an invoice for a confirmed payment.
- Maintain the invoice identifier and number.
- Reference the associated payment.
- Reference the relevant client.
- Preserve the payment amount and currency.
- Preserve the cumulative paid amount and remaining balance at generation time.
- Maintain the invoice record.

### 9.2 Invoice Invariants

1. An invoice must reference a confirmed payment.
2. Each confirmed payment must have one corresponding invoice.
3. An invoice must not be generated more than once for the same payment.
4. An invoice must preserve the financial snapshot used when it was generated.
5. Invoice generation must not wait until full booking settlement.
6. Invoice generation and invoice delivery are separate operations.
7. Previously issued invoices must not be silently rewritten when booking terms change.

A database uniqueness constraint on the payment reference must reinforce the domain-level one-invoice-per-payment rule.

### 9.3 Invoice Generation Flow

When Payment Management confirms a payment:

1. Payment Management updates its payment and financial information.
2. It publishes `PaymentConfirmed`.
3. Invoice Management receives the event.
4. Invoice Management checks whether an invoice already exists for the payment.
5. If no invoice exists, it creates the Invoice Aggregate.
6. Invoice Management emits `InvoiceGenerated`.
7. Notification Management may process the invoice-generation event and request delivery.

Repeated processing of the same event must not create a second invoice.

### 9.4 Invoice Financial Snapshot

The invoice must preserve:

- The amount of the individual payment.
- The cumulative paid amount at the relevant confirmation point.
- The remaining balance at that point.
- The relevant booking and client references.

The cumulative amounts must be supplied or derived using the authoritative financial information provided by Payment Management.

An invoice is a historical financial document. A later payment or price change must not silently alter its original snapshot.

### 9.5 Invoice Domain Events

Invoice Management owns:

- `InvoiceGenerated`

This event means the invoice has been generated. It does not mean that it has been delivered successfully.

---

## 10. Inquiry Management

### 10.1 Inquiry Aggregate

**Aggregate Root:** `Inquiry`

**Responsibilities:**

- Record an inquiry.
- Maintain the sender's contact information and message.
- Track the inquiry lifecycle.
- Maintain handling status and relevant follow-up information.

### 10.2 Invariants

1. An inquiry must satisfy its required input rules.
2. It may reference an existing Client.
3. It may be submitted by a visitor who has no Client record.
4. An inquiry does not automatically create a Client.
5. An inquiry does not automatically create a Booking Request.
6. Inquiry state changes must follow the inquiry lifecycle.

Inquiry Management remains independent of Booking Management.

---

## 11. Portfolio Management

### 11.1 Album Aggregate

**Aggregate Root:** `Album`

**Responsibilities:**

- Maintain album identity and metadata.
- Organize portfolio content.
- Manage publication status.
- Maintain references to stored media.
- Support public portfolio presentation.

The Album Aggregate owns its album-level rules and the content that belongs to its consistency boundary.

The detailed model of individual portfolio items, categories, and media references will be defined in database design.

Portfolio Management does not own service definitions, bookings, or client photo-delivery workflows.

---

## 12. Business Information

### 12.1 BusinessProfile Aggregate

**Aggregate Root:** `BusinessProfile`

The initial version supports one logical business profile.

**Responsibilities:**

- Maintain public business name and contact details.
- Maintain relevant public location and business information.
- Validate profile updates.
- Provide information to the public website and authorized contexts.

A change to the current public profile must not silently rewrite values that have already become part of an agreed booking.

The initial design does not introduce multi-business or multi-tenant abstractions.

---

## 13. Notification Management

### 13.1 Notification Aggregate

**Aggregate Root:** `Notification`

Each Notification represents one logical notification request and its delivery lifecycle.

**Responsibilities:**

- Record a notification request.
- Identify the notification type and recipient.
- Maintain the delivery state.
- Record delivery attempts and failures.
- Support retry operations.
- Prevent duplicate logical notifications when the same source event is processed more than once.

### 13.2 Delivery Rules

1. A notification is created in response to an eligible business event or explicit application request.
2. The recipient and template must follow the notification rules.
3. Delivery is delegated to the external Email Service.
4. Failed delivery must not reverse the underlying business operation.
5. Retrying delivery must not repeat the original business operation.
6. Duplicate source-event processing must not create duplicate logical notifications.

The final notification types, template rules, and retry policy will be specified during implementation design.

---

## 14. Cross-Context Integration Contracts

The following contracts define the principal communication between contexts.

| Contract or event | Producer | Consumer | Purpose |
|---|---|---|---|
| Client identity and contact information | Client Management | Booking Management | Associate a booking with the correct client. |
| Service and package offering information | Services & Packages | Booking Management | Support booking requests and agreed terms. |
| `BookingPriceChanged` | Booking Management | Payment Management | Update the agreed financial total. |
| `BookingAgreementTerminated` | Booking Management | Payment Management | Evaluate financial consequences of termination. |
| `PaymentConfirmed` | Payment Management | Booking Management | Evaluate booking confirmation and completion conditions. |
| `PaymentConfirmed` | Payment Management | Invoice Management | Generate the invoice for the confirmed payment. |
| `RefundConfirmed` | Payment Management | Relevant consuming contexts | Communicate the confirmed refund and updated financial facts. |
| Booking lifecycle events | Booking Management | Notification Management | Trigger booking-related notifications. |
| Payment events | Payment Management | Notification Management | Trigger payment-related notifications. |
| `InvoiceGenerated` | Invoice Management | Notification Management | Trigger invoice-delivery notifications. |
| Inquiry lifecycle events | Inquiry Management | Notification Management | Trigger inquiry-related notifications. |

### 14.1 PaymentConfirmed Contract

The `PaymentConfirmed` contract must provide enough information for consumers to perform their responsibilities.

It should include:

- Event identifier and schema version.
- Payment identifier.
- Booking identifier.
- Client identifier where required.
- Confirmed payment amount and currency.
- Confirmation timestamp.
- Cumulative confirmed payments.
- Cumulative confirmed refunds.
- Net amount paid.
- Remaining balance.
- Financial revision or source version where required.

Booking Management uses the financial facts to evaluate its own rules.

Invoice Management uses the payment details and the relevant financial snapshot to generate the invoice.

The contract must not expose unnecessary internal payment data.

### 14.2 RefundConfirmed Contract

The `RefundConfirmed` contract should include:

- Event identifier and schema version.
- Refund identifier.
- Payment identifier.
- Booking identifier.
- Confirmed refund amount and currency.
- Confirmation timestamp.
- Updated financial position.
- Financial revision or source version where required.

Consumers apply their own rules when processing the event.

### 14.3 BookingPriceChanged Contract

The `BookingPriceChanged` contract should include:

- Event identifier and schema version.
- Booking identifier.
- Previous agreed total.
- New agreed total.
- Currency.
- Effective date or timestamp.
- Booking terms revision.

Payment Management uses this information to update the financial position without altering confirmed payment history.

### 14.4 BookingAgreementTerminated Contract

The `BookingAgreementTerminated` contract should include:

- Event identifier and schema version.
- Booking identifier.
- Termination timestamp.
- Relevant termination reason or category.
- Booking terms revision.
- Financial information needed to evaluate payment consequences.

Payment Management evaluates refund eligibility independently.

### 14.5 Event Identity and Correlation

Where applicable, integration events should carry:

- A stable event identifier.
- Event type and schema version.
- Occurrence timestamp.
- Aggregate identifier.
- Relevant business identifiers.
- Correlation identifier.
- Causation identifier.

The exact schemas will be maintained in the canonical event and integration documentation.

---

## 15. Application Services

Application Services coordinate use cases and transactions.

The following are candidate use-case names. They describe application behavior and do not mandate one class per name.

### 15.1 Booking Management

- `SubmitBookingRequest`
- `UpdateBookingRequest`
- `ApproveBookingRequest`
- `RejectBookingRequest`
- `ConfirmBooking`
- `StartPhotographySession`
- `CompletePhotographySession`
- `ChangeBookingTerms`
- `TerminateBookingAgreement`
- `CompleteBooking`
- `HandlePaymentConfirmed`

### 15.2 Client Management

- `CreateOrIdentifyClient`
- `UpdateClientInformation`

### 15.3 Services & Packages

- `CreatePhotographyService`
- `UpdatePhotographyService`
- `CreatePackage`
- `UpdatePackage`

### 15.4 Payment Management

- `RecordPayment`
- `ConfirmPayment`
- `RejectPayment`
- `ConfirmRefund`
- `HandleBookingPriceChanged`
- `HandleBookingAgreementTerminated`
- `GetBookingFinancialPosition`

### 15.5 Invoice Management

- `GenerateInvoiceForConfirmedPayment`
- `GetInvoiceDetails`

### 15.6 Inquiry Management

- `SubmitInquiry`
- `UpdateInquiryStatus`

### 15.7 Portfolio Management

- `CreateAlbum`
- `UpdateAlbum`
- `PublishAlbum`
- `UnpublishAlbum`

### 15.8 Business Information

- `UpdateBusinessProfile`

### 15.9 Notification Management

- `HandleBusinessEvent`
- `RequestNotificationDelivery`
- `RetryNotificationDelivery`

An Application Service should generally:

1. Receive a validated application command.
2. Obtain the required Aggregate through its repository.
3. Invoke the relevant domain behavior.
4. Persist the changed Aggregate.
5. Publish resulting events through the reliable event mechanism.
6. Return an application-level result.

Application Services must not bypass Aggregate invariants.

---

## 16. Domain Services and Policies

Domain Services should be introduced only when a business rule does not naturally belong to a particular Aggregate.

The following concepts may be implemented as policies or domain services, depending on the final detailed model:

### 16.1 Initial Payment Requirement

Represents the rule that determines whether the confirmed payments satisfy the initial-payment condition for booking confirmation.

The requirement must be based on the agreed booking terms. No fixed percentage or amount is assumed by this document.

### 16.2 Refund Eligibility Policy

Evaluates refund eligibility using the applicable booking agreement, payment history, termination circumstances, and approved refund rules.

Payment Management remains responsible for recording and confirming the refund.

### 16.3 Booking Completion Policy

Evaluates whether all required sessions are complete and the remaining balance is zero.

The policy may be implemented directly within the Booking Aggregate if the rule does not require a separate domain service.

### 16.4 Booking Price Change Policy

Evaluates whether a proposed change to agreed booking terms is permitted.

Booking Management owns this decision.

---

## 17. Transactions and Persistence Boundaries

### 17.1 Aggregate Transactions

A change to one Aggregate must preserve its invariants within its own transaction.

An application operation that involves multiple Aggregates must explicitly define its consistency requirements.

### 17.2 Cross-Context Transactions

Cross-context operations must not depend on unrestricted direct modifications of another context's records.

The fact that the application uses one database server does not remove Bounded Context ownership boundaries.

Each context owns its data and exposes information through defined contracts.

### 17.3 Transactional Outbox

The platform will use the Transactional Outbox pattern for reliable publication of integration events where a business transaction must not be separated from recording the event to be published.

The intended process is:

1. An Aggregate changes state.
2. The context persists the change and the corresponding outbox message in the same database transaction.
3. A publisher reads pending outbox messages.
4. The message is delivered to the appropriate consumer.
5. The message is marked as processed or retried according to the delivery policy.

This mechanism prevents the application from committing a business change while silently losing the corresponding integration event.

The implementation may use Laravel-compatible infrastructure, but the domain model must not depend on a particular queue or broker.

### 17.4 Event Consumers and Idempotency

Consumers must safely handle duplicate delivery.

The implementation must maintain enough processing information to recognize previously processed event identifiers.

Examples:

- Processing `PaymentConfirmed` more than once must not create duplicate invoices.
- Processing a booking financial update more than once must not double-count a payment.
- Processing a notification-triggering event more than once must not create duplicate logical notifications.

### 17.5 Event Ordering

Consumers must not assume that messages from different contexts arrive in a globally ordered sequence.

Where order matters, the contract must include the relevant Aggregate or financial revision.

Consumers must reject, ignore, or reconcile stale updates according to explicit rules rather than blindly overwriting newer state.

### 17.6 Repositories

Repositories are defined around Aggregate Roots, not individual database tables or every internal Entity.

The initial repository interfaces are:

- `BookingRepository`
- `ClientRepository`
- `PhotographyServiceRepository`
- `PackageRepository`
- `PaymentRepository`
- `InvoiceRepository`
- `InquiryRepository`
- `AlbumRepository`
- `BusinessProfileRepository`
- `NotificationRepository`

Financial-position queries belong to Payment Management and may be implemented using a dedicated query interface rather than an Aggregate repository.

Repository interfaces and implementations will be placed according to the final Clean Architecture structure.

### 17.7 Queries

Read operations may use query-specific projections when loading complete Aggregates is unnecessary.

Examples include:

- Public portfolio listings.
- Service and package catalogs.
- Booking dashboards.
- Payment histories.
- Invoice listings.
- Client summaries.

Query projections must not become an alternative route for changing business state or bypassing Aggregate invariants.

---

## 18. Testing Strategy

The tactical model must support a layered testing strategy.

### 18.1 Domain Unit Tests

Domain tests must verify business behavior without requiring Laravel bootstrapping or a database.

Important cases include:

**Booking Management**

- Booking cannot be confirmed without satisfying the initial-payment requirement.
- A payment confirmation does not directly mutate booking state.
- A booking cannot complete while a required session remains incomplete.
- A booking cannot complete while its remaining balance is greater than zero.
- Completing one session does not complete the booking.
- Changes to the current catalog do not modify agreed booking terms.

**Payment Management**

- A payment amount must be greater than zero.
- A payment cannot be confirmed without satisfying verification rules.
- A rejected payment does not contribute to confirmed-payment totals.
- A confirmed payment is counted only once.
- A price change preserves confirmed payment history.
- A refund must follow the applicable refund rules.
- Booking termination alone does not confirm a refund.

**Invoice Management**

- An invoice cannot be generated for an unconfirmed payment.
- One payment cannot produce multiple invoices.
- An invoice preserves its financial snapshot.
- Full booking settlement is not required to generate an invoice.

**Client and Inquiry Management**

- An inquiry does not automatically create a Client.
- Client identity remains stable.
- An inquiry may be associated with an existing Client.

**Notification Management**

- A delivery failure does not reverse the underlying business operation.
- Retrying delivery does not repeat the original business operation.
- Duplicate event processing does not create duplicate logical notifications.

### 18.2 Integration Tests

Integration tests must verify:

- Repository implementations correctly persist and restore Aggregates.
- Database constraints protect uniqueness requirements.
- Outbox messages are persisted atomically with business changes.
- Outbox messages are eventually published.
- Consumers handle repeated events safely.
- `PaymentConfirmed` provides the information needed by Booking Management and Invoice Management.
- Financial snapshots remain consistent with the relevant source revision.
- Failed notification delivery can be retried safely.

### 18.3 Feature Tests

Feature tests must cover important user-facing workflows, including:

- Booking request submission.
- Booking approval and rejection.
- Booking confirmation after qualifying payment confirmation.
- Photography session management.
- Booking completion.
- Manual payment recording and confirmation.
- Invoice generation and access.
- Inquiry submission.
- Client information management.
- Authorization of protected operations.

### 18.4 Test-Driven Development

Implementation follows the agreed TDD cycle:

1. Write a failing test for the expected behavior.
2. Implement the smallest change that makes it pass.
3. Refactor while keeping tests green.
4. Run relevant regression tests.
5. Commit the completed change with a meaningful commit message.

---

## 19. Relationship to Clean Architecture

The tactical model must remain independent of framework-specific concerns.

The intended dependency direction is:

- **Domain:** Aggregates, Entities, Value Objects, domain policies, and domain events.
- **Application:** Use cases, application services, commands, query interfaces, and orchestration.
- **Infrastructure:** Database repositories, event publication, outbox processing, queues, and external service adapters.
- **Presentation:** Laravel controllers, request validation, Blade views, and JavaScript interactions.

The Domain layer must not depend on Laravel or infrastructure.

Laravel controllers must not contain booking, payment, invoice, or refund business rules.

Blade templates and JavaScript must not become authoritative sources of domain invariants.

The exact directory structure, namespaces, and dependency rules will be maintained in:

- `docs/03-architecture/02-project-structure.md`
- `docs/03-architecture/06-clean-architecture.md`

---

## 20. Decisions Deferred to Detailed Design

The following are implementation details that must be specified in their relevant design documents:

1. Exact fields and methods of each Aggregate, Entity, and Value Object.
2. Complete booking, session, payment, refund, invoice, and inquiry state transitions.
3. Client identity matching and duplicate-resolution rules.
4. The exact initial-payment requirement.
5. Refund eligibility, partial refund limits, and financial accounting rules.
6. Currency and monetary precision rules.
7. Event schema versions and final payload definitions.
8. Outbox publishing and retry implementation.
9. Event-consumer idempotency storage and processing rules.
10. Database relationships, unique constraints, indexes, and migration strategy.
11. API request and response contracts.
12. Authorization policies and validation rules.
13. Laravel module structure and concrete repository implementations.
14. Detailed sequence diagrams, state diagrams, and acceptance tests.

These items do not change the approved Aggregate ownership boundaries.

Any future change to a strategic or tactical decision must be documented and reviewed for its impact on dependent documents.

---

## 21. Documentation Synchronization

The approved tactical model becomes the reference for synchronizing existing analysis and design documents.

| Document | Required synchronization |
|---|---|
| `docs/02-analysis/04-domain-events.md` | Add approved events, clarify ownership, and distinguish internal domain events from integration events. |
| `docs/02-analysis/06-domain-model.md` | Replace preliminary Aggregate candidates with the approved Aggregate boundaries and ownership. |
| `docs/02-analysis/03-booking-lifecycle.md` | Align booking and session lifecycle rules with the approved Aggregate behavior. |
| `docs/02-analysis/05-ubiquitous-language.md` | Align domain terminology with the approved tactical model. |
| `docs/03-architecture/02-project-structure.md` | Reflect the final logical module and layer boundaries. |
| `docs/03-architecture/06-clean-architecture.md` | Align dependency rules and layer responsibilities with the tactical design. |
| `docs/04-design/03-api-design.md` | Define the application-facing interfaces and request/response contracts. |
| `docs/04-design/04-validation-rules.md` | Document validation rules and business invariants. |
| `docs/04-design/database/` | Define ERD, tables, relationships, constraints, indexes, and migration strategy. |

The synchronization must preserve the approved ownership decisions:

- Payment Management owns payment records and confirmation.
- Payment is not part of the Booking Aggregate.
- Booking owns its photography sessions.
- Session completion is an internal Booking Management event.
- Invoice Management owns invoice records.
- One invoice is generated for each individually confirmed payment.

---

## 22. Related Documents

- `docs/00-project/roadmap.md`
- `docs/00-project/sprint-backlog.md`
- `docs/01-requirements/06-functional-requirements.md`
- `docs/01-requirements/07-non-functional-requirements.md`
- `docs/02-analysis/03-booking-lifecycle.md`
- `docs/02-analysis/04-domain-events.md`
- `docs/02-analysis/05-ubiquitous-language.md`
- `docs/02-analysis/06-domain-model.md`
- `docs/03-architecture/02-project-structure.md`
- `docs/03-architecture/06-clean-architecture.md`
- `docs/04-design/01-strategic-design.md`
- `docs/04-design/03-api-design.md`
- `docs/04-design/04-validation-rules.md`

**End of document.**
