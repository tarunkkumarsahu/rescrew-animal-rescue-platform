# RESCREW Implementation Backlog

Planning baseline. Convert each item into a GitHub issue and refine estimates during technical discovery.

## Phase 0 — Foundation
- [ ] RES-001: Define repository conventions, branch strategy, pull request template, issue templates.
- [ ] RES-002: Scaffold FastAPI backend with config, health endpoint, logging, tests.
- [ ] RES-003: Scaffold Next.js web application with TypeScript, linting, and basic layout.
- [ ] RES-004: Scaffold React Native Expo application with TypeScript and navigation baseline.
- [ ] RES-005: Add Docker local environment for API, PostgreSQL/PostGIS, and Redis.
- [ ] RES-006: Add CI checks for tests, lint, type checks, builds, and migration validation.
- [ ] RES-007: Establish database migrations and enable PostGIS.
- [ ] RES-008: Record architecture decisions for auth, tenancy, storage, maps, and notifications.

## Phase 1 — Identity and organizations
- [ ] RES-010: Authentication and session lifecycle.
- [ ] RES-011: Roles and permission checks.
- [ ] RES-012: Organization registration and verification workflow.
- [ ] RES-013: Organization memberships and service areas.
- [ ] RES-014: Volunteer profiles, availability, and verification fields.

## Phase 2 — SOS and triage
- [ ] RES-020: Citizen SOS submission with photos, condition, description, GPS, and manual location correction.
- [ ] RES-021: Private media upload and access control.
- [ ] RES-022: NGO report inbox and service-area filtering.
- [ ] RES-023: Report review, priority, verification, rejection, and clarification.
- [ ] RES-024: Duplicate incident suggestions with human review.

## Phase 3 — Rescue operations
- [ ] RES-030: Case creation and incident linkage.
- [ ] RES-031: Volunteer/team assignment and accept/decline.
- [ ] RES-032: Validated rescue status transitions and case timeline.
- [ ] RES-033: Map display, routing, and ETA adapter.
- [ ] RES-034: Notification outbox, delivery attempts, and retry.
- [ ] RES-035: Emergency escalation rules and audit trail.

## Phase 4 — Animal, shelter, and care
- [ ] RES-040: Animal registry and permanent RESCREW Animal ID.
- [ ] RES-041: QR token issuance, scan, and protected lookup.
- [ ] RES-042: RFID/microchip identifier registration and verification.
- [ ] RES-043: Shelter/facility profiles and capacity.
- [ ] RES-044: Admission confirmation and handover acknowledgement.
- [ ] RES-045: Medical, vaccination, and follow-up records.
- [ ] RES-046: Lost-and-found matching and verified reunification.

## Phase 5 — Reliability and intelligence
- [ ] RES-050: Offline responder queue and safe synchronization.
- [ ] RES-051: Analytics and welfare follow-up reminders.
- [ ] RES-052: Role-scoped audit views and exports.
- [ ] RES-053: Evaluate AI image assistance with human review.
- [ ] RES-054: Multilingual voice-assisted reporting.
- [ ] RES-055: Disaster/mass-rescue mode.
- [ ] RES-056: Evaluate optional GPS collar/device integration.

## Cross-cutting acceptance criteria
Every feature must enforce authentication and organization scope; validate inputs; handle loading/empty/success/error states; record audit history where appropriate; include tests for critical and unauthorized paths; protect private data; update documentation; and avoid committing secrets or real personal/animal data.

## First milestone — Foundation Ready
- Backend and web/mobile scaffolds exist.
- Docker starts PostgreSQL/PostGIS and Redis locally.
- Migrations run successfully.
- CI passes on pull requests.
- Health endpoint responds.
- A new developer can run the project using documented steps.

## Recommended first implementation task
Start with RES-001, then RES-002. Keep the first backend task limited to a maintainable API skeleton, configuration, health endpoint, logging, and tests. Do not implement all rescue business logic in the bootstrap task.
