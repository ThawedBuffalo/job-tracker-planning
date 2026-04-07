# TODAY-RUN (Focused Queue)

Focused execution sheet for the next BMAD stories.

## Related Runbooks

- `MASTER-RUN-SHEET.md`
- `TOMORROW-RUN.md`
- `NEXT-BLOCKERS.md`

## Scope

Run only these stories today in strict order:

1. `E001-S003` (`job-tracker-api-gateway`)
2. `E002-S002` (`job-tracker-job-service`)
3. `E003-S001` (`job-tracker-job-service`)

## Global Flow Per Story

- `VS` -> `DS` -> `CR`
- If `CR` has findings, loop `DS` -> `CR` until clean.
- Do not start the next story until current story is `done`.

## Hard Stops

- Missing story file in target repo
- Failed dependency gate
- Contract mismatch against shared-libs OpenAPI/events
- Missing required tests
- Unresolved High/Critical code-review findings

## Story 1: E001-S003

- [ ] File: `job-tracker-api-gateway/.bmad/stories/E001-S003-implement-gateway-auth-routing-jwt-enforcement.md`
- [ ] Gate check: `E001-S002` auth service implemented and tested
- [ ] Gate check: `openapi/user-service.v1.yaml` exists in shared-libs
- [ ] Outputs: auth route forwarding + JWT enforcement + tests

## Story 2: E002-S002

- [ ] File: `job-tracker-job-service/.bmad/stories/E002-S002-implement-job-service-application-crud.md`
- [ ] Gate check: `E002-S001` contract complete (`openapi/job-service.v1.yaml`)
- [ ] Gate check: infra baseline/migrations available (`E008-S002`, `E008-S003`)
- [ ] Outputs: application CRUD + ownership checks + list filters/sort/pagination + tests

## Story 3: E003-S001

- [ ] File: `job-tracker-job-service/.bmad/stories/E003-S001-implement-job-service-url-parse-endpoint.md`
- [ ] Gate check: `E002-S002` is done
- [ ] Outputs: URL parse endpoint + validation/failure handling + tests

## Per-Story Reporting Template

1. Story ID + phase (`VS`/`DS`/`CR`)
2. Status (`in-progress` | `review` | `done` | `blocked`)
3. Files changed
4. Acceptance checklist (`pass`/`fail`)
5. Findings by severity with file references
6. Next action (`proceed` | `loop DS fixes` | `stop`)

## End-of-Day Closeout

- [ ] All three stories marked `done`
- [ ] `sprint-status.yaml` updated
- [ ] Blockers captured for the next run

