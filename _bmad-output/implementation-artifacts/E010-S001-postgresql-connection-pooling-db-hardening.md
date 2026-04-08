# Story E010-S001: PostgreSQL connection pooling + DB hardening

Status: done

## Story

As a platform engineer,
I want the core services to use pooled PostgreSQL connections with hardened query paths,
so that the stack can sustain higher concurrency and provide a measurable latency improvement over the Wave 2 baseline.

## Acceptance Criteria

1. `job-tracker-infra/docker-compose.yml` includes a pooled PostgreSQL access path for application services.
2. Environment configuration supports pooled DB access without breaking local development defaults.
3. The hottest endpoints exercised by the load suite are compatible with pooled database access:
   - `POST /api/v1/auth/login`
   - `GET /api/v1/applications`
   - `GET /api/v1/applications/:id/timeline`
4. Required indexes/config changes for the read-heavy application list and timeline paths are present.
5. `scripts/smoke-test.sh` still passes without changes to its interface.
6. `scripts/load-test.js` is re-run and `BASELINE.md` is updated with post-change measurements.

## Tasks / Subtasks

- [x] Capture the untouched baseline context in `job-tracker-infra/BASELINE.md` (documented that the pre-change run was not recorded during E010-S003 and preserved the template rather than inventing historical numbers)
- [x] Add PgBouncer (or equivalent pooled access) to `job-tracker-infra/docker-compose.yml`
- [x] Extend `job-tracker-infra/.env.example` with pooling settings
- [x] Add pooled DB config to the service `src/config.js` files
- [x] Introduce PostgreSQL client dependencies where missing
- [x] Replace in-memory hot-path storage for auth/application/timeline with DB-backed reads/writes
- [x] Add any required migration/index updates under `job-tracker-infra/db/migrations/`
- [x] Re-run smoke + load validation and record results

## Dev Notes

- Current service configs expose DB env vars in Compose, but the service code does not yet consume them.
- Current hot-path stores are still in-memory (`Map`) implementations in:
  - `job-tracker-user-service/src/store.js`
  - `job-tracker-job-service/src/store.js`
  - `job-tracker-pipeline-service/src/store.js`
- Current Node services do not yet depend on a PostgreSQL client package, so this story includes foundational DB wiring before pooling can deliver value.
- Start with the endpoints already covered by `job-tracker-infra/scripts/load-test.js` so before/after comparison is meaningful.

### Project Structure Notes

- Target repos: `job-tracker-infra`, `job-tracker-user-service`, `job-tracker-job-service`, `job-tracker-pipeline-service`
- Follows: `E010-S003` load testing suite + baseline tooling
- Blocks: `E010-S002` Redis caching

### References

- `WAVE2.md`
- `job-tracker-infra/README.md`
- `job-tracker-infra/BASELINE.md`
- `job-tracker-infra/scripts/load-test.js`

## Dev Agent Record

### Agent Model Used

GitHub Copilot

### Completion Notes List

- Story opened after verifying Wave 1 completion and confirming `E010-S003` is already done.
- Scope explicitly accounts for the current codebase state: DB env vars exist in infra, but service persistence remains in-memory and requires first-pass PostgreSQL integration.
- Added a first foundation slice: PgBouncer service in Compose, pool-related env defaults, service DB config objects, and startup-time PostgreSQL connectivity probes gated behind `PERSISTENCE_MODE=postgres`.
- Existing service tests remain green after the foundation changes: user-service (4), job-service (10), pipeline-service (15).
- Implemented PostgreSQL-backed store paths for the Wave 2 hot endpoints while preserving `memory` mode as the default: user auth/session storage, job applications CRUD/list queries, and pipeline timeline/transition persistence.
- Added `V4__job_service_activity_indexing.sql` to introduce `job_svc.applications.last_status_changed_at`, a DB trigger to maintain it on status changes, and composite indexes for user-scoped recency/status list queries.
- `pipeline-service` now uses the shared `job_svc.applications` table in postgres mode for ownership/status lookups, allowing transition/timeline flows to work against job-created applications.
- Closed two infra validation gaps discovered during story closure:
  - published `api-gateway` on host port `8081` so the existing smoke/load tooling works unchanged
  - passed `JWT_ACCESS_SECRET` through to `job-service` and `pipeline-service` so downstream bearer validation matches the gateway
- Fixed `scripts/smoke-test.sh` token/application-id capture so logging does not corrupt the Authorization header during command substitution.
- Fixed `scripts/load-test.js` timeline assertion to accept the current `data.items` response shape used by the timeline endpoint.
- Validation evidence on 2026-04-07:
  - `docker-compose -f docker-compose.yml -f docker-compose.localbuild.yml up -d --build` in `postgres` + pool mode succeeded
  - Flyway validated all 4 migrations and reported schema up to date
  - `scripts/smoke-test.sh` passed end-to-end (`login -> create -> transition -> timeline -> dashboard`)
  - final k6 rerun passed all thresholds with `http_req_failed=0.00%`, `list p95=12.70ms`, `timeline p95=12.43ms`, `login p95=465.71ms`
  - `BASELINE.md` now includes the post-`E010-S001` measurement set and explicitly notes that the pre-change baseline was not captured during `E010-S003`

### File List

- `job-tracker-planning/_bmad-output/implementation-artifacts/E010-S001-postgresql-connection-pooling-db-hardening.md` (new)
- `job-tracker-planning/_bmad-output/implementation-artifacts/sprint-status.yaml` (modified)
- `job-tracker-infra/docker-compose.yml` (modified)
- `job-tracker-infra/.env.example` (modified)
- `job-tracker-infra/scripts/smoke-test.sh` (modified)
- `job-tracker-infra/scripts/load-test.js` (modified)
- `job-tracker-infra/BASELINE.md` (modified)
- `job-tracker-user-service/src/config.js` (modified)
- `job-tracker-user-service/src/db.js` (new)
- `job-tracker-user-service/src/server.js` (modified)
- `job-tracker-user-service/src/store.js` (modified)
- `job-tracker-user-service/src/auth.js` (modified)
- `job-tracker-user-service/src/app.js` (modified)
- `job-tracker-user-service/package.json` (modified)
- `job-tracker-user-service/package-lock.json` (modified)
- `job-tracker-job-service/src/config.js` (modified)
- `job-tracker-job-service/src/db.js` (new)
- `job-tracker-job-service/src/server.js` (modified)
- `job-tracker-job-service/src/store.js` (modified)
- `job-tracker-job-service/src/app.js` (modified)
- `job-tracker-job-service/package.json` (modified)
- `job-tracker-job-service/package-lock.json` (modified)
- `job-tracker-pipeline-service/src/config.js` (modified)
- `job-tracker-pipeline-service/src/db.js` (new)
- `job-tracker-pipeline-service/src/server.js` (modified)
- `job-tracker-pipeline-service/src/store.js` (modified)
- `job-tracker-pipeline-service/src/app.js` (modified)
- `job-tracker-pipeline-service/src/stalled-reminder.js` (modified)
- `job-tracker-pipeline-service/package.json` (modified)
- `job-tracker-pipeline-service/package-lock.json` (modified)
- `job-tracker-infra/db/migrations/V4__job_service_activity_indexing.sql` (new)

