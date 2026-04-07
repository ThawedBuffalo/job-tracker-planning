# NEXT-BLOCKERS

Fast fallback guide when BMAD hard stops trigger during `TODAY-RUN.md` and `TOMORROW-RUN.md`.

## Related Runbooks

- `MASTER-RUN-SHEET.md`
- `TODAY-RUN.md`
- `TOMORROW-RUN.md`

## Global Triage (Use First)

- [ ] Confirm story file exists in target repo `.bmad/stories/` (or `_bmad/stories/` for Flutter).
- [ ] Confirm dependency stories are actually `done` (repo state first, then `sprint-status.yaml`).
- [ ] Confirm shared contract file exists and matches expected endpoints/schemas.
- [ ] Confirm required tests exist and are runnable in target repo.
- [ ] If CR reports High/Critical findings, stop progression and loop `DS -> CR`.

## Blocker Type -> Immediate Action

### 1) Missing story file

- Check: target path in run sheet (`TODAY-RUN.md` or `TOMORROW-RUN.md`).
- Look: `job-tracker-planning/_bmad-output/implementation-artifacts/` for source artifact.
- Run next: copy artifact into target repo `.bmad/stories/`, then run `VS`.

### 2) Dependency not done

- Check: dependency story output exists in target repo(s).
- Look: `job-tracker-planning/_bmad-output/planning-artifacts/STORY-MAP.md` for `depends_on`.
- Run next: execute blocking story first (`VS -> DS -> CR`).

### 3) Contract mismatch

- Check: OpenAPI/event files in `job-tracker-shared-libs`.
- Look:
  - `job-tracker-shared-libs/openapi/user-service.v1.yaml`
  - `job-tracker-shared-libs/openapi/job-service.v1.yaml`
  - `job-tracker-shared-libs/openapi/pipeline-service.v1.yaml`
  - `job-tracker-shared-libs/openapi/notification-service.v1.yaml`
  - `job-tracker-shared-libs/events/*.json`
- Run next (in `job-tracker-shared-libs`):
  - `npm run lint:openapi`
  - `npm run validate:events`
  - `npm run check:compat`

### 4) Missing tests

- Check: target repo has tests for happy + failure + security paths.
- Look: story acceptance criteria and `Tasks/Subtasks` section.
- Run next: add tests in DS, then rerun local test command before CR.

### 5) CR High/Critical findings

- Check: CR output file/notes for exact file+line findings.
- Look: ownership/auth bypass, contract drift, missing negative tests.
- Run next: patch in DS, rerun tests, rerun CR.

## Story-Specific Fallbacks

### E001-S003 (`job-tracker-api-gateway`)

- Gate fails if `E001-S002` not truly working.
- Verify:
  - `job-tracker-user-service` auth endpoints pass tests.
  - `job-tracker-shared-libs/openapi/user-service.v1.yaml` is present.
- Run next:
  - Re-run `job-tracker-user-service` tests.
  - Then resume `VS -> DS -> CR` for `E001-S003`.

### E002-S002 (`job-tracker-job-service`)

- Gate fails if contract/infra/auth chain is incomplete.
- Verify:
  - `openapi/job-service.v1.yaml` contains required CRUD routes.
  - Infra baseline/migrations exist in `job-tracker-infra`.
- Run next:
  - Complete missing dependency story.
  - Resume `VS -> DS -> CR` for `E002-S002`.

### E003-S001 (`job-tracker-job-service`)

- Gate fails if `E002-S002` not done.
- Verify:
  - CRUD baseline implemented and tested.
- Run next:
  - Finish `E002-S002` first, then run `E003-S001`.

### E004-S002 (`job-tracker-pipeline-service`)

- Gate fails if transition contract or job-service baseline missing.
- Verify:
  - `openapi/pipeline-service.v1.yaml` transition endpoint exists.
  - `E002-S002` outputs available for ownership/status context.
- Run next:
  - Fix dependency, then continue `VS -> DS -> CR`.

### E004-S003 (`job-tracker-pipeline-service`)

- Gate fails if `E004-S002` not done.
- Verify:
  - Append-only events and transition validation already in place.
- Run next:
  - Close `E004-S002` CR findings first.

### E006-S002 (`job-tracker-notification-service`)

- Gate fails if notification contracts/events not aligned.
- Verify:
  - `openapi/notification-service.v1.yaml` and `events/notification-events.v1.json`.
  - upstream event producers available (`E004-S002`).
- Run next:
  - Re-validate shared contracts, then continue story flow.

## Quick Commands

Run from each repo root as applicable.

```zsh
# shared-libs contract gates
npm run lint:openapi
npm run validate:events
npm run check:compat
```

```zsh
# user-service baseline tests
cd /Users/jc/Documents/SW_DEV/Projects/job-tracker/job-tracker-user-service
npm test
```

```zsh
# infra compose syntax check (if Docker installed)
cd /Users/jc/Documents/SW_DEV/Projects/job-tracker/job-tracker-infra
docker compose --env-file .env.example config
```

## Progress Rule

If blocked for more than one DS/CR loop, log blocker in run sheet closeout and move to the next unblocked story only if dependencies allow.

