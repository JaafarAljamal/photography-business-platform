# Ubiquitous Language

## Purpose

This document defines the official business vocabulary used throughout the Photography Business Platform.

The goal of the Ubiquitous Language is to ensure that business stakeholders, software developers, testers, and future maintainers use the same terminology when discussing, designing, implementing, and maintaining the system.

Every term defined in this document becomes the authoritative language for:

- Domain Models
- Entities
- Aggregates
- Value Objects
- Domain Events
- Services
- APIs
- Database Schema
- Documentation

No alternative terminology should be introduced unless explicitly documented here.

## Naming Principles

The project follows these rules:

- One business concept must always have one official name.
- One official name must never represent multiple business concepts.
- Software class names should follow the business terminology.
- Database table names should reflect the same terminology whenever possible.
- New terms must be added to this document before being introduced into the software.

## Business Terms

| Business Term | Definition | Software Representation | Notes |
|---------------|------------|-------------------------|-------|
| `Photographer` | The business owner who provides photography services and manages the platform | Photographer | Platform administrator |
| `Visitor` | Anyone browsing the platform without becoming a client yet | Not persisted | Anonymous user |
| `Client` | A person requesting photography services | Client | A visitor becomes a client after submitting a booking request |
| `Inquiry` | A communication initiated by a visitor or client requesting information | Inquiry | Independent from Booking |
| `Photography Service` | A photography offering provided by the photographer | Service | Weddings, Events, Products, etc. |
| `Package` | A predefined bundle of photography services with predefined pricing and features | Package | Optional selection during booking |
| `Booking` | A confirmed business agreement between the photographer and a client | Booking | Central aggregate of the domain |
| `Booking Request` | A client request awaiting photographer approval | Booking Lifecycle Stage | Not a separate entity |
| `Photography Session` | A scheduled photography activity belonging to a booking | Session | One or more sessions may be scheduled under a single booking |
| `Payment` | A monetary transaction received from the client | Payment | Multiple payments may belong to one booking |
| `Invoice` | A financial document generated after payment confirmation | Invoice | Generated after a payment is confirmed |
| `Portfolio` | Public showcase of the photographer's previous work | Portfolio | Marketing purpose |
| `Album` | A collection of portfolio images | Album | Belongs to Portfolio |
| `Category` | Classification used to organize portfolio content and services | Category | Shared taxonomy |

---

## Relationships Between Terms

```
Visitor
 ├── Inquiry
 └── Booking Request

Client
 └── Booking
       └── Photography Session(s)
             └── Payment(s)
                   └── Invoice(s)
```

---

## Synonyms and Forbidden Terms

| Avoid | Use Instead |
|--------|-------------|
| Customer | Client |
| Reservation | Booking |
| Order | Booking |
| Admin | Photographer |
| User | Visitor / Client / Photographer |
| Gallery | Portfolio |
| Payment Confirmation Record | Payment |
| Appointment | Photography Session |
| Event | Photography Session |
| Booking Confirmation | Booking |
| Booking Payment | Payment |

---

## Naming Conventions

Software objects should use singular nouns.

### Software Classes
```
Booking
Client
Inquiry
Package
Payment
Invoice
Portfolio
Album
Category
PhotographySession
```
### Database Tables
```
bookings
clients
inquiries
packages
payments
invoices
portfolios
albums
categories
photography_sessions
```

---

## Future Extensions

New business concepts must be introduced here before being added to the software implementation.

Any change to the terminology requires updating all related documentation to preserve consistency across the project.

---

