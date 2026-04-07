# Story E008-S004: Integration smoke tests and release candidate compose profile

Status: ready-for-dev

## Story

As a platform developer,
I want a repeatable integration smoke suite and release compose profile,
so that end-to-end regressions are caught before deployment.

## Acceptance Criteria

1. A smoke workflow verifies: login -> create application -> transition stage -> notification send path.
2. Release compose profile exists for candidate validation on VPS-like environment.
3. Smoke workflow returns actionable pass/fail output with trace references.
4. Critical service health checks are validated before smoke run starts.
5. CI can run smoke tests in an isolated environment.
6. Runbook documents execution, troubleshooting, and rollback triggers.

## Tasks / Subtasks

- [ ] Define smoke test scenarios and test data setup
- [ ] Implement smoke runner scripts or test harness
- [ ] Add release compose override/profile for candidate runs
- [ ] Wire smoke workflow into CI pipeline
- [ ] Add service readiness checks and timeout handling
- [ ] Document smoke execution and failure triage process

## Dev Notes

- Keep smoke suite fast and deterministic; focus on critical path only.
- Prefer test IDs/data prefixes to avoid collisions.
- Capture enough logs to debug gateway/service boundaries quickly.

### Project Structure Notes

- Target repo: `job-tracker-infra`
- Depends on: `E005-S002`, `E006-S003`, `E007-S001`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- `_bmad-output/planning-artifacts/repo-map.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E008-S004-integration-smoke-tests-release-compose-profile.md`

