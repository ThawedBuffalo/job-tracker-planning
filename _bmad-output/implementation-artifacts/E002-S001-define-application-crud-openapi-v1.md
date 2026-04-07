# Story E002-S001: Define Application CRUD OpenAPI v1

Status: ready-for-dev

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a platform developer,
I want a complete `openapi/job-service.v1.yaml` contract for application CRUD,
so that gateway, job-service, and Flutter can implement application capture and dashboard list behaviors from one pinned contract.

## Acceptance Criteria

1. Given the shared contracts repo, when this story is complete, then `openapi/job-service.v1.yaml` defines these routes under `/api/v1`:
   - `POST /applications`
   - `GET /applications`
   - `GET /applications/{id}`
   - `PATCH /applications/{id}`
   - `DELETE /applications/{id}`
2. Given list behavior requirements, when defining `GET /applications`, then contract includes query params for filtering and retrieval controls:
   - `status` (enum filter)
   - `q` (search text for company/title)
   - `sort` (for example `last_status_changed_at:desc`)
   - `page`, `page_size`
3. Given request/response schemas, when generating clients, then the spec includes reusable components for:
   - `CreateApplicationRequest`, `UpdateApplicationRequest`
   - `ApplicationResponse`, `ApplicationListResponse`
   - `ApplicationSummary` and optional detail fields
   - normalized `ErrorEnvelope`
4. Given ownership and security constraints, when reviewing endpoint contracts, then all `/applications*` operations require bearer auth and document user-scoped access (no cross-user reads/writes).
5. Given data integrity expectations, when designing write operations, then create/update payloads enforce required fields and validation error responses with stable error codes.
6. Given API quality gates, when CI runs for contract changes, then OpenAPI lint and compatibility checks pass (or correctly fail for breaking changes).
7. Given downstream consumer repos, when this contract is released, then version pinning and upgrade notes are documented.

## Tasks / Subtasks

- [ ] Author `openapi/job-service.v1.yaml`
  - [ ] Define `info`, `servers`, tags, and version metadata
  - [ ] Define five required paths and methods
  - [ ] Define operation IDs and standard response status codes
- [ ] Define list query contract for `GET /applications`
  - [ ] Add `status`, `q`, `sort`, `page`, and `page_size` query params
  - [ ] Define pagination response shape (`items`, `page`, `page_size`, `total`)
  - [ ] Add examples for filtered and searched responses
- [ ] Define schema components
  - [ ] Add request DTOs for create/update
  - [ ] Add response DTOs for item and list views
  - [ ] Include optional fields used by capture/dashboard (notes, URL, location, compensation, follow-up date)
  - [ ] Add shared `ErrorEnvelope` (`error`, `message`, `trace_id`, `details`)
- [ ] Define security and ownership semantics
  - [ ] Add bearer auth scheme and apply to all `/applications*` operations
  - [ ] Document ownership failures and expected status/error responses
  - [ ] Document required-field validation responses
- [ ] Align error code vocabulary for v1 application CRUD
  - [ ] Include at minimum: `VALIDATION_ERROR`, `APPLICATION_NOT_FOUND`, `UNAUTHORIZED`, `FORBIDDEN`
  - [ ] Map each error to HTTP status and example payload
- [ ] Add quality gates for this contract
  - [ ] Ensure OpenAPI lint command is configured and documented
  - [ ] Ensure breaking-change compatibility diff check is configured
  - [ ] Ensure release notes include consumer impact section
- [ ] Document consumer pinning and upgrade steps
  - [ ] Update shared contracts `README.md` with `contract_version` usage for `job-service`
  - [ ] Add short upgrade checklist for gateway and Flutter consumers

## Dev Notes

- Keep this story scoped to CRUD/search/filter contracts only; URL parsing contract depth belongs to `E003-S001`.
- Canonical route base is `/api/v1/applications` to match gateway mapping.
- Contract must preserve user-scoped ownership rules from service boundaries.
- This is a contract-first story; no service implementation code is required here.

### Project Structure Notes

- Target implementation repo: `job-tracker-shared-libs`.
- Primary artifact for this story: `openapi/job-service.v1.yaml`.
- Story artifact is generated in planning repo and then copied to `job-tracker-shared-libs/.bmad/stories/`.

### References

- Story map dependency and routing: `_bmad-output/planning-artifacts/STORY-MAP.md` (`E002-S001`)
- Gateway route ownership and API catalog: `_bmad-output/planning-artifacts/api-contract-index.md`
- Boundary and ownership constraints: `_bmad-output/planning-artifacts/service-boundaries.md`
- Microservices architecture baseline: `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- CI/versioning conventions: `_bmad-output/planning-artifacts/repo-map.md`
- FR/NFR source for application behavior: `_bmad-output/planning-artifacts/epics.md`
- Multi-repo process context: `docs/project_prompt_directions.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E002-S001-define-application-crud-openapi-v1.md`

