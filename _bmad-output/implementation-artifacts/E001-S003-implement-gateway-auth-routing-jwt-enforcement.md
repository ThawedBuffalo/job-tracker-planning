# Story E001-S003: Implement gateway auth route forwarding and JWT enforcement

Status: ready-for-dev

## Story

As a platform developer,
I want the API gateway to forward auth routes and enforce JWT for protected routes,
so that all clients use one secure entrypoint.

## Acceptance Criteria

1. Gateway forwards `/api/v1/auth/*` routes to `user-service` per contract.
2. Gateway enforces bearer token checks on protected routes and bypasses public routes.
3. Gateway propagates correlation IDs and standardized error envelopes.
4. Unauthorized and forbidden responses are mapped consistently.
5. Route config and middleware are covered by tests.
6. Gateway build and integration checks pass in CI.

## Tasks / Subtasks

- [ ] Implement gateway route map for auth endpoints
- [ ] Add JWT validation middleware and route policy config
- [ ] Add public/protected route allowlist
- [ ] Normalize auth-related error responses
- [ ] Add tests for pass-through and enforcement paths
- [ ] Document gateway auth behavior and config variables

## Dev Notes

- Gateway should validate access tokens, not issue them.
- Keep auth policy config explicit and easy to audit.
- Use contract version pinning from shared-libs.

### Project Structure Notes

- Target repo: `job-tracker-api-gateway`
- Depends on: `E001-S002`

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

- `_bmad-output/implementation-artifacts/E001-S003-implement-gateway-auth-routing-jwt-enforcement.md`

