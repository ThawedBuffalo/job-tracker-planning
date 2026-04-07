# Story E008-S002: Compose Stack Baseline (Gateway, Services, Postgres, Nginx)

Status: done

## Story

As a platform developer,
I want a baseline Docker Compose stack for all core services,
so that local and VPS deployment topology stays consistent.

## Acceptance Criteria

1. `docker-compose.yml` defines containers for gateway, user, job, pipeline, notification, postgres, and nginx.
2. Services are networked correctly and reachable through nginx/gateway entrypoint.
3. Environment variables are externalized via `.env` and `.env.example`.
4. Health checks are present for each service and postgres.
5. Compose startup order and restart policies support deterministic bring-up.
6. Stack boot command and expected endpoints are documented in `README.md`.

## Tasks / Subtasks

- [x] Create/update `docker-compose.yml` with all required services
- [x] Add service-level health checks and dependency wiring
- [x] Add `nginx` service baseline config and route passthrough
- [x] Add `.env.example` for ports, DB, JWT, SMTP placeholders
- [x] Add startup/shutdown/reset commands to README
- [x] Validate `docker compose config` in CI

## Dev Notes

- Keep baseline minimal; no advanced scaling or broker required for v1.
- Follow single-VPS assumptions from architecture docs.
- Use stable container naming to simplify smoke tests.

### Project Structure Notes

- Target repo: `job-tracker-infra`
- Primary artifacts: `docker-compose.yml`, `.env.example`, `nginx/*`, `README.md`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- `_bmad-output/planning-artifacts/repo-map.md`

## Dev Agent Record

### Agent Model Used

GitHub Copilot (April 6, 2026)

### Debug Log References

- YAML validated via Python pyyaml: compose.yml OK, compose.release.yml OK
- nginx.conf upstream port documented as hardcoded (8081 = default API_GATEWAY_PORT)

### Completion Notes List

- All compose services, health checks, network wiring, and env externalization were already in place.
- Added explanatory comment to `nginx/nginx.conf` documenting the hardcoded upstream port limitation.
- Added port note to `README.md` under Expected Entry Points.
- CI workflow `compose-validate.yml` covers `docker compose --env-file .env.example config`.

### File List

- `job-tracker-infra/docker-compose.yml`
- `job-tracker-infra/docker-compose.release.yml`
- `job-tracker-infra/.env.example`
- `job-tracker-infra/nginx/nginx.conf`
- `job-tracker-infra/README.md`
- `job-tracker-infra/.github/workflows/compose-validate.yml`
- `_bmad-output/implementation-artifacts/E008-S002-compose-stack-baseline.md`

