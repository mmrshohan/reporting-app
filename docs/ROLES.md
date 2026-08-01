# Roles and ownership

Roles are responsibilities, not required headcount. Initially one engineer may wear several hats; the ownership map prevents decisions from falling between them.

| Area | Primary owner | Required consultation |
|---|---|---|
| Product scope and acceptance | Product | CTO |
| Architecture and ADRs | Staff Engineer | Security and affected domain |
| Next.js web | Web Engineer | Design, Staff Engineer |
| Expo/React Native mobile | Mobile Engineer | Design, Staff Engineer |
| FastAPI and OpenAPI | Backend Engineer | Security, Staff Engineer |
| PostgreSQL and migrations | Backend Engineer | Staff Engineer, Operations |
| Authentication/privacy | Security Engineer | Backend, Product |
| Transcription/future AI | AI Engineer | Security, Product, Backend |
| Design system/accessibility | Designer | Web, Mobile |
| CI/CD/backups/incidents | Operations Engineer | Backend, Security |
| Review quality | Reviewer | Relevant owner |

## CTO

Owns technical direction, build-vs-buy decisions, and complexity budget. Approves stack/architecture changes through ADRs and rejects novelty without current benefit.

## Product

Owns the problem, users, scope, acceptance criteria, and explicit non-goals. Features must trace to the reporting workflow.

## Staff Engineer

Owns cross-system coherence, module boundaries, API/data evolution, performance budgets, and the engineering constitution. Reviews non-trivial designs before implementation.

## Web Engineer

Owns `apps/web`: Next.js rendering, web accessibility, browser draft persistence, generated-client integration, and web performance. Does not create a parallel business backend.

## Mobile Engineer

Owns `apps/mobile`: Expo/React Native UI, permissions, recording, secure storage, local drafts, native builds, and store behavior. Uses development builds for production-grade work.

## Backend Engineer

Owns `apps/api`, OpenAPI, services, repositories, PostgreSQL schema/migrations, authentication implementation, and durable report/transcription behavior.

## Security Engineer

Owns trust boundaries, identity, tenant isolation, secrets, user-content handling, provider privacy review, threat analysis, and incident security response. Can block unsafe release.

## AI Engineer

Owns provider adapters, evaluation, language quality, latency/cost controls, model/version configuration, and future prompts. Cannot bypass Product or Security approval for user-content processing.

## Designer

Owns design tokens, interaction states, accessibility, motion, responsive behavior, and product language across web and mobile.

## Operations Engineer

Owns Docker and production-edge deployment, CI/CD, monitoring, backups, restore testing, releases, rollback, and incident runbooks.

## Reviewer

Applies [CODE_REVIEW.md](CODE_REVIEW.md), focusing on behavior, security, data safety, clarity, and simplicity. Automated review supplements but does not pretend to be independent human approval.

## Scaling ownership

Add headcount or split roles when sustained load—not aspiration—requires it. Likely first splits are mobile platform, QA, customer support, and operations. Every new role needs a clear mandate, owned surfaces, and review gate.
