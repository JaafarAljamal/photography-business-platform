# Domain Events

## 1. Purpose

This document identifies the significant business events that occur within the Photography Business Platform.

A Domain Event represents a business fact that has already occurred and is meaningful within the domain.

The purpose of identifying Domain Events is to:

- capture significant changes in the business domain;
- connect business events to the Booking Lifecycle;
- identify reactions between different parts of the domain;
- provide a foundation for Tactical Design;
- support the later identification of business rules and cross-aggregate interactions.

Domain Events are described from a business perspective and intentionally avoid implementation details.

---

## 2. Domain Event Discovery

Domain Events were identified by analyzing:

- Functional Requirements
- Business Process Documentation
- Use Cases
- Booking Lifecycle
- Ubiquitous Language
- Domain Model

The discovery process focuses on significant business facts rather than every system operation.

A Domain Event describes a business fact that has already occurred, while a Command represents an action requested by an actor.

The distinction is important: a requested action does not necessarily result in a Domain Event. The relevant business rules must first be satisfied, and the resulting event records the business fact that actually occurred.

The Commands and their relationships with Domain Events will be refined during Tactical Design.

---

## 3. Booking Domain Events

### 3.1 BookingRequestSubmitted

**Meaning**

A client has successfully submitted a booking request containing the required information for photographer review.

**Triggered By**

Client

**Business Fact**

A new booking request has entered the booking lifecycle and is now available for photographer review.

**Source Use Case**

UC-021 — Submit Booking Request

**Business Significance**

This event marks the transition from a client preparing a booking request to a submitted booking request that can be processed by the photographer.

---

### 3.2 BookingRequestUpdated

**Meaning**

A client has successfully updated a submitted booking request before the booking is confirmed.

**Triggered By**

Client

**Business Fact**

The information associated with the booking request has changed and the updated request is available for Photographer review.

**Source Use Case**

UC-023 — Update Booking Request

**Business Significance**

This event records a meaningful change to a pending booking request and indicates that the Photographer may need to review the updated information before making a booking decision.

---

### 3.3 BookingRequestCancelled

**Meaning**

A client has cancelled a submitted booking request before the booking is confirmed.

**Triggered By**

Client

**Business Fact**

The booking request has been withdrawn and is no longer subject to photographer review.

**Source Use Case**

UC-024 — Cancel Booking Request

**Business Significance**

This event marks the end of the booking request lifecycle without the request becoming an approved or confirmed booking.

---

### 3.4 AdditionalBookingInformationRequested

**Meaning**

The Photographer has determined that additional information is required before making a decision about a booking request.

**Triggered By**

Photographer

**Business Fact**

Additional information is now required from the Client before the booking request can proceed.

**Source Use Case**

UC-025 — Request Additional Booking Information

**Business Significance**

This event indicates that the booking request cannot yet proceed to a final decision because additional information is required.

---

### 3.5 AdditionalBookingInformationProvided

**Meaning**

The Client has successfully provided the additional booking information requested by the Photographer.

**Triggered By**

Client

**Business Fact**

The requested information has been provided and the booking request can return to the photographer's review process.

**Source Use Case**

UC-026 — Submit Additional Booking Information

**Business Significance**

This event indicates that the information gap identified during booking review has been resolved and the booking request can continue through its lifecycle.

---

### 3.6 BookingRequestApproved

**Meaning**

The Photographer has approved a booking request and authorized the Client to proceed with the required initial payment.

**Triggered By**

Photographer

**Business Fact**

The booking request has been approved but the booking has not yet been confirmed.

**Source Use Case**

UC-029 — Approve Booking Requests

**Business Significance**

This event marks the transition from Photographer review to an approved booking request awaiting the required payment for booking confirmation.

---

### 3.7 BookingRequestRejected

**Meaning**

The Photographer has rejected a booking request.

**Triggered By**

Photographer

**Business Fact**

The booking request has been rejected and will not become a confirmed booking.

**Source Use Case**

UC-030 — Reject Booking Requests

**Business Significance**

This event marks the end of the booking request lifecycle without establishing a confirmed booking.

---

### 3.8 BookingConfirmed

**Meaning**

The required initial payment has been confirmed and the Booking has satisfied the business requirement for confirmation.

**Triggered By**

Booking domain after PaymentConfirmed satisfies the initial payment requirement.

**Business Fact**

The Booking has satisfied the initial payment requirement and has entered the Confirmed state.

**Related Domain Event**

PaymentConfirmed

**Source Use Case**

UC-035 — Confirm Payment

**Business Significance**

This event marks the transition from an approved booking request awaiting payment to a confirmed Booking.

---

### 3.9 PhotographySessionStarted

**Meaning**

A Photography Session belonging to a confirmed Booking has started.

**Triggered By**

Photographer

**Business Fact**

The Photography Session has started and the Booking has entered the In Progress state.

**Source Use Case**

UC-031 — Manage Confirmed Bookings

**Business Significance**

This event marks the beginning of an individual Photography Session and may cause the Booking to enter the In Progress state.

---

### 3.10 PhotographySessionCompleted

**Meaning**

A Photography Session belonging to a Booking has been completed.

**Triggered By**

Photographer

**Business Fact**

The specific Photography Session has finished.

**Source Use Case**

UC-031 — Manage Confirmed Bookings

**Business Significance**

This event records the completion of an individual Photography Session.

A Booking containing multiple Photography Sessions does not become Completed merely because one session has been completed.

---

### 3.11 BookingCompleted

**Meaning**

The Photographer has completed the Booking after all required Photography Sessions have finished and the remaining Booking balance is zero.

**Triggered By**

