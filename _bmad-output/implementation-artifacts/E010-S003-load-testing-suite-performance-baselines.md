# Story E010-S003: Load Testing Suite + Performance Baselines

Status: done

## Story

As an engineer,
I want a load testing suite with documented performance baselines,
so that I can measure the impact of Wave 2 optimisation work (E010-S001, E010-S002).

## Acceptance Criteria

1. A runnable k6 load test script covers the three highest-traffic endpoints:
   - `POST /api/v1/auth/login`
   - `GET /api/v1/applications`
   - `GET /api/v1/applications/:id/timeline`
2. The script supports configurable concurrency (VUs) and duration via env vars.
3. A `README` documents how to run the suite against the local compose stack.
4. Thresholds are defined so that `k6 run` exits non-zero when SLAs are missed:
   - p95 latency < 500ms (current tolerance — to be tightened after pooling)
   - Error rate < 1%
5. A `BASELINE.md` file captures the first-run results as the Wave 2 starting point.
6. The smoke-test CI script (`scripts/smoke-test.sh`) is unaffected.

## Tasks / Subtasks

- [x] Create `job-tracker-infra/scripts/load-test.js` — k6 script with login, list, timeline scenarios
- [x] Create `job-tracker-infra/scripts/load-test-env.example` — documented env var reference
- [x] Create `job-tracker-infra/BASELINE.md` — first-run results template + guidance
- [x] Update `job-tracker-infra/README.md` — add "Load Testing" section

## Dev Notes

- k6 runs as a Docker container (`grafana/k6:latest`) — no local install required.
- The login scenario captures a JWT and passes it to subsequent scenarios.
- VUs default to 20; duration defaults to 30s — safe for a laptop dev environment.
- Thresholds are intentionally loose for Wave 2 start; tighten in E010-S001 PR.

### Project Structure Notes

- Target repo: `job-tracker-infra`
- Depends on: docker compose stack healthy (E008-S002)

### References

- `WAVE2.md` — Wave 2 roadmap + performance targets
- `job-tracker-infra/scripts/smoke-test.sh` — existing smoke test for reference
- `ARCHITECTURE.md` — service port map

## Dev Agent Record

### Agent Model Used

GitHub Copilot (claude-sonnet-4-5)

### Completion Notes List

- k6 script uses three scenarios: `login`, `list_applications`, `get_timeline`.
- Login scenario stores bearer token in a shared `__ENV` / SharedArray pattern.
- All thresholds are tagged per scenario for granular SLA tracking.
- BASELINE.md contains placeholder tables — engineer runs the suite then fills them in.

### File List

- `job-tracker-infra/scripts/load-test.js` (new)
- `job-tracker-infra/scripts/load-test-env.example` (new)
- `job-tracker-infra/BASELINE.md` (new)
- `job-tracker-infra/README.md` (modified)

