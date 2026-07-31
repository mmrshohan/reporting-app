# Incident response runbook

## Severity

- **SEV-1:** cross-workspace disclosure, unrecoverable report loss, active credential compromise, widespread outage, or failed recovery capability during an event.
- **SEV-2:** major feature unavailable, significant transcription/authentication failure, or elevated errors affecting many users without confirmed data loss.
- **SEV-3:** limited degradation or defect with a practical workaround.

## Response

1. **Declare:** name the incident, severity, owner, start time, and affected surface.
2. **Mitigate:** stop the bleeding first—disable provider capability, revoke credentials, roll back images, block traffic, or stop writes as appropriate.
3. **Preserve evidence:** retain relevant logs, image SHAs, migration revision, provider request IDs, and timelines without copying sensitive content into chat/docs.
4. **Communicate:** provide factual updates, current impact, mitigation, and next update time. Do not speculate.
5. **Diagnose:** reproduce or trace systematically after impact is controlled.
6. **Recover:** execute the reviewed deployment or database recovery runbook and verify critical journeys.
7. **Close:** confirm monitoring is normal and customer impact has ended.

## Security/data incidents

- Rotate exposed credentials and revoke affected sessions.
- Determine affected users/workspaces, fields, and time range.
- Preserve access/audit evidence.
- Engage legal/privacy obligations and user notification requirements when applicable.
- Do not place report bodies, transcript text, raw audio, or secrets in the incident document.

## Follow-up

Within a reasonable interval, write a short blameless review:

- What happened and customer impact.
- Detection and response timeline.
- Technical and organizational contributing factors.
- What limited the impact.
- Corrective actions with owners and due dates.
- Tests, alerts, documentation, or architecture changes preventing recurrence.

Correct the system, not just the final broken line of code.
