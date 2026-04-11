# Story E001-S002: Implement user-service register/login/refresh/logout

Status: done

## Story

As an authenticated user,
I want account registration and session lifecycle endpoints,
so that I can securely access my workspace.

## Acceptance Criteria

1. `user-service` implements `POST /auth/register`, `POST /auth/login`, `POST /auth/refresh`, and `POST /auth/logout` per v1 contract.
2. Passwords are hashed (BCrypt/Argon2) and never stored in plaintext.
3. Access token TTL is 15 minutes; refresh token TTL is 7 days.
4. Refresh token is stored/rotated securely and returned via HttpOnly cookie behavior defined by contract.
5. Errors follow normalized `ErrorEnvelope` and documented error codes.
6. Unit/integration tests cover success and failure flows.

## Tasks / Subtasks

- [ ] Implement auth controllers/services per `openapi/user-service.v1.yaml`
- [ ] Implement password hashing and credential validation
- [ ] Implement JWT issue/verify/refresh logic with rotation strategy
- [ ] Implement logout and refresh invalidation behavior
- [ ] Add structured auth audit logs for security-relevant events
- [ ] Add integration tests for register/login/refresh/logout paths

## Dev Notes

- Keep account preference/profile endpoints out of this story unless required for auth flow.
- Follow error and response shapes from shared contract.
- Ensure secrets/config are environment-driven.

### Project Structure Notes

- Target repo: `job-tracker-user-service`
- Depends on: `E001-S001`, `E008-S003`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E001-S002-implement-user-service-auth.md`
