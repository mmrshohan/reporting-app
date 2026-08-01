# Report

Report is a focused commercial reporting application for individuals and organizations. Users can type a report or hold a microphone control to transcribe speech, review the result, and submit it from the web or mobile app.

> **Status:** architecture approved; implementation has not started.

## Stack

| Layer | Choice |
|---|---|
| Web | React, TypeScript, Next.js App Router |
| Mobile | Expo SDK, React Native, TypeScript, Hermes, Expo Router |
| API | Python, FastAPI, Uvicorn, Pydantic |
| Database | Self-hosted PostgreSQL, SQLAlchemy 2, Psycopg 3, Alembic |
| Contract | FastAPI OpenAPI document and generated TypeScript client |
| Deployment | Docker Compose on a VPS; production HTTPS edge selected at deployment |
| Speech-to-text | Provider-neutral FastAPI adapter |

The stack is recorded in [ADR-0002](docs/decisions/ADR-0002-platform-stack.md); deferred deployment and provider choices are recorded in [ADR-0003](docs/decisions/ADR-0003-deferred-edge-and-ai-providers.md).

## Planned repository layout

```text
apps/
  web/                 Next.js web application
  mobile/              Expo and React Native application
  api/                 FastAPI application
packages/
  api-client/          generated TypeScript API client
  design-tokens/       shared visual constants
  config/              shared TypeScript configuration
database/
  migrations/          Alembic migrations
infrastructure/        Docker Compose and deployment configuration
docs/                   product, architecture, engineering, and operations
```

## Documentation reading order

1. [Product overview](docs/OVERVIEW.md)
2. [Non-negotiable rules](docs/RULES.md)
3. [Architecture](docs/ARCHITECTURE.md)
4. [Engineering standards](docs/ENGINEERING.md)
5. [API standards](docs/API.md)
6. [Security](docs/SECURITY.md)
7. [Contribution workflow](CONTRIBUTING.md)
8. [Code review](docs/CODE_REVIEW.md)
9. [Shipping](docs/SHIPPING.md)
10. [Architecture decisions](docs/decisions/)
11. [Operational runbooks](docs/runbooks/)

## Local development

Implementation commands will be added with the initial scaffold. The intended entry points are `pnpm` for TypeScript projects, `uv` for Python, and Docker Compose for PostgreSQL and the production-shaped local environment. No setup command should depend on undocumented machine state.

## Guiding principle

Powerful but not chaotic: use common tools, keep boundaries explicit, automate mechanical standards, and never trade user data safety for development speed.
