# Project Structure

## 1. Purpose

This document defines the repository structure of the Photography Business Platform and establishes the responsibilities and organization of its files and directories.

It provides a shared reference for organizing project documentation, application source code, development infrastructure, utility scripts, and supporting artifacts. Its purpose is to keep the repository maintainable and ensure consistency between the documented architecture and the actual implementation.

The project follows a documentation-first development approach and is intended to demonstrate professional software engineering practices through Domain-Driven Design (DDD), Test-Driven Development (TDD), Clean Architecture, and incremental Scrum delivery.

---

## 2. Scope and Status

This document describes the repository structure currently present in the project. It does not prescribe the final internal structure of the Laravel application.

The repository currently contains:

- Project-wide configuration and contribution files.
- Documentation organized by project phase and responsibility.
- Business process models and use-case diagrams.
- Architecture Decision Records (ADRs).
- Initial database, sequence, state, testing, and deployment documentation.
- Placeholder directories for application source code, Docker support, scripts, and assets.

The `src/` directory currently contains only `.gitkeep`. The Laravel application has not yet been scaffolded there.

The internal application structure will be finalized during **Sprint 2 — System Design**. Laravel scaffolding and initial environment setup are planned for **Sprint 3 — Application Setup**, following approval of the relevant design decisions.

---

## 3. Structural Principles

### 3.1. Separation of Concerns

Project management, requirements, domain analysis, architecture, design, testing, deployment, and decisions are maintained in separate documentation areas.

Application source code is kept separate from project documentation and supporting repository files.

---

### 3.2. Documentation-First Development

Documentation is developed alongside the project and provides traceability from business requirements and domain analysis to architecture, design, testing, and implementation.

The repository structure must reflect the actual state of the project. Planned directories and files must not be presented as existing implementation artifacts.

---

### 3.3. Single Responsibility

Each document and directory has a primary responsibility. New artifacts should be placed in the appropriate existing section rather than creating duplicate locations for the same concern.

---

### 3.4. Architecture Consistency

The repository structure must remain consistent with the accepted Architecture Decision Records, the system overview, and the approved design.

Significant architectural changes must follow the project's decision process and be reflected in the relevant documentation.

---

### 3.5. Incremental Evolution

The repository evolves through the approved Scrum roadmap. Directories and application abstractions should be introduced when justified by the requirements and design, rather than anticipating unspecified future needs.

---

## 4. Current Repository Structure

The following tree reflects the repository structure provided for this document.

