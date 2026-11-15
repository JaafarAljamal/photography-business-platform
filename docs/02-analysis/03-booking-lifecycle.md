# Booking Lifecycle

## 1. Purpose

This document defines the lifecycle of a booking from the moment a client submits a booking request until the booking is completed, rejected, cancelled, or terminated.

The lifecycle standardizes the business states that a booking may pass through during its existence and defines the business conditions governing transitions between those states.

This lifecycle serves as the foundation for:

- Domain Model
- State Diagrams
- Domain Events
- Business Rules
- Application Logic

---

## 2. Scope

This lifecycle applies to every booking managed by the Photography Business Platform Version 1.0.

It covers:

- Client actions related to booking requests.
- Photographer decisions concerning booking requests and agreements.
- Payment-related conditions that affect booking confirmation and completion.
- Photography Session progress.
- Changes to agreed booking terms.
- Booking agreement termination.

Payment recording, confirmation, and refund processing are owned by Payment Management. This document describes how their resulting financial facts affect the Booking lifecycle without transferring payment ownership to Booking Management.

---

## 3. Lifecycle Concepts

### 3.1 Booking Request

A Booking Request represents the client's request for photography services before the photographer has approved it and the required initial payment has been confirmed.

### 3.2 Booking Agreement

Once the booking request is approved and the required initial payment is confirmed, the booking becomes a confirmed photography engagement governed by the agreed terms.

### 3.3 Booking State

A Booking State represents the current business condition of the booking.

State transitions occur only when the applicable business rules are satisfied.

### 3.4 Photography Sessions

A Booking may contain one or more Photography Sessions, potentially scheduled on different days.

Photography Sessions are managed within Booking Management as internal Entities of the Booking Aggregate.

Completing an individual session does not automatically complete the entire booking.

### 3.5 Financial Position

Payment Management owns the authoritative financial facts and calculates the financial position associated with a booking.

Booking Management maintains a BookingFinancialSnapshot for evaluating its own lifecycle requirements.

The remaining balance is calculated using:

**Net Amount Paid = Confirmed Payments − Confirmed Refunds**

**Remaining Balance = Agreed Total − Net Amount Paid**

Pending or rejected payments and unconfirmed refunds do not contribute to these calculations.

---

## 4. Booking States

### 4.1 Submitted

**Description**

The client has successfully submitted a booking request.

The booking awaits photographer review.

**Entry Conditions**

- The client successfully submits a booking request.
- The required booking request information is provided.

**Exit Conditions**

- Photographer requests additional information.
- Photographer approves the booking request.
- Photographer rejects the booking request.
- Client cancels the booking request.

**Related Domain Event**

`BookingRequestSubmitted`

### 4.2 Additional Information Requested

**Description**

The photographer requires additional information before making a decision about the booking request.

**Entry Conditions**

- The photographer requests additional information.

**Exit Conditions**

- Client provides the requested information.
- Photographer rejects the booking request.
- Client cancels the booking request.

**Related Domain Events**

- `AdditionalBookingInformationRequested`
- `AdditionalBookingInformationProvided`

**Notes**

A booking request may enter this state multiple times before approval.

After the requested information is provided, the booking returns to the Submitted state for further review.

### 4.3 Approved

**Description**

The photographer has approved the booking request.

The booking is awaiting confirmation of the required initial payment.

Approval does not mean that the booking has been confirmed.

**Entry Conditions**

- Photographer approves the booking request.

**Exit Conditions**

- The required initial payment is confirmed and Booking Management confirms the booking.
- The photographer accepts and executes the client's request to withdraw the booking request.

**Related Domain Events**

- `BookingRequestApproved`
- `BookingRequestCancelled` (when the photographer executes an accepted withdrawal request)

**Notes**

The client cannot directly cancel an approved booking request.

The client may request its withdrawal from the photographer. The booking remains in the Approved state until the photographer decides to execute the withdrawal.

