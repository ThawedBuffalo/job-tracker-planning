# Story E002-S002: Implement job-service create/read/update/delete applications

Status: done

## Story

As a user,
I want to manage application records,
so that my job tracking data is complete and current.

## Acceptance Criteria

1. `job-service` implements `POST/GET /applications` and `GET/PATCH/DELETE /applications/{id}` per v1 contract.
2. Required fields are validated on create/update and return structured validation errors.
3. List endpoint supports status/search/sort/pagination query params.
4. All operations enforce user ownership and reject cross-user access.
5. Timestamps (`created_at`, `updated_at`) are maintained.
6. Unit/integration tests cover CRUD, filtering, and auth/ownership failures.

## Tasks / Subtasks

- [ ] Implement application controller/service/repository layers
- [ ] Implement request validation and error mapping
- [ ] Implement list query handling for filter/search/sort/pagination
- [ ] Enforce user-scoped access checks in data layer/service layer
- [ ] Add schema migrations for job-service core tables if needed
- [ ] Add tests for endpoint behavior and negative cases

## Dev Notes

- Keep stage transition logic out of this story; pipeline owns state transitions.
- Align payloads strictly with `openapi/job-service.v1.yaml`.
- Avoid direct coupling to notification concerns.

### Project Structure Notes

- Target repo: `job-tracker-job-service`
- Depends on: `E002-S001`, `E001-S002`, `E008-S003`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E002-S002-implement-job-service-application-crud.md`