```text
photography-business-platform/
├── .editorconfig
├── .gitattributes
├── .gitignore
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── .gitkeep
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/
│       └── .gitkeep
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
├── docker-compose.yml
├── docker/
│   └── .gitkeep
├── scripts/
│   └── .gitkeep
├── src/
│   └── .gitkeep
└── docs/
    ├── README.md
    ├── 00-project/
    │   ├── changelog.md
    │   ├── roadmap.md
    │   └── sprint-backlog.md
    ├── 01-requirements/
    │   ├── 01-vision.md
    │   ├── 02-business-goals.md
    │   ├── 03-stakeholders.md
    │   ├── 04-assumptions-register.md
    │   ├── 05-constraints.md
    │   ├── 06-functional-requirements.md
    │   └── 07-non-functional-requirements.md
    ├── 02-analysis/
    │   ├── 01-business-process.md
    │   ├── 02-use-cases.md
    │   ├── 03-booking-lifecycle.md
    │   ├── 04-domain-events.md
    │   ├── 05-ubiquitous-language.md
    │   ├── 06-domain-model.md
    │   ├── bpmn/
    │   │   ├── BP-01-Portfolio-Management.bpmn
    │   │   ├── BP-01-Portfolio-Management.png
    │   │   ├── BP-02-Service-Management.bpmn
    │   │   ├── BP-02-Service-Management.png
    │   │   ├── BP-03-Package-Management.bpmn
    │   │   ├── BP-03-Package-Management.png
    │   │   ├── BP-04-Business-Information-Management.bpmn
    │   │   ├── BP-04-Business-Information-Management.png
    │   │   ├── BP-05-Client-Acquisition.bpmn
    │   │   ├── BP-05-Client-Acquisition.png
    │   │   ├── BP-06-Inquiry-Management.bpmn
    │   │   ├── BP-06-Inquiry-Management.png
    │   │   ├── BP-07-Booking-Management.bpmn
    │   │   ├── BP-07-Booking-Management.png
    │   │   ├── BP-08-Payment-Management.bpmn
    │   │   ├── BP-08-Payment-Management.png
    │   │   ├── BP-09-Invoice-Management.bpmn
    │   │   └── BP-09-Invoice-Management.png
    │   └── use-case-diagrams/
    │       ├── UC-01-Portfolio.drawio
    │       ├── UC-01-Portfolio.drawio.png
    │       ├── UC-02-Services.drawio
    │       ├── UC-02-Services.drawio.png
    │       ├── UC-03-Inquiry.drawio
    │       ├── UC-03-Inquiry.drawio.png
    │       ├── UC-04A-Booking-Client.drawio
    │       ├── UC-04A-Booking-Client.drawio.png
    │       ├── UC-04B-Booking-Photographer.drawio
    │       ├── UC-04B-Booking-Photographer.drawio.png
    │       ├── UC-05-Payment.drawio
    │       ├── UC-05-Payment.drawio.png
    │       ├── UC-06-Invoice.drawio
    │       ├── UC-06-Invoice.drawio.png
    │       ├── UC-07-Business-Administration.drawio
    │       └── UC-07-Business-Administration.drawio.png
    ├── 03-architecture/
    │   ├── 01-system-overview.md
    │   ├── 02-project-structure.md
    │   ├── 03-c4-context.md
    │   ├── 04-c4-container.md
    │   ├── 05-c4-component.md
    │   ├── 06-clean-architecture.md
    │   ├── diagrams/
    │   │   ├── 03-c4-context.drawio
    │   │   ├── 03-c4-context.drawio.png
    │   │   ├── 04-c4-container.drawio
    │   │   ├── 04-c4-container.drawio.png
    │   │   ├── 05-c4-component.drawio
    │   │   └── 05-c4-component.drawio.png
    │   └── adrs/
    │       ├── ADR-001-project-structure.md
    │       ├── ADR-002-monolith.md
    │       ├── ADR-003-blade.md
    │       ├── ADR-004-tailwind-css.md
    │       ├── ADR-005-ddd.md
    │       └── ADR-006-tdd.md
    ├── 04-design/
    │   ├── 01-strategic-design.md
    │   ├── 02-tactical-design.md
    │   ├── 03-api-design.md
    │   ├── 04-validation-rules.md
    │   ├── database/
    │   │   ├── 01-conceptual-model.md
    │   │   ├── 02-logical-model.md
    │   │   ├── 03-physical-model.md
    │   │   ├── 04-erd.md
    │   │   ├── 05-indexes.md
    │   │   ├── 06-constraints.md
    │   │   ├── 07-naming-conventions.md
    │   │   └── 08-migration-strategy.md
    │   ├── sequence-diagrams/
    │   │   ├── booking.md
    │   │   ├── notification.md
    │   │   ├── payment.md
    │   │   └── portfolio.md
    │   └── state-diagrams/
    │       ├── booking-state.md
    │       ├── inquiry-state.md
    │       └── payment-state.md
    ├── 05-testing/
    │   ├── 01-testing-strategy.md
    │   ├── 02-test-plan.md
    │   ├── 03-unit-testing.md
    │   ├── 04-feature-testing.md
    │   └── 05-tdd-workflow.md
    ├── 06-deployment/
    │   ├── 01-development-environment.md
    │   ├── 02-production-deployment.md
    │   ├── 03-security-checklist.md
    │   └── 04-maintenance.md
    ├── 07-assets/
    │   └── .gitkeep
    └── 08-decisions/
        └── trade-offs.md
```