A recorded or pending payment is not sufficient to confirm the booking. The initial payment must be confirmed by Payment Management.

### 4.4 Confirmed

**Description**

The required initial payment has been confirmed and Booking Management has determined that the booking satisfies its confirmation requirements.

The booking officially becomes a scheduled photography engagement.

**Entry Conditions**

- The booking request has been approved.
- Payment Management confirms the required initial payment.
- Booking Management verifies that its confirmation requirements are satisfied.

**Exit Conditions**

- The first Photography Session starts, causing the booking to enter In Progress.
- The booking agreement is terminated according to the applicable business rules.

**Related Domain Events**

- `PaymentConfirmed` (originating from Payment Management)
- `BookingConfirmed`
- `BookingPriceChanged` (when agreed terms change)
- `BookingAgreementTerminated` (if the agreement is terminated)

**Notes**

Additional payments, price changes, and financial position updates may occur while the booking remains Confirmed.

These activities do not automatically change the booking's lifecycle state.

### 4.5 In Progress

**Description**

At least one Photography Session belonging to the booking has started.

The booking is undergoing service delivery.

**Entry Conditions**

- The photographer starts a Photography Session belonging to a confirmed booking.

**Exit Conditions**

- All required Photography Sessions are completed and the remaining balance becomes zero, causing the booking to enter Completed.
- The booking agreement is terminated according to the applicable business rules.

**Related Domain Events**

- `PhotographySessionStarted`
- `PhotographySessionCompleted`
- `BookingCompleted` (when completion requirements are satisfied)
- `BookingAgreementTerminated` (if the agreement is terminated)

**Notes**

A booking may contain multiple Photography Sessions.

The booking remains In Progress while any required Photography Session is still incomplete, even if other sessions have already been completed.

### 4.6 Completed

**Description**

All required Photography Sessions have been completed and the remaining booking balance is zero.

The booking has successfully completed its lifecycle.

**Entry Conditions**

Both conditions must be satisfied:

- All required Photography Sessions have been completed.
- The remaining booking balance equals zero.

**Exit Conditions**

- None during normal business operations.

**Related Domain Event**

`BookingCompleted`

**Notes**

Completed bookings are treated as terminal states.

Any exceptional administrative correction must follow a separately authorized process and must not silently alter the historical business facts.

### 4.7 Rejected

**Description**

The photographer has rejected the booking request.

The request will not proceed to a confirmed booking.

**Entry Conditions**

- Photographer rejects the booking request.

**Exit Conditions**

- None.

**Related Domain Event**

`BookingRequestRejected`

### 4.8 Cancelled

**Description**

The booking request has been withdrawn before the booking becomes confirmed.

The request will not proceed to a confirmed booking.

**Entry Conditions**

- Client cancels the booking request while it is in a state where direct cancellation is permitted.
- Client requests withdrawal after approval and the photographer accepts and executes that request.

**Exit Conditions**

- None.

**Related Domain Event**

`BookingRequestCancelled`

**Notes**

The client may directly cancel a booking request while it is in a state where cancellation is permitted.

After the photographer approves the request, the client cannot cancel it independently. The client may request withdrawal, and the photographer must execute the cancellation.

Cancelled represents the withdrawal of a booking request before confirmation.

It must not be used to represent the termination of an already confirmed booking agreement.

### 4.9 Terminated

**Description**

An established booking agreement has been terminated according to the applicable business rules.

The booking will not continue through its normal service delivery lifecycle.

**Entry Conditions**

- The booking has been confirmed.
- The booking agreement has been terminated after the applicable termination conditions have been satisfied.

**Exit Conditions**

- None.

**Related Domain Event**

`BookingAgreementTerminated`

**Notes**

A confirmed booking agreement may be terminated whether or not Photography Sessions have started.

Terminated is distinct from Cancelled:

- Cancelled represents withdrawal of a booking request before confirmation.
- Terminated represents termination of an established booking agreement.