Booking domain after the completion conditions are satisfied.

**Business Fact**

The Booking has reached the Completed state because all required Photography Sessions have been completed and the remaining Booking balance is zero.

**Source Use Case**

UC-031 — Manage Confirmed Bookings

**Business Significance**

This event marks the successful completion of the Booking lifecycle.

---

## 4. Payment Domain Events

### 4.1 PaymentRecorded

**Meaning**

A Payment has been recorded against a Booking.

**Triggered By**

Photographer

**Business Fact**

A payment transaction has been recorded and is awaiting confirmation.

**Source Use Case**

UC-034 — Record Payment

**Business Significance**

This event records a new payment transaction without treating it as confirmed payment.

---

### 4.2 PaymentConfirmed

**Meaning**

A recorded Payment has been verified and confirmed by the Photographer.

**Triggered By**

Photographer

**Business Fact**

The Payment is now confirmed and can contribute to the financial status of its related Booking.

**Source Use Case**

UC-035 — Confirm Payment

**Business Significance**

This event records the business fact that a Payment has been verified and accepted.

The confirmed Payment may cause the Booking balance to be recalculated and may satisfy the Booking confirmation requirement.

---

## 5. Invoice Domain Events

### 5.1 InvoiceGenerated

**Meaning**

An Invoice has been successfully generated for a confirmed Payment.

**Triggered By**

System

**Business Fact**

A valid Invoice now exists for the confirmed Payment.

**Source Use Case**

UC-035 — Confirm Payment

**Business Significance**

This event indicates that the financial document associated with the confirmed Payment has been created and can proceed to delivery.

---

## 6. Event Flow

Domain Events form a sequence of significant business facts throughout the Booking lifecycle.

The primary Booking Request flow is:

```text
BookingRequestSubmitted
        ↓
Submitted
        │
        ├── AdditionalBookingInformationRequested
        │          ↓
        │   Additional Information Requested
        │          │
        │          └── AdditionalBookingInformationProvided
        │                     ↓
        │                  Submitted
        │
        ├── BookingRequestApproved
        │          ↓
        │       Approved
        │          │
        │          └── PaymentConfirmed
        │                     ↓
        │              Confirmation Requirement Met
        │                     ↓
        │              BookingConfirmed
        │                     ↓
        │                 Confirmed
        │
        ├── BookingRequestRejected
        │          ↓
        │       Rejected
        │
        └── BookingRequestCancelled
                   ↓
                Cancelled
```

After confirmation, the Booking enters the service delivery phase:

```text
Confirmed
    ↓
PhotographySessionStarted
    ↓
In Progress
    │
    ├── PhotographySessionCompleted
    │
    ├── PhotographySessionCompleted
    │
    └── ...
    │
    ↓
All Photography Sessions Completed
    +
Remaining Balance = 0
    ↓
BookingCompleted
    ↓
Completed
```

Payment and Invoice events may occur independently during the confirmed Booking lifecycle:

```text
PaymentRecorded
        ↓
PaymentConfirmed
        ↓
Booking Balance Recalculated
        │
        ├── Initial Payment Requirement Satisfied
        │              ↓
        │       BookingConfirmed
        │
        └── InvoiceGenerated
```
> `BookingConfirmed` occurs only when the required initial payment is confirmed.

> `InvoiceGenerated` occurs after each confirmed Payment, according to the business rule defined for invoice generation.

Invoice generation and delivery follow Payment confirmation:

```text
PaymentConfirmed
        ↓
InvoiceGenerated
        ↓
Invoice Delivery
        │
        └── Successful Delivery
```

---

## 7. Design Notes

### Booking and Payment Events

`PaymentConfirmed` and `BookingConfirmed` represent different business facts.

`PaymentConfirmed` means that a specific Payment has been verified and accepted.

`BookingConfirmed` means that the Booking has satisfied the business requirement necessary to become confirmed.

A confirmed Payment may therefore lead to `BookingConfirmed`, but the two events are not interchangeable.

---

### Fully Paid Condition

The Booking being fully paid is currently treated as a financial condition rather than a separate Domain Event.

When a Payment is confirmed, the Booking balance is recalculated.

If the remaining balance becomes zero, the Booking is considered fully paid.

No separate `BookingFullyPaid` event is currently defined.

This decision may be revisited during Tactical Design if a separate business behavior is discovered that depends on the event.

---

### Photography Session Completion

`PhotographySessionCompleted` represents the completion of an individual Photography Session.

Because a Booking may contain multiple sessions, completion of one session does not automatically complete the Booking.

`BookingCompleted` requires:

- all required Photography Sessions to be completed;
- the remaining Booking balance to be zero.

---

### Booking Completion

BookingCompleted represents the business fact that a Booking has reached the Completed state.

The Booking can enter the Completed state only when both conditions are satisfied:

- All required Photography Sessions have been completed.

- The remaining Booking balance is zero.

These conditions are defined by the Booking Lifecycle and will be enforced during Tactical Design.

---

### Invoice Delivery

`InvoiceGenerated` represents the creation of the Invoice.

Email delivery is a separate concern.

A successful Invoice generation does not depend on successful email delivery.

---

### Event Refinement

The Domain Events identified in this document are the current conceptual events derived from the available business artifacts.

They are not yet final implementation events.

Further refinement may occur during Tactical Design when:

- Aggregate boundaries are finalized;
- Business invariants are defined;
- Commands are identified;
- Cross-Aggregate interactions are analyzed;
- Domain Services are identified;
- Event payload requirements are determined.

Any new business concept discovered during this process must first be evaluated against the Ubiquitous Language.

---