---

## 5. Directory Responsibilities

### 5.1. Repository Root

The repository root contains project-wide configuration, entry-point documentation, contribution guidance, version history, and environment orchestration.

| Path | Responsibility |
|---|---|
| `README.md` | Introduces the project and directs readers to relevant documentation. |
| `CHANGELOG.md` | Records notable project changes. |
| `CONTRIBUTING.md` | Defines contribution and collaboration guidance. |
| `LICENSE` | Specifies the project's licensing terms. |
| `.gitignore` | Excludes files and directories that should not be tracked by Git. |
| `.gitattributes` | Defines Git attributes and file-handling conventions. |
| `.editorconfig` | Promotes consistent editor formatting. |
| `docker-compose.yml` | Defines the current Docker Compose configuration. Its services and operational status are determined by its actual contents. |
| `.github/` | Contains GitHub collaboration templates and workflow locations. |
| `docker/` | Reserved for supporting Docker files. |
| `scripts/` | Reserved for repeatable project utility scripts. |
| `src/` | Application source-code root. |

The existence of a configuration file does not establish that the associated environment or process is fully operational.

---

### 5.2. Project Management — `docs/00-project/`

Contains project planning and progress records.

- `roadmap.md` defines the high-level sprint sequence and goals.
- `sprint-backlog.md` records planned and actionable sprint work.
- `changelog.md` records project progress at the documentation level.

The roadmap governs sequencing; the sprint backlog provides more detailed work tracking.

---

### 5.3. Requirements — `docs/01-requirements/`

Contains the vision, business goals, stakeholders, assumptions, constraints, functional requirements, and non-functional requirements.

These documents define the business problem, intended outcomes, project boundaries, and requirements against which the system will be designed and evaluated.

---

### 5.4. Domain Analysis — `docs/02-analysis/`

Contains business process analysis, use cases, booking lifecycle, domain events, ubiquitous language, and the domain model.

The `bpmn/` directory contains editable BPMN process models and PNG previews. The `use-case-diagrams/` directory contains editable Draw.io diagrams and PNG previews.

Editable source diagrams are the primary artifacts for future changes. Preview images should be regenerated when their source diagrams change.

The analysis documents establish the business and domain understanding used to guide architecture and design.

---

### 5.5. Software Architecture — `docs/03-architecture/`

Contains the system overview, project structure, C4 architecture documentation, and Clean Architecture guidance.

The `adrs/` directory contains the current Architecture Decision Records:

- `ADR-001-project-structure.md`
- `ADR-002-monolith.md`
- `ADR-003-blade.md`
- `ADR-004-tailwind-css.md`
- `ADR-005-ddd.md`
- `ADR-006-tdd.md`

ADRs record significant decisions and their rationale. This document describes the repository organization and must remain consistent with the accepted ADRs.

---

### 5.6. System Design — `docs/04-design/`

Contains strategic design, tactical design, API design, validation rules, database artifacts, sequence diagrams, and state diagrams.

The `database/` directory separates database documentation into conceptual, logical, and physical models, an ERD, indexes, constraints, naming conventions, and migration strategy.

The `sequence-diagrams/` directory contains the current booking, notification, payment, and portfolio interaction documents. The `state-diagrams/` directory contains the current booking, inquiry, and payment state documents.

These documents describe the intended design. They must be reconciled with the domain model and approved architecture before implementation relies on them.

---

### 5.7. Testing — `docs/05-testing/`

Contains the testing strategy, test plan, unit-testing guidance, feature-testing guidance, and TDD workflow.

These documents define how quality will be verified and how tests will guide implementation. Their guidance must remain consistent with the approved application architecture and selected testing tools.

---

### 5.8. Deployment — `docs/06-deployment/`

Contains the development environment guide, production deployment guide, security checklist, and maintenance guide.

These documents cover environment setup, deployment, operational security, and maintenance. Instructions must be verified against the actual infrastructure before being treated as executable procedures.

---

