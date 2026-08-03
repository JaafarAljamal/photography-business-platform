# Booking Lifecycle

## Purpose

This document defines the lifecycle of a booking from the moment a client submits a booking request until the booking is completed or closed.

The lifecycle standardizes the business states that every booking may pass through during its existence.

This lifecycle serves as the foundation for:

- Domain Model
- State Diagrams
- Domain Events
- Business Rules
- Application Logic

---

# Scope

This lifecycle applies to every booking managed by the Photography Business Platform Version 1.0.

It includes both client actions and photographer decisions together with the automated transitions performed by the platform.

---

# Lifecycle Overview

A booking progresses through several well-defined business states.

Each state represents the current business condition of the booking.

State transitions occur only when specific business rules are satisfied.

---

# Booking States

## 1. Submitted

### Description

The client has successfully submitted a booking request.

The booking awaits photographer review.

### Entry Conditions

- Client submits booking request successfully.

### Exit Conditions

- Photographer requests additional information.
- Photographer approves booking.
- Photographer rejects booking.
- Client cancels booking request.

---

## 2. Additional Information Requested

### Description

The photographer requires more information before making a booking decision.

### Entry Conditions

- Photographer requests additional information.

### Exit Conditions

- Client submits requested information.
- Photographer rejects booking.
- Client cancels booking request.

---

## 3. Approved

### Description

The photographer accepts the booking request.

The booking is waiting for the required initial payment.

### Entry Conditions

- Photographer approves booking.

### Exit Conditions

- Initial payment confirmed.
- Photographer cancels approval.
- Client withdraws before payment (if business policy allows).

---

## 4. Confirmed

### Description

The required initial payment has been confirmed.

The booking officially becomes a scheduled photography job.

### Entry Conditions

- Initial payment confirmed.

### Exit Conditions

- Booking updated.
- Additional installment payments.
- Photography sessions completed.
- Booking cancelled after confirmation (if business policy allows).

---

## 5. In Progress

### Description

One or more photography sessions have started.

### Entry Conditions

- Photographer begins photography work.

### Exit Conditions

- Photography sessions completed.

---

## 6. Completed

### Description

All photography sessions have finished and the remaining balance has been fully paid.

### Entry Conditions

- Photography work completed.
- Remaining balance equals zero.

### Exit Conditions

- None.

---

## 7. Rejected

### Description

The photographer rejects the booking request.

### Entry Conditions

- Photographer rejects booking.

### Exit Conditions

- None.

---

## 8. Cancelled

### Description

The booking request is cancelled before completion.

### Entry Conditions

- Client cancels booking request.
- Photographer cancels confirmed booking (if business policy permits).

### Exit Conditions

- None.

---

# Lifecycle Summary

| State | Description |
|--------|-------------|
| Submitted | Booking request submitted |
| Additional Information Requested | Photographer requests more information |
| Approved | Waiting for initial payment |
| Confirmed | Booking officially confirmed |
| In Progress | Photography work has started |
| Completed | Photography completed and fully paid |
| Rejected | Photographer rejects booking |
| Cancelled | Booking cancelled |

---

# Business Rules

## BR-01

Every booking begins in the **Submitted** state.

---

## BR-02

A booking cannot become **Confirmed** until the required initial payment has been confirmed.

---

## BR-03

A booking may enter **Additional Information Requested** multiple times before approval.

---

## BR-04

Rejected bookings cannot return to active states.

---

## BR-05

Completed bookings are immutable except for administrative corrections.

---

## BR-06

A booking becomes **Completed** only when:

- All photography sessions have finished.
- Remaining balance equals zero.

---

## Related Use Cases

- UC-021 — Submit Booking Request
- UC-023 — Update Booking Request
- UC-024 — Cancel Booking Request
- UC-025 — Request Additional Booking Information
- UC-026 — Submit Additional Booking Information
- UC-028 — Review Booking Requests
- UC-029 — Approve Booking Requests
- UC-030 — Reject Booking Requests
- UC-031 — Manage Confirmed Bookings
- UC-034 — Record Payment
- UC-035 — Confirm Payment

---

## Related Business Process

- BP-07 — Booking Management
- BP-08 — Payment Management
