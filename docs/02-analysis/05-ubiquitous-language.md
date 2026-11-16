# Ubiquitous Language

## 1. Purpose

This document defines the official business vocabulary used throughout the Photography Business Platform.

The goal of the Ubiquitous Language is to ensure that business stakeholders, software developers, testers, and future maintainers use the same terminology when discussing, designing, implementing, and maintaining the system.

The terminology defined here is the common reference for:

- Domain Models
- Entities and Aggregates
- Value Objects
- Domain Events
- Application Services
- APIs
- Database Schema
- Technical Documentation

Business terminology must remain consistent across all these artifacts.

New business terms must be evaluated and documented before being introduced into the domain model or software implementation.

---

## 2. Naming Principles

The project follows these principles:

- One business concept must have one official name.
- One official name must not represent multiple distinct business concepts.
- Software names should reflect the established business terminology.
- Database names should reflect the same terminology whenever appropriate.
- Technical terms must not replace established business language when describing business concepts.
- Terms belonging to different Bounded Contexts must retain their meaning within their respective contexts.
- A new term must be reviewed before being introduced into the software implementation.
- A term's software representation must not imply an Aggregate or Entity boundary that has not been established by Tactical Design.

---

## 3. Business Terms

| Business Term | Definition | Software Representation | Notes |
|---|---|---|---|
| `Photographer` | The business owner who provides photography services and manages the platform | `Photographer` | Primary business operator and platform administrator |
| `Visitor` | A person browsing the public platform without having submitted a booking request | Not persisted as a domain entity | Anonymous visitor |
| `Client` | A person identified by the platform through submitting a booking request | `Client` | A visitor becomes a client after submitting a booking request; this does not automatically create an authenticated account |
| `Inquiry` | A communication initiated by a visitor or client to request information | `Inquiry` | Independently managed from Booking |
| `Photography Service` | A photography or related service offered by the photographer | `PhotographyService` | Examples include weddings, events, product photography, and editing |
| `Package` | A predefined offering containing one or more Photography Services with agreed pricing and features | `Package` | A package can be selected as part of a booking |
| `Booking` | A business record representing a client's request and its subsequent lifecycle through approval, confirmation, service delivery, and closure | `Booking` | Aggregate Root owned by Booking Management |
| `Booking Request` | The initial request submitted by a client for photography services, before a confirmed booking agreement exists | Booking lifecycle concept | Not a separate Aggregate or independent business record |
| `Booking Agreement` | The agreed business arrangement between the photographer and client concerning the photography services, sessions, price, and other applicable terms | Part of `Booking` | Established through the booking approval and confirmation process |
| `Booking Terms` | The agreed details governing a booking, including services, sessions, price, and other applicable conditions | Part of `Booking` | Owned and managed by Booking Management |
| `Photography Session` | A scheduled photography activity belonging to a booking | `PhotographySession` | Internal Entity within the Booking Aggregate; a booking may contain multiple sessions |
| `Payment` | A monetary transaction recorded against a booking and independently verified by the photographer | `Payment` | Aggregate Root owned by Payment Management |
| `Refund` | A confirmed return of money associated with a previously confirmed payment | Internal Entity within `Payment` | Refund confirmation is owned by Payment Management |
| `Financial Position` | The calculated financial standing of a booking based on its agreed total, confirmed payments, and confirmed refunds | Financial Position Projection | Owned by Payment Management; not an Aggregate Root |
| `Booking Financial Snapshot` | A snapshot of relevant financial information maintained by Booking Management for evaluating booking lifecycle requirements | `BookingFinancialSnapshot` | Not the authoritative financial record |
| `Agreed Total` | The current total price agreed upon by the photographer and client for a booking | Booking term / financial input | Changes are managed by Booking Management |
| `Net Amount Paid` | The cumulative amount of confirmed payments minus confirmed refunds | Financial calculation | Used when calculating the Financial Position |
| `Remaining Balance` | The amount still owed against the current Agreed Total | Financial calculation | Agreed Total minus Net Amount Paid |
| `Invoice` | A financial document generated for an individually confirmed payment | `Invoice` | Aggregate Root owned by Invoice Management; one invoice per confirmed payment |
| `Invoice Generation` | The process of creating an invoice for a confirmed payment | Invoice Management operation | Independent of invoice delivery |
| `Invoice Delivery` | The process of delivering a generated invoice to the client | Notification / delivery capability | Delivery failure does not invalidate the generated invoice |
| `Portfolio` | The public showcase of the photographer's previous work | Portfolio concept | Owned by Portfolio Management |
| `Album` | A collection of portfolio images organized for public presentation | `Album` | Aggregate Root owned by Portfolio Management |
| `Business Profile` | The business information presented publicly on the platform | `BusinessProfile` | Aggregate Root owned by Business Information |
| `Notification` | A record representing a notification managed by the platform | `Notification` | Aggregate Root owned by Notification Management |
| `Category` | A classification used to organize relevant business offerings or portfolio content | Context-dependent representation | Its meaning and ownership depend on the context in which it is used |

