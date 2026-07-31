# Shipping

Goal: releases are small, observable, and reversible. Production does not become the first realistic test environment.

## Environments

| Environment | Purpose | Data |
|---|---|---|
| Local | development and isolated tests | disposable/seeded |
| Staging | production-shaped verification | synthetic, never copied casually from production |
| Production | customer use | sensitive, backed up, access controlled |

Environment configuration uses documented variables and protected secrets. `.env.example` contains names and safe defaults only.

## Required CI gates

1. Locked dependency installation (`pnpm`, `uv`).
2. TypeScript formatting, ESLint, and strict type checking.
3. Python Ruff, mypy, and Pytest.
4. API integration tests against PostgreSQL.
5. OpenAPI generation and generated-client drift check.
6. Web and mobile unit/component tests.
7. Next.js, Expo, FastAPI container, and migration build/validation.
8. Documentation-link and secret scanning when configured.

No merge on red.

## Web and API deployment

- Merge to `main` produces immutable container images identified by commit SHA.
- Deploy to staging automatically; run health and critical-journey smoke tests.
- Production promotion is explicit until release confidence justifies further automation.
- Caddy routes only to healthy application processes.
- Rollback redeploys the previous known-good images.

## Database migrations

- Back up and verify prerequisites before risky migration work.
- Run reviewed Alembic migrations as a dedicated deployment step, not independently in every API replica.
- Apply expand–migrate–contract so old and new application versions coexist during rollout.
- Never rely on reversing a destructive migration to recover data; restore/forward repair procedures are planned.

## Mobile releases

- Expo development builds are used during development; production artifacts are real iOS/Android builds.
- Native dependency, permission, or SDK changes require store builds.
- JavaScript over-the-air updates are deferred until an explicit update/rollback policy is approved.
- Roll out store releases gradually when the stores support it.
- The API preserves compatibility with released mobile clients throughout rollout.
- EAS services are optional; local or independent CI builds remain supported.

## Observability

Minimum signals before commercial launch:

- Structured API logs with request IDs and no sensitive report/audio content.
- Health/readiness checks for Next.js, FastAPI, and PostgreSQL connectivity.
- Error-rate and latency monitoring for API and transcription.
- Mobile/web crash and client-error reporting under an approved privacy configuration.
- Disk, memory, CPU, database connections, backup age, and certificate-expiry monitoring.

## Release checklist

- [ ] CI green and artifacts immutable.
- [ ] Staging smoke tests passed.
- [ ] Migration compatibility and rollback/recovery reviewed.
- [ ] Backup age and restore capability acceptable.
- [ ] Required environment variables/secrets present.
- [ ] Alerts and dashboards can reveal the changed failure modes.
- [ ] Changelog/version updated when user-visible.
- [ ] Previous application images and rollback commands known.
- [ ] Post-deploy verification completed.

## Incidents

Mitigate customer impact first, preserve evidence, communicate clearly, then diagnose. Follow [runbooks/incident-response.md](runbooks/incident-response.md). Any cross-workspace access, data loss, credential leak, or unrecoverable backup failure is highest severity.
