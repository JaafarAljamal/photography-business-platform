# Domain Events

## 1. Purpose

This document identifies the significant business events that occur within the Photography Business Platform.

A Domain Event represents a business fact that has already occurred and is meaningful within the domain.

The purpose of identifying Domain Events is to:

- capture significant changes in the business domain;
- connect business events to the Booking Lifecycle;
- identify reactions between different parts of the domain;
- clarify event ownership across Bounded Contexts;
- distinguish internal domain events from cross-context integration events;
- provide a foundation for application behavior and business rules.

Domain Events are described from a business perspective. This document identifies their meaning, ownership, and business significance without defining implementation-specific event payloads or infrastructure.

---

## 2. Domain Event Principles

### 2.1 Events Represent Facts

A Domain Event records something that has already happened.

A Command represents an action requested by an actor or another part of the system. A command may succeed or fail, whereas an event records a fact resulting from successfully applying the relevant business rules.

For example:

- `ConfirmPayment` is a command.
- `PaymentConfirmed` is the resulting business event.

### 2.2 Event Ownership

Every Domain Event belongs to the Bounded Context responsible for the business fact it represents.

A context may publish selected events to other contexts when those contexts need to react to the business fact.

The receiving context does not take ownership of the originating event or of the business decision that produced it.

### 2.3 Internal and Cross-Context Events

Not every Domain Event needs to be communicated outside its owning Bounded Context.

This document distinguishes between:

- **Internal Domain Events:** meaningful within their owning context and not intended as cross-context communication.
- **Cross-Context Integration Events:** business facts made available to other Bounded Contexts when they need to react to them.

An event may be meaningful within its owning context while also being published as an integration event. Its ownership remains with the originating context.

### 2.4 Event Significance

Only meaningful business facts are identified as Domain Events.

Routine operations, technical activities, and changes that do not represent significant business facts are not automatically treated as Domain Events.

---

## 3. Booking Management Events

Booking Management owns the Booking lifecycle, agreed booking terms, and the management of required Photography Sessions.

The following events represent significant facts within this context.

### 3.1 BookingRequestSubmitted

**Meaning**

A client has successfully submitted a booking request containing the required information for photographer review.

**Triggered By**

Client.

**Business Fact**

A new booking request has entered the booking lifecycle and is available for photographer review.

**Source Use Case**

UC-021 — Submit Booking Request.

**Business Significance**

This event marks the transition from preparing a booking request to submitting a request that can be processed by the photographer.

### 3.2 BookingRequestUpdated

**Meaning**

A client has successfully updated a submitted booking request before the booking is confirmed.

**Triggered By**

Client.

**Business Fact**

The information associated with the booking request has changed and the updated request is available for photographer review.

**Source Use Case**

UC-023 — Update Booking Request.

**Business Significance**

This event records a meaningful change to a pending booking request and indicates that the photographer may need to review the updated information.

### 3.3 BookingRequestCancelled

**Meaning**

A client has cancelled a submitted booking request before the booking is confirmed.

**Triggered By**

Client.

**Business Fact**

The booking request has been withdrawn and is no longer subject to photographer review.

**Source Use Case**

UC-024 — Cancel Booking Request.

**Business Significance**

This event marks the end of the booking request lifecycle without the request becoming a confirmed booking.

### 3.4 AdditionalBookingInformationRequested

**Meaning**

The photographer has determined that additional information is required before making a decision about a booking request.

**Triggered By**

Photographer.

**Business Fact**

Additional information is now required from the client before the booking request can proceed.

**Source Use Case**

UC-025 — Request Additional Booking Information.

**Business Significance**

This event indicates that the booking request cannot yet proceed to a final decision because additional information is required.

### 3.5 AdditionalBookingInformationProvided

**Meaning**

The client has successfully provided the additional booking information requested by the photographer.

**Triggered By**

Client.

**Business Fact**

The requested information has been provided and the booking request can return to the photographer's review process.

**Source Use Case**

UC-026 — Submit Additional Booking Information.

**Business Significance**

This event indicates that the information gap identified during booking review has been resolved.

