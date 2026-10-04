# Architecture

Owner: Staff Engineer. Material changes require an architecture decision record (ADR).

## System shape

```text
Web browser ──▶ Next.js ───────────────┐
                                       │ HTTPS /api/v1
Mobile app ──▶ Expo + React Native ────┤
                                       ▼
                               NestJS modular API
                              ┌────────┴────────┐
                              ▼                 ▼
                         PostgreSQL        AI providers
```

Production requires an HTTPS edge that routes `/api/*` to NestJS and other requests to Next.js. The specific reverse proxy, load balancer, or managed edge is deliberately selected during deployment, when the host and operational constraints are known. It is infrastructure and never part of NestJS or the product contract.

## Responsibilities

### Next.js web

- Web rendering, navigation, forms, accessibility, and local draft recovery.
- Uses the generated API client; does not own business rules or direct database access.
- May perform web-specific rendering on the server, but the public product API remains NestJS.

### Expo and React Native mobile

- Native iOS and Android interface, recording, permissions, secure token storage, and local drafts.
- Uses Expo development builds rather than Expo Go for production work.
- Hermes is the JavaScript runtime; native code remains available through Expo prebuild and development builds.

### NestJS core API

- The single core API for web and mobile, running on Node.js LTS with the default Express adapter.
- Owns authentication, authorization, workspace rules, report lifecycle, AI orchestration, and the OpenAPI contract.
- Controllers remain thin; services own use cases and transaction boundaries; repositories own persistence.
- Begins as one deployable modular monolith. Domain modules are explicit boundaries, not separate services.

### PostgreSQL

- System of record for accounts, workspaces, memberships, reports, revisions, sessions, and transcription metadata.
- Self-hosted in the initial architecture.
- Accessed through the latest stable Prisma ORM behind repository interfaces; changed only through reviewed migrations.
- Sensitive tenant tables use PostgreSQL row-level security as defense in depth in addition to application authorization.

### AI providers

- Receives bounded audio through a server-side adapter.
- Provider credentials never enter a client bundle.
- Raw audio is not retained in version one.

## Dependency direction

```text
UI → generated API client → controller → service → repository → PostgreSQL
                                      ↓
                               provider interface
```

- Controllers validate HTTP input, authenticate, and translate results.
- Services enforce business rules and transaction boundaries.
- Repositories execute persistence operations and do not return HTTP responses.
- Provider implementations translate third-party APIs behind stable internal interfaces.
- Cross-feature use goes through a feature's public service interface, never deep imports.

## Planned repository layout

```text
apps/
  web/src/
    app/
    features/
    components/ui/
    lib/
  mobile/src/
    app/
    features/
    components/ui/
    lib/
  api/src/
    main.ts
    app.module.ts
    platform/
    modules/
      identity/
      workspaces/
      reports/
      transcriptions/
      ai/
      audit/
packages/
  api-client/
  design-tokens/
  config/
database/migrations/
infrastructure/
```

Create files and layers only when they contain a real responsibility. The layout is a boundary map, not permission to generate empty abstractions.

## Core data model

- `users` — account identity and lifecycle.
- `authentication_methods` — password and future identity methods owned by a user.
- `workspaces` — `personal` or `organization` ownership boundary.
- `workspace_members` — user membership and role within a workspace.
- `reports` — title, body, lifecycle status, workspace, author, and timestamps.
- `report_revisions` — durable history for traceability and recovery.
- `refresh_sessions` — hashed refresh-token state and revocation metadata.
- `transcription_requests` — provider, status, duration, cost metadata, and errors; no raw audio.
- `ai_jobs` and `ai_results` — durable status and safe operational metadata for AI capabilities.
- `audit_events` — append-only security and lifecycle metadata without report bodies.

Every tenant-owned query is scoped by `workspace_id` and authorized against `workspace_members`. Client-supplied ownership identifiers are never trusted without a server-side membership check.

## Report lifecycle

Version one uses explicit states:

```text
draft → submitted
  │         │
  └─────────┴──▶ archived
```

State changes occur through application services and produce revision/audit information where recovery or traceability requires it. Duplicate mutation requests use idempotency protection.

## Speech-to-text flow

1. The client requests microphone permission.
2. Holding the control records bounded audio.
3. The client sends multipart audio to `POST /api/v1/transcriptions`.
4. NestJS validates authentication, workspace access, format, duration, and size.
5. The transcription service calls the configured provider with a timeout.
6. The API returns editable transcript text and non-sensitive metadata.
7. The client inserts the transcript into the local draft.
8. Temporary audio is discarded.

Transcription failure never deletes or replaces existing draft content.

## Authentication

- Passwords are hashed with Argon2id.
- Access tokens are short lived.
- Refresh tokens rotate, are stored hashed in PostgreSQL, and can be revoked.
- Web refresh credentials use Secure, HttpOnly, SameSite cookies.
- Mobile refresh credentials use the platform secure store.
- Authorization is enforced in services/repositories for every workspace resource.

See [SECURITY.md](SECURITY.md) for the full boundary.

## Local drafts and offline behavior

Version one preserves unsent drafts locally and retries failed submissions. It does not implement a general multi-device synchronization protocol. Web local persistence and mobile local persistence are adapters behind a draft-store interface so full synchronization can be designed later without embedding storage calls throughout UI components.

## Deployment

The initial production deployment is a small Docker Compose stack on a VPS. The production edge is a deployment-time decision:

```text
HTTPS edge → Next.js
           → Node.js/NestJS → PostgreSQL
```

PostgreSQL is not publicly exposed. Encrypted off-server backups and restore verification are mandatory. A separate database host, worker, Redis, or load balancer is added only after reliability or measured load requires it.

## Non-goals

No microservices, GraphQL, Kubernetes, Redis, Celery, Elasticsearch, retained audio, full offline sync, or separate web/mobile business logic in version one.
