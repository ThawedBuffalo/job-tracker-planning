# Story E005-S001: Implement gateway application and pipeline route map

Status: ready-for-dev

## Story

As a platform developer,
I want gateway routing for application and pipeline APIs,
so that clients use one stable API surface.

## Acceptance Criteria

1. Gateway maps `/api/v1/applications*` and `/api/v1/dashboard` routes to owning services.
2. Gateway forwards auth context/correlation headers consistently.
3. Route-level policies enforce protected access for user-scoped endpoints.
4. Error responses are normalized to `ErrorEnvelope` shape.
5. Route integration tests cover CRUD, transition, and timeline forwarding.
6. Route configuration is documented for operations and debugging.

## Tasks / Subtasks

- [ ] Add route definitions for job and pipeline service endpoints
- [ ] Add middleware hooks for auth, tracing, and response normalization
- [ ] Implement dashboard aggregation route behavior if needed
- [ ] Add integration tests for routed endpoint success/failure
- [ ] Add gateway config docs and endpoint matrix
- [ ] Validate route set against pinned contract versions

## Dev Notes

- Gateway should avoid business logic beyond orchestration and policy.
- Preserve backend status/error semantics while normalizing envelope shape.
- Keep route mapping explicit to reduce accidental exposure.

### Project Structure Notes

- Target repo: `job-tracker-api-gateway`
- Depends on: `E002-S002`, `E004-S003`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`
- `_bmad-output/planning-artifacts/repo-map.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E005-S001-implement-gateway-application-pipeline-route-map.md`