---

## 4. Booking Lifecycle Terminology

The following terms describe the business states of a Booking.

| Term | Definition |
|---|---|
| `Submitted` | A booking request has been submitted and is awaiting photographer review |
| `Additional Information Requested` | The photographer requires additional information before deciding on the booking request |
| `Approved` | The photographer has approved the booking request, which is awaiting confirmation of the required initial payment |
| `Confirmed` | The required initial payment has been confirmed and Booking Management has confirmed the booking |
| `In Progress` | At least one Photography Session belonging to the booking has started |
| `Completed` | All required Photography Sessions have been completed and the Remaining Balance is zero |
| `Rejected` | The photographer has rejected the booking request |
| `Cancelled` | A booking request has been withdrawn before the booking becomes confirmed |
| `Terminated` | An established booking agreement has been terminated after confirmation |

### 4.1 Cancelled vs. Terminated

These terms represent different business outcomes and must not be used interchangeably.

**Cancelled**

- Applies to a booking request before confirmation.
- May result from a client's direct cancellation when permitted.
- After approval, the client may request withdrawal, but the photographer must accept and execute it.
- Does not represent termination of an established booking agreement.

**Terminated**

- Applies to an established booking agreement after confirmation.
- May occur before or after Photography Sessions have started.
- May have financial consequences, including compensation or refunds.
- Does not automatically mean that a refund has been confirmed.

### 4.2 Approved vs. Confirmed

**Approved** means that the photographer has accepted the booking request and authorized the client to proceed with the required initial payment.

**Confirmed** means that the required initial payment has been confirmed by Payment Management and Booking Management has satisfied its own confirmation requirements.

Approval and confirmation are separate business facts.

### 4.3 Completed vs. Terminated

**Completed** means that the booking has successfully fulfilled its agreed service and financial completion conditions.

**Terminated** means that an established booking agreement has ended before normal completion.

---

## 5. Financial Terminology

### 5.1 Payment States and Financial Facts

| Term | Definition |
|---|---|
| `Payment Recorded` | A payment transaction has been recorded but has not yet been confirmed |
| `Payment Confirmed` | A recorded payment has been verified and accepted by the photographer |
| `Refund Confirmed` | A refund has been verified and accepted as an actual financial transaction |

A recorded payment must not be treated as confirmed money received.

Only confirmed payments and confirmed refunds contribute to the Financial Position.

### 5.2 Financial Position

The Financial Position represents the calculated financial standing of a booking.

It is based on:

**Net Amount Paid = Confirmed Payments − Confirmed Refunds**

**Remaining Balance = Agreed Total − Net Amount Paid**

Payment Management owns this calculation and its underlying financial facts.

Booking Management may maintain a Booking Financial Snapshot to evaluate booking lifecycle conditions, but that snapshot is not the authoritative financial record.