Termination does not automatically mean that a refund has been issued or confirmed.

Any applicable compensation or refund must be evaluated and processed according to the detailed business rules and the agreement between the photographer and the client. Refund confirmation remains the responsibility of Payment Management.

---

## 5. Lifecycle Summary

| State | Description | State Type |
|---|---|---|
| Submitted | Booking request submitted and awaiting review | Active |
| Additional Information Requested | Photographer requests additional information | Active |
| Approved | Booking request approved and awaiting initial payment | Active |
| Confirmed | Required initial payment confirmed and booking established | Active |
| In Progress | At least one Photography Session has started | Active |
| Completed | All required sessions completed and fully paid | Terminal |
| Rejected | Photographer rejects the booking request | Terminal |
| Cancelled | Booking request withdrawn before confirmation | Terminal |
| Terminated | Established booking agreement terminated | Terminal |

---

## 6. Lifecycle Transitions

| Current State | Trigger / Condition | Next State |
|---|---|---|
| Submitted | Additional information requested | Additional Information Requested |
| Submitted | Booking request approved | Approved |
| Submitted | Booking request rejected | Rejected |
| Submitted | Booking request cancelled by client | Cancelled |
| Additional Information Requested | Requested information provided | Submitted |
| Additional Information Requested | Booking request rejected | Rejected |
| Additional Information Requested | Booking request cancelled by client | Cancelled |
| Approved | Required initial payment confirmed and confirmation requirements satisfied | Confirmed |
| Approved | Photographer accepts and executes client's withdrawal request | Cancelled |
| Confirmed | First Photography Session started | In Progress |
| Confirmed | Booking agreement terminated | Terminated |
| In Progress | All required sessions completed and remaining balance equals zero | Completed |
| In Progress | Booking agreement terminated | Terminated |

**Notes**

- A client withdrawal request after approval does not itself change the booking state.
- Only the photographer can execute the withdrawal of an approved booking request.
- A state transition must be authorized by the business rules of Booking Management.
- The transition from Approved to Confirmed depends on a confirmed payment from Payment Management.
- A booking cannot return to an earlier lifecycle state after entering a terminal state.
- Payment recording, additional payment confirmation, refunds, and booking price changes do not automatically create booking lifecycle state transitions.
- The detailed conditions governing termination and any resulting compensation or refunds are defined separately.

---

## 7. Business Rules

### BR-01 — Initial State

Every booking begins in the **Submitted** state after a booking request has been successfully submitted.

### BR-02 — Initial Payment Requirement

A booking cannot become **Confirmed** until the required initial payment has been confirmed by Payment Management and Booking Management has verified its confirmation requirements.

### BR-03 — Additional Information

A booking may enter **Additional Information Requested** multiple times before approval.

### BR-04 — Rejected Bookings

Rejected bookings cannot return to active states.

### BR-05 — Client Cancellation Before Approval

A client may directly cancel a booking request while it is in a state where cancellation is permitted, before photographer approval.

### BR-06 — Withdrawal Request After Approval

After a booking request has been approved, the client cannot cancel it independently.

The client may request withdrawal from the photographer. The booking remains Approved until the photographer accepts and executes the withdrawal.

### BR-07 — Booking Completion

A booking becomes **Completed** only when:

- All required Photography Sessions have been completed.
- The remaining booking balance equals zero.

### BR-08 — Session Completion

Completing an individual Photography Session does not automatically complete the booking.

Booking Management evaluates the completion of all required sessions.

### BR-09 — Payment Ownership

Payment Management owns payment recording, confirmation, and refund confirmation.

Booking Management consumes the relevant financial facts to evaluate its own lifecycle requirements.

### BR-10 — Financial Position

The financial position used for booking lifecycle decisions is based on:

- Confirmed payments.
- Confirmed refunds.
- The current agreed booking total.

Pending or rejected transactions must not affect the remaining balance.

