# Report

Report is a focused commercial reporting application for individuals and organizations. Users can type a report or hold a microphone control to transcribe speech, review the result, and submit it from the web or mobile app.

> **Status:** production foundation approved; implementation has not started.

## Stack

| Layer | Choice |
|---|---|
| Web | React, TypeScript, Next.js App Router |
| Mobile | Expo SDK, React Native, TypeScript, Hermes, Expo Router |
| API | Node.js LTS, TypeScript, NestJS modular monolith, Express adapter |
| Database | Self-hosted PostgreSQL and stable Prisma ORM |
| Contract | Versioned REST, generated OpenAPI, and generated TypeScript client |
| Deployment | Docker Compose on a VPS; production HTTPS edge selected at deployment |
| AI and speech | Provider-neutral NestJS capability adapters |

The active foundation is in [TECH_STACK.md](docs/TECH_STACK.md) and [ADR-0004](docs/decisions/ADR-0004-typescript-modular-monolith.md). Deferred deployment and provider choices are recorded in [ADR-0003](docs/decisions/ADR-0003-deferred-edge-and-ai-providers.md).

## Planned repository layout

```text
apps/
  web/                 Next.js web application
  mobile/              Expo and React Native application
  api/                 NestJS core API
packages/
  api-client/          generated TypeScript API client
  design-tokens/       shared visual constants
  config/              shared TypeScript configuration
database/
  migrations/          reviewed Prisma/PostgreSQL migrations
infrastructure/        Docker Compose and deployment configuration
docs/                   product, architecture, engineering, and operations
```

## Documentation reading order

1. [Product overview](docs/OVERVIEW.md)
2. [Non-negotiable rules](docs/RULES.md)
3. [Technical foundation](docs/TECH_STACK.md)
4. [Architecture](docs/ARCHITECTURE.md)
5. [Engineering standards](docs/ENGINEERING.md)
6. [API standards](docs/API.md)
7. [Security](docs/SECURITY.md)
8. [Contribution workflow](CONTRIBUTING.md)
9. [Code review](docs/CODE_REVIEW.md)
10. [Shipping](docs/SHIPPING.md)
11. [Architecture decisions](docs/decisions/)
12. [Operational runbooks](docs/runbooks/)

## Local development

Implementation commands will be added with the initial scaffold. The intended entry points are `pnpm` for the TypeScript monorepo and Docker Compose for PostgreSQL and the production-shaped local environment. No setup command should depend on undocumented machine state.

## Guiding principle

Powerful but not chaotic: use common tools, keep boundaries explicit, automate mechanical standards, and never trade user data safety for development speed.
