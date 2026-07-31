# Security

Owner: Security Engineer. Authentication, authorization, user-content handling, provider integration, and production access require security review proportional to risk.

## Trust boundaries

- Web and mobile clients are untrusted.
- FastAPI is the only public business API and validates every boundary.
- PostgreSQL is private and reachable only by approved application/administrative paths.
- Transcription providers are external processors; content sent to them leaves our trust boundary.
- Caddy is the public TLS termination point.

## Authentication

- Passwords are hashed with Argon2id using reviewed parameters.
- Access tokens are short lived and contain minimal claims.
- Refresh tokens are random, rotate on use, and are stored hashed with session metadata.
- Reuse of an invalidated refresh token revokes the affected token family.
- Web refresh credentials use `Secure`, `HttpOnly`, appropriate `SameSite`, narrow path/domain, and CSRF protections.
- Mobile credentials use platform secure storage; never general local storage.
- Password reset and email verification tokens are single-use, expiring, and stored hashed.
- Login, reset, and transcription endpoints are rate limited before public launch.

## Authorization and tenancy

- Every tenant-owned object has a `workspace_id`.
- Every read and write verifies current membership and required role server side.
- Queries scope ownership in the database operation rather than fetching broadly and filtering in application memory.
- Tests attempt horizontal privilege escalation between workspaces.
- Administrative access is explicit, audited, and not implemented through hidden client flags.

## Secrets

- Secrets enter production through protected environment/secrets configuration, never Git.
- `.env.example` documents names with no real values.
- Provider and signing keys are scoped, rotated, and revoked after suspected exposure.
- Secrets and tokens are redacted from logs, traces, errors, and analytics.

## User content and privacy

- Reports and transcripts are sensitive content.
- Logs never include report bodies, transcript text, raw audio, passwords, or tokens.
- Raw audio is not retained in version one.
- Provider data handling, retention, training use, and region are reviewed before activation.
- The user interface clearly indicates when audio is recorded and sent for transcription.
- Data export, deletion, and retention behavior must be specified before general commercial launch.

## Application security

- Validate all request data with Pydantic and enforce payload limits at Caddy and FastAPI.
- Use parameterized SQL through SQLAlchemy/Psycopg; never concatenate untrusted SQL.
- Escape/sanitize user content according to output context.
- Configure restrictive CORS; prefer same-origin web API routing.
- Configure security headers and HTTPS-only production traffic.
- File/audio handling uses generated temporary names and cannot control filesystem paths.
- Dependencies and container images receive regular vulnerability review.

## Database and operations

- Application database credentials use least privilege and are distinct from migration/administrative credentials.
- PostgreSQL is not exposed to the public internet.
- Backups are encrypted, access controlled, retained deliberately, and restore tested.
- Production access is logged and limited.
- Security events carry request/session metadata without sensitive content.

## Incident priorities

Credential exposure, cross-workspace access, report loss, unauthorized content disclosure, and backup failure are highest-severity events. Follow [runbooks/incident-response.md](runbooks/incident-response.md), preserve evidence, rotate affected credentials, and notify users/regulators when required.
