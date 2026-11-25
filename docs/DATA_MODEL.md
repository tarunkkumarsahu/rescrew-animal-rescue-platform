# RESCREW Conceptual Data Model

This is an initial domain model for planning, not a finalized SQL schema or migration.

## Modeling rules
1. Incident is an incoming report.
2. Animal is a persistent identity record for an individual animal.
3. Case is a coordinated rescue operation.
4. One incident may involve multiple animals.
5. An animal may appear in multiple incidents and cases over its lifetime.
6. Every animal can have a RESCREW Animal ID even without a physical microchip.
7. RFID identifies; it does not provide GPS location.
8. Record case status changes, assignments, and handovers as history.

## Entity groups

### Identity and organizations
- users: account identity and profile.
- sessions: active/revocable sessions.
- organizations: NGO, shelter operator, clinic, or partner.
- memberships: user-to-organization role and status.
- volunteer_profiles: responder availability and emergency details.
- skills and volunteer_skills: skill catalogue and verified skill links.

### Facilities
- facilities: shelters, clinics, and temporary care locations.
- facility_spaces: capacity units, species/size suitability, availability.

### Animal registry
- animals: persistent profile and RESCREW Animal ID.
- animal_identifiers: QR token, RFID/microchip number, or other identifier, issuer, status, and verification metadata.
- QR tokens must be opaque, non-sequential, and revocable. Do not encode private data in QR payloads.

### Reports and rescue operations
- incidents: report, location, condition, priority, and review status.
- incident_animals: optional links to known animals; supports multiple animals per report.
- cases: rescue operation, owning organization, status, and outcome.
- case_animals: animals involved in a case.
- assignments: volunteer/team assignment, acceptance, timestamps, outcome.
- case_events: append-oriented status and activity timeline.
- handovers: transfer between responsible parties with receiving-party acknowledgement.

### Care and supporting services
- care_records: assessment, diagnosis, treatment, medication, vaccination, follow-up.
- placements: animal placement at a facility, admission and exit.
- media_assets and media_links: private media and links to incidents, animals, or cases.
- consents: authorization and scope.
- notifications and delivery_attempts: notification intent and provider outcomes.
- audit_events: security and operational actions.
- outbox_events: reliable asynchronous dispatch.
- ai_requests and ai_results: model/version, input reference, output, confidence, human review state.

## Relationship sketch

    USERS 1---* MEMBERSHIPS *---1 ORGANIZATIONS
    ORGANIZATIONS 1---* FACILITIES
    USERS 1---0..1 VOLUNTEER_PROFILES
    VOLUNTEER_PROFILES 1---* VOLUNTEER_SKILLS *---1 SKILLS
    INCIDENTS 1---* INCIDENT_ANIMALS *---1 ANIMALS
    INCIDENTS 1---* CASES
    CASES 1---* CASE_ANIMALS *---1 ANIMALS
    CASES 1---* ASSIGNMENTS
    CASES 1---* CASE_EVENTS
    CASES 1---* HANDOVERS
    ANIMALS 1---* ANIMAL_IDENTIFIERS
    ANIMALS 1---* CARE_RECORDS
    ANIMALS 1---* PLACEMENTS *---1 FACILITIES
    FACILITIES 1---* FACILITY_SPACES

## Important field planning

### incidents
UUID, nullable reporter reference if anonymous reporting is approved, source, description, species/count if known, condition/category, urgency, PostGIS point, accuracy, landmark/address text, status, verifier, timestamps.

### animals
UUID, internal/public RESCREW ID, species, optional name, optional estimated age/sex, identifying description, profile status, timestamps.

### animal_identifiers
UUID, animal reference, identifier type (QR, RFID microchip, other), issuer, protected identifier value, status, issue/verification/revocation timestamps. Enforce uniqueness for active identifiers in the relevant namespace.

### cases
UUID, incident reference, owning organization, status, priority, summary, open/close timestamps. Cross-organization access must be explicit.

### assignments
UUID, case reference, assigned user/team, assigner, status, accepted/declined/started/completed timestamps, decline or unable-to-complete reason.

### handovers
UUID, case reference, sender, receiving party/facility, initiated and received timestamps, acknowledgement status, notes. Do not complete until receipt is acknowledged.

### care_records
UUID, animal reference, optional case/facility, author, record type, clinical details, recorded time. Restrict access to authorized care providers and responsible organizations.

## Integrity and privacy
- Use UUID primary keys; do not expose sequential database IDs.
- Use foreign keys and check constraints.
- Index foreign keys, status/date filters, and PostGIS geography columns.
- Preserve history for corrections and important changes.
- Restrict exact location to authorized operational use.
- Protect owner contacts and clinical records with least privilege.
- Define retention and deletion/anonymization behavior before launch.
- Never put personal or medical details inside QR payloads.

## Open decisions
Anonymous SOS policy; animal merge permissions; case split/merge behavior; cross-NGO collaboration rules; shelter capacity reservation expiry; clinical record correction/retention; identifier protection and scanner protocols; audit retention/export.

## Implementation sequence
1. Identity, organizations, memberships, permissions.
2. Facilities and service areas.
3. Incidents and media.
4. Animals and identifiers.
5. Cases, assignments, events, handovers.
6. Care records and placements.
7. Notifications, outbox, audit, analytics.
