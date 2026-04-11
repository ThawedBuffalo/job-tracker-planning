# Story E008-S001: Bootstrap Shared Contract Repo Structure

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a platform developer,
I want a dedicated `job-tracker-shared-libs` contract foundation,
so that all services and Flutter consume a single versioned source of truth for API and event contracts.

## Acceptance Criteria

1. Given a fresh `job-tracker-shared-libs` repo, when bootstrapped, then it contains this contract-first structure:
   - `openapi/user-service.v1.yaml`
   - `openapi/job-service.v1.yaml`
   - `openapi/pipeline-service.v1.yaml`
   - `openapi/notification-service.v1.yaml`
   - `events/application-events.v1.json`
   - `events/notification-events.v1.json`
   - `schemas/common/` (shared schema fragments)
   - `README.md` with versioning and release rules
2. Given the repo policy, when contracts change, then semantic versioning is enforced:
   - `MAJOR`: breaking API/event changes
   - `MINOR`: additive backward-compatible changes
   - `PATCH`: non-breaking docs/fix updates
3. Given CI runs on PR, when contract files are modified, then CI validates:
   - OpenAPI lint passes
   - OpenAPI compatibility check against previous release passes (or correctly flags breaking changes)
   - Event JSON schemas validate against defined schema rules
4. Given CI runs on main, when contract validation passes, then a versioned artifact/tag can be produced and referenced by service repos.
5. Given downstream repos consume contracts, when they pin `contract_version`, then build docs describe where and how to update pinned versions.

## Tasks / Subtasks

- [ ] Create foundational contract directory layout
  - [ ] Add `openapi/`, `events/`, and `schemas/common/` directories
  - [ ] Add placeholder v1 contract files for all four services and two event catalogs
  - [ ] Add `.gitkeep` only where empty dirs are needed
- [ ] Add base contract metadata and contribution docs
  - [ ] Create `README.md` describing contract ownership and release flow
  - [ ] Add `CONTRACT_VERSIONING.md` with MAJOR/MINOR/PATCH examples
  - [ ] Document required fields for event envelopes (`event_id`, `event_type`, `occurred_at`, `schema_version`)
- [ ] Establish lint and compatibility quality gates
  - [ ] Add OpenAPI lint configuration and command
  - [ ] Add compatibility diff command/check against previous tagged version
  - [ ] Add event schema validation command/check
- [ ] Add CI workflow for contract validation
  - [ ] On PR: run lint + compatibility + schema validation
  - [ ] On main: allow release/tag job after validation passes
  - [ ] Fail fast with actionable error output for contract drift
- [ ] Document consumer pinning guidance
  - [ ] Add section in `README.md` showing how service repos pin `contract_version`
  - [ ] Include upgrade checklist for breaking vs non-breaking updates

## Dev Notes

- This story is the dependency root for `E001-S001`, `E002-S001`, `E004-S001`, and `E006-S001` in `STORY-MAP.md`.
- Keep contracts tool-agnostic in structure, but enforce machine-checkable quality gates in CI.
- Contract files should reflect gateway route ownership and service boundaries from planning artifacts.
- Prefer minimal valid starter contracts over complete endpoint detail in this story; endpoint depth belongs to downstream stories.

### Project Structure Notes

- Target implementation repo: `job-tracker-shared-libs`.
- Planning source of truth remains in this repo under `_bmad-output/planning-artifacts/`.
- Story artifact is generated here first, then copied to `job-tracker-shared-libs/.bmad/stories/` for `*dev` execution.

### References

- Story map entry: `_bmad-output/planning-artifacts/STORY-MAP.md` (`E008-S001`)
- Architecture baseline: `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- Repo governance and CI minimums: `_bmad-output/planning-artifacts/repo-map.md`
- Ownership boundaries and event rules: `_bmad-output/planning-artifacts/service-boundaries.md`
- Contract inventory and compatibility expectations: `_bmad-output/planning-artifacts/api-contract-index.md`
- Multi-repo process guidance: `docs/project_prompt_directions.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E008-S001-bootstrap-shared-contract-repo-structure.md`
