# Reporting platform design

- **Date:** 2026-08-01
- **Status:** Approved in conversation; written for repository review

## Objective

Build a minimal commercial reporting application where individuals and organization members can type or dictate a report, review it, and submit it from the web or mobile app. The system must be maintainable by one engineer and immediately understandable to a new hire.

## Scope

Version one includes accounts, personal and organization workspaces, membership roles, report creation and submission, revision/recovery foundations, local unsent drafts, and provider-backed speech-to-text. Full offline synchronization, real-time collaboration, advanced analytics, retained audio, and AI report generation are deferred.

## Architecture

The monorepo contains a Next.js web client, an Expo/React Native mobile client, and a FastAPI core API. FastAPI owns business rules and the versioned REST contract. PostgreSQL is the system of record. The OpenAPI document generates a shared TypeScript client. Production uses HTTPS and routes web and API traffic through an edge selected when the deployment environment is known; the application does not depend on a particular proxy.

## Ownership model

Every user receives a personal workspace and may join organization workspaces. Every report belongs to one workspace. Authorization always derives from authenticated identity and server-side workspace membership; the API never accepts client ownership claims without verification.

## Report and transcription flow

A local draft is updated immediately while the user types or inserts a transcript. A bounded audio recording is sent to FastAPI, which validates it and calls a configured provider through an adapter. Returned text is editable and raw audio is discarded. Submission is idempotent and creates durable report/revision state in PostgreSQL. Network and provider failures preserve the local draft.

## Deferred voice and AI capabilities

Speech-to-text converts user-initiated audio into editable text and remains in version one, but its provider is not selected. AI refinement may later turn selected draft text into clearer, more mature language after an explicit user request and before user acceptance. Text-to-speech may later read a report aloud, but is not required for the first release. These are separate provider-neutral capabilities. OpenAI, ElevenLabs, and other candidates will be evaluated later for quality, language support, privacy, retention, latency, and cost; no provider may be activated without an ADR and security review.

## Engineering design

Code follows explicit UI → client → router → service → repository boundaries. React components and hooks are pure. Python uses PEP 8 naming and typed public interfaces. TypeScript uses strict checking. API contracts, migrations, decisions, and documentation each have one source of truth. Mechanical rules are enforced by Ruff, mypy, Pytest, ESLint, Prettier, TypeScript, and CI.

## Error handling

Errors have stable machine codes and user-safe messages. Domain errors map to HTTP responses centrally. Third-party calls have timeouts and retry only when safe. Logs are structured and exclude report content, raw audio, passwords, and tokens. Every request carries a request identifier.

## Testing

Unit tests cover business rules; integration tests use FastAPI and PostgreSQL; contract tests detect OpenAPI/client drift; component tests cover interaction; a small end-to-end suite covers authentication, workspace isolation, draft preservation, transcription failure, and report submission. Tests are deterministic and behavior-focused.

## Security and operations

Passwords use Argon2id. Access tokens are short lived; refresh tokens rotate and are stored hashed. Web credentials use secure cookies and mobile credentials use platform secure storage. Production uses HTTPS, private PostgreSQL networking, least-privilege credentials, encrypted off-server backups, tested restoration, health checks, and reversible deployments.

## Deliberate exclusions

No microservices, Redis, Celery, Kubernetes, GraphQL, Elasticsearch, external auth platform, retained audio, or speculative abstraction is included in version one.

## Acceptance criteria

- The written architecture contains no active Fastify, Supabase, Drizzle, WatermelonDB, bare-React-Native, or mobile-only guidance.
- A new engineer can identify each application, its responsibility, and the local/production toolchain from the root documentation.
- Naming, boundaries, testing, security, database, API, documentation, review, and shipping rules are explicit and internally consistent.
- The stack decision and its alternatives are preserved in ADRs.
- Missing operational knowledge is represented by substantive runbooks, not empty placeholders.
- Deployment and external-provider choices that depend on future constraints are explicitly deferred rather than prematurely standardized.
