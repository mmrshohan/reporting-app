# Deployment runbook

Use this runbook after the implementation scaffold supplies the exact commands. Never invent production commands during an incident or release; update this document when commands change.

## Preconditions

- CI is green for the exact commit SHA.
- Container images are immutable and identifiable by SHA.
- Required secrets/configuration exist in the target environment.
- Database backup is current and the migration has been reviewed.
- Previous known-good image versions are recorded.

## Staging

1. Pull/deploy the candidate images.
2. Run the dedicated Alembic migration step.
3. Start/update FastAPI and Next.js; confirm Caddy routes only to healthy services.
4. Verify health/readiness endpoints and PostgreSQL connectivity.
5. Exercise registration/login, workspace selection, report draft/submit, access denial across workspaces, and transcription with the staging provider/fake.
6. Confirm logs contain request IDs and no report/audio content.

## Production

1. Announce or record the deployment window and owner.
2. Confirm backup age and migration compatibility.
3. Deploy the same images verified in staging.
4. Run the migration once through the controlled deployment step.
5. Update services gradually where supported.
6. Run read-only health checks, then a safe synthetic critical-journey check.
7. Watch error rate, latency, database connections, resource usage, and transcription failures.
8. Record deployed SHA, migration revision, time, operator, and verification result.

## Rollback

1. Stop or pause further rollout.
2. Redeploy the previous known-good application images.
3. Do not downgrade a destructive schema change. Expand–migrate–contract should leave the previous code compatible; otherwise execute the reviewed recovery plan.
4. Verify critical journeys and monitoring.
5. Open an incident record if customers were affected.

## Failure rule

If verification is ambiguous, treat the deployment as failed and roll back application code. Protect data before investigating convenience issues.
