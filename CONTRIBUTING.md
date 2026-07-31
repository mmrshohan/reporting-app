# Contributing

## Start here

Read [README.md](README.md), [docs/RULES.md](docs/RULES.md), [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md), and [docs/ENGINEERING.md](docs/ENGINEERING.md) before changing code. Read the relevant ADR and feature documentation for the area you touch.

## Toolchain

- Node.js and pnpm for Next.js, Expo, and shared TypeScript packages.
- Python and uv for FastAPI.
- Docker and Docker Compose for PostgreSQL and the production-shaped local stack.
- Xcode and Android Studio for local mobile development builds.

Exact versions and working commands will be committed with the implementation scaffold. Lockfiles are authoritative.

## Expected root commands

The initial scaffold must expose one obvious command for each task:

```text
setup       install locked dependencies and initialize local config
dev         run the development environment
test        run the standard test suite
lint        run format/lint checks
typecheck   run TypeScript and Python type checks
build       build production artifacts
```

These may be implemented as documented package scripts or a Makefile, but contributors should not need to memorize application-specific command sequences.

## Branch and commit conventions

- Branches: `feat/...`, `fix/...`, `docs/...`, `chore/...`, `refactor/...`.
- Commits: Conventional Commits such as `feat(reports): add submission endpoint`.
- Keep branches short lived and rebase before merge when necessary.
- One pull request represents one coherent change.

## Pull requests

Describe:

- The problem and why it matters.
- The chosen approach and relevant alternative.
- How behavior was tested and manually verified.
- UI evidence for visual changes.
- Security, tenancy, migration, compatibility, and rollback impact.
- Documentation or ADR updates.

Follow [docs/CODE_REVIEW.md](docs/CODE_REVIEW.md). Do not request review with known failing checks unless the PR is explicitly a draft explaining the failure.

## Naming quick reference

- TypeScript: `camelCase` functions, `PascalCase` components/types, `kebab-case` files, `use...` hooks.
- Python: `snake_case` modules/functions, `PascalCase` classes, `...Error` exceptions.
- PostgreSQL: plural `snake_case` tables, `snake_case` columns, `<entity>_id` foreign keys.
- API: plural resource paths and `camelCase` JSON.

The detailed rules are in [docs/ENGINEERING.md](docs/ENGINEERING.md).

## Documentation responsibility

Behavior and documentation change together. Do not copy the same truth into multiple files. Use:

- ADRs for consequential decisions.
- Architecture for boundaries and data flow.
- Engineering for code standards.
- API for protocol policy.
- Security for trust boundaries.
- Runbooks for operational procedures.

## Getting help

If repository behavior contradicts documentation, raise the contradiction explicitly. Do not silently choose one or create a third pattern.
