# Story E004-S001: Define Pipeline OpenAPI v1

Status: ready-for-dev

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a platform developer,
I want a complete `openapi/pipeline-service.v1.yaml` contract for pipeline transitions and timeline history,
so that gateway, pipeline-service, and Flutter can implement stage progression, audit history, and next-step guidance from one pinned contract.

## Acceptance Criteria

1. Given the shared contracts repo, when this story is complete, then `openapi/pipeline-service.v1.yaml` defines these routes under `/api/v1`:
   - `POST /applications/{id}/transition`
   - `GET /applications/{id}/timeline`
2. Given transition behavior requirements, when defining `POST /applications/{id}/transition`, then contract includes:
   - `TransitionRequest` with target status and optional rationale/notes
   - explicit validation/error behavior for invalid or disallowed transitions
   - response model containing resulting status, timestamp, and next-step guidance
3. Given timeline requirements, when defining `GET /applications/{id}/timeline`, then contract returns:
   - ordered append-only timeline items
   - event metadata (`event_id`, `occurred_at`, `actor/source`, `from_status`, `to_status`)
   - immutable semantics documented (events cannot be edited/deleted)
4. Given guidance requirements, when consuming timeline responses, then contract exposes next-step guidance (for example `next_step_prompt`) in a stable location in the response model.
5. Given ownership and security constraints, when reviewing endpoint contracts, then all `/applications/{id}/...` pipeline operations require bearer auth and document user-scoped access.
6. Given normalized API behavior, when operations fail, then responses use the shared `ErrorEnvelope` shape.
7. Given API quality gates, when CI runs for contract changes, then OpenAPI lint and compatibility checks pass (or correctly fail on breaking changes).
8. Given downstream consumer repos, when this contract is released, then version pinning and upgrade notes are documented.

## Tasks / Subtasks

- [ ] Author `openapi/pipeline-service.v1.yaml`
  - [ ] Define `info`, `servers`, tags, and version metadata
  - [ ] Define required paths and methods
  - [ ] Define operation IDs and standard response status codes
- [ ] Define transition contract for `POST /applications/{id}/transition`
  - [ ] Add `TransitionRequest` and `TransitionResponse` schemas
  - [ ] Define allowed status enum and transition validation notes
  - [ ] Include examples for successful transition and invalid transition attempt
- [ ] Define timeline contract for `GET /applications/{id}/timeline`
  - [ ] Add `TimelineResponse` and `TimelineEvent` schemas
  - [ ] Document default ordering and pagination strategy (if used)
  - [ ] Include examples with multiple historical status events
- [ ] Define immutable event semantics
  - [ ] Document append-only timeline rule in endpoint descriptions
  - [ ] Include immutable event metadata fields (`event_id`, `occurred_at`, `schema_version`)
  - [ ] Ensure no update/delete timeline operations are present in v1
- [ ] Define security and ownership semantics
  - [ ] Add bearer auth scheme and apply to both pipeline endpoints
  - [ ] Document ownership failures and expected status/error responses
  - [ ] Document handling of missing application IDs
- [ ] Align error code vocabulary for v1 pipeline contracts
  - [ ] Include at minimum: `VALIDATION_ERROR`, `INVALID_STATUS_TRANSITION`, `APPLICATION_NOT_FOUND`, `UNAUTHORIZED`, `FORBIDDEN`
  - [ ] Map each error to HTTP status and example payload
- [ ] Add quality gates for this contract
  - [ ] Ensure OpenAPI lint command is configured and documented
  - [ ] Ensure breaking-change compatibility diff check is configured
  - [ ] Ensure release notes include consumer impact section
- [ ] Document consumer pinning and upgrade steps
  - [ ] Update shared contracts `README.md` with `contract_version` usage for `pipeline-service`
  - [ ] Add short upgrade checklist for gateway and Flutter consumers

## Dev Notes

- Keep this story scoped to transition/timeline contracts only; event producer implementation belongs to downstream service stories.
- Canonical route base is `/api/v1/applications/{id}` to match gateway mapping.
- Contract must preserve append-only status event history and user-scoped ownership constraints.
- This is a contract-first story; no service implementation code is required here.

### Project Structure Notes

- Target implementation repo: `job-tracker-shared-libs`.
- Primary artifact for this story: `openapi/pipeline-service.v1.yaml`.
- Story artifact is generated in planning repo and then copied to `job-tracker-shared-libs/.bmad/stories/`.

### References

- Story map dependency and routing: `_bmad-output/planning-artifacts/STORY-MAP.md` (`E004-S001`)
- Gateway route ownership and API catalog: `_bmad-output/planning-artifacts/api-contract-index.md`
- Boundary and ownership constraints: `_bmad-output/planning-artifacts/service-boundaries.md`
- Microservices architecture baseline: `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- CI/versioning conventions: `_bmad-output/planning-artifacts/repo-map.md`
- FR/NFR source for pipeline behavior: `_bmad-output/planning-artifacts/epics.md`
- Multi-repo process context: `docs/project_prompt_directions.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E004-S001-define-pipeline-openapi-v1.md`

