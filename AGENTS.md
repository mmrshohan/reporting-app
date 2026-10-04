# AGENTS.md

## Mission

Build a production-ready web and mobile product with exceptional
engineering, UX and visual quality.

Quality is non-negotiable.

Optimize token/context usage aggressively when quality is unaffected,
but never choose a weaker approach merely to save tokens.

## Project Foundation

Before architecture or implementation work, read `docs/TECH_STACK.md`,
`docs/ARCHITECTURE.md`, and the relevant ADRs. The active foundation is
a mobile-first TypeScript monorepo with Expo/React Native, Next.js,
NestJS, PostgreSQL, versioned REST/OpenAPI, and provider-neutral AI.

Treat this as a production-grade, evolving startup product. One engineer
may perform several expert roles; that changes sequencing and automation,
not the quality standard. A material stack change requires an ADR.

## Model / Reasoning Routing

Use the strongest available reasoning when the task materially benefits
from it.

Prefer GPT-6 Astra / highest-capability available Codex reasoning for:

- product and technical architecture
- planning major features
- UI/UX direction and critique
- complex frontend architecture
- difficult backend architecture
- agent architecture
- security-sensitive decisions
- difficult debugging
- major refactors
- ambiguous or expensive-to-reverse decisions
- final review of critical work

Use faster/lower-cost execution for routine work when it can achieve
the SAME production quality:

- implementing an established design
- straightforward components
- routine API endpoints
- tests
- repetitive refactors
- documentation
- mechanical changes
- simple bug fixes

If uncertain whether stronger reasoning would materially improve an
important result, escalate.

Never downgrade UI, architecture, security or correctness to save usage.

## Roles

Think in specialized roles and delegate independent work when useful:

- Product/UX Architect
- UI Design Reviewer
- Frontend Engineer
- Backend Engineer
- Mobile Engineer
- AI/Agent Engineer
- Security Reviewer
- QA/Test Engineer

Do not create agents merely for ceremony. Delegate when parallel work,
specialized reasoning or isolated context improves the result.

## Planning

Plan before coding significant features.

For major features/refactors:

1. understand the existing system
2. identify requirements and edge cases
3. decide architecture
4. define implementation sequence
5. identify testing strategy
6. then implement

Use an ExecPlan for substantial multi-step work.

Small, reversible changes do not require elaborate planning.

## UI/UX: Zero Compromise

UI quality is a first-class product requirement.

Do not ship a UI merely because it works.

Important interfaces must be reviewed for:

- information hierarchy
- interaction design
- typography
- spacing
- consistency
- responsiveness
- mobile behavior
- accessibility
- loading/empty/error/success states
- perceived quality

Avoid generic AI-generated SaaS aesthetics:

- unnecessary cards
- excessive pills
- gratuitous gradients
- cards inside cards
- arbitrary shadows
- excessive rounding
- generic dashboard layouts

Prefer restraint, clarity, excellent typography, deliberate spacing,
strong hierarchy and purposeful interaction.

If visual judgment is important, use the strongest available reasoning.

After implementing an important UI, review the rendered result and
iterate. Do not judge UI from source code alone.

## Engineering

Production quality only.

Prefer:

- simple maintainable architecture
- strong typing where appropriate
- reusable but non-overengineered components
- secure defaults
- clear boundaries
- minimal duplication
- meaningful error handling

Avoid speculative abstractions and unnecessary dependencies.

Before changing architecture, understand the existing architecture.

## Web + Mobile

Treat web and mobile as intentional experiences.

Do not merely shrink desktop UI for mobile.

Reuse business logic and design primitives where appropriate while
allowing platform-specific interaction patterns.

## Testing

Testing is part of implementation, not an optional final step.

For meaningful changes:

- run relevant unit/integration tests
- test important user flows
- test edge cases
- test failures
- check responsive behavior
- check regressions

Critical flows should receive end-to-end coverage when practical.

Never claim completion when relevant tests are failing.

## Debugging

Investigate root cause before patching symptoms.

Escalate reasoning when:

- normal debugging fails
- several systems interact
- data integrity/security is involved
- architecture may be responsible

## Context / Token Efficiency

Save tokens by reducing waste, not intelligence.

- inspect relevant files before broad repository scans
- avoid repeatedly reading unchanged files
- delegate isolated investigations when useful
- summarize findings between agents
- reuse existing components and architecture knowledge
- keep plans concise but sufficient
- compact long-running context when appropriate

Never omit necessary investigation just to reduce context usage.

## Definition of Done

A feature is done only when:

- behavior is correct
- UI/UX meets the product standard
- desktop/mobile behavior is appropriate
- important states are handled
- tests pass
- no obvious regression is introduced
- security implications are addressed
- code is maintainable

Working != finished.

## Final Rule

Use the cheapest/faster path that preserves exceptional quality.

When stronger reasoning can materially improve an important outcome,
use it.

QUALITY > TOKEN SAVINGS.
EFFICIENCY > WASTE.
