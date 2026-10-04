# API standards

Owner: Backend Engineer. NestJS's generated OpenAPI document is the protocol source of truth.

## Contract

- Base path: `/api/v1`.
- Resource paths use plural nouns and kebab-case when multiple words are required.
- JSON fields use `camelCase`.
- Requests and responses use dedicated DTOs, not Prisma models.
- Web and mobile consume a generated TypeScript client from the committed OpenAPI document.
- CI regenerates the contract and fails when generated output differs.

## HTTP behavior

- `GET` reads without side effects.
- `POST` creates or invokes a non-idempotent command with explicit idempotency protection where retries are expected.
- `PATCH` performs partial updates.
- `DELETE` removes or archives according to the documented resource policy.
- Status codes and response bodies are consistent across modules.
- List endpoints are paginated and bounded.

## Authentication and authorization

- Public endpoints are explicitly marked; all others require authentication.
- Authentication establishes identity only. Services separately authorize the requested workspace action.
- Client-provided `userId`, `workspaceId`, or role never establishes permission by itself.
- Resource-not-found and unauthorized behavior avoids leaking cross-workspace existence.

## Error shape

```json
{
  "code": "report_not_found",
  "message": "The report could not be found.",
  "details": {},
  "requestId": "01..."
}
```

- `code` is stable and machine-readable.
- `message` is safe for users and contains no internal implementation detail.
- `details` is optional structured context safe to expose.
- `requestId` correlates the response with structured server logs.
- Validation errors follow the same envelope.

## Compatibility

- Additive optional fields are allowed within `/v1`.
- Removing/renaming fields or changing meaning requires `/v2` or a staged compatibility plan.
- Mobile rollout means the API supports at least the currently released client throughout migration.
- Database and API changes use expand–migrate–contract.

## Idempotency and concurrency

- Report submission and other retry-prone commands accept an idempotency key.
- Duplicate keys for the same authenticated scope return the original outcome.
- Updates use a version or timestamp precondition when silent overwrites would lose work.
- Transactions protect multi-record invariants.

## Transcription

- Accept only documented audio formats and bounded size/duration.
- Validate declared and detected media type where feasible.
- Provider calls have explicit connection and response timeouts.
- Raw audio is temporary and deleted after success or failure.
- Provider-specific errors map to stable internal codes.
- Rate and cost limits apply per account/workspace.

## Documentation

Endpoint summaries, request/response DTOs, authentication requirements, and error responses are declared in NestJS and visible through generated OpenAPI documentation. Narrative policy belongs here; do not manually maintain a second endpoint catalog.