### 5.3 Invoice

An Invoice is generated for each individually confirmed payment.

Each invoice preserves the relevant financial information at the time of generation, including:

- The individual confirmed payment.
- The cumulative amount paid.
- The remaining balance.

An invoice is generated independently of whether the booking has been fully paid.

Invoice generation and Invoice Delivery are separate activities.

---

## 6. Domain Design Terminology

The following terms have specific meanings in the project's domain design.

| Term | Definition | Project Usage |
|---|---|---|
| `Bounded Context` | A boundary within which a particular domain model and its terminology have a consistent meaning | Defines the logical business modules of the platform |
| `Aggregate` | A cluster of domain objects treated as a consistency boundary | Defines transactional consistency within a domain model |
| `Aggregate Root` | The main entity through which an Aggregate is accessed and its rules are enforced | Examples include Booking, Payment, and Invoice |
| `Entity` | A domain object identified by its identity and lifecycle | PhotographySession is an Entity within Booking |
| `Value Object` | An immutable domain concept defined by its values rather than identity | BookingFinancialSnapshot is a candidate Value Object |
| `Domain Event` | A business fact that has already occurred within a Bounded Context | Examples include PaymentConfirmed and BookingConfirmed |
| `Integration Event` | A business fact published for other Bounded Contexts to consume | Examples include PaymentConfirmed and BookingPriceChanged when communicated across contexts |
| `Financial Position Projection` | A calculated representation derived from underlying financial facts | Not an Aggregate Root |
| `Transactional Outbox` | A reliability pattern used to persist business changes and pending event publication together | Used for reliable cross-context event publication |

---

## 7. Bounded Context Terminology

The following names are the official Bounded Context names established by Strategic Design.

| Bounded Context | Responsibility |
|---|---|
| `Booking Management` | Owns booking requests, booking agreements, booking terms, lifecycle decisions, and Photography Sessions |
| `Payment Management` | Owns payment recording, confirmation, refunds, and the Financial Position |
| `Client Management` | Owns client records and client information |
| `Services & Packages` | Owns Photography Services and Packages |
| `Invoice Management` | Owns invoice generation and invoice records |
| `Inquiry Management` | Owns inquiries submitted by visitors or clients |
| `Portfolio Management` | Owns albums and portfolio publication |
| `Business Information` | Owns the public business profile |
| `Notification Management` | Owns notification orchestration and delivery tracking |

These are logical contexts within one Laravel monolithic application. They do not represent separate deployable services.

Generic capabilities such as Authentication & Authorization, Localization, and File Storage are not automatically treated as Bounded Contexts.

---

## 8. Relationships Between Terms

The following conceptual relationships summarize the main business concepts:

```text
Visitor
   |
   +---- Inquiry
   |
   +---- Booking Request
             |
             v
           Booking
             |
             +---- Booking Agreement / Terms
             |
             +---- Photography Session(s)
             |
             +---- Payment Reference(s)
                         |
                         v
                      Payment
                         |
                         +---- Refund(s)
                         |
                         v
                  Financial Position
                         |
                         v
                     Invoice(s)
```

**Important relationship notes:**

- A Visitor may submit an Inquiry without becoming a Client.
- A Visitor becomes a Client after submitting a Booking Request.
- A Booking Request is the initial stage of a Booking, not a separate Aggregate.
- Booking Management owns the Booking and its Photography Sessions.
- Payment Management owns independent Payment Aggregates.
- A Payment may contain one or more Refund Entities.
- The Financial Position is calculated by Payment Management; it is not an independent Aggregate.
- Invoice Management generates one Invoice for each individually confirmed Payment.
- The diagram is conceptual and does not imply that Payments or Invoices are contained within the Booking Aggregate.

---

## 9. Synonyms and Forbidden Terms

The following alternatives should be avoided when referring to the corresponding business concepts.

