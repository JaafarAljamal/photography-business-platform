# Business Stakeholders

## Purpose

This document identifies the people, organizations, and external systems that interact with or are affected by the Photography Business Platform.

Understanding stakeholders helps ensure that business requirements, software design, and implementation decisions align with business objectives throughout the project lifecycle.

---

# Business Stakeholders

## Photographer (Business Owner)

The photographer owns and operates the photography business through the platform.

### Responsibilities

- Manage the public portfolio.
- Manage photography services and packages.
- Manage custom photography service requests.
- Review booking requests.
- Approve or reject booking requests.
- Request additional booking information when necessary.
- Modify confirmed bookings when required.
- Manage client inquiries.
- Record and confirm client payments manually.
- Generate invoices after confirmed payments.
- Manage business information.
- View booking and payment history.

### Goals

- Establish a professional online presence.
- Increase client acquisition.
- Reduce administrative effort.
- Organize photography bookings efficiently.
- Improve the overall client experience.

---

# System Users

## Guest Visitor

A visitor who explores the platform without submitting a booking request or inquiry.

### Responsibilities

- Browse the website.
- View the photographer's portfolio.
- Explore photography services.
- Review available packages.
- Read business information.
- Submit an inquiry through the contact form.

### Goals

- Discover the photographer.
- Evaluate the photographer's work.
- Decide whether to become a client.

---

## Client

A visitor becomes a client after submitting a booking request.

### Responsibilities

- Browse the photographer's portfolio.
- Explore available services and packages.
- Submit booking requests.
- Submit custom photography service requests.
- Modify or cancel a booking request before booking confirmation.
- Provide photography session requirements.
- Follow the photographer's payment instructions.
- Receive booking updates.
- Receive invoices by email.

### Goals

- Discover professional photography services.
- Book photography sessions easily.
- Communicate session requirements before the appointment.
- Receive a professional booking experience.

---

# External Systems

## Email Service

Provides email delivery for:

- Booking updates.
- Payment confirmations.
- Invoices.
- Inquiry notifications.
- System notifications.

---

# Future Stakeholders and Integrations

The following stakeholders or external systems are intentionally excluded from the current project scope but may be introduced in future releases:

- Online payment providers.
- Banking integrations.
- SMS providers.
- Accounting systems.
- Multiple photography businesses.
- Staff photographers.
- Online gallery delivery.
- Online file downloads.

---

# Notes

- The current release supports a single photography business managed by one photographer.
- Clients are not required to create an account to submit booking requests or inquiries.
- Payments are completed outside the platform (for example, by bank transfer or cash).
- The photographer manually records and confirms each payment received.
- A booking may receive multiple payments until the agreed total amount has been paid.
- The platform generates an invoice after each confirmed payment, including the payment amount, the cumulative amount paid, and the remaining balance.
- A booking is considered financially completed only when the remaining balance reaches zero.
- Photography deliverables are provided to clients outside the platform in the current release.
