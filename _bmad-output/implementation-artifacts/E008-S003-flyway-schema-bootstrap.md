# Story E008-S003: Flyway Schema Bootstrap for Service Schemas

Status: done

## Story

As a platform developer,
I want Flyway migrations that bootstrap service-owned schemas,
so that data ownership rules are enforced from day one.

## Acceptance Criteria

1. Migrations create schemas: `user_svc`, `job_svc`, `pipeline_svc`, `notification_svc`.
2. Baseline tables exist per schema for each service's minimum runtime needs.
3. Migration order is deterministic and rerunnable in clean environments.
4. Roll-forward migration strategy is documented; no destructive rollback dependency.
5. CI validates migrations against disposable PostgreSQL.
6. Schema ownership notes are documented for developers.

## Tasks / Subtasks

- [x] Create baseline Flyway migration set under infra migration directory
- [x] Define schema creation migrations and core tables
- [x] Add indexes/constraints for primary access patterns
- [x] Add migration README with naming and sequencing rules
- [x] Add migration dry-run check in CI
- [x] Document local reset and reapply workflow

## Dev Notes

- Each service may read other service data only via APIs/events, not cross-schema writes.
- Keep tables minimal; feature-specific expansions happen in later stories.
- Align naming to service boundaries doc.

### Project Structure Notes

- Target repo: `job-tracker-infra`
- Primary artifacts: `db/migrations/*`, migration docs, CI migration check

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

GitHub Copilot (April 6, 2026)

### Debug Log References

- V1 + V2 migrations existed and covered schemas, baseline tables, and indexes.
- V3 added to complete all durable persistence tables reflecting each service's full in-memory model.

### Completion Notes List

- V1: creates 4 service schemas + baseline tables (users, sessions, applications, status_events, notifications).
- V2: adds primary access pattern indexes for all schemas.
- V3: adds pipeline_svc.applications (local cache), pipeline_svc.outbox, pipeline_svc.stalled_reminders, adds `rationale` column to status_events; adds notification_svc.delivery_attempts, notification_svc.user_preferences, notification_svc.reminder_cadence; adds corresponding indexes.
- Migration CI workflow validates against disposable PostgreSQL 16-alpine.
- Roll-forward strategy documented: no destructive rollbacks, additive V* versioning only.

### File List

- `job-tracker-infra/db/migrations/V1__bootstrap_service_schemas.sql`
- `job-tracker-infra/db/migrations/V2__baseline_indexes_constraints.sql`
- `job-tracker-infra/db/migrations/V3__complete_service_schemas.sql`
- `job-tracker-infra/db/migrations/README.md`
- `job-tracker-infra/db/flyway/flyway.conf`
- `job-tracker-infra/.github/workflows/migration-validate.yml`
- `_bmad-output/implementation-artifacts/E008-S003-flyway-schema-bootstrap.md`