### BR-11 — Booking Price Changes

Changes to the agreed booking total must be established through the applicable agreement process.

Booking Management owns the booking terms and publishes `BookingPriceChanged` when the agreed total changes.

Payment Management recalculates the financial position using the updated agreed total.

A price change does not automatically change the booking lifecycle state.

### BR-12 — Agreement Termination

A confirmed booking agreement may be terminated according to the applicable business rules, whether or not Photography Sessions have started.

The termination is recorded through `BookingAgreementTerminated`.

A terminated booking cannot return to an active lifecycle state.

### BR-13 — Compensation and Refunds

The financial consequences of terminating a booking agreement, including compensation and any applicable refund, are determined by the detailed business rules and the agreement between the photographer and the client.

Payment Management is responsible for processing and confirming any refund.

Termination does not automatically confirm a refund.

### BR-14 — Completed Bookings

Completed bookings are terminal under normal business operations.

Exceptional administrative corrections must preserve the historical business facts and follow a separately authorized process.

---

## 8. Relationship with Domain Events

The lifecycle transitions are associated with the following Domain Events:

| Lifecycle Transition / Business Fact | Domain Event | Owning Bounded Context |
|---|---|---|
| Request submitted | BookingRequestSubmitted | Booking Management |
| Additional information requested | AdditionalBookingInformationRequested | Booking Management |
| Additional information provided | AdditionalBookingInformationProvided | Booking Management |
| Request approved | BookingRequestApproved | Booking Management |
| Request rejected | BookingRequestRejected | Booking Management |
| Request cancelled or withdrawal executed | BookingRequestCancelled | Booking Management |
| Required payment confirmed | PaymentConfirmed | Payment Management |
| Booking confirmed | BookingConfirmed | Booking Management |
| Photography Session started | PhotographySessionStarted | Booking Management |
| Photography Session completed | PhotographySessionCompleted | Booking Management |
| Booking price changed | BookingPriceChanged | Booking Management |
| Booking agreement terminated | BookingAgreementTerminated | Booking Management |
| Refund confirmed | RefundConfirmed | Payment Management |
| Booking completed | BookingCompleted | Booking Management |

**Important distinctions**

- `PaymentConfirmed` is a cross-context event received from Payment Management. It is not a Booking Management event.
- `BookingConfirmed` is a separate business decision made by Booking Management.
- `PhotographySessionCompleted` is an internal event within Booking Management.
- `BookingRequestCancelled` after approval is produced by the photographer's execution of an accepted client withdrawal request.
- `BookingPriceChanged` and `BookingAgreementTerminated` communicate changes to booking terms and agreement status to interested contexts.
- `RefundConfirmed` is owned by Payment Management and does not itself determine the booking lifecycle state.

---

## 9. Relationship with Other Domain Artifacts

This document must remain consistent with:

- `docs/02-analysis/01-business-process.md`
- `docs/02-analysis/02-use-cases.md`
- `docs/02-analysis/04-domain-events.md`
- `docs/02-analysis/05-ubiquitous-language.md`
- `docs/02-analysis/06-domain-model.md`
- `docs/04-design/01-strategic-design.md`
- `docs/04-design/02-tactical-design.md`

The Domain Model defines the important domain concepts and their relationships.

The Domain Events document identifies the significant business facts occurring within the domain and their ownership.

The Strategic Design establishes Bounded Context boundaries and responsibilities.

The Tactical Design defines Aggregate boundaries and how booking lifecycle rules are enforced.

This document defines the business lifecycle of a booking independently of its technical implementation.

---

## 10. Change History

| Version | Description |
|---|---|
| 1.0 | Defined the initial booking lifecycle states and business rules. |
| 2.0 | Aligned lifecycle states and transitions with the approved Strategic and Tactical Design; distinguished request cancellation from agreement termination; clarified payment ownership, financial position, session completion, and booking completion. |