### 5.9. Project Assets — `docs/07-assets/`

Reserved for supporting project assets. It currently contains only `.gitkeep`.

Assets placed here should have a clear purpose and must not duplicate editable diagrams or source artifacts maintained in their designated locations.

---

### 5.10. Project Decisions — `docs/08-decisions/`

Contains `trade-offs.md`, which records relevant trade-offs that do not necessarily require a dedicated ADR.

This document complements the ADRs; it must not replace the formal decision process for significant architectural changes.

---

### 5.11. Development Infrastructure and Scripts

The `docker/` and `scripts/` directories currently contain only `.gitkeep`.

They are reserved locations, not evidence of completed Docker configuration or utility scripts. Their contents should be added when required by the approved development and deployment design.

---

## 6. Application Source Structure

The `src/` directory currently contains only `.gitkeep`. Its internal Laravel structure has not yet been defined.

The source structure will be designed during Sprint 2, informed by:

- The accepted monolithic architecture.
- The system overview and C4 architecture.
- The domain model, ubiquitous language, and bounded-context decisions.
- The Clean Architecture approach.
- The DDD and TDD decisions recorded in the ADRs.
- The business processes and lifecycle rules the application must support.

Sprint 3 will establish the Laravel application and its initial directory structure after those decisions are approved.

No proposed Laravel subdirectories are represented as existing repository content in this document.

---

## 7. Structural Rules

The following rules govern future repository changes.

### 7.1. Naming and Placement

1. Preserve existing file and directory names unless a change is explicitly approved.
2. Follow the established numbering and naming conventions within documentation directories.
3. Place new documents in the section corresponding to their primary responsibility.
4. Keep editable diagrams alongside their preview images, as currently established.
5. Avoid duplicate documents that describe the same concern without a documented reason.

---

### 7.2. Documentation Integrity

1. The repository tree must reflect the actual repository whenever this document is updated.
2. Planned artifacts must be described as planned rather than presented as existing.
3. Changes to the repository structure must be reflected in this document.
4. Significant architectural changes must be evaluated against the relevant ADRs.
5. Related documents must be updated when a structural change affects their content or references.
6. All referenced paths must use the actual repository names and capitalization.

---

### 7.3. Application Structure

1. Do not scaffold Laravel before the internal application structure has been approved.
2. Do not introduce application directories merely to anticipate unspecified future needs.
3. Keep application code under `src/` once implementation begins.
4. Align implementation with the approved architecture and domain boundaries.
5. Introduce additional directories only when they serve an identified responsibility.

---

### 7.4. Git and Review

Repository changes must be version-controlled and reviewable. Each meaningful change should have a clear Git commit message describing the change.

Changes involving architecture, documentation, and implementation must be reviewed for consistency across affected artifacts.

---

## 8. Relationship to the Roadmap

The repository evolves according to `docs/00-project/roadmap.md`.

| Sprint | Structural relevance | Expected outcome |
|---|---|---|
| Sprint 0 — Foundation & Planning | Establish repository conventions, documentation sections, initial ADRs, roadmap, and backlog. | A consistent project foundation. |
| Sprint 1 — Requirements & Domain Analysis | Establish the requirements and domain artifacts that inform the design. | An agreed understanding of the business and domain. |
| Sprint 2 — System Design | Finalize the architecture, application structure, component boundaries, and database design. | An approved design that guides implementation. |
| Sprint 3 — Application Setup | Scaffold Laravel and establish the approved source structure and development environment. | A runnable application foundation aligned with the approved design. |

This table summarizes the relationship between the repository and the roadmap; it does not replace the roadmap or sprint backlog.

---

## 9. Related Documents

