# TOMORROW-RUN (Focused Queue)

Focused execution sheet for the next BMAD stories after `TODAY-RUN.md`.

## Related Runbooks

- `MASTER-RUN-SHEET.md`
- `TODAY-RUN.md`
- `NEXT-BLOCKERS.md`

## Scope

Run only these stories tomorrow in strict order:

1. `E004-S002` (`job-tracker-pipeline-service`)
2. `E004-S003` (`job-tracker-pipeline-service`)
3. `E006-S002` (`job-tracker-notification-service`)

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

## Story 1: E004-S002

- [ ] File: `job-tracker-pipeline-service/.bmad/stories/E004-S002-implement-pipeline-transition-validation-events.md`
- [ ] Gate check: `E004-S001` pipeline contract complete
- [ ] Gate check: `E002-S002` job-service CRUD is done
- [ ] Gate check: infra migrations available (`E008-S003`)
- [ ] Outputs: transition validation + append-only status events + tests

## Story 2: E004-S003

- [ ] File: `job-tracker-pipeline-service/.bmad/stories/E004-S003-implement-timeline-next-step-guidance-query.md`
- [ ] Gate check: `E004-S002` is done
- [ ] Outputs: timeline query + `next_step_prompt` + ordering/ownership tests

## Story 3: E006-S002

- [ ] File: `job-tracker-notification-service/.bmad/stories/E006-S002-implement-notification-consumers-smtp-sender.md`
- [ ] Gate check: `E006-S001` notification contracts/events complete
- [ ] Gate check: `E004-S002` status event production available
- [ ] Gate check: `E001-S002` auth/user context available
- [ ] Outputs: event consumers + SMTP sender + idempotency + tests

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
- [ ] Blockers captured for the next run (`E006-S003`, `E006-S004`, `E005-S001`)

