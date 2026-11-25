# RESCREW System Architecture

## Overview
Use an API-first modular monolith for the pilot. Web and mobile clients share one FastAPI backend and one authoritative PostgreSQL database. Background workers and AI capabilities may run as separate processes. Avoid premature microservice complexity.

## Logical components

| Component | Responsibility |
|---|---|
| Next.js web client | NGO, shelter, veterinary, and platform admin portals |
| React Native + Expo | Citizen and volunteer/responder apps |
| FastAPI | Authentication, authorization, domain rules, validation, APIs |
| PostgreSQL + PostGIS | Transactional records and geospatial queries |
| Redis | Cache, task queue broker, and ephemeral coordination |
| Background worker | Notifications, media processing, reminders, retryable jobs |
| S3-compatible storage | Private photos, videos, and documents |
| AI adapter | Optional human-reviewed image/triage assistance |
| External adapters | Maps, push, SMS/email, QR/RFID, optional GPS devices |

## High-level flow

    Citizen / Volunteer / NGO / Vet / Shelter
                       |
                 Web or Mobile
                       |
               FastAPI AuthN/AuthZ
                       |
                  Domain Modules
          /          |          |          \
 PostgreSQL       Redis      Object      External
 + PostGIS                   Storage     Providers
                               |
                        Background Worker
                               |
                       Push / Email / SMS

## Multi-NGO model
- Users may belong to multiple organizations through memberships.
- Organizations have verification status, service areas, capabilities, and operating status.
- Each case has an owning organization and controlled collaboration/transfer records.
- Cross-NGO visibility is granted explicitly; do not expose every report to every NGO.
- Every write checks both role permission and organization/resource scope.
- Consider PostgreSQL row-level security only after policies and tests are designed; application authorization remains mandatory.

## Domain modules
- Incident: report intake, media, location, duplicate candidates, verification, priority.
- Animal registry: persistent profiles and RESCREW, QR, and RFID identifiers.
- Case management: rescue lifecycle, animal links, assignments, events, handovers.
- Organization: NGO verification, memberships, service areas, capabilities.
- Facility: shelters, services, capacity, admission.
- Care: assessment, diagnosis, treatment, vaccination, follow-up.
- Notification: preferences, templates, delivery attempts, retries.
- Audit: security and operational history.
- AI adapter: model invocation, provenance, confidence, human review, safe failure.

These modules can initially live inside one FastAPI application.

## Reliability and event flow
1. Client authenticates and calls the API over HTTPS.
2. API validates input, role, membership, and resource scope.
3. Domain service applies rules and commits changes.
4. Important state changes append event/audit records.
5. An outbox/background worker handles notifications and asynchronous side effects.
6. External provider failures must not erase committed rescue records; retry and expose delivery status.

## Security baseline
- HTTPS everywhere.
- Modern password hashing; never store plaintext passwords.
- Short-lived access tokens and revocable sessions/refresh tokens.
- Role plus organization/resource-scope authorization.
- Rate limits and abuse controls for public SOS and OTP endpoints.
- Validate upload size, MIME type, extension, and content.
- Private object storage and short-lived media access URLs.
- Secrets in environment files excluded from Git or a deployment secret manager.
- Audit sensitive actions and administrator operations.
- Encrypted backups and tested restore procedures.
- Public animal lookup must not expose owner contacts, exact sensitive locations, or full medical records.

## Location and identity
- Phone GPS records report/responder location only with platform permission.
- PostGIS supports nearby searches and service-area checks.
- A maps adapter provides routing and ETA.
- QR is an optical identifier.
- RFID/microchip is read by a compatible scanner; a typical implanted identification chip does not provide GPS.
- Live animal tracking requires a separate GPS-enabled device/collar, power, connectivity, and authorization.

## Offline responder approach
- Cache only assigned cases and minimum field data.
- Queue permitted updates locally with idempotency keys.
- Store client event time and server receipt time.
- On sync, revalidate authorization and case state.
- Return conflicts for explicit resolution; never silently overwrite.
- Show last successful sync and pending updates.
- Protect local data and clear it on logout/device revocation as appropriate.

## Initial deployment topology
- Web application deployment.
- FastAPI application deployment.
- Background worker.
- Managed PostgreSQL with PostGIS.
- Managed Redis.
- Private object storage.
- External maps and notification providers.
- Docker-based local development; CI for lint, tests, migrations, and builds; staging before production.

## Architecture decisions to record
Create decision records in docs/architecture/decisions for authentication, modular monolith boundaries, multi-tenant authorization, maps/notifications, object storage/retention, offline sync conflicts, and AI governance.