### 3.6 BookingRequestApproved

**Meaning**

The photographer has approved a booking request and authorized the client to proceed with the required initial payment.

**Triggered By**

Photographer.

**Business Fact**

The booking request has been approved but the booking has not yet been confirmed.

**Source Use Case**

UC-029 — Approve Booking Requests.

**Business Significance**

This event marks the transition from photographer review to an approved booking request awaiting the required payment for booking confirmation.

### 3.7 BookingRequestRejected

**Meaning**

The photographer has rejected a booking request.

**Triggered By**

Photographer.

**Business Fact**

The booking request has been rejected and will not become a confirmed booking.

**Source Use Case**

UC-030 — Reject Booking Requests.

**Business Significance**

This event marks the end of the booking request lifecycle without establishing a confirmed booking.

### 3.8 BookingConfirmed

**Meaning**

The booking has satisfied the business requirements necessary for confirmation.

**Triggered By**

Booking Management, after processing the relevant payment confirmation and verifying that the initial payment requirement has been satisfied.

**Business Fact**

The booking has entered the Confirmed state.

**Related Event**

`PaymentConfirmed`, originating from Payment Management.

**Source Use Case**

UC-035 — Confirm Payment.

**Business Significance**

This event marks the transition from an approved booking request awaiting the required payment to a confirmed booking.

`BookingConfirmed` is a Booking Management decision. A confirmed payment does not automatically mean that the booking has been confirmed; Booking Management must evaluate its own confirmation requirements.

### 3.9 PhotographySessionStarted

**Meaning**

A Photography Session belonging to a confirmed booking has started.

**Triggered By**

Photographer.

**Business Fact**

The specific Photography Session has started.

**Source Use Case**

UC-031 — Manage Confirmed Bookings.

**Business Significance**

This event marks the beginning of an individual Photography Session. It may also cause the Booking to enter the In Progress state, depending on its current lifecycle state.

### 3.10 PhotographySessionCompleted

**Meaning**

A Photography Session belonging to a booking has been completed.

**Triggered By**

Photographer.

**Business Fact**

The specific Photography Session has finished.

**Source Use Case**

UC-031 — Manage Confirmed Bookings.

**Business Significance**

This event records the completion of an individual Photography Session.

A booking containing multiple Photography Sessions does not become Completed merely because one session has been completed.

**Event Scope**

This is an internal Domain Event within Booking Management. It is not a cross-context integration event because PhotographySession is an internal Entity within the Booking Aggregate.

### 3.11 BookingPriceChanged

**Meaning**

The agreed total price of a booking has been changed following an agreement between the photographer and the client.

**Triggered By**

Photographer, after agreement with the client.

**Business Fact**

The booking's agreed financial terms have changed from the previous total to a new total.

**Business Significance**

This event records a change to the financial terms owned by Booking Management.

Payment Management must be informed of the change because the agreed total affects the financial position and remaining balance associated with the booking.

The event does not itself record a payment or modify an existing payment.

### 3.12 BookingAgreementTerminated

**Meaning**

The agreement associated with a booking has been terminated following the applicable business decision.

**Triggered By**

Photographer, after the termination decision has been established according to the applicable business rules and agreement with the client where required.

**Business Fact**

The booking agreement has been terminated.

**Business Significance**

This event records the termination of the booking agreement and allows other contexts, particularly Payment Management and Notification Management, to react according to their responsibilities.

Termination does not itself represent a refund. Any refund must be evaluated and confirmed separately by Payment Management.

### 3.13 BookingCompleted

**Meaning**

The booking has reached the Completed state after all required Photography Sessions have been completed and the remaining booking balance is zero.

**Triggered By**

Booking Management, after the booking completion conditions have been satisfied.

**Source Use Case**

UC-031 — Manage Confirmed Bookings.

**Business Fact**

The booking has successfully completed its lifecycle.

**Business Significance**

This event marks the successful completion of the booking.

---

## 4. Payment Management Events

Payment Management owns payment recording, confirmation, and refund confirmation.

Payment is an independent Aggregate Root and is not part of the Booking Aggregate.

