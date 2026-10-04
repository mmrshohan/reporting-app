# Transcription and future AI

Owner: AI Engineer with Security and Product review. Speech-to-text is version-one scope; AI refinement and text-to-speech are later capabilities.

## Capability boundaries

- **Speech-to-text:** converts a user-initiated recording into editable report text. This is part of the core reporting flow.
- **AI refinement:** rewrites selected draft text into clearer, more mature language only after an explicit user action. This is deferred.
- **Text-to-speech:** reads text aloud. This is independent of transcription and is deferred until a concrete product need exists.

No provider is selected yet. OpenAI, ElevenLabs, and other providers may be evaluated later, but mentioning a candidate does not create an architectural dependency or approval to send it customer data.

## Boundary

Clients never call an AI or transcription provider directly. NestJS exposes product-level capabilities and provider adapters translate external APIs:

```text
Web/mobile → NestJS service → provider interface → configured provider
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

The provider is selected only after its accuracy, language coverage, latency, pricing, retention, training use, and regional handling are reviewed and recorded in an ADR. The public API and internal capability interface must not expose vendor-specific concepts.

## Adapter contracts

TypeScript interfaces describe product capabilities rather than vendor endpoints. Implementations contain vendor translation; services contain product policy. A deterministic fake provider supports tests.

Conceptual operations:

```text
transcribe(audio, language_hint) → transcript
draft_report(source, template) → draft          # later
refine_report(text, instruction) → revised text # later
synthesize_speech(text, voice_preferences) → audio # later
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
- Refinement never silently replaces text or submits a report; the user reviews and accepts the result.
- Future prompts are versioned and reviewed like code.

## Deliberate exclusions

No autonomous agents, background analysis of customer reports, retained training corpus, fine-tuning, or on-device model is included without a separate product, privacy, security, and cost decision.
