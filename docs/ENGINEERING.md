# Engineering standards

Owner: Staff Engineer. Mechanical rules are automated; review focuses on correctness, clarity, product behavior, security, and architecture.

## Engineering constitution

1. Optimize for understanding; code is read more than written.
2. Prefer common, boring technology; novelty must solve a demonstrated problem.
3. Keep UI, transport, business rules, persistence, and providers separate.
4. Maintain one source of truth for contracts, schema, decisions, and configuration.
5. Make invalid states difficult through types, validation, constraints, and explicit lifecycle states.
6. Prefer composition and focused functions over inheritance and framework-like abstractions.
7. Treat errors and recovery as part of feature design.
8. Test observable behavior, not private implementation.
9. Update documentation in the same change as behavior.
10. Keep changes small, reversible, and safe for user data.
11. Measure before adding caching, concurrency, services, or infrastructure.
12. Leave code simpler or better explained in the area being changed; avoid unrelated rewrites.

## Dependency direction

```text
UI → generated API client → router → service → repository → PostgreSQL
                                      ↓
                               provider adapter
```

- A router validates HTTP input, authenticates, and translates results.
- A service owns a use case, authorization policy, and transaction boundary.
- A repository owns persistence operations and has no HTTP knowledge.
- A provider adapter translates an external API and has explicit timeout/error behavior.
- Cross-feature calls use public service interfaces; deep imports are forbidden.
- SQL does not appear in routers. HTTP responses do not appear in repositories.

## Structure

Organize code by product feature (`auth`, `workspaces`, `reports`, `transcriptions`) and then by responsibility. Shared folders contain only concepts with multiple real consumers. Do not create generic dumping grounds named `utils`, `helpers`, `common`, or `misc`.

Create a class only when it represents state, lifecycle, a domain concept, or an injected contract. Prefer ordinary functions for stateless behavior. React components are functions. Avoid `BaseService`, `Manager`, and deep inheritance trees.

Apply the rule of three to abstractions: a second similar case may be coincidence; abstract when the shared contract is stable and a third use makes the benefit clear. Security and protocol interfaces are exceptions when the boundary itself is the requirement.

## TypeScript, React, and Expo

### Compiler settings

Enable `strict`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `useUnknownInCatchVariables`, and `noImplicitOverride`. `any` requires a written justification at the narrowest boundary. Prefer `unknown` and narrow it.

### Naming

| Element | Convention | Example |
|---|---|---|
| Variables and functions | `camelCase` | `submitReport` |
| Components, types, interfaces | `PascalCase` | `ReportEditor` |
| Hooks | `use` + PascalCase | `useReportDraft` |
| Boolean values | `is`, `has`, `can`, `should` | `canSubmitReport` |
| True global constants | `UPPER_SNAKE_CASE` | `MAX_AUDIO_SECONDS` |
| Source files | `kebab-case` | `report-editor.tsx` |
| Tests | source name + `.test` | `report-editor.test.tsx` |

- Do not prefix interfaces with `I`.
- Functions use verb phrases; types and components use noun phrases.
- Event handlers use `handle...`; callback props use `on...`.
- Acronyms use normal word casing: `ApiClient`, `HttpError`.
- Avoid barrel files except at a package's intentional public boundary.

### React rules

- Components and hooks are pure; do not mutate props, state, or hook arguments.
- Side effects run in effects or event handlers, never during render.
- Keep server state in the API/query layer and ephemeral UI state near its consumer.
- Do not introduce global state until state is demonstrably cross-feature.
- Accessibility labels, focus behavior, reduced motion, loading, empty, and error states are part of completion.

## Python and FastAPI

Use Ruff for formatting/linting and mypy in strict mode. Public functions and methods are typed. Pydantic models define validated API boundaries; SQLAlchemy models are persistence models and do not double as API schemas.

### Naming

