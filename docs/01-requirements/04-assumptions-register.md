# Assumptions Register

## Purpose

This document records the business assumptions that influence the design and implementation of the Photography Business Platform.

These assumptions establish the project's initial scope and provide a shared understanding for requirements analysis, system design, and implementation. They may be reviewed and updated if business requirements evolve.

---

## Assumptions

The system design is based on the following assumptions:

### Business

- The platform serves a single photography business.
- The photographer is the business owner and the sole administrator of the platform.
- The first release provides a single management dashboard for the photographer.
- The platform supports Arabic and English.

### Users

- The platform distinguishes between visitors and clients.
- Visitors become clients by submitting a booking request.
- Clients are not required to create an account to submit booking requests or inquiries.

### Services

- Photography services may be offered as predefined packages or as custom service requests.
- The photographer and the client may agree to modify the requested services before or after the booking is confirmed.
- Modifying the agreed services may increase or decrease the total booking price.
- A booking may consist of one or multiple photography sessions.

### Booking

- A visitor submits a booking request rather than directly creating a confirmed booking.
- The photographer reviews every booking request before making a decision.
- The photographer may approve, reject, request additional information, or modify the booking request.
- A booking becomes confirmed only after the required deposit has been manually verified by the photographer.
- Clients may modify or cancel their booking requests before the booking is confirmed.
- After confirmation, booking modifications are managed by the photographer in agreement with the client.

### Payments

- Payments are completed outside the platform using payment methods agreed upon between the photographer and the client.
- The platform does not integrate with online payment gateways in the first release.
- The photographer manually confirms every payment received.
- A booking may include one or more payments.
- The platform maintains the complete payment history for every booking.
- A booking is considered fully paid only when the outstanding balance reaches zero.

### Invoicing

- An invoice is automatically generated whenever a payment is confirmed.
- Each invoice records:
  - The confirmed payment amount.
  - The cumulative amount paid.
  - The remaining balance.
  - The current total booking amount.
- Invoices are delivered to clients by email.

### Portfolio & Delivery

- The platform provides a public portfolio for showcasing the photographer's work.
- Final photographs are delivered outside the platform in the first release.

### Communication

- Visitors may submit inquiries without creating an account.
- Inquiries are managed independently from booking requests.
