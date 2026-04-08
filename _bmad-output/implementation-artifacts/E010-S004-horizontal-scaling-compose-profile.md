# Story E010-S004: Horizontal scaling (Docker Compose profile)

Status: done

## Story

As a platform engineer,
I want horizontal scaling support in the local Compose profile,
so that stateless services can run with multiple replicas during Wave 2 validation.

## Acceptance Criteria

1. Compose configuration supports scaling stateless services without naming conflicts.
2. Scaled stack can run with multiple `user-service`, `job-service`, and `pipeline-service` replicas.
3. Existing smoke/load scripts still work against the same external gateway endpoint.
4. Operator documentation includes scaling commands and limitations.
5. Post-change baseline evidence is captured in `BASELINE.md`.

## Tasks / Subtasks

- [x] Remove fixed container naming that prevents `--scale`
- [x] Validate scaled compose bring-up
- [x] Run smoke test against scaled stack
- [x] Run load test and capture metrics
- [x] Update docs and status artifacts

## Dev Notes

- Compose host-port binding for `api-gateway` keeps gateway single-instance in this profile.
- Scaling focus is downstream stateless services while preserving `http://localhost:8081` for existing scripts.

## Dev Agent Record

### Agent Model Used

GitHub Copilot

### Completion Notes List

- Removed fixed `container_name` fields for `user-service`, `job-service`, `pipeline-service`, and `notification-service` in `docker-compose.yml`.
- Added scaling runbook section to infra `README.md` with `docker-compose ... --scale` commands.
- Verified scaled bring-up with two replicas each for `user-service`, `job-service`, and `pipeline-service`.
- Smoke test passed unchanged on `http://localhost:8081` (7/7 checks).
- k6 load test passed all thresholds on scaled stack.
- Appended `Post-E010-S004 Baseline (2026-04-07)` in `BASELINE.md`.

### File List

- `job-tracker-infra/docker-compose.yml` (modified)
- `job-tracker-infra/README.md` (modified)
- `job-tracker-infra/BASELINE.md` (modified)
- `job-tracker-planning/_bmad-output/implementation-artifacts/E010-S004-horizontal-scaling-compose-profile.md` (new)


