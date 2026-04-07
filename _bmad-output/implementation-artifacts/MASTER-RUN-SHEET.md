# MASTER-RUN-SHEET (Wave 1)

Single execution sheet for BMAD story delivery across repos.

## Related Runbooks

- `TODAY-RUN.md`
- `TOMORROW-RUN.md`
- `NEXT-BLOCKERS.md`

## How To Use

- Run each story in order.
- For each story execute: `VS` -> `DS` -> `CR`.
- If `CR` has findings, loop `DS` -> `CR` until clean.
- Do not start next story until current story is `done`.
- Update `sprint-status.yaml` after each story completion.

## Global Hard Stops

- Missing story file in target repo
- Failed dependency gate
- Contract mismatch against shared-libs artifacts
- Missing required tests
- Unresolved High/Critical code review findings

## Execution Order

### Phase A - Infra + Auth Foundations

- [ ] `E008-S002` -> `job-tracker-infra/.bmad/stories/E008-S002-compose-stack-baseline.md`
- [ ] `E008-S003` -> `job-tracker-infra/.bmad/stories/E008-S003-flyway-schema-bootstrap.md`
- [ ] `E001-S002` -> `job-tracker-user-service/.bmad/stories/E001-S002-implement-user-service-auth.md`
- [ ] `E001-S003` -> `job-tracker-api-gateway/.bmad/stories/E001-S003-implement-gateway-auth-routing-jwt-enforcement.md`
- [ ] `E001-S004` -> `job-tracker-flutter/_bmad/stories/E001-S004-flutter-auth-flow-integration.md`

### Phase B - Core Job + Pipeline

- [ ] `E002-S002` -> `job-tracker-job-service/.bmad/stories/E002-S002-implement-job-service-application-crud.md`
- [ ] `E003-S001` -> `job-tracker-job-service/.bmad/stories/E003-S001-implement-job-service-url-parse-endpoint.md`
- [ ] `E004-S002` -> `job-tracker-pipeline-service/.bmad/stories/E004-S002-implement-pipeline-transition-validation-events.md`
- [ ] `E004-S003` -> `job-tracker-pipeline-service/.bmad/stories/E004-S003-implement-timeline-next-step-guidance-query.md`

### Phase C - Notifications + Security Hardening

- [ ] `E006-S002` -> `job-tracker-notification-service/.bmad/stories/E006-S002-implement-notification-consumers-smtp-sender.md`
- [ ] `E006-S003` -> `job-tracker-notification-service/.bmad/stories/E006-S003-implement-notification-retry-backoff-delivery-log.md`
- [ ] `E006-S004` -> `job-tracker-pipeline-service/.bmad/stories/E006-S004-implement-stalled-application-reminder-loop.md`
- [ ] `E007-S001` -> `job-tracker-api-gateway/.bmad/stories/E007-S001-enforce-user-scope-access-checks-across-services.md`

### Phase D - Experience + Release

- [ ] `E005-S001` -> `job-tracker-api-gateway/.bmad/stories/E005-S001-implement-gateway-application-pipeline-route-map.md`
- [ ] `E005-S002` -> `job-tracker-flutter/_bmad/stories/E005-S002-flutter-dashboard-list-status-sort-search.md`
- [ ] `E008-S004` -> `job-tracker-infra/.bmad/stories/E008-S004-integration-smoke-tests-release-compose-profile.md`

## Per-Story Reporting Template

Use this after each phase (`VS`, `DS`, `CR`):

1. Story ID + phase
2. Status (`in-progress` | `review` | `done` | `blocked`)
3. Files changed
4. Acceptance checklist (`pass`/`fail`)
5. Findings by severity with file references
6. Next action (`proceed` | `loop DS fixes` | `stop`)

## Endgame Checklist

- [ ] All Wave 1 stories marked `done`
- [ ] `sprint-status.yaml` updated
- [ ] Integration smoke path validated (`login -> create -> transition -> email`)
- [ ] Optional epic retrospectives (`ER`) completed