| Document | Relationship |
|---|---|
| `docs/README.md` | Index of the project documentation. |
| `docs/00-project/roadmap.md` | Defines the high-level sprint sequence. |
| `docs/00-project/sprint-backlog.md` | Tracks planned sprint work. |
| `docs/01-requirements/01-vision.md` | Defines the project vision. |
| `docs/01-requirements/04-assumptions-register.md` | Records assumptions that influence scope and design. |
| `docs/01-requirements/05-constraints.md` | Defines project constraints. |
| `docs/01-requirements/06-functional-requirements.md` | Defines required business capabilities. |
| `docs/01-requirements/07-non-functional-requirements.md` | Defines quality and operational requirements. |
| `docs/02-analysis/01-business-process.md` | Describes the business processes. |
| `docs/02-analysis/03-booking-lifecycle.md` | Defines the booking lifecycle. |
| `docs/02-analysis/04-domain-events.md` | Documents domain events. |
| `docs/02-analysis/05-ubiquitous-language.md` | Establishes shared domain terminology. |
| `docs/02-analysis/06-domain-model.md` | Describes the domain model. |
| `docs/03-architecture/01-system-overview.md` | Describes the system at a high level. |
| `docs/03-architecture/03-c4-context.md` | Describes the system context. |
| `docs/03-architecture/04-c4-container.md` | Describes the container-level architecture. |
| `docs/03-architecture/05-c4-component.md` | Describes the component-level architecture. |
| `docs/03-architecture/06-clean-architecture.md` | Defines the intended Clean Architecture approach. |
| `docs/03-architecture/adrs/ADR-001-project-structure.md` | Records the project-structure decision. |
| `docs/03-architecture/adrs/ADR-002-monolith.md` | Records the monolithic architecture decision. |
| `docs/03-architecture/adrs/ADR-003-blade.md` | Records the Blade decision. |
| `docs/03-architecture/adrs/ADR-004-tailwind-css.md` | Records the Tailwind CSS decision. |
| `docs/03-architecture/adrs/ADR-005-ddd.md` | Records the DDD decision. |
| `docs/03-architecture/adrs/ADR-006-tdd.md` | Records the TDD decision. |
| `docs/04-design/01-strategic-design.md` | Documents strategic domain design. |
| `docs/04-design/02-tactical-design.md` | Documents tactical domain design. |
| `docs/04-design/03-api-design.md` | Documents API design. |
| `docs/04-design/04-validation-rules.md` | Documents validation rules. |
| `docs/04-design/database/01-conceptual-model.md` | Describes the conceptual database model. |
| `docs/04-design/database/02-logical-model.md` | Describes the logical database model. |
| `docs/04-design/database/03-physical-model.md` | Describes the physical database model. |
| `docs/04-design/database/04-erd.md` | Documents the entity-relationship diagram. |
| `docs/04-design/database/05-indexes.md` | Documents database indexes. |
| `docs/04-design/database/06-constraints.md` | Documents database constraints. |
| `docs/04-design/database/07-naming-conventions.md` | Defines database naming conventions. |
| `docs/04-design/database/08-migration-strategy.md` | Documents the migration strategy. |
| `docs/05-testing/01-testing-strategy.md` | Defines the testing strategy. |
| `docs/05-testing/05-tdd-workflow.md` | Defines the TDD workflow. |
| `docs/06-deployment/01-development-environment.md` | Documents the development environment. |
| `docs/06-deployment/02-production-deployment.md` | Documents production deployment. |
| `docs/06-deployment/03-security-checklist.md` | Provides the deployment security checklist. |
| `docs/06-deployment/04-maintenance.md` | Documents maintenance practices. |
| `docs/08-decisions/trade-offs.md` | Records relevant project trade-offs. |

---

## 10. Document Maintenance

This document must be updated when:

- A directory or file is added, removed, or renamed in a way that changes the documented structure.
- An approved architecture decision changes the repository organization.
- Laravel scaffolding introduces the application structure under `src/`.
- The responsibilities of existing directories materially change.
- A structural change affects references in other documentation.

Updates must preserve consistency with the roadmap, sprint backlog, relevant ADRs, and actual repository state.

**Current status:** This document describes the repository before Laravel application scaffolding. The internal application structure remains subject to the approved Sprint 2 design decisions.

---