| Avoid | Use Instead | Reason |
|---|---|---|
| Customer | Client | Official business terminology |
| Reservation | Booking | The platform manages a broader booking lifecycle |
| Order | Booking | The domain does not represent bookings as eCommerce orders |
| Admin | Photographer | Identifies the actual business actor |
| User | Visitor / Client / Photographer | Distinguishes the different business roles |
| Gallery | Portfolio or Album | Use the appropriate concept according to its meaning |
| Payment Confirmation Record | Payment | Payment is the financial transaction, not merely its confirmation record |
| Appointment | Photography Session | Represents the actual photography activity |
| Event | Photography Session | Avoids confusion with general events and business events |
| Booking Confirmation | BookingConfirmed | Refers to the confirmation event when discussing the business fact |
| Booking Payment | Payment | A Payment is an independent financial transaction associated with a Booking |
| Cancelled Agreement | Terminated Agreement | Use Terminated for an established booking agreement that has ended |
| Fully Paid Booking | Completed Booking | A fully paid booking is not necessarily completed until all required sessions are finished |
| Invoice Email | Invoice Delivery | Separates the financial document from its delivery process |
| Booking Balance | Remaining Balance | Uses the established financial terminology |
| Refund Request | Refund | Use Refund only when referring to the financial transaction/entity; a request is not a confirmed refund |

---

## 10. Naming Conventions

Software names should use the established business terminology and follow the conventions of the implementation technology.

### 10.1 Software Classes

The following names are established domain concepts or representations:

```text
Booking
PhotographySession
Client
Inquiry
PhotographyService
Package
Payment
Refund
Invoice
Album
BusinessProfile
Notification
BookingFinancialSnapshot
```

The following names represent domain concepts or operations rather than necessarily requiring standalone classes:

```text
BookingAgreement
BookingTerms
FinancialPosition
NetAmountPaid
RemainingBalance
```

The exact class, interface, and service organization will be established during implementation design.

### 10.2 Database Tables

The following are candidate table names based on established domain concepts:

```text
bookings
photography_sessions
clients
inquiries
photography_services
packages
payments
refunds
invoices
albums
business_profiles
notifications
```

These names are terminology references, not a finalized database schema.

The final table structure, including whether a concept is represented by a dedicated table, columns, or another persistence mechanism, will be established during Database Design.

### 10.3 Domain Events

Domain Event names should describe past business facts using the established terminology.

Examples:

```text
BookingRequestSubmitted
BookingRequestApproved
BookingRequestCancelled
BookingConfirmed
PhotographySessionStarted
PhotographySessionCompleted
BookingPriceChanged
BookingAgreementTerminated
BookingCompleted
PaymentRecorded
PaymentConfirmed
RefundConfirmed
InvoiceGenerated
```

Event ownership and cross-context communication are defined in:

`docs/02-analysis/04-domain-events.md`

---

## 11. Future Extensions

New business concepts must be reviewed and documented here before being introduced into the domain model or software implementation.

Any terminology change must be reflected in all related documentation to preserve consistency across the project.

A change to a term must also be evaluated for its impact on:

- Domain Models
- Bounded Contexts
- Aggregate boundaries
- Domain Events
- Business Rules
- Use Cases
- Database Design
- Application Services

---

## 12. Related Documents

- `docs/02-analysis/01-business-process.md`
- `docs/02-analysis/02-use-cases.md`
- `docs/02-analysis/03-booking-lifecycle.md`
- `docs/02-analysis/04-domain-events.md`
- `docs/02-analysis/06-domain-model.md`
- `docs/04-design/01-strategic-design.md`
- `docs/04-design/02-tactical-design.md`

---

## 13. Change History

| Version | Description |
|---|---|
| 1.0 | Established the initial business vocabulary and naming principles. |
| 2.0 | Aligned terminology with the approved Strategic and Tactical Design; clarified Booking, Booking Request, Booking Agreement, Financial Position, BookingFinancialSnapshot, Refund, and the distinction between Cancelled and Terminated. |

