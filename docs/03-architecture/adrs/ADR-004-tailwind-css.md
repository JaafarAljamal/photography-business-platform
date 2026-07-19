# ADR-004 — Tailwind CSS

## Purpose

This document records the architectural decision to adopt Tailwind CSS as the primary styling framework for the Photography Business Platform.

It explains the context of the decision, the alternatives that were considered, and the expected consequences for the project.

---

## Status

Accepted

---

## Context

The Photography Business Platform requires a modern, responsive, and maintainable user interface that provides a consistent experience across public pages and the administrative dashboard.

The project is implemented as a Laravel Monolith using Blade for server-side rendering. The styling solution should integrate naturally with this architecture, encourage reusable user interface components, and support rapid development without introducing unnecessary complexity.

A utility-first CSS framework aligns well with the project's goals of maintainability, consistency, and production readiness.

---

## Decision

The system shall use Tailwind CSS as the primary CSS framework.

All application interfaces shall be styled using Tailwind CSS utility classes. Reusable Blade components shall be used to promote consistency and reduce duplicated markup.

Custom CSS shall be introduced only when Tailwind CSS cannot adequately satisfy a specific design requirement.

---

## Consequences

### Positive

- Integrates seamlessly with Laravel and Blade.
- Promotes consistent user interface design.
- Encourages reusable UI components.
- Reduces the amount of custom CSS.
- Improves long-term maintainability.
- Supports responsive design by default.
- Enables rapid interface development.

### Negative

- HTML templates contain more utility classes.
- Developers must become familiar with the utility-first approach.
- Highly customized visual designs may still require additional CSS.

---

## Alternatives Considered

### Bootstrap

Rejected because it relies on predefined components and styles that provide less flexibility for building a custom user interface tailored to the project's needs.

### Traditional Custom CSS

Rejected because maintaining a growing custom stylesheet would increase long-term maintenance effort and make design consistency more difficult to enforce.

---

## Rationale

Tailwind CSS provides the best balance between flexibility, maintainability, and development productivity for the Photography Business Platform.

Its utility-first approach complements Laravel and Blade, supports reusable interface components, and enables the project to deliver a modern, responsive, and production-ready user experience while remaining consistent with the project's architectural principles.
