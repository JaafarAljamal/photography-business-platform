# System Overview

## 1. Purpose

This document provides a high-level overview of the Photography Business Platform, including its purpose, system boundary, primary actors, major logical components, external dependencies, and principal business workflows.

It establishes a shared understanding of the system before detailed architectural design and implementation.

This document describes the system at a conceptual level. Detailed component responsibilities, domain boundaries, database structures, API contracts, and implementation decisions are documented separately.

---

## 2. System Purpose

The Photography Business Platform is a web-based application designed to help a single photographer manage the business activities associated with providing photography services.

The platform brings together the photographer's online portfolio, service and package presentation, booking requests, client inquiries, photography session scheduling, payment tracking, invoice generation, and relevant email communication.

The system aims to:

- Provide visitors with a professional online presence through which they can explore the photographer's work and services.
- Convert booking requests into confirmed photography bookings through a structured workflow.
- Help the photographer manage bookings, photography sessions, and payment progress.
- Maintain consistent business information across booking, payment, and invoicing activities.
- Reduce administrative effort through organized workflows and appropriate automation.
- Provide a maintainable and testable foundation for future development.

---

## 3. System Scope

### 3.1 In Scope

The initial release includes the following capabilities:

- **Portfolio Management:** Present and organize photography work.
- **Service and Package Management:** Maintain photography services, predefined packages, and their associated information.
- **Booking Management:** Receive, review, approve, reject, update, cancel, and track booking requests according to the booking lifecycle.
- **Client Management:** Maintain client information associated with submitted booking requests and subsequent business interactions.
- **Photography Session Management:** Manage the sessions associated with a booking and track their progress.
- **Payment Tracking:** Record payments, allow the photographer to confirm receipt, and calculate outstanding balances.
- **Invoice Generation:** Generate an invoice for each confirmed payment according to the agreed business rule.
- **Inquiry Management:** Receive and manage inquiries independently of the booking workflow.
- **Email Communication:** Send invoices and other supported business notifications by email.
- **Photographer Dashboard:** Provide the photographer with an interface for managing the platform's operational activities.

---

### 3.2 Out of Scope

The initial release does not include:

- A marketplace for multiple photographers or photography businesses.
- Social networking or social media platform functionality.
- An image editing application.
- A real-time messaging platform.
- In-platform payment processing through a payment gateway.
- Hosting a complete client photo-delivery and gallery-sharing workflow as part of the initial release.

These exclusions define the initial product boundary and do not prevent future scope changes through an explicit decision.

---

## 4. System Users

### 4.1 Visitor

A Visitor can browse publicly available portfolio content, photography services, packages, and business information.

A Visitor can submit a booking request or an inquiry without first creating an account.

Submitting an inquiry alone does not make the Visitor a Client.

---

### 4.2 Client

A Visitor becomes a Client when they submit a Booking Request.

A Client can provide the information required for a booking, respond to requests for additional information, and perform the booking-related actions permitted by the booking lifecycle and applicable business rules.

Any account or authentication requirements will be defined in the relevant requirements and design documents.

---

### 4.3 Photographer

The Photographer is the owner and administrator of the single photography business represented by the platform.

The Photographer uses the administrative interface to manage portfolio content, services, packages, booking requests, client information, photography sessions, payment confirmation, and invoice-related activities.

The Photographer also reviews inquiries and performs the business decisions that require human intervention.

---

## 5. System Boundary and External Dependencies

The platform is a single application that interacts with external infrastructure and services where necessary.

Its primary external dependencies are:

- **Web Browser:** Provides the interface used by Visitors, Clients, and the Photographer.
- **Email Service:** Delivers invoices and supported business notifications.
- **File and Image Storage:** Stores portfolio images and other application files according to the selected deployment configuration.
- **Payment Channels:** Bank transfer, cash, or other agreed external payment methods. Payment receipt is verified and confirmed manually by the Photographer.
- **Application Hosting Environment:** Runs the application and provides access to its required infrastructure.

The platform records and manages payment information but does not directly process external payments in the initial release.

The specific email provider, storage implementation, hosting arrangement, and operational infrastructure will be defined in subsequent design and deployment documentation.

---

## 6. High-Level Architecture

The system follows a monolithic architecture implemented with Laravel.

The initial application uses:

- **Laravel and PHP:** Application framework and server-side business processing.
- **Blade:** Server-rendered user interfaces.
- **Tailwind CSS:** Styling and responsive layout.
- **JavaScript:** Client-side interaction and progressive enhancement.
- **MySQL:** Persistent relational data storage.
- **Docker:** A consistent development and deployment environment.

The application is organized into logical areas of responsibility while remaining one deployable application.

The monolithic architecture does not require every business capability to be implemented as an independent service. Logical separation is intended to improve maintainability, testability, and clarity without introducing unnecessary distributed-system complexity.

Detailed architectural layers, dependency rules, and project directory conventions will be defined in the corresponding architecture documents.

---

## 7. Major Logical Components

The following components describe logical responsibilities, not independent deployment units.

### 7.1 Public Experience

Responsible for presenting the portfolio, services, packages, business information, and public forms through which visitors can submit booking requests or inquiries.

---

### 7.2 Booking Management

Responsible for coordinating the booking lifecycle, including request submission, review, additional information, approval, rejection, cancellation, confirmation, and completion.

It ensures that booking transitions comply with the business rules established in the Booking Lifecycle document.

---

### 7.3 Client Management

Responsible for maintaining client information associated with booking requests and relevant business activities.

Client creation and association must follow the agreed business terminology and lifecycle rules.

---