### 4.1 PaymentRecorded

**Meaning**

A payment transaction has been recorded against a booking.

**Triggered By**

Photographer.

**Business Fact**

A payment transaction has been recorded and is awaiting confirmation.

**Source Use Case**

UC-034 — Record Payment.

**Business Significance**

This event records a new payment transaction without treating it as confirmed payment.

A recorded but unconfirmed payment does not contribute to the confirmed financial position of the booking.

### 4.2 PaymentConfirmed

**Meaning**

A recorded payment has been verified and confirmed by the photographer.

**Triggered By**

Photographer.

**Business Fact**

The payment has been confirmed and is now included in the financial position associated with its booking.

**Source Use Case**

UC-035 — Confirm Payment.

**Business Significance**

This event records the business fact that a payment has been verified and accepted.

It may cause:

- the financial position of the booking to be recalculated;
- Booking Management to evaluate whether the initial payment requirement has been satisfied;
- Invoice Management to generate an invoice for the confirmed payment;
- Notification Management to initiate relevant notifications.

**Event Scope**

`PaymentConfirmed` is owned by Payment Management and is made available as a cross-context integration event to the contexts that need to react to it.

The confirmation of a payment does not itself confirm the booking or generate an invoice. Those are separate business responsibilities.

### 4.3 RefundConfirmed

**Meaning**

A refund associated with a previously confirmed payment has been verified and confirmed.

**Triggered By**

Photographer, after verifying that the refund has been carried out through the relevant external payment channel.

**Business Fact**

A refund has been confirmed and must be reflected in the financial position associated with the booking.

**Business Significance**

This event records the financial fact that a refund has been confirmed.

It allows the financial position to be recalculated and enables other contexts to react where necessary.

A refund does not automatically change the booking's lifecycle state. Any resulting booking decision remains the responsibility of Booking Management.

**Event Scope**

This event is owned by Payment Management and is made available to other contexts when they require the updated financial information or need to react to the refund.

---

## 5. Invoice Management Events

Invoice Management owns invoice generation and the business records associated with generated invoices.

### 5.1 InvoiceGenerated

**Meaning**

An invoice has been successfully generated for an individually confirmed payment.

**Triggered By**

Invoice Management, after processing the relevant `PaymentConfirmed` event.

**Business Fact**

A valid invoice now exists for the confirmed payment.

**Business Significance**

This event records the creation of the financial document associated with a specific confirmed payment.

The invoice represents that individual payment and preserves the relevant financial information at the time of generation, including:

- the individual confirmed payment;
- the cumulative amount paid;
- the remaining balance.

**Business Rule**

One invoice is generated for each individually confirmed payment.

Invoice generation does not depend on the booking being fully paid.

**Event Scope**

This event is owned by Invoice Management.

Invoice generation and invoice delivery are separate business activities. `InvoiceGenerated` does not mean that the invoice has been successfully delivered to the client.

---

## 6. Event Ownership and Cross-Context Communication

The following table summarizes the ownership and communication scope of the identified events.

| Bounded Context | Event | Scope |
|---|---|---|
| Booking Management | BookingRequestSubmitted | Internal |
| Booking Management | BookingRequestUpdated | Internal |
| Booking Management | BookingRequestCancelled | Internal |
| Booking Management | AdditionalBookingInformationRequested | Internal |
| Booking Management | AdditionalBookingInformationProvided | Internal |
| Booking Management | BookingRequestApproved | Internal |
| Booking Management | BookingRequestRejected | Internal |
| Booking Management | BookingConfirmed | Cross-context, when relevant |
| Booking Management | PhotographySessionStarted | Internal |
| Booking Management | PhotographySessionCompleted | Internal |
| Booking Management | BookingPriceChanged | Cross-context |
| Booking Management | BookingAgreementTerminated | Cross-context |
| Booking Management | BookingCompleted | Cross-context, when relevant |
| Payment Management | PaymentRecorded | Internal |
| Payment Management | PaymentConfirmed | Cross-context |
| Payment Management | RefundConfirmed | Cross-context |
| Invoice Management | InvoiceGenerated | Cross-context, when relevant |

