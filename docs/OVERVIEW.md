# Product overview

## Problem

Important work is often poorly traced because reporting is slow, inconsistent, or scattered across messaging and note-taking tools. A report should be quick to capture, easy to review, and reliably attributable to the right person or organization.

## Product

Report provides one focused workflow:

> Type or dictate a report, review it, and submit it to the correct workspace.

The product is available through a Next.js web application and an Expo/React Native mobile application. Both clients use the same FastAPI contract.

## Customers and ownership

The commercial model supports two workspace types:

- **Personal workspace:** owned and used by an individual.
- **Organization workspace:** contains multiple members with explicit roles.

Every report belongs to exactly one workspace. The same user may own a personal workspace and belong to one or more organizations.

## Version-one scope

- Account registration and authentication.
- Personal and organization workspaces.
- Organization membership and basic roles.
- Create, edit, submit, view, and recover reports.
- Hold-to-record speech-to-text through a provider-neutral API adapter.
- Local preservation of unsent drafts.
- Web, iOS, and Android access.

## Explicitly later

- Full multi-device offline synchronization.
- Real-time collaborative editing.
- AI-generated report drafting or refinement.
- Custom report-template marketplace.
- Calendar and third-party integrations.
- Advanced analytics and enterprise administration.
- Retention of raw audio.

## Product principles

1. **The report is the center.** New features must shorten, clarify, protect, or trace the reporting workflow.
2. **The user remains in control.** Transcription and future AI produce editable drafts, never invisible final decisions.
3. **Never lose work.** Typed or transcribed content survives recoverable failures.
4. **Minimal surface, high craft.** Few workflows, completed carefully.
5. **Web and mobile are peers.** Neither client receives a private or divergent business API.
6. **Privacy is a product feature.** Content leaving the system for transcription is explicit and controlled.
7. **Commercial foundations without enterprise theater.** Multi-tenancy and authorization are built correctly; speculative scale is not.

## Version-one success signals

- A new user can submit a first report without training.
- Draft input responds immediately and survives a failed submission.
- A workspace member cannot access another workspace's report.
- A user can dictate, review, and submit a short report reliably.
- Critical API requests are observable and recoverable.
- A new engineer can run the full system by following the repository documentation.
