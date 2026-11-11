# HEALTH’YS — multi-facility consultation core

HEALTH’YS organizes patient intake, cash payment, scheduling and consultation, then covered follow-up visits within the same facility. A single patient identity enables a shared record, which can only be viewed according to the temporary authorizations in effect.

## Active scope

- Administration of facilities, Stripe licenses, plans, quotas and teams.
- Single patient record, nursing intake, vital signs and care episodes.
- Rates, care invoices, cash collections, receipts, refunds and cash register closings.
- Scheduling, postponement and reassignment of visits, calendar and doctor queue.
- Consultation, versioned and validated reports, prescriptions and private MinIO documents.
- Covered follow-up within the same facility, temporary access and audit.
- Optional Python/OpenAI assistant depending on the plan: summary, assisted writing and dictation.
- Configurable retention (30 years by default), deletion requests and controlled historical migration.

Teleconsultation, the mother-child health record, messaging, laboratory operations, dispensing and pharmacy stock are outside the current scope. Historical data and migrations are kept to preserve history and allow the switchover; they must not reactivate these flows.

## Stack

Java 21, Spring Boot/Modulith, PostgreSQL/Flyway, Keycloak, MinIO; React/Vite/TypeScript frontend; private Python/OpenAI AI service. Patient payments are exclusively cash. Stripe is used for facility subscriptions.

## Guides

- [Core and breakdown](docs/consultation-core-refactor.md)
- [Administration and licenses](docs/PHASE2_LICENSING_API.md)
- [Patient and care](docs/PHASE3_PATIENT_CARE.md)
- [Cash billing](docs/PHASE4_CASH_BILLING.md)
- [Scheduling and queue](docs/PHASE4_PROGRAMMING_QUEUE.md)
- [Consultation and follow-up](docs/PHASE5_CONSULTATION_FOLLOW_UP.md)
- [Cash register control](docs/PHASE5_CASH_REGISTER.md)
- [AI assistant](docs/PHASE6_AI_ASSISTANT.md)
- [Retention, migration and delivery](docs/PHASE7_RELEASE.md)
- [Scope cleanup](docs/SCOPE_CLEANUP.md)
- [Keycloak theme](docs/keycloak-theme.md) and [professional registration](docs/professional-registration.md)

## Pilot switchover

The shipped mode remains `LEGACY` until the migration and the cash flow without AI have been validated. The commands of the former consultation flow become unavailable after controlled activation; the history remains viewable. Run the native PostgreSQL integration tests and an isolated PostgreSQL/MinIO restore before this activation, following the Phase 7 guide.

The [previous mobile documentation](docs/archive/PREVIOUS_MOBILE_FEATURES.md) is archived and does not describe the active scope of this version.
