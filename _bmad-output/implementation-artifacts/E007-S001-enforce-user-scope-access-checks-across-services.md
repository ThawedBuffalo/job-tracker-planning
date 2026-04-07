# Story E007-S001: Enforce user-scope access checks across services

Status: ready-for-dev

## Story

As a user,
I want strict data isolation,
so that only my own application data is visible and mutable.

## Acceptance Criteria

1. Gateway and service layers enforce user-scoped access on all protected data routes.
2. Cross-user access attempts are denied with consistent error responses.
3. Ownership checks are applied to reads, writes, transitions, and timeline queries.
4. Security-relevant denied-access events are logged with correlation IDs.
5. Automated tests cover positive ownership and cross-user denial cases.
6. Access-control behavior is documented for all relevant endpoints.

## Tasks / Subtasks

- [ ] Implement shared ownership check strategy in gateway/service layers
- [ ] Add middleware/interceptor hooks for auth context propagation
- [ ] Enforce ownership in repository/service query paths
- [ ] Standardize denial response mapping and logging
- [ ] Add integration tests for cross-user access attempts
- [ ] Document endpoint-by-endpoint ownership policy

## Dev Notes

- Do not rely on frontend filtering for security enforcement.
- Prefer explicit query constraints by `user_id` at data access boundaries.
- Keep denial responses consistent with `ErrorEnvelope` policy.

### Project Structure Notes

- Target repo: `job-tracker-api-gateway`
- Depends on: `E001-S003`, `E002-S002`, `E004-S002`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`
- `_bmad-output/planning-artifacts/epics.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E007-S001-enforce-user-scope-access-checks-across-services.md`

