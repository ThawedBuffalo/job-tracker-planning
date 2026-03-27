# Story 8.1: Docker Compose Local Development Environment

Status: ready-for-dev

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a developer,
I want a Docker Compose dev stack that mirrors production,
so that I can develop and test locally without environment drift.

## Acceptance Criteria

1. Given I clone the repo and run `docker-compose -f docker-compose.dev.yml up`, when all services start, then these are running and healthy:
   - PostgreSQL 16 on `localhost:5432`
   - Spring Boot API on `localhost:8080` with hot reload via `spring-boot-devtools`
   - Flutter dev server on `localhost:8081`
   - Health verification criteria:
     - `db`: accepts a connection on `localhost:5432` (for example via `pg_isready`)
     - `api`: returns HTTP 200 for a health/readiness endpoint (or equivalent app-level startup check)
     - `frontend`: returns HTTP 200 for `/` on `localhost:8081`
2. Given I modify a Spring Boot source file, when the file is saved, then Spring Boot devtools restarts the app and the change is live within 5 seconds.
3. Given the dev environment starts for the first time, when Spring Boot initializes, then Flyway runs all pending migrations automatically and creates `job_tracker` schema objects.
4. Given I need to reset the dev database, when I run `docker-compose -f docker-compose.dev.yml down -v`, then the PostgreSQL volume is removed and a fresh schema is created on next `up`.

## Tasks / Subtasks

- [ ] Create local development compose stack
  - [ ] Add `docker-compose.dev.yml` at repo root with services for `db`, `api`, and `frontend` (or equivalent Flutter dev container)
  - [ ] Map ports: `5432` (PostgreSQL), `8080` (Spring Boot), `8081` (Flutter web dev server)
  - [ ] Ensure service dependencies and restart behavior make startup deterministic
- [ ] Configure environment variable strategy
  - [ ] Use `.env` for all configurable values; do not hardcode secrets
  - [ ] Add/update `.env.example` with required variables for local run
- [ ] Enable backend developer hot reload
  - [ ] Configure backend dev service to include `spring-boot-devtools`
  - [ ] Mount source or use equivalent workflow so Java edits restart within 5 seconds
- [ ] Ensure database migration behavior in local startup
  - [ ] Confirm Flyway executes automatically on startup against PostgreSQL 16
  - [ ] Verify migration scripts are read from `backend/src/main/resources/db/migration/`
- [ ] Verify reset/rebuild workflow
  - [ ] Run `docker-compose -f docker-compose.dev.yml down -v` and confirm local DB volume reset
  - [ ] Re-run `up` and confirm clean schema bootstrap via Flyway
- [ ] Add a lightweight repeatable smoke-check routine
  - [ ] Document a minimal command checklist (or script) that verifies: services up + healthy, Flyway migrations applied, and reset cycle (`down -v` then `up`) succeeds
  - [ ] Place the routine in `README.md` so validation is repeatable across runs
- [ ] Add quickstart documentation
  - [ ] Document local startup, teardown, and reset commands in root `README.md`
  - [ ] Document expected healthy endpoints/ports for fast troubleshooting

## Dev Notes

- Keep local environment aligned with VPS deployment conventions (same core services, env var pattern, and DB major version), while allowing dev-only behaviors like hot reload. Source: `_bmad-output/planning-artifacts/prd.md` (Maintainability).
- Use PostgreSQL 16 and Flyway 10.x as baseline; Flyway auto-runs on application startup. Source: `_bmad-output/planning-artifacts/architecture.md` (Technology Stack, Flyway Migration Order).
- Preserve repo-level file layout decisions (`docker-compose.dev.yml`, `.env.example`, `backend/.../db/migration`). Source: `_bmad-output/planning-artifacts/architecture.md` (Unified Project Structure).
- Epic-level intent for this story is fast onboarding plus reproducible local setup before feature development starts. Source: `_bmad-output/planning-artifacts/epics.md` (Epic 8: Infrastructure & DevOps).

### Project Structure Notes

- Planned architecture currently assumes a monorepo shape (`job-tracker/backend`, `job-tracker/frontend`, root compose files). This planning repo is the orchestration source, so adapt paths when implementing in target code repo(s) while preserving the same structure intent. Source: `_bmad-output/planning-artifacts/architecture.md` (Unified Project Structure).
- `docs/project_prompt_directions.md` includes optional multi-repo guidance; treat it as advisory unless architecture/epics are intentionally re-baselined to polyrepo. Source: `docs/project_prompt_directions.md` (Multi-Repo Coordination Strategy).

### References

- Story definition and acceptance criteria: `_bmad-output/planning-artifacts/epics.md` (Story 8.1: Docker Compose Local Development Environment).
- Epic goal and NFR scope: `_bmad-output/planning-artifacts/epics.md` (Epic 8: Infrastructure & DevOps).
- NFR constraints (env parity, env-var config, Flyway-managed schema): `_bmad-output/planning-artifacts/prd.md` (Maintainability).
- Reliability and security constraints relevant to infra setup: `_bmad-output/planning-artifacts/prd.md` (Reliability & Availability, Security).
- Stack and infra topology baseline: `_bmad-output/planning-artifacts/architecture.md` (Technology Stack, Docker Compose & Infrastructure).

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

- Ultimate context engine analysis completed - comprehensive developer guide created.

### File List

- `_bmad-output/implementation-artifacts/8-1-docker-compose-local-development-environment.md`

