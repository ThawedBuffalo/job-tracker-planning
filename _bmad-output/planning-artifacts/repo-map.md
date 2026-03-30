---
workflowType: architecture-support
project_name: job-tracker
author: JC
date: 2026-03-30
status: Draft
---

# Repo Map (Polyrepo)

## 1) Repository Inventory

| Repo | Type | Primary Responsibility | Inputs | Outputs |
|---|---|---|---|---|
| `job-tracker-planning` | Planning | Source-of-truth for PRD, architecture, epics, story mapping | Product brief, UX, requirements | Stories, architecture decisions |
| `job-tracker-flutter` | App | Flutter Web UI and API client integration | OpenAPI client models, story files | Deployable web artifact |
| `job-tracker-api-gateway` | Service | BFF routing, auth enforcement, response normalization | OpenAPI route map, auth config | Stable public API |
| `job-tracker-user-service` | Service | Accounts, login, token lifecycle, user preferences | Auth stories, contract specs | User/auth APIs, auth events |
| `job-tracker-job-service` | Service | Application capture, CRUD, search/filter | Capture stories, parsing rules | Job domain APIs, job events |
| `job-tracker-pipeline-service` | Service | Status transitions, append-only timeline, prompts | Pipeline stories, event schemas | Pipeline APIs, status events |
| `job-tracker-notification-service` | Service | Email notifications, retries, stale checks | Notification stories, event subscriptions | Delivery outcomes, notification logs |
| `job-tracker-shared-libs` | Shared | OpenAPI specs, event schemas, shared DTO libs | Service contracts | Versioned contract artifacts |
| `job-tracker-infra` | Infra | Docker Compose, Nginx, Flyway migrations, backup jobs | Service images and env templates | Running stack on VPS |

## 2) Ownership and Change Policy

- Default owner for all repos: `JC` (solo engineer mode).
- Each repo must have `CODEOWNERS` even if single owner (future-proofing).
- No direct edits to contracts inside service repos; contracts originate in `job-tracker-shared-libs`.
- Story files are generated in planning and copied/synced into target repo `.bmad/stories/`.

## 3) Branching and Versioning

- Default branch: `main`.
- Feature branches: `feat/<story-id>-<short-name>`.
- Contract repo (`job-tracker-shared-libs`) uses semantic version tags:
  - `MAJOR` for breaking API/event changes
  - `MINOR` for backward-compatible endpoint/event additions
  - `PATCH` for documentation/non-breaking fixes

## 4) Minimum CI per Repo

### Required checks (all repos)
- Build passes
- Unit tests pass
- Lint/format pass
- Dependency vulnerability scan

### Service repo checks
- OpenAPI compatibility check against published contract version
- Consumer-driven compatibility smoke tests for gateway-routed endpoints

### Flutter checks
- Generated API client in sync with pinned contract version
- Widget tests for critical dashboard/capture flows

### Infra checks
- `docker compose config` validation
- Migration dry-run against disposable PostgreSQL

## 5) Artifact Flow

1. Planning updates docs and story map.
2. Contracts update in `job-tracker-shared-libs` and release version tag.
3. Services and Flutter consume new contract version.
4. Infra pins image tags and contract versions for deployment.

## 6) Story-to-Repo Routing Conventions

- Story ID format: `E<epic>-S<story>` (example: `E004-S003`).
- Required metadata in each story:
  - `target_repo`
  - `depends_on`
  - `contract_version`
  - `api_or_event_refs`
- Planning repo maintains `STORY-MAP.md` with global dependency order.

## 7) First Implementation Wave (recommended)

1. `job-tracker-shared-libs` (contract skeletons)
2. `job-tracker-api-gateway` + `job-tracker-user-service`
3. `job-tracker-job-service`
4. `job-tracker-pipeline-service`
5. `job-tracker-notification-service`
6. `job-tracker-flutter`
7. `job-tracker-infra` hardening and release

