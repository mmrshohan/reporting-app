# ADR-0004 — TypeScript modular monolith and mobile-first delivery

- **Status:** Accepted
- **Date:** 2026-10-04
- **Deciders:** Product owner and CTO
- **Supersedes:** The core API, database-access, and delivery-order decisions in [ADR-0002](ADR-0002-platform-stack.md)

## Context

Report is a production-grade startup product intended to improve organizational communication over time. Mobile is the first product surface, web follows, AI is a core capability, and one engineer initially carries several specialist responsibilities. The foundation must support high quality and meaningful expansion without requiring a near-term rewrite.

The earlier design selected FastAPI and a mixed Python/TypeScript repository. FastAPI remains capable, but the product's initial AI work calls external providers and does not require a Python-native runtime. A single TypeScript toolchain reduces context switching, broadens the pool of engineers who can work across the product, and simplifies shared tooling without combining the web and business APIs.

## Decision

- Use a Git monorepo with `pnpm` workspaces and strict TypeScript.
- Build mobile with Expo, React Native, Expo Router, and Hermes.
- Build web with React and Next.js App Router.
- Build the independent core API as a NestJS modular monolith on Node.js LTS, using NestJS's default Express adapter.
- Publish a versioned REST/OpenAPI contract and generate the TypeScript client consumed by mobile and web.
- Use self-hosted PostgreSQL with the latest stable Prisma ORM behind repositories and reviewed migrations.
- Own authentication and authorization code/data while using established libraries for cryptography and protocol primitives.
- Keep AI capabilities behind provider-neutral server-side interfaces.
- Deliver mobile first in vertical slices, then build the intentional web experience through the same public API.
- Keep domain boundaries extraction-ready, but do not create microservices without a measured need.

## Rationale

- One language and package system lets one engineer move safely across all primary applications.
- NestJS supplies recognizable modules, dependency injection, validation, OpenAPI, and testing conventions without assembling an ad hoc Express architecture.
- An independent API protects mobile compatibility and prevents Next.js from becoming an accidental business backend.
- Expo is an open-source production framework and retains native-code access while reducing platform maintenance.
- PostgreSQL fits relational tenancy, authorization, reporting, audit, search, and transaction requirements.
- Provider-neutral AI boundaries keep product behavior stable while vendors, models, prices, and privacy terms change.
- A modular monolith supplies strong boundaries with substantially less operational risk than microservices.

## Alternatives considered

- **FastAPI/Python core:** excellent for Python-native ML, but introduces a second primary language and toolchain before that need exists.
- **Next.js as the core API:** initially smaller, but couples the mobile contract and business lifecycle to the web application.
- **Bare Express:** common and flexible, but requires the project to invent and enforce more application structure.
- **Bare React Native:** maximum direct control, but unnecessary native setup and upgrade work for one engineer.
- **Microservices:** independent deployment boundaries without current scale or team ownership to justify their operational cost.

## Consequences

- The old FastAPI, SQLAlchemy, Psycopg, Alembic, Python, and `uv` guidance is no longer active.
- TypeScript strictness, dependency direction, generated-contract checks, and module boundaries must prevent a single-language codebase from becoming a coupled codebase.
- Python may be added later as an isolated worker only when a real Python-native workload requires it.
- Stable/LTS versions are preferred over pre-release novelty; upgrades are deliberate and tested.
- The active stack is summarized in [TECH_STACK.md](../TECH_STACK.md).
