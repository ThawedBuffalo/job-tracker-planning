# Story E010-S002: Redis caching layer for applications list

Status: done

## Story

As a platform engineer,
I want a Redis caching layer for the application list endpoint,
so that list reads scale better under load while preserving correctness after writes and status transitions.

## Acceptance Criteria

1. `GET /api/v1/applications` supports Redis-backed read-through caching.
2. Cache keys are scoped to user and query parameters (`status`, `q`, `sort`, `page`, `page_size`).
3. Cache entries are invalidated when applications are created/updated/deleted.
4. Cache entries are invalidated when pipeline status transitions mutate application status.
5. Redis behavior is fail-open (service continues with DB reads if Redis is unavailable).
6. `docker-compose` and environment config include Redis and cache settings.
7. Smoke/load validation passes and `BASELINE.md` includes post-change measurements.

## Tasks / Subtasks

- [x] Add Redis service to infra compose and env defaults
- [x] Add Redis cache config to `job-service`
- [x] Implement read-through list caching in `job-service` store
- [x] Implement user-scoped cache invalidation in `job-service` mutations
- [x] Implement user-scoped cache invalidation in `pipeline-service` transitions
- [x] Add Redis dependencies and graceful shutdown hooks
- [x] Validate tests + smoke + load and update `BASELINE.md`

## Dev Notes

- Cache is optional via `LIST_CACHE_ENABLED`; default remains disabled for local safety.
- Cache key format uses a deterministic base64url suffix over normalized query options.
- User-level key index (`namespace:index:<user_id>`) enables efficient invalidation of all cached list variants for that user.

### Project Structure Notes

- Target repos: `job-tracker-infra`, `job-tracker-job-service`, `job-tracker-pipeline-service`
- Depends on: `E010-S001` (pooled Postgres path)
- Blocks: `E010-S004` scaling work

### References

- `WAVE2.md`
- `job-tracker-infra/BASELINE.md`
- `job-tracker-infra/scripts/load-test.js`

## Dev Agent Record

### Agent Model Used

GitHub Copilot

### Completion Notes List

- Added Redis service (`redis:7-alpine`) to compose with healthcheck.
- Introduced list-cache env vars in `.env.example`: `LIST_CACHE_ENABLED`, `LIST_CACHE_TTL_SECONDS`, `LIST_CACHE_NAMESPACE`, `REDIS_PORT`, `REDIS_DB`.
- Implemented `job-service/src/cache.js` and integrated it into `store.listByUser` with read-through caching.
- Added cache invalidation on `createApplication`, `updateApplication`, and `deleteById`.
- Implemented `pipeline-service/src/cache.js` and invalidation call in `setApplicationStatus`.
- Added redis client shutdown in both `job-service` and `pipeline-service` server shutdown hooks.
- Local validation summary:
  - job-service tests: 10/10 pass (`PERSISTENCE_MODE=memory LIST_CACHE_ENABLED=false`)
  - pipeline-service tests: 15/15 pass (`PERSISTENCE_MODE=memory LIST_CACHE_ENABLED=false`)
  - smoke tests (Redis enabled stack): pass (7 passed, 0 failed)
  - k6 thresholds (Redis enabled stack): all pass; `list_duration p95=7.74ms`

### File List

- `job-tracker-infra/docker-compose.yml` (modified)
- `job-tracker-infra/.env.example` (modified)
- `job-tracker-infra/README.md` (modified)
- `job-tracker-infra/BASELINE.md` (modified)
- `job-tracker-job-service/src/cache.js` (new)
- `job-tracker-job-service/src/config.js` (modified)
- `job-tracker-job-service/src/store.js` (modified)
- `job-tracker-job-service/src/server.js` (modified)
- `job-tracker-job-service/package.json` (modified)
- `job-tracker-job-service/package-lock.json` (modified)
- `job-tracker-pipeline-service/src/cache.js` (new)
- `job-tracker-pipeline-service/src/config.js` (modified)
- `job-tracker-pipeline-service/src/store.js` (modified)
- `job-tracker-pipeline-service/src/server.js` (modified)
- `job-tracker-pipeline-service/package.json` (modified)
- `job-tracker-pipeline-service/package-lock.json` (modified)

