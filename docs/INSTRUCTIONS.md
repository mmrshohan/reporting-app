# How work gets done

## Delivery loop

```text
Request → Frame → Design → Build → Review → Ship → Verify → Learn
```

### 1. Frame

Write one sentence describing who needs what and why. Add acceptance criteria, failure/recovery behavior, and explicit out-of-scope items. If the work does not support the product job, stop.

### 2. Design proportionally

- **XS:** copy, documentation typo, isolated configuration correction — no design note.
- **S:** one component or endpoint — acceptance criteria and test approach.
- **M:** a screen, report workflow, schema change — short design note with alternatives and risks.
- **L:** subsystem, auth change, provider integration — ADR, security review, rollout and rollback plan.

Design identifies boundaries, data flow, errors, testing, migration, and observability before implementation.

### 3. Build vertically

Implement the smallest end-to-end slice that proves the behavior. Follow the dependency direction in [ARCHITECTURE.md](ARCHITECTURE.md). Write or update tests with behavior. Avoid speculative extension points and unrelated cleanup.

### 4. Self-review

Read the full diff as a reviewer. Remove debug code, accidental generated changes, dead code, secrets, vague names, duplicated contracts, and unnecessary dependencies. Confirm documentation and `.env.example` are current.

### 5. Review

Follow [CODE_REVIEW.md](CODE_REVIEW.md). When only one engineer is available, use the same checklist, automated checks, and a deliberate second-pass self-review. Once another engineer is available, sensitive or production changes require human review.

### 6. Ship

Follow [SHIPPING.md](SHIPPING.md). Deploy to staging, run smoke tests, promote deliberately, and preserve a rollback path. Database migrations precede destructive cleanup through expand–migrate–contract.

### 7. Verify

Verify behavior in the real target: browser for web, development/production build for mobile, test PostgreSQL for persistence, and staging for integrations. "Tests pass" is evidence, not the entire verification.

### 8. Learn

Record decisions, incidents, surprising constraints, and runbook improvements. Fix the system that allowed a failure, not only the immediate symptom.

## Working agreements

- Make reasonable, reversible assumptions and record consequential ones.
- Surface conflicts between requirements and rules early.
- Report test failures and skipped verification honestly.
- Prefer one obvious way to perform common tasks.
- Commands documented in the repository must work from a clean checkout.
- Do not depend on unrecorded local machine state.

## Definition of Done

The acceptance criteria are met; tests and static checks pass; security and tenancy are verified; documentation/contracts/migrations are current; deployment and rollback are understood; the feature is exercised in its real environment; observability can reveal failure.