**Notes:**

- Internal events remain within their owning Bounded Context.
- Cross-context events communicate business facts; they do not transfer ownership of the originating business rules.
- The table identifies the intended communication scope, not a requirement that every event must be consumed by another context.
- Notification Management may react to relevant business events without becoming the owner of those events.

---

## 7. Event Flows

### 7.1 Booking Request Flow

The primary booking request flow is:

```text
BookingRequestSubmitted
        |
        v
     Submitted
        |
        +---- AdditionalBookingInformationRequested
        |                |
        |                v
        |   Additional Information Requested
        |                |
        |                v
        |   AdditionalBookingInformationProvided
        |                |
        |                v
        |             Submitted
        |
        +---- BookingRequestApproved
        |                |
        |                v
        |             Approved
        |                |
        |                v
        |         PaymentConfirmed
        |                |
        |                v
        |   Initial Payment Requirement Met
        |                |
        |                v
        |         BookingConfirmed
        |                |
        |                v
        |             Confirmed
        |
        +---- BookingRequestRejected
        |                |
        |                v
        |             Rejected
        |
        +---- BookingRequestCancelled
                         |
                         v
                      Cancelled
```

### 7.2 Booking Service Delivery Flow

After confirmation, the booking enters the service delivery phase.

```text
Confirmed
    |
    v
PhotographySessionStarted
    |
    v
In Progress
    |
    +---- PhotographySessionCompleted
    |
    +---- PhotographySessionCompleted
    |
    +---- ...
    |
    v
All Required Photography Sessions Completed
    +
Remaining Balance = 0
    |
    v
BookingCompleted
    |
    v
Completed
```

The completion of an individual session is an internal event. Booking Management evaluates the completion of all required sessions together with the remaining balance before completing the booking.

### 7.3 Payment and Financial Position Flow

Payment confirmation and refund confirmation affect the financial position maintained by Payment Management.

```text
PaymentRecorded
       |
       v
PaymentConfirmed
       |
       v
Financial Position Recalculated
       |
       +---- Booking Management
       |          |
       |          v
       |   Evaluate Initial Payment Requirement
       |          |
       |          v
       |   BookingConfirmed, if requirements are met
       |
       +---- Invoice Management
       |          |
       |          v
       |      InvoiceGenerated
       |
       +---- Notification Management
                  |
                  v
          Relevant Notifications
```

Refunds follow a separate flow:

```text
Refund Confirmed
       |
       v
RefundConfirmed
       |
       v
Financial Position Recalculated
       |
       +---- Booking Management, when relevant
       |
       +---- Notification Management, when relevant
```

The financial position is based on confirmed payments and confirmed refunds. Pending or rejected transactions are not included.

### 7.4 Booking Terms and Agreement Changes

Changes to the booking agreement may affect Payment Management and other interested contexts.

```text
Booking Terms Changed
       |
       v
BookingPriceChanged
       |
       v
Payment Management Recalculates
Financial Position
```

Termination follows a separate flow:

```text
Booking Agreement Terminated
       |
       v
BookingAgreementTerminated
       |
       +---- Payment Management
       |          |
       |          v
       |   Evaluate Financial Consequences
       |          |
       |          v
       |   Refund, if applicable
       |          |
       |          v
       |   RefundConfirmed
       |
       +---- Notification Management
                  |
                  v
          Relevant Notifications
```

The termination of an agreement and the confirmation of a refund are distinct business facts. A refund is not assumed to have occurred merely because an agreement has been terminated.

### 7.5 Invoice Generation and Delivery

Invoice generation follows payment confirmation:

```text
PaymentConfirmed
       |
       v
InvoiceGenerated
       |
       v
Invoice Delivery Requested
       |
       v
Delivery Attempt
       |
       +---- Successful Delivery
       |
       +---- Failed Delivery / Retry
```

The delivery process is separate from invoice generation. A delivery failure does not invalidate an already generated invoice.

---

## 8. Important Domain Distinctions

### 8.1 Payment Confirmation vs. Booking Confirmation

