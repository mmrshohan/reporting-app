# Non-negotiable rules

If a change violates one of these rules, it does not ship until the rule is deliberately changed through an ADR or policy update.

## Product and scope

1. Every feature must improve capture, review, submission, recovery, or traceability of reports.
2. Add complexity only for a current requirement or measured problem.
3. Web and mobile use the same versioned business API.

## Data and tenancy

4. Never lose an acknowledged report or silently discard a local draft.
5. Every tenant-owned record belongs to a workspace.
6. Every workspace read or write rechecks server-side membership and permission.
7. Database invariants are enforced with constraints, not comments alone.
8. Automated encrypted backups require periodic restore verification.

## Security and privacy

9. No secret, provider key, or privileged credential enters a client bundle.
10. Passwords use Argon2id; refresh tokens rotate and are stored hashed.
11. Report content, transcripts, raw audio, passwords, and tokens are never logged.
12. User content sent to a transcription or AI provider requires an approved provider/data-handling decision and clear user behavior.
13. Raw audio is not retained in version one.

## Architecture

14. UI → client → controller → service → repository is the permitted dependency direction.
15. OpenAPI is the client/server contract; generated-client drift fails CI.
16. Schema changes use reviewed Prisma/PostgreSQL migrations and safe rollout patterns.
17. No microservice, queue, cache, or infrastructure component without a demonstrated need and ADR.

## Engineering

18. TypeScript strict checks pass; unexplained `any` does not ship.
19. No silent failures, bare exception swallowing, or user-facing internal errors.
20. Tests prove changed behavior at the lowest useful layer and critical ownership/data-safety paths explicitly.
21. Documentation changes with behavior; meaningful decisions are recorded.
22. Dependencies are locked, deliberate, and reviewed.
23. Pull requests contain one coherent, reversible change.
24. `main` remains releasable; red CI does not merge.

## Design

25. One primary action per screen; states for loading, empty, error, offline, and success are intentional.
26. Design tokens, accessibility, responsive behavior, and reduced motion are requirements, not polish.
27. Draft input responds immediately; network work cannot block typing.
