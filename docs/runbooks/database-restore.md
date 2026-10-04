# PostgreSQL backup and restore runbook

An untested backup is not a recovery capability. Production backups must be encrypted, stored off-server, access controlled, monitored for freshness, and restored on a schedule.

## Backup policy requirements

- Automated logical and/or physical backup appropriate to the deployment.
- Defined frequency, retention, encryption, and off-server destination.
- Backup failure alerting and ownership.
- Credentials separate from ordinary application credentials.
- Documented recovery point objective (RPO) and recovery time objective (RTO) before commercial launch.

## Restore test

1. Select a recent backup without modifying production.
2. Provision an isolated PostgreSQL instance of a compatible version.
3. Verify backup checksum/integrity where supported.
4. Restore using the documented tool and credentials.
5. Run Prisma migration-history and database-contract checks.
6. Run integrity checks for users, workspaces, memberships, reports, revisions, and sessions.
7. Start the API against the restored database in an isolated environment.
8. Run critical API tests, including workspace isolation.
9. Record backup identifier, duration, data timestamp, result, and corrective actions.
10. Destroy the isolated restored data securely after the test.

## Production recovery

1. Declare an incident and stop writes if continued writes would worsen loss/corruption.
2. Preserve affected storage and logs for diagnosis.
3. Identify the required recovery point and expected data loss window.
4. Restore to a new database instance; do not overwrite the only damaged copy.
5. Validate integrity, migrations, and critical journeys.
6. Switch application connectivity deliberately and monitor.
7. Communicate impact and document reconciliation for writes after the recovery point.

Exact provider/tool commands must be added when the VPS and backup destination are selected. The deployment is not commercially ready until those commands have been executed successfully in a restore test.
