# ADR-0002 — Commercial reporting platform stack

- **Status:** Accepted; deployment edge and provider timing amended by [ADR-0003](ADR-0003-deferred-edge-and-ai-providers.md)
- **Date:** 2026-08-01
- **Deciders:** Product owner and CTO

## Context

Report is a commercial reporting application available on the web, iOS, and Android. Individuals own personal workspaces; organizations own shared workspaces with memberships and roles. Users type or dictate reports. The product is maintained initially by one engineer, so the stack must be conventional, powerful, and operationally small.

The product owner prefers self-hosted infrastructure over a backend-as-a-service. External transcription providers are allowed behind the server boundary.

## Decision

| Concern | Choice |
|---|---|
| Repository | Git monorepo; pnpm workspaces for TypeScript, uv for Python |
| Web | React, TypeScript, Next.js App Router |
| Mobile | Expo SDK, React Native, TypeScript, Hermes, Expo Router |
| Mobile workflow | Expo development builds; EAS optional, not required |
| Core API | Python, FastAPI, Uvicorn, Pydantic |
| API contract | Versioned REST and OpenAPI-generated TypeScript client |
| Database | Self-hosted PostgreSQL |
| Database access | SQLAlchemy 2, Psycopg 3, Alembic |
| Authentication | Argon2id passwords, short-lived access tokens, rotating refresh tokens |
| Speech-to-text | Server-side provider adapter; provider selected by configuration |
| Deployment | Docker Compose on a VPS; production HTTPS edge deferred until deployment |

## Rationale

- FastAPI provides an independent contract for two equal clients and a direct Python path for transcription or future AI work.
- Expo development builds reduce iOS and Android configuration overhead without removing access to native code or requiring EAS cloud services.
- PostgreSQL matches workspace membership, ownership, authorization, reporting, search, and transaction requirements.
- SQLAlchemy, Psycopg, and Alembic are common Python/PostgreSQL tools a new backend engineer can recognize.
- OpenAPI generation prevents manual duplication of Python and TypeScript API models.
- Keeping the production edge outside the application contract allows the deployment environment to determine the simplest appropriate HTTPS and routing solution.
- Docker Compose is sufficient for the initial deployment and local production-shaped environment.

## Alternatives considered

- **Next.js as the core API:** fewer processes, but a weaker independent boundary for two clients and Python-centered transcription work.
- **Django:** strong all-in-one framework, but more framework surface than the focused API requires.
- **Laravel:** mature, but adds PHP without a product-specific advantage.
- **Bare React Native:** maximum control, but higher native configuration and upgrade cost for one engineer.
- **Supabase/Firebase:** faster hosted primitives, but conflicts with the self-hosted product constraint.
- **MongoDB:** weaker fit than PostgreSQL for memberships, roles, constraints, and reporting queries.

## Consequences

- The repository contains both TypeScript and Python dependency systems.
- The OpenAPI document is the cross-language contract and generated client drift must fail CI.
- Authentication and database operations are our operational responsibility.
- Expo SDK upgrades and FastAPI dependency upgrades are deliberate, tested changes.
- Full offline synchronization is deferred; clients preserve local drafts only.
- The production edge and external speech/AI providers require later deployment or provider ADRs.

## Revisit when

- Long-running transcription requires a durable worker.
- PostgreSQL or application availability requires separate hosts or managed failover.
- EAS Build demonstrably saves more engineering time than it costs.
- A third-party/public API requires a broader compatibility and rate-limit program.
- A production host, domain, or platform is selected; record its HTTPS edge and certificate-management decision.