| Element | Convention | Example |
|---|---|---|
| Modules, functions, variables | `snake_case` | `submit_report` |
| Classes | `PascalCase` | `ReportRepository` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_AUDIO_SECONDS` |
| Internal members | leading underscore | `_decode_token` |
| Exceptions | PascalCase + `Error` | `ReportNotFoundError` |
| Pydantic command/input | action suffix | `ReportCreate` |
| Pydantic representation | clear output suffix | `ReportRead` |
| SQLAlchemy model | singular PascalCase | `WorkspaceMember` |

- Follow PEP 8 and prefer absolute imports within the application package.
- Catch specific exceptions and preserve causes with exception chaining.
- Use timezone-aware UTC datetimes.
- Avoid boolean positional arguments; use named parameters or explicit types.
- Public APIs receive concise docstrings explaining contract or constraints. Comments explain why, not what.

## API contract

FastAPI/Pydantic is the server source of truth. Generate the TypeScript types/client from OpenAPI and fail CI on drift. Do not manually recreate response shapes in clients. See [API.md](API.md).

## Database rules

- Tables and columns use plural-table `snake_case`: `workspace_members.workspace_id`.
- Primary keys are `id`; foreign keys are `<entity>_id`.
- Timestamps use `created_at`, `updated_at`, and optional `deleted_at`.
- Use UUIDs for identifiers exposed outside the database.
- Required invariants belong in PostgreSQL constraints as well as application validation.
- Every tenant-owned record has `workspace_id`; every access path enforces membership.
- Transactions cover operations that must succeed or fail together.
- All schema changes are Alembic migrations, reviewed and tested.
- Production migrations are forward-only and use expand–migrate–contract for breaking changes.
- Never edit a migration already applied outside local development.
- Query plans and indexes are measured; no speculative indexes or caches.

## Error handling

- Never swallow an exception.
- Catch only when the current layer can recover, add context, or translate the error.
- Domain errors map to stable API errors centrally.
- Third-party operations have timeouts and bounded payloads.
- Retry only safe/idempotent work, with bounded backoff.
- User-facing errors state what happened and how to recover without exposing internals.
- Logs are structured and include a request identifier, operation, duration, and non-sensitive ownership identifiers.
- Never log passwords, tokens, report bodies, transcripts, or raw audio.

## Testing strategy

| Layer | Purpose | Tools |
|---|---|---|
| Python unit | domain rules and pure transformations | Pytest |
| API integration | FastAPI, PostgreSQL, auth, permissions | Pytest + HTTPX + test PostgreSQL |
| Contract | OpenAPI and generated TypeScript client | generation/drift check |
| Web/mobile unit | functions and hooks | Vitest/Jest |
| Component | rendering and interaction | Testing Library |
| End-to-end | a few critical journeys | Playwright; mobile E2E when justified |

Tests use behavior names such as `test_submit_report_rejects_non_member`. Do not mock the database in tests claiming to prove database behavior. External transcription is replaced by a deterministic fake adapter. A flaky test is a broken test; diagnose it rather than hiding it behind retries.

Mandatory behavior includes workspace isolation, local draft preservation, idempotent submission, transcription failure recovery, token rotation/revocation, migration compatibility, and backup restoration.

Coverage is a diagnostic, not the goal. Critical behavior requires explicit tests even when overall coverage is high.

## Dependencies

- Prefer the language/platform standard library first.
- A dependency must have a clear owner, active maintenance, compatible license, security posture, and removal path.
- Pin runtime dependencies through committed `pnpm-lock.yaml` and `uv.lock` files.
- Dependency upgrades are focused, tested changes; security fixes jump the queue.
- Do not add Redis, a queue, a state library, or a service because it might be useful later.

## Performance

- Measure user-visible latency, API p95, error rate, mobile startup, and transcription duration.
- Set budgets per critical journey before optimizing it.
- Avoid N+1 queries and unbounded list endpoints.
- Bound audio size/duration and report request size.
- Profile before caching or parallelizing.

## Documentation

- `README.md` gets a new engineer running quickly.
- `CONTRIBUTING.md` owns workflow and commands.
- Architecture belongs in `ARCHITECTURE.md`; decisions belong in ADRs.
- API policy belongs in `API.md`; endpoint details are generated from OpenAPI.
- Operational procedures belong in `docs/runbooks/` and must be executable.
- Behavior-changing PRs update documentation in the same change.
- Superseded decisions are marked rather than erased.
- Do not create empty placeholder documents; missing knowledge is either written or explicitly scoped out.

## Definition of engineering done

Acceptance criteria met; boundaries respected; names follow conventions; types, lint, format, tests, and builds pass; authorization and privacy considered; documentation and generated contracts current; migration and rollback understood; behavior verified in a real environment; no debug code, secret, dead code, or silent failure remains.
