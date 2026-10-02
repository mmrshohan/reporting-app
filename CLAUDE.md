# MODEL ROUTING

## Principle

Quality is non-negotiable.

Choose models based on the intelligence required by the task,
not simply on cost.

Never use a weaker model when doing so could materially reduce:

- product quality
- UI/UX quality
- architecture quality
- correctness
- security
- maintainability

At the same time, do not waste frontier-model tokens on mechanical
implementation that another model can perform at equivalent quality.

The objective is:

MAXIMUM PRODUCT QUALITY PER UNIT OF COMPUTE.

---

## SONNET 5 — PRIMARY BUILDER

Use Sonnet as the default implementation engine when it can achieve
production-quality results.

Best suited for:

- frontend implementation
- backend implementation
- mobile implementation
- implementing established designs
- components
- APIs
- database work
- integrations
- tests
- routine debugging
- refactoring
- documentation
- repetitive engineering
- straightforward features

Sonnet may complete a task independently when additional intelligence
would not materially improve the outcome.

---

## OPUS — SENIOR SPECIALIST

Escalate to the latest available Opus when stronger reasoning or
judgment would materially improve the result.

Best suited for:

- difficult UI/UX decisions
- visual critique
- interaction design
- product decisions
- architecture review
- complex debugging
- difficult engineering trade-offs
- security-sensitive work
- major refactoring
- reviewing important implementations

Opus can plan, review, critique, diagnose, or implement.

Do not restrict Opus to review if direct implementation by Opus would
produce a meaningfully better result.

---

## FABLE — FRONTIER ADVISOR

Use the latest available Fable for the highest-value problems.

Fable should be considered when:

- defining overall product architecture
- designing a major new product experience
- solving highly ambiguous problems
- making difficult-to-reverse decisions
- planning large multi-stage features
- designing AI/agent architecture
- reasoning across frontend + backend + data + AI simultaneously
- evaluating major architectural alternatives
- solving problems that remain unresolved after normal escalation
- reviewing a critical product experience before release
- major UI/UX direction could materially affect the product
- long-horizon autonomous reasoning is valuable

Fable should often operate as an ARCHITECT / ADVISOR.

Example:

FABLE
  ↓
creates architecture / UX / implementation strategy

OPUS or SONNET
  ↓
implements or supervises implementation

SONNET
  ↓
handles routine execution and testing

FABLE
  ↓
may perform final strategic review when warranted

Fable may implement directly when doing so materially improves quality.

Do NOT use Fable for routine CRUD, repetitive components, simple tests,
minor styling changes, or mechanical refactors unless the task has
unexpected complexity.

---

# AUTONOMOUS ESCALATION

Do not wait for the user to request a stronger model.

For important work, ask:

1. Can Sonnet produce an excellent result?
2. Would Opus materially improve it?
3. Would Fable materially improve the strategy or result?
4. Is this difficult to reverse?
5. Does this materially affect the user's experience?
6. Does this require unusual creativity?
7. Does this cross several technical/product domains?
8. Is there significant uncertainty?
9. Has a lower-tier model struggled with the problem?

Then select the appropriate model.

Routine + clear:
SONNET

Complex + judgment-heavy:
OPUS

Frontier reasoning / major architecture / highly consequential:
FABLE

When uncertain between two models on an IMPORTANT task,
prefer the stronger model.

---

# UI / UX ROUTING

UI quality is non-negotiable.

For routine implementation of an established design:
SONNET

For meaningful design decisions, visual judgment,
interaction design or critique:
OPUS

For defining a major product experience, unusually difficult UX,
new interaction paradigms, or strategically important design direction:
FABLE may be used.

A high-value UI workflow can therefore be:

FABLE or OPUS
→ product/UX direction

SONNET
→ implementation

OPUS
→ rendered UI critique

SONNET
→ refinement

FABLE
→ final review only when the experience is strategically critical

Do not invoke every model mechanically.
Use escalation only when it can improve the result.

---

# PLANNING

Planning is mandatory for substantial work.

For major features:

1. Inspect the existing product.
2. Understand the user's objective.
3. Understand existing architecture.
4. Identify constraints.
5. Identify edge cases.
6. Design UX where applicable.
7. Decide architecture.
8. Define implementation stages.
9. Define testing strategy.
10. Implement.
11. Test.
12. Review the rendered/working product.
13. Iterate.

For highly consequential work, Fable should be considered during
planning before significant implementation begins.

Do not over-plan small, reversible changes.

---

# SPECIALIST ROLES

Use specialized agents when isolated context or expertise improves
quality or reduces unnecessary context consumption.

Product / UX Architect
→ Fable or Opus

UI Design Reviewer
→ Opus; Fable for exceptionally important experiences

System Architect
→ Fable or Opus

Frontend Engineer
→ Sonnet normally

Backend Engineer
→ Sonnet normally

Mobile Engineer
→ Sonnet normally

AI / Agent Architect
→ Fable for major architecture; Opus/Sonnet for implementation

QA / Test Engineer
→ Sonnet normally

Security Reviewer
→ Opus; escalate further when warranted

Do not create agents merely to create agents.

---

# TOKEN / CONTEXT EFFICIENCY

Save tokens through intelligence, not quality reduction.

DO:

- delegate isolated work
- keep subagents narrowly scoped
- read relevant files first
- reuse established decisions
- summarize handoffs
- avoid rereading unchanged files
- compact obsolete context
- use Sonnet for mechanical execution

DO NOT:

- skip planning to save tokens
- skip testing
- reduce UI quality
- avoid Opus when needed
- avoid Fable when frontier reasoning has meaningful value
- accept a weaker architecture because it is cheaper

Expensive models should primarily spend tokens on expensive THINKING,
not repetitive typing.

---

# FINAL STANDARD

SONNET = workhorse
OPUS = senior specialist
FABLE = frontier architect / advisor

Escalate automatically when stronger intelligence can materially
improve an important outcome.

De-escalate automatically when the strategy is established and the
remaining work is mechanical.

QUALITY > MODEL COST.

But:

SMART ROUTING > USING THE MOST EXPENSIVE MODEL FOR EVERYTHING.
