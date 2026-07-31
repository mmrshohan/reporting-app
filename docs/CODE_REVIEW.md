# Code review

Review protects correctness, user data, product quality, and the next engineer's ability to understand the system. It is not a formatting debate; automation owns mechanical style.

## Author checklist

- [ ] The PR explains problem, approach, verification, risks, and rollback.
- [ ] One coherent change; unrelated cleanup removed.
- [ ] Full diff self-reviewed; no debug code, dead code, secret, or accidental generated output.
- [ ] Format, lint, types, tests, contract drift, and builds pass.
- [ ] New behavior has meaningful tests at the lowest useful layer.
- [ ] Workspace authorization and data-loss paths were considered.
- [ ] UI includes evidence for relevant sizes/themes/states and accessibility.
- [ ] API, migration, documentation, `.env.example`, ADR, and runbook changes are included when applicable.

## Reviewer priorities

### Correctness and recovery

- Does behavior match acceptance criteria under empty, slow, failed, repeated, and concurrent conditions?
- Can a failed request lose or overwrite a draft/report?
- Are retries idempotent and bounded?
- Are lifecycle transitions explicit and valid?

### Security and tenancy

- Is identity authenticated and the workspace action separately authorized?
- Does the database query itself scope tenant ownership?
- Are input size/type and provider boundaries validated?
- Are secrets and sensitive content absent from clients, logs, and errors?
- Does this change need a threat review or provider/privacy decision?

### Architecture and simplicity

- Does dependency direction remain UI → client → router → service → repository?
- Is business logic duplicated or placed in transport/UI code?
- Is a new abstraction proven, named by responsibility, and simpler than repetition?
- Is a new dependency or service necessary now?
- Can the code be understood without knowing an undocumented convention?

### Types, contracts, and data

- Are invalid states prevented with types, validation, or constraints?
- Is OpenAPI still the cross-language source of truth?
- Is the migration safe for deployed clients and rollback?
- Are query count, pagination, indexes, and transaction boundaries appropriate?

### Tests and verification

- Do tests prove behavior rather than private implementation?
- Are integration claims backed by a real test database/provider fake at the right boundary?
- Are failure paths and tenant isolation explicit?
- Was the change exercised in its real environment?

### UI quality

- Loading, empty, error, offline, success, and permission-denied states are intentional.
- Keyboard, focus, screen reader, touch target, contrast, and reduced motion behavior are correct.
- Tokens are used and typing remains responsive during network operations.

## Review language

- **Blocker:** correctness, security, data loss, contract break, rule violation; must resolve.
- **Should:** strong maintainability/product recommendation; fix or record why not.
- **Nit:** optional polish; never hold a merge hostage.

Critique the code and its effect, not the person. Ask for clarification when intent is unclear; be direct when evidence shows a defect.

## Approval policy

With one engineer, the author performs the checklist, all automated gates, and a separate final diff review. Once a second engineer is available, production changes require one human approval; authentication, authorization, migrations, and provider/privacy changes require the relevant specialist review.

Merge only when blockers are resolved, CI is green, and the author can state how the result was verified and rolled back.
