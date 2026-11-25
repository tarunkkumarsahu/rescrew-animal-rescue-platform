# RESCREW Development Guidelines

## Branches and commits
- Branches: feat/name, fix/name, docs/name, chore/name.
- Use Conventional Commit style, e.g. feat(api): add health endpoint.
- Do not develop features directly on main.
- Keep pull requests focused and link the issue.

## Code quality
- Use TypeScript in web/mobile clients.
- Use Python type hints in backend code.
- Keep business rules in domain/service modules, not route handlers or UI components.
- Validate request/response contracts and document APIs.
- Test authorization failures as well as happy paths.
- Use migrations for every database schema change.

## API conventions
- Version routes under /api/v1.
- Use consistent JSON response and error formats.
- Use UUID identifiers in public API resources.
- Paginate list endpoints.
- Use idempotency keys for retryable mutations such as SOS submissions and status updates.
- Never trust client-supplied organization IDs or role claims without server-side authorization.

## Security and privacy
- Keep secrets in environment variables or secret managers.
- Commit only placeholder variable names in .env.example.
- Never commit real citizen, volunteer, owner, animal medical, or rescue-location data.
- Use private media storage and short-lived access URLs.
- Apply least privilege and organization-scoped authorization.
- Avoid unnecessary personal or clinical details in logs.

## Rescue workflow rules
- Incident, Animal, and Case are separate concepts.
- Validate case status transitions on the server.
- Record status changes in the case timeline.
- A handover requires receiving-party acknowledgement.
- AI outputs are advisory and require human review for consequential decisions.
- RFID identifies; it does not locate. GPS tracking requires a separate GPS device.

## Pull request checklist
- [ ] Related issue linked.
- [ ] Scope and behavior explained.
- [ ] Tests added/updated and passing.
- [ ] Authorization and organization-scope checks covered.
- [ ] Database migrations reviewed, if applicable.
- [ ] Error/loading/empty states handled.
- [ ] Documentation updated.
- [ ] No secrets or real personal/animal data included.