`PaymentConfirmed` and `BookingConfirmed` represent different business facts.

- `PaymentConfirmed` means that a specific payment has been verified and accepted by Payment Management.
- `BookingConfirmed` means that Booking Management has determined that the booking satisfies its confirmation requirements.

A confirmed payment may lead to `BookingConfirmed`, but the two events are not interchangeable.

### 8.2 Payment Confirmation vs. Invoice Generation

`PaymentConfirmed` records a financial fact owned by Payment Management.

`InvoiceGenerated` records the creation of an invoice owned by Invoice Management.

Invoice Management reacts to the confirmed payment, but it independently owns the invoice generation process.

### 8.3 Session Completion vs. Booking Completion

`PhotographySessionCompleted` records the completion of one individual Photography Session.

`BookingCompleted` records the completion of the entire booking.

A booking can contain multiple Photography Sessions. Therefore, completing one session does not automatically complete the booking.

Booking completion requires:

- all required Photography Sessions to be completed;
- the remaining booking balance to be zero.

### 8.4 Payment Confirmation vs. Refund Confirmation

`PaymentConfirmed` records money received and verified.

`RefundConfirmed` records money returned and verified.

Both events affect the booking's financial position, but they represent different financial facts.

### 8.5 Booking Agreement Termination vs. Refund Confirmation

`BookingAgreementTerminated` records a decision concerning the booking agreement.

`RefundConfirmed` records a financial transaction that has actually been carried out and verified.

Terminating an agreement does not automatically mean that a refund has been issued or confirmed.

### 8.6 Booking Price Change vs. Payment

`BookingPriceChanged` records a change to the agreed booking total.

It does not represent a payment, refund, or financial transaction.

Payment Management uses the updated agreed total when calculating the booking's financial position.

### 8.7 Fully Paid Condition

The booking being fully paid is treated as a financial condition rather than a separate Domain Event.

When a payment or refund is confirmed, Payment Management recalculates the financial position.

If the remaining balance becomes zero, Booking Management can use that financial fact when evaluating its completion requirements.

No separate `BookingFullyPaid` event is currently defined.

This decision may be revisited if a distinct business behavior requiring such an event is identified.

---

## 9. Event Consistency and Reliability

Cross-context events communicate business facts between independently responsible Bounded Contexts.

The following principles apply:

- Each Bounded Context remains responsible for its own business state.
- A receiving context reacts to an event according to its own business rules.
- Cross-context communication must not require a single database transaction spanning multiple contexts.
- Event consumers must tolerate duplicate event delivery.
- Consumers must account for stale or out-of-order information when processing events that affect financial positions or booking terms.
- Events that affect financial calculations must provide sufficient information to interpret the relevant financial position and its revision.

The technical mechanism for reliable event publication and consumption is defined in the Tactical Design document. Cross-context event contracts and their detailed payloads are refined during Detailed Design.

---

## 10. Relationship to Other Domain Artifacts

This document must remain consistent with:

- `docs/02-analysis/01-business-process.md`
- `docs/02-analysis/02-use-cases.md`
- `docs/02-analysis/03-booking-lifecycle.md`
- `docs/02-analysis/05-ubiquitous-language.md`
- `docs/02-analysis/06-domain-model.md`
- `docs/04-design/01-strategic-design.md`
- `docs/04-design/02-tactical-design.md`

The Domain Model identifies the important domain concepts and their relationships.

The Booking Lifecycle describes the progression of a booking.

The Strategic Design establishes Bounded Context boundaries and event ownership.

The Tactical Design establishes Aggregate boundaries, internal event scope, and cross-context interaction responsibilities.

This document identifies the significant business events that connect these domain artifacts.

---

## 11. Change History

| Version | Description |
|---|---|
| 1.0 | Identified the initial conceptual Domain Events derived from business requirements and analysis. |
| 2.0 | Aligned event ownership and scope with the approved Strategic and Tactical Design; added BookingPriceChanged, BookingAgreementTerminated, and RefundConfirmed; clarified internal and cross-context event responsibilities. |
