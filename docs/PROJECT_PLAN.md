# RESCREW Project Plan

## Vision
Build a connected multi-NGO animal rescue platform connecting citizens, verified NGOs, rescue volunteers, veterinarians, and shelters. Initial pilot: Goa. Long-term direction: expansion across India.

## Product goals
1. Make animal emergency reporting simple and location-aware.
2. Route reports to relevant verified organizations.
3. Help coordinators assign suitable teams and track rescue progress.
4. Maintain a persistent digital identity and care history for each animal.
5. Improve shelter placement and cross-NGO handovers.
6. Provide accountable records, privacy controls, and operational reporting.

## Scope

### MVP — build first
- Authentication, roles, and organization membership.
- NGO registration, verification status, service areas, and capabilities.
- Citizen SOS with photos, location, condition, and description.
- NGO report inbox, verification, rejection reasons, and priority.
- Case creation, volunteer assignment, and status timeline.
- Basic notifications and audit history.
- Animal registry with permanent RESCREW Animal ID and QR identifier.
- Registration of existing RFID/microchip numbers.
- Basic shelter profiles, capacity confirmation, and handover.
- Basic medical and care records.

### Phase 2
- Team matching using distance/ETA, availability, skills, workload, vehicles, and equipment.
- Offline-first responder workflows and safe synchronization.
- Emergency escalation when no eligible team accepts a high-priority report.
- Duplicate incident/animal suggestions with human confirmation.
- Lost-and-found and verified reunification.
- Follow-up reminders and operational analytics.

### Later / optional
- AI-assisted image classification and triage support.
- Multilingual voice reporting.
- Optional GPS collar/device integrations.
- Disaster and mass-rescue mode.
- Foster/adoption workflows and external registry integrations.

## Key user journeys

### Citizen
Submit animal photos, GPS location, corrected pin or landmark, condition, animal count/species if known, and description. Receive a reference and privacy-safe updates.

### NGO coordinator
Verify organization and service area; review reports; check duplicates; prioritize; verify or reject with reason; create a case; assign responders; confirm shelter admission and handover; close the case with an outcome.

### Volunteer
Maintain verified profile, skills, availability, and emergency contact; accept or decline assignments; navigate; update progress; record rescue outcome and handover; synchronize queued updates after connectivity returns.

### Shelter / veterinary team
Maintain facility services and capacity; confirm admission; record intake, assessment, diagnosis, treatment, vaccination, and follow-up; acknowledge transfers.

## Domain and status principles
- Incident = incoming report.
- Animal = persistent individual animal identity.
- Case = coordinated rescue operation.
- One incident may involve multiple animals; an animal may appear in multiple incidents/cases over time.
- Suggested incident statuses: submitted, under review, verified, rejected, duplicate, closed.
- Suggested case statuses: open, assigned, accepted, en route, arrived, rescue in progress, rescued, transporting, awaiting handover, completed, unable to complete, cancelled.
- Validate transitions on the server and record them in a case timeline.
- A handover is complete only after receiving-party acknowledgement where applicable.

## Non-functional requirements
- Security: role-based access, organization scoping, secure authentication, input validation, protected media.
- Privacy: consent and data minimization; never expose private owner, volunteer, medical, or sensitive location details publicly.
- Reliability: backups, health checks, notification retries, and auditable state changes.
- Performance: pagination, indexed spatial queries, asynchronous processing for slow tasks.
- Accessibility: mobile-first forms, clear status language, multilingual-ready content.
- Resilience: manual location correction and planned offline support for responders.

## Success metrics
Establish pilot baselines before setting numeric targets:
- Report submission to first NGO review time.
- Verification to assignment time.
- Assignment acceptance rate.
- Assignment to responder arrival time.
- Rescue completion and unable-to-complete rates.
- Confirmed shelter admission and handover rate.
- Completeness of medical and follow-up records.
- Duplicate report rate and resolution time.
- Notification delivery and synchronization failure rates.

## Delivery phases

| Phase | Deliverables | Exit condition |
|---|---|---|
| 0. Foundation | Repo conventions, architecture decisions, local setup, CI, database migrations | New contributor can run checks locally |
| 1. Identity | Auth, roles, organizations, memberships, verification | Users only access permitted organization data |
| 2. SOS | Citizen reports, media, location, NGO inbox, review | Report can be submitted and reviewed end-to-end |
| 3. Rescue | Cases, assignments, status updates, notifications | Verified report can be assigned and tracked |
| 4. Animal and care | Animal IDs, QR, RFID records, shelters, medical records, handovers | Animal journey is traceable |
| 5. Reliability | Offline queue, escalation, duplicate suggestions, analytics | Field workflow handles connectivity and exceptions |
| 6. Expansion | AI assistance, voice, disaster mode, optional GPS devices | Features pass safety, privacy, and pilot validation |

## Decisions to validate before production
- NGO verification and volunteer-check processes.
- Anonymous reporting and follow-up policy.
- Priority definitions and escalation timing.
- Consent, retention, and sensitive-location policy.
- Cloud, storage, maps, SMS/WhatsApp providers, and cost limits.
- RFID scanner standards and supported chip frequencies.
- GPS collar procurement, connectivity, battery, and support.
- Veterinary review and liability boundaries for AI suggestions.

## Definition of done
A feature is complete when acceptance criteria are met; authorization and organization-scope tests pass; validation and failure states are handled; audit history exists where needed; automated tests pass; UI loading/empty/success/error states exist; docs and environment examples are updated; and no secrets or real personal/animal data are committed.
