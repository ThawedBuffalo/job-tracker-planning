# Story E001-S001: Define Auth/Account OpenAPI v1

Status: ready-for-dev

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a platform developer,
I want a complete `openapi/user-service.v1.yaml` contract for auth and account APIs,
so that gateway, user-service, and Flutter can implement against one pinned source of truth.

## Acceptance Criteria

1. Given the shared contracts repo, when this story is complete, then `openapi/user-service.v1.yaml` defines these routes under `/api/v1`:
   - `POST /auth/register`
   - `POST /auth/login`
   - `POST /auth/refresh`
   - `POST /auth/logout`
   - `GET /account`
   - `PATCH /account`
2. Given route definitions, when reviewing security, then contract marks public routes (`/auth/register`, `/auth/login`, `/auth/refresh`) and protected routes (`/auth/logout`, `/account`) using explicit OpenAPI security schemes.
3. Given request/response schemas, when generating clients, then the spec includes reusable components for:
   - `RegisterRequest`, `RegisterResponse`
   - `LoginRequest`, `LoginResponse` (access token + metadata)
   - `RefreshResponse`
   - `AccountResponse`, `UpdateAccountRequest`
   - normalized `ErrorEnvelope`
4. Given auth/session rules, when calling refresh/logout, then contract documents refresh-token cookie expectations (`HttpOnly`, `Secure`, path/scope notes) and response behavior without exposing refresh token in JSON payload.
5. Given API quality gates, when CI runs for contract changes, then OpenAPI lint and compatibility checks pass (or correctly fail for breaking changes).
6. Given downstream consumer repos, when this contract is released, then version pinning guidance references semantic version policy and upgrade expectations.

## Tasks / Subtasks

- [ ] Author `openapi/user-service.v1.yaml`
  - [ ] Define `info`, `servers`, tags, and version metadata
  - [ ] Define six required paths and methods
  - [ ] Define operation IDs and standard response status codes
- [ ] Define reusable schema components
  - [ ] Add request/response DTOs for register/login/refresh/logout/account read/update
  - [ ] Add shared `ErrorEnvelope` (`error`, `message`, `trace_id`, `details`)
  - [ ] Add examples for success and common failure cases
- [ ] Define security model in contract
  - [ ] Add bearer auth scheme for access-token-protected routes
  - [ ] Document refresh cookie contract behavior for refresh/logout operations
  - [ ] Mark protected operations with security requirements
- [ ] Align error code vocabulary for v1 auth/account
  - [ ] Include at minimum: `VALIDATION_ERROR`, `EMAIL_ALREADY_REGISTERED`, `INVALID_CREDENTIALS`, `UNAUTHORIZED`, `FORBIDDEN`
  - [ ] Map each error to HTTP status and example payload
- [ ] Add quality gates for this contract
  - [ ] Ensure OpenAPI lint command is configured and documented
  - [ ] Ensure breaking-change compatibility diff check is configured
  - [ ] Ensure contract release notes include consumer impact section
- [ ] Document consumer pinning and upgrade steps
  - [ ] Update `README.md` in shared contracts repo with `contract_version` usage
  - [ ] Add a short upgrade checklist for service and Flutter repos

## Dev Notes

- Canonical path for v1 is `/account` (not `/users/me`) to match current gateway route map.
- Keep endpoint scope to auth + account management only; authorization/profile expansion belongs to later stories.
- Contract must preserve session model from architecture: 15-minute access token and refresh token cookie workflow.
- This story should stay contract-first; no service implementation code is required here.

### Project Structure Notes

- Target implementation repo: `job-tracker-shared-libs`.
- Primary artifact for this story: `openapi/user-service.v1.yaml`.
- Story artifact is generated in planning repo and then copied to `job-tracker-shared-libs/.bmad/stories/`.

### References

- Story map dependency and routing: `_bmad-output/planning-artifacts/STORY-MAP.md` (`E001-S001`)
- Gateway route ownership: `_bmad-output/planning-artifacts/api-contract-index.md`
- Auth/session architecture decisions: `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- Boundary and ownership constraints: `_bmad-output/planning-artifacts/service-boundaries.md`
- CI/versioning conventions: `_bmad-output/planning-artifacts/repo-map.md`
- Multi-repo process context: `docs/project_prompt_directions.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E001-S001-define-auth-account-openapi-v1.md`

