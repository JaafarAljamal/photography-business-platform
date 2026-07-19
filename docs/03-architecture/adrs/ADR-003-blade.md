# ADR-003 — Blade Templating Engine

## Purpose

This document records the architectural decision to adopt Blade as the server-side templating engine for the Photography Business Platform.

It explains the context of the decision, the alternatives that were considered, and the expected consequences for the project.

---

## Status

Accepted

---

## Context

The Photography Business Platform is designed as a Laravel Monolith that primarily delivers server-rendered web pages.

The platform focuses on presenting photography portfolios, business information, photography services, booking forms, and an administrative dashboard.

Most user interactions are based on standard web navigation and form submissions, while only limited client-side interactivity is required.

Selecting a templating solution that integrates naturally with Laravel supports faster development, lower complexity, and improved long-term maintainability.

---

## Decision

The system shall use Blade as the server-side templating engine.

Blade shall be responsible for rendering all public pages and administrative interfaces.

Reusable layouts, components, partials, and template inheritance shall be used to maximize consistency and reduce duplication throughout the application.

Client-side JavaScript shall be introduced only where additional interactivity provides clear business value.

---

## Consequences

### Positive

- Native integration with Laravel.
- Simple and maintainable view layer.
- Fast server-side rendering.
- Improved SEO through fully rendered HTML.
- Reduced application complexity.
- Reusable layouts and components.
- Easier maintenance and long-term consistency.

### Negative

- Less suitable for highly interactive single-page applications.
- Some dynamic user experiences require additional JavaScript.
- Rich client-side interactions may require progressive enhancement.

---

## Alternatives Considered

### Single Page Application (SPA)

Rejected because the project does not require a JavaScript-heavy client application.

Introducing a SPA would increase development complexity, duplicate routing logic, and provide limited business value for the current project scope.

### Hybrid Server-Side Rendering with JavaScript Frameworks

Rejected because the expected level of client-side interactivity does not justify the additional architectural complexity.

Progressive enhancement with JavaScript provides a simpler and more maintainable solution.

---

## Rationale

Blade provides the best balance between simplicity, performance, maintainability, and SEO for the Photography Business Platform.

Its native integration with Laravel supports rapid development while remaining fully aligned with the project's architectural principles, including Clean Architecture, Domain-Driven Design, and Test-Driven Development.

The selected approach also allows additional JavaScript functionality to be introduced incrementally without changing the overall application architecture.
