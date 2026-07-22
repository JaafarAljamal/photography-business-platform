# Functional Requirements

## Purpose

This document defines the functional requirements of the Photography Business Platform.

The requirements are expressed as user stories that describe the capabilities expected by the system users. They provide the functional baseline for business analysis, software design, implementation, and testing.

Unless otherwise specified, every capability available to a Guest Visitor remains available to a Client.

---

## Scope

The functional requirements defined in this document apply to Version 1.0 of the Photography Business Platform.

---

# Portfolio

### FR-001 — Browse Portfolio

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to browse the photographer's portfolio,

so that I can evaluate the photographer's work before requesting a photography service.

---

### FR-002 — Browse Portfolio Albums

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to browse portfolio albums,

so that I can explore photography projects organized into meaningful collections.

---

### FR-003 — Browse Portfolio Categories

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to browse portfolio categories,

so that I can quickly find the type of photography that interests me.

---

### FR-004 — View Portfolio Item

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to open a portfolio item in full-screen mode,

so that I can examine the photographer's work in greater detail.

---

### FR-005 — Close Portfolio Item

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to exit full-screen mode,

so that I can continue browsing the portfolio.

---

### FR-006 — View Portfolio Item Details

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to view the description and additional information of a portfolio item when provided,

so that I can better understand the context of the photographer's work.

---

### FR-007 — Manage Portfolio

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage portfolio items,

so that my portfolio accurately represents my professional work.

---

### FR-008 — Manage Portfolio Albums

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage portfolio albums,

so that portfolio items remain organized into meaningful collections.

---

### FR-009 — Manage Portfolio Categories

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage portfolio categories,

so that visitors can browse my work by photography type.

---

### FR-010 — Protect Portfolio Content

**Actor**

Photographer

**User Story**

As the photographer,

I want the platform to discourage unauthorized copying of my portfolio content,

so that my work is less likely to be misused without permission.

---

# Services

### FR-011 — Browse Photography Services

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to browse the photography services offered by the photographer,

so that I can understand the types of photography services available.

---

### FR-012 — View Photography Service Details

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to view the details of a photography service,

so that I can determine whether it meets my requirements.

---

### FR-013 — Browse Service Packages

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to browse the available service packages,

so that I can compare the available offers.

---

### FR-014 — View Service Package Details

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to view the details of a service package,

so that I can understand what is included in the package before requesting a booking.

---

### FR-015 — Manage Photography Services

**Actor**

Photographer

**User Story**

As the photographer,

I want to create, update, and remove photography services,

so that the services offered by my business remain accurate and up to date.

---

### FR-016 — Manage Service Packages

**Actor**

Photographer

**User Story**

As the photographer,

I want to create, update, and remove service packages,

so that I can offer commercial packages that combine one or more photography services.

---

# Inquiries

### FR-017 — Submit Inquiry

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to submit an inquiry,

so that I can request information before deciding whether to request a booking.

---

### FR-018 — Receive Inquiry Confirmation

**Actor**

Guest Visitor

**User Story**

As a guest visitor,

I want to receive confirmation that my inquiry has been successfully submitted,

so that I know the photographer has received my inquiry.

---

### FR-019 — View Inquiries

**Actor**

Photographer

**User Story**

As the photographer,

I want to view submitted inquiries,

so that I can review and respond to potential clients.

---

### FR-020 — Respond to Inquiry

**Actor**

Photographer

**User Story**

As the photographer,

I want to respond to inquiries,

so that I can provide the requested information and encourage visitors to become clients.

---

### FR-021 — Manage Inquiries

**Actor**

Photographer

**User Story**

As the photographer,

I want to organize and manage inquiries,

so that I can efficiently track communication with potential clients.

---

# Bookings

### FR-022 — Submit Booking Request

**Actor**

Client

**User Story**

As a client,

I want to submit a booking request,

so that I can request a photography service.

---

### FR-023 — Select an Available Booking Date and Time

**Actor**

Client

**User Story**

As a client,

I want to select my preferred booking date and time from a booking calendar that displays available dates and time slots,

so that I can request an appointment without choosing an unavailable date or time.

### FR-024 — Update Booking Request

**Actor**

Client

**User Story**

As a client,

I want to update my booking request before it is confirmed,

so that I can keep my booking information accurate.

---

### FR-025 — Cancel Booking Request

**Actor**

Client

**User Story**

As a client,

I want to cancel my booking request before it is confirmed,

so that I can withdraw my request if my plans change.

---

### FR-026 — Provide Additional Booking Information

**Actor**

Client

**User Story**

As a client,

I want to provide additional booking information when requested,

so that the photographer can evaluate my booking request.

---

### FR-027 — Receive Booking Notifications

**Actor**

Client

**User Story**

As a client,

I want to receive notifications about my booking,

so that I remain informed throughout the booking process.

---

### FR-028 — Review Booking Requests

**Actor**

Photographer

**User Story**

As the photographer,

I want to review booking requests,

so that I can decide how to handle each request.

---

### FR-029 — Request Additional Booking Information

**Actor**

Photographer

**User Story**

As the photographer,

I want to request additional information from the client,

so that I can evaluate the booking request before making a decision.

---

### FR-030 — Approve Booking Requests

**Actor**

Photographer

**User Story**

As the photographer,

I want to approve booking requests,

so that accepted requests can proceed to booking confirmation.

---

### FR-031 — Reject Booking Requests

**Actor**

Photographer

**User Story**

As the photographer,

I want to reject booking requests,

so that I can decline requests that cannot be accommodated.

---

### FR-032 — Manage Confirmed Bookings

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage confirmed bookings,

so that booking information remains accurate throughout the booking lifecycle.

---

# Payments

### FR-033 — Receive Payment Instructions

**Actor**

Client

**User Story**

As a client,

I want to receive payment instructions,

so that I know how to complete the required payment.

---

### FR-034 — Complete Booking Payments

**Actor**

Client

**User Story**

As a client,

I want to complete one or more payments using the payment method agreed with the photographer,

so that I can fulfill my financial obligations for the booking.

---

### FR-035 — Record Payment

**Actor**

Photographer

**User Story**

As the photographer,

I want to record each payment received,

so that every client payment is documented.

---

### FR-036 — Confirm Payment

**Actor**

Photographer

**User Story**

As the photographer,

I want to confirm each recorded payment,

so that the booking payment records remain accurate.

---

### FR-037 — View Booking Payment History

**Actor**

Photographer

**User Story**

As the photographer,

I want to view the payment history of a booking,

so that I can review all payments received for that booking.

---

# Invoices

### FR-038 — Receive Invoices

**Actor**

Client

**User Story**

As a client,

I want to receive an invoice by email after each confirmed payment,

so that I have an accurate financial record of my booking.

---

### FR-039 — View Booking Invoices

**Actor**

Photographer

**User Story**

As the photographer,

I want to view the invoices associated with a booking,

so that I can review the complete invoicing history for that booking.

---

### FR-040 — Manage Invoices

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage booking invoices,

so that accurate financial records are maintained.

---

# Business Administration

### FR-041 — Manage Business Information

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage my business information,

so that clients can access accurate and up-to-date business details.

---

### FR-042 — Manage Contact Information

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage my business contact information,

so that clients can communicate with me using the correct contact details.

---

### FR-043 — Manage Business Policies

**Actor**

Photographer

**User Story**

As the photographer,

I want to manage my business policies,

so that clients understand the terms and conditions of my photography services.

---