### 7.4 Photography Session Management

Responsible for managing photography sessions associated with bookings, including their scheduled information and progress.

Completing an individual Photography Session does not, by itself, complete the associated Booking.

---

### 7.5 Payment Management

Responsible for recording payments, tracking their confirmation status, maintaining payment history, and calculating the outstanding booking balance.

The Photographer confirms receipt of payments made through external payment channels.

A booking may have multiple payments. The system must not assume that every booking has exactly two payments.

---

### 7.6 Invoice Management

Responsible for generating and maintaining invoice records associated with confirmed payments.

Invoice generation is a business responsibility distinct from invoice email delivery. A delivery failure must not be treated as proof that invoice generation failed.

---

### 7.7 Inquiry Management

Responsible for receiving and managing inquiries independently of booking requests.

An inquiry does not automatically create a Client or a Booking.

---

### 7.8 Notification Management

Responsible for coordinating supported email notifications, including invoice delivery.

Notification delivery is an external interaction and must remain distinct from the business event that initiates it.

---

### 7.9 Photographer Administration

Provides the Photographer with the administrative interface for managing the business capabilities available in the initial release.

The interface is part of the same monolithic application and does not represent a separate backend service.

---

## 8. Principal Business Workflows

### 8.1 Booking Workflow

1. A Visitor explores the portfolio, services, and packages.
2. The Visitor submits a Booking Request.
3. The system records the request and the information supplied by the prospective Client.
4. The Photographer reviews the request and may request additional information, approve it, or reject it.
5. An approved booking awaits confirmation of the required initial payment.
6. The Photographer records and confirms receipt of the required payment.
7. Once the initial payment requirement is satisfied, the Booking becomes Confirmed.
8. The Photographer manages the associated Photography Sessions and records subsequent payments as needed.
9. The Booking becomes Completed only when all required Photography Sessions are complete and the remaining balance is zero.

> The detailed states, transitions, and business rules are defined in `docs/02-analysis/03-booking-lifecycle.md`.

---

### 8.2 Payment and Invoice Workflow

1. The Photographer records a payment received through an external payment channel.
2. The payment remains unconfirmed until the Photographer verifies and confirms receipt.
3. Upon confirmation, the system updates the payment status and recalculates the outstanding balance.
4. The system evaluates whether the required initial payment condition for Booking confirmation has been satisfied.
5. The system generates an invoice for the confirmed payment.
6. The invoice is sent by email through the configured email service.

> The precise event definitions and their relationships are documented in `docs/02-analysis/04-domain-events.md`. Detailed payment and invoice behavior will be specified during Tactical Design.

---

### 8.3 Inquiry Workflow

1. A Visitor or existing Client submits an inquiry.
2. The system records the inquiry.
3. The Photographer reviews and responds through the supported business process.
4. If the inquiry later leads to a booking, the booking follows its own lifecycle.

> Inquiry handling remains independent of booking management.

---

## 9. Architectural Principles

The system design and implementation will follow these principles:

- **Domain-Driven Design (DDD):** Model the business domain using the agreed Ubiquitous Language and domain rules.
- **Clean Architecture:** Separate business rules from framework-specific and infrastructure concerns.
- **Test-Driven Development (TDD):** Develop behavior through a cycle of writing a failing test, implementing the required behavior, and refactoring.
- **Single Responsibility:** Keep each logical component focused on a coherent business or technical responsibility.
- **Explicit Business Rules:** Enforce booking, payment, session, and invoice rules consistently.
- **Consistency Across Documentation and Code:** Keep architecture, requirements, domain models, tests, and implementation aligned.
- **Security by Design:** Protect administrative operations, client information, and application data through appropriate authorization, validation, and secure configuration.
- **Pragmatic Simplicity:** Prefer the simplest design that satisfies the current business requirements without premature distribution or unnecessary abstractions.

> The detailed enforcement of these principles belongs in the Clean Architecture, DDD, Tactical Design, and Testing Strategy documents.

---

## 10. Deployment Overview

The application is intended to run as a web application in a controlled environment.

Docker provides a reproducible environment for the application and its supporting services. MySQL provides persistent relational storage.

The deployed system will also require configured email delivery and file/image storage.

The production deployment topology, environment configuration, backups, monitoring, logging, and release procedures will be documented as part of the relevant deployment and production-readiness work.

This document does not prescribe a specific hosting provider or production topology.

---

## 11. Related Documents

This document should be read alongside the following project artifacts:

- `docs/01-requirements/01-vision.md`
- `docs/01-requirements/02-business-goals.md`
- `docs/01-requirements/06-functional-requirements.md`
- `docs/01-requirements/07-non-functional-requirements.md`
- `docs/02-analysis/03-booking-lifecycle.md`
- `docs/02-analysis/04-domain-events.md`
- `docs/02-analysis/05-ubiquitous-language.md`
- `docs/02-analysis/06-domain-model.md`

The following Sprint 2 documents will provide the detailed design derived from this overview:

- `docs/03-architecture/02-project-structure.md`
- `docs/03-architecture/03-c4-context.md`
- `docs/03-architecture/04-c4-container.md`
- `docs/03-architecture/05-c4-component.md`
- `docs/03-architecture/06-clean-architecture.md`
- The Strategic Design and Tactical Design documents.
- The database, API, sequence diagram, state diagram, and testing documentation.

---

## 12. Status

This document establishes the high-level system view for Sprint 2 — System Design.

It does not mark the Sprint as completed. The remaining design artifacts will refine and validate the responsibilities, boundaries, interactions, and implementation decisions introduced here.

---

