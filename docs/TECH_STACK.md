# Technical foundation

Owner: CTO and Staff Engineer. This document is the active source of truth for the product technology stack. A material change requires an ADR.

## Product posture

Report is a startup product built to production standards, not a disposable prototype. It will improve continuously as customer needs become clearer, but the foundation must support change without routine rewrites.

The product is developed with experienced product, design, mobile, web, backend, AI, security, and quality perspectives. Initially, one engineer may perform several of these roles. That constraint favors a coherent toolchain, explicit boundaries, automation, and a narrow mobile-first delivery sequence; it does not lower the quality bar.

## Approved stack

| Concern | Choice |
|---|---|
| Repository | Git monorepo with `pnpm` workspaces |
| Language | TypeScript in strict mode across mobile, web, API, and shared packages |
| Mobile | Expo, React Native, Expo Router, Hermes; development builds for production work |
| Web | React and Next.js App Router |
| Core API | Node.js Active LTS and NestJS modular monolith |
| HTTP platform | NestJS default Express adapter |
| API contract | Versioned REST under `/api/v1`, generated OpenAPI, generated TypeScript client |
| Database | Self-hosted PostgreSQL |
| Database access | Latest stable Prisma ORM behind repository interfaces; reviewed migrations |
| Authentication | Product-owned identity, credential, session, recovery, and authorization modules using reviewed cryptographic libraries |
| AI | Product-level capability interfaces with replaceable server-side provider adapters |
| Local drafts | Expo SQLite on mobile; IndexedDB on web behind platform storage adapters |
| Server state | TanStack Query in mobile and web clients |
| Web UI foundation | Tailwind CSS and accessible headless primitives; product-owned visual design |
| Mobile UI foundation | React Native primitives, shared design tokens, Reanimated, and Gesture Handler |
| Local infrastructure | Docker Compose for PostgreSQL and production-shaped dependencies |
| CI/CD | GitHub Actions |
| Observability | Structured JSON logs, request IDs, health checks, metrics, and standard telemetry boundaries |

Use the latest compatible stable releases when scaffolding. Production runtimes use Active or Maintenance LTS releases. Do not start production work on canary, experimental, beta, or release-candidate software without an ADR demonstrating a necessary benefit.

## Architecture

The system begins as one deployable modular API, not microservices:

```text
Expo/React Native mobile ─┐
                          ├── REST/OpenAPI → NestJS → PostgreSQL
Next.js web ──────────────┘                    │
                                               └── AI provider adapters
```

NestJS modules align with product domains such as identity, workspaces, memberships, reports, revisions, transcription, AI, notifications, and audit. Controllers handle transport, services own use cases and transactions, repositories own persistence, and provider adapters own external integrations.

Mobile leads the first delivery sequence. Web follows through the same public API and receives an intentional web experience, not a stretched mobile screen. The API remains independent from Next.js so installed mobile clients are not coupled to web deployments.

## API durability

- OpenAPI is the external protocol source of truth and generates the client SDK.
- Public endpoints live under `/api/v1`; changes are additive by default.
- Released mobile clients receive a documented compatibility window.
- Stable error codes, request IDs, cursor pagination, idempotency, bounded payloads, and explicit timeouts are part of the contract.
- ORM models and database rows never become public response types directly.
- Provider-specific AI models, errors, and credentials never enter the public contract.
- CI fails on undocumented OpenAPI or generated-client drift.

## Data, identity, and tenancy

PostgreSQL is the system of record. Every tenant-owned record has a `workspace_id`. Services and repositories verify membership and role for every tenant operation; PostgreSQL row-level security is defense in depth for sensitive tenant tables.

Authentication is implemented in the product, but cryptography is not invented. Passwords use Argon2id. Access credentials are short lived; refresh tokens rotate and are stored hashed. Web refresh credentials use secure HttpOnly cookies. Mobile refresh credentials use the platform secure store. The identity model allows later passkey and enterprise OIDC/SAML methods without redesigning users.

Reports have durable revisions and idempotent submission. Important activity produces audit metadata without copying report content into logs.

## AI boundary

Speech-to-text, refinement, summarization, extraction, and text-to-speech are separate product capabilities. Each sits behind a server-side interface so different providers can be evaluated or replaced independently.

AI output is proposed, reviewable content. It never silently replaces or submits a user's report. Provider/model version, prompt/configuration version, latency, usage, cost, and outcome are observable without logging sensitive content. Long-running work may use a worker from the same repository and deployment, backed initially by PostgreSQL rather than introducing Redis or microservices prematurely.

## Cross-platform sharing

Share the generated API client, design tokens, domain terminology, selected validation rules, analytics event names, and pure business utilities. Do not force-share screens, navigation, native interaction code, or complete visual components when platform-specific behavior produces a better experience.

## Testing foundation

| Layer | Tools and purpose |
|---|---|
| Domain and API unit tests | Vitest |
| API integration | NestJS test utilities, Supertest, and real PostgreSQL through Testcontainers |
| Contract | OpenAPI generation and generated-client drift checks |
| Web components | Testing Library |
| Web journeys | Playwright |
| Mobile components | React Native Testing Library |
| Mobile journeys | Maestro |
| AI | Deterministic fake adapters plus controlled quality/cost/latency evaluations |

Critical tests cover authentication, token rotation, workspace isolation, row-level policies, local draft recovery, idempotent submission, transcription failure, migration compatibility, and backup restoration.

## Deliberate exclusions

No microservices, Kubernetes, Redis, GraphQL, hosted backend/authentication platform, universal cross-platform UI framework, retained raw audio, or general real-time synchronization is included without a demonstrated need and ADR.
