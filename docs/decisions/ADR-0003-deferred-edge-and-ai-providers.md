# ADR-0003 — Defer production edge and voice/AI providers

- **Status:** Accepted
- **Date:** 2026-08-01
- **Deciders:** Product owner and CTO

## Context

The application requires HTTPS routing in production and provider-backed capabilities over time. The hosting environment is not selected, so choosing a reverse proxy now would optimize for unknown constraints. Provider candidates also differ in transcription quality, language coverage, refinement quality, speech synthesis, privacy terms, latency, and price.

Speech-to-text, AI text refinement, and text-to-speech are different product capabilities. Treating them as one vendor feature would couple the product contract to a provider before the requirements are known.

## Decision

- Require HTTPS, certificate management, health-aware routing, request limits, and appropriate security headers in production, but select the edge implementation during deployment.
- Keep the application independent of a specific reverse proxy, cloud load balancer, or managed edge.
- Keep speech-to-text behind a server-side product capability interface; select its provider through a later evaluated ADR.
- Defer AI refinement and text-to-speech until their product behavior, privacy requirements, and acceptance criteria are approved.
- If added, refinement is explicit and reviewable: it never silently replaces or submits user text.
- Evaluate OpenAI, ElevenLabs, and other candidates without assuming that one provider must supply every capability.

## Consequences

- Infrastructure documentation describes required behavior until a real deployment target exists.
- Provider-specific request models, names, and credentials do not enter public API contracts or clients.
- Deployment and provider ADRs must record the chosen option, operating cost, security/privacy review, failure behavior, and replacement path.
- Version one may use a deterministic fake transcription adapter until a production provider is approved.

## Revisit when

- A hosting provider, VPS, domain, or production networking model is selected.
- Representative audio and target languages are available for transcription evaluation.
- Product approves AI refinement or read-aloud behavior and its user experience.
