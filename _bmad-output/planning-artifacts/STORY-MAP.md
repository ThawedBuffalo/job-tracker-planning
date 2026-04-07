---
workflowType: scrum-master
project_name: job-tracker
author: JC
date: 2026-03-30
status: Active - CS/VS Complete, Ready for DS
inputs:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/epics.md
  - _bmad-output/planning-artifacts/architecture-polyrepo.md
  - _bmad-output/planning-artifacts/repo-map.md
  - _bmad-output/planning-artifacts/service-boundaries.md
  - _bmad-output/planning-artifacts/api-contract-index.md
---

# STORY-MAP (Polyrepo Microservices)

## Sequencing Policy

- Contracts-first, then edge/auth, then core domain, then notifications, then integration hardening.
- Dependencies are a cross-repo DAG and intentionally not strict epic-linear.
- Status for all entries starts as `Planned`, then transitions to `ready-for-dev` after story creation and validation.

## Initial Story Backlog (Wave 1)

| Story ID | Title | target_repo | depends_on | contract_version | api_or_event_refs | status |
|---|---|---|---|---|---|---|
| E008-S001 | Bootstrap shared contract repo structure | `job-tracker-shared-libs` | - | `v1.0.0` | `openapi/*`, `events/*` | ready-for-dev |
| E001-S001 | Define auth/account OpenAPI v1 | `job-tracker-shared-libs` | E008-S001 | `v1.0.0` | `openapi/user-service.v1.yaml` | ready-for-dev |
| E002-S001 | Define application CRUD OpenAPI v1 | `job-tracker-shared-libs` | E008-S001 | `v1.0.0` | `openapi/job-service.v1.yaml` | ready-for-dev |
| E004-S001 | Define pipeline OpenAPI v1 | `job-tracker-shared-libs` | E008-S001 | `v1.0.0` | `openapi/pipeline-service.v1.yaml` | ready-for-dev |
| E006-S001 | Define notification API/event schemas v1 | `job-tracker-shared-libs` | E008-S001 | `v1.0.0` | `openapi/notification-service.v1.yaml`, `events/*.json` | ready-for-dev |
| E008-S002 | Compose stack baseline (gateway, services, postgres, nginx) | `job-tracker-infra` | E008-S001 | `v1.0.0` | `docker-compose.yml` | ready-for-dev |
| E008-S003 | Flyway schema bootstrap for `user_svc`,`job_svc`,`pipeline_svc`,`notification_svc` | `job-tracker-infra` | E008-S002 | `v1.0.0` | `db/migrations/*` | ready-for-dev |
| E001-S002 | Implement `user-service` register/login/refresh/logout | `job-tracker-user-service` | E001-S001,E008-S003 | `v1.0.0` | `/auth/register,/auth/login,/auth/refresh,/auth/logout` | ready-for-dev |
| E001-S003 | Implement gateway auth route forwarding and JWT enforcement | `job-tracker-api-gateway` | E001-S002 | `v1.0.0` | `/api/v1/auth/*` | ready-for-dev |
| E001-S004 | Flutter auth flow integration (register/login/logout) | `job-tracker-flutter` | E001-S003 | `v1.0.0` | `/api/v1/auth/*` | ready-for-dev |
| E002-S002 | Implement `job-service` create/read/update/delete applications | `job-tracker-job-service` | E002-S001,E001-S002,E008-S003 | `v1.0.0` | `/applications`, `/applications/{id}` | ready-for-dev |
| E003-S001 | Implement URL parse endpoint in `job-service` | `job-tracker-job-service` | E002-S002 | `v1.0.0` | `/applications/parse-url` | ready-for-dev |
| E004-S002 | Implement `pipeline-service` transition validation + append-only events | `job-tracker-pipeline-service` | E004-S001,E002-S002,E008-S003 | `v1.0.0` | `/applications/{id}/transition`, `ApplicationStatusChanged` | ready-for-dev |
| E004-S003 | Implement timeline and next-step guidance query | `job-tracker-pipeline-service` | E004-S002 | `v1.0.0` | `/applications/{id}/timeline` | ready-for-dev |
| E005-S001 | Implement gateway application and pipeline route map | `job-tracker-api-gateway` | E002-S002,E004-S003 | `v1.0.0` | `/api/v1/applications*`, `/api/v1/dashboard` | ready-for-dev |
| E005-S002 | Flutter dashboard list with status, sort, and search | `job-tracker-flutter` | E005-S001 | `v1.0.0` | `/api/v1/dashboard`, `/api/v1/applications` | ready-for-dev |
| E006-S002 | Implement `notification-service` event consumers + SMTP sender | `job-tracker-notification-service` | E006-S001,E004-S002,E001-S002 | `v1.0.0` | `ApplicationStatusChanged`, `PreferenceUpdated` | ready-for-dev |
| E006-S003 | Implement notification retry/backoff and delivery log | `job-tracker-notification-service` | E006-S002 | `v1.0.0` | `NotificationSent`, `NotificationFailed` | ready-for-dev |
| E006-S004 | Implement stalled-application event producer and reminder loop | `job-tracker-pipeline-service` | E004-S002,E006-S002 | `v1.0.0` | `ApplicationStalled` | ready-for-dev |
| E007-S001 | Enforce user-scope access checks across services | `job-tracker-api-gateway` | E001-S003,E002-S002,E004-S002 | `v1.0.0` | auth middleware + ownership checks | ready-for-dev |
| E008-S004 | Integration smoke tests and release candidate compose profile | `job-tracker-infra` | E005-S002,E006-S003,E007-S001 | `v1.0.0` | login->create->transition->email | ready-for-dev |

## BMad CS/VS Execution Loop

For each row in order:

1. `CS` (Create Story) for the specific `Story ID` and `target_repo`.
2. Save story artifact under `target_repo/.bmad/stories/<story-id>.md`.
3. `VS` (Validate Story) and resolve issues until approved.
4. Hand off to `DS` (Dev Story) in target repo.
5. After implementation, run `CR` (Code Review) and loop back to `DS` if needed.

## Current Recommendation

Start with `E008-S001` and `E001-S001` in implementation repos, then run `CR` after each `DS` completion before moving to dependent stories.

