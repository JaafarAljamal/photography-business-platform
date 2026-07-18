# Assumptions Register

## Purpose

This document records the business assumptions that influence the design and implementation of the Photography Business Platform.

These assumptions define the project's initial scope and provide a common understanding for requirements analysis, system design, and implementation. They may be reviewed and updated if business requirements change.

---

## Assumptions

The system design is based on the following assumptions:

- The platform serves a single photography business.
- The photographer is the business owner and manages the platform.
- The platform supports two user types: visitors and clients.
- Visitors become clients by submitting a booking request.
- Clients are not required to create an account to request a booking.
- Photography services may be booked either as predefined packages or as custom service requests agreed upon between the photographer and the client.
- A booking may consist of one or multiple photography sessions.
- Payments are completed outside the platform using payment methods agreed upon by the photographer and the client.
- The photographer manually confirms deposit and final payment receipt.
- A booking is considered confirmed only after the deposit payment has been manually verified by the photographer.
- The remaining balance is paid after completing the agreed photography sessions.
- The platform generates an invoice after confirming full payment.
- Invoices are delivered to clients by email.
- Final photographs are delivered outside the platform in the first release.
- The first release supports Arabic and English.
