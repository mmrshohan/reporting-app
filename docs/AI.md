# Transcription and future AI

Owner: AI Engineer with Security and Product review. Speech-to-text is version-one scope; generative drafting/refinement is later.

## Boundary

Clients never call an AI or transcription provider directly. FastAPI exposes product-level capabilities and provider adapters translate external APIs:

```text
Web/mobile → FastAPI service → provider interface → configured provider
```

Provider keys, cost controls, timeouts, retries, consent enforcement, and audit metadata remain server side.

## Version-one transcription

- `POST /api/v1/transcriptions` accepts bounded multipart audio.
- The API validates identity, workspace access, format, duration, and size.
- The configured adapter returns transcript text and safe metadata.
- The transcript is an editable draft inserted by the client.
- Raw audio is deleted after success or failure and is not logged.
- Existing draft content remains intact on permission, network, provider, or timeout failure.
- Provider-specific errors do not leak through the public contract.

The provider is selected only after its accuracy, language coverage, latency, pricing, retention, training use, and regional handling are reviewed and recorded in an ADR.

## Adapter contracts

Python protocols/interfaces describe product capabilities rather than vendor endpoints. Implementations contain vendor translation; services contain product policy. A deterministic fake provider supports tests.

Conceptual operations:

```text
transcribe(audio, language_hint) → transcript
draft_report(source, template) → draft          # later
refine_report(text, instruction) → revised text # later
```

## Privacy and security

- Recording is visible and user initiated.
- Content leaving our system is disclosed appropriately.
- No provider call occurs without the required product/privacy approval.
- Requests have size limits, timeouts, rate limits, and cost attribution.
- Logs include provider, model/version, duration, outcome, and request ID—not content.
- Provider credentials are server secrets and rotate independently.

## Quality and change control

- Provider/model versions are explicit configuration, not invisible defaults.
- Representative audio fixtures and expected quality criteria form a small evaluation set.
- Changes run evaluations and compare latency/cost before rollout.
- Transcription remains editable and never submits a report automatically.
- Future prompts are versioned and reviewed like code.

## Deliberate exclusions

No autonomous agents, background analysis of customer reports, retained training corpus, fine-tuning, or on-device model is included without a separate product, privacy, security, and cost decision.
