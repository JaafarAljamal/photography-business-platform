# ADR-001 — Project Structure

## Purpose

This document records the architectural decision to adopt the project's repository and documentation structure.

It explains why the selected structure was chosen, the alternatives that were considered, and the consequences of this decision for the Photography Business Platform.

---

## Status

Accepted

---

## Context

The Photography Business Platform is intended to serve as a production-ready software engineering project that demonstrates professional development practices.

The project includes not only application source code but also comprehensive documentation covering business analysis, software architecture, system design, testing, deployment, and architectural decisions.

To support long-term maintainability, all project artifacts must remain organized, consistent, and easy to navigate throughout the project lifecycle.

---

## Decision

The project adopts a documentation-first repository structure.

Documentation is maintained separately from the application source code and is organized according to the software development lifecycle.

The repository is divided into the following primary areas:

- Root-level project configuration and repository files.
- Project documentation (`docs/`).
- Application source code (`src/`).
- Development infrastructure (`docker/`).
- Utility scripts (`scripts/`).

The documentation itself is organized into independent sections that represent successive project phases:

- Project Management
- Requirements
- Domain Analysis
- Software Architecture
- System Design
- Testing
- Deployment
- Architectural Decisions
- Project Assets

Each document has a single responsibility and belongs to exactly one section of the documentation.

---

## Consequences

### Positive

- Clear separation between documentation and implementation.
- Documentation evolves alongside the software.
- Improved maintainability and navigation.
- Better traceability from business requirements to implementation.
- Consistent organization throughout the project lifecycle.
- Suitable for portfolio presentation and production-oriented development.

### Negative

- Requires disciplined maintenance to keep documentation synchronized.
- Produces a larger documentation set than small software projects.

---

## Alternatives Considered

### Minimal documentation

Rejected because it provides insufficient traceability for a production-oriented project.

### Mixed documentation inside the source code

Rejected because it makes documentation harder to navigate and maintain as the project grows.

### Flat documentation structure

Rejected because the number of project documents is expected to increase significantly during development.

---

## Rationale

A structured repository reflects the project's documentation-first development approach.

Organizing documentation according to the software engineering lifecycle improves readability, consistency, maintainability, and collaboration while supporting Domain-Driven Design, Test-Driven Development, and Clean Architecture.
