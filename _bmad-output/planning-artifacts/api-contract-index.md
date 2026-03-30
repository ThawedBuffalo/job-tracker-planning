---
workflowType: architecture-support
project_name: job-tracker
author: JC
date: 2026-03-30
status: Draft
---

# API Contract Index

## 1) Contract Source of Truth

All API and event contracts are versioned in `job-tracker-shared-libs`:

- `openapi/user-service.v1.yaml`
- `openapi/job-service.v1.yaml`
- `openapi/pipeline-service.v1.yaml`
- `openapi/notification-service.v1.yaml`
- `events/application-events.v1.json`
- `events/notification-events.v1.json`

Services consume pinned contract versions; gateway exposes a unified route surface.

## 2) Gateway Route Map

Base path: `/api/v1`

| Public Route | Method | Owner Service | Contract Ref |
|---|---|---|---|
| `/auth/register` | POST | `user-service` | `user-service.v1` |
| `/auth/login` | POST | `user-service` | `user-service.v1` |
| `/auth/refresh` | POST | `user-service` | `user-service.v1` |
| `/auth/logout` | POST | `user-service` | `user-service.v1` |
| `/account` | GET/PATCH | `user-service` | `user-service.v1` |
| `/applications` | POST/GET | `job-service` | `job-service.v1` |
| `/applications/{id}` | GET/PATCH/DELETE | `job-service` | `job-service.v1` |
| `/applications/{id}/timeline` | GET | `pipeline-service` | `pipeline-service.v1` |
| `/applications/{id}/transition` | POST | `pipeline-service` | `pipeline-service.v1` |
| `/dashboard` | GET | `job-service` + `pipeline-service` | `job-service.v1`, `pipeline-service.v1` |

## 3) Service API Highlights

### `user-service`
- Registration, login/logout, token refresh
- Account updates (email/password)
- Notification preference management

### `job-service`
- Manual application create/update/delete
- URL intake and metadata parse endpoint
- Query by status/search/sort for dashboard lists

### `pipeline-service`
- Transition command endpoint with validation
- Append-only event timeline query
- Next-step guidance resolution endpoint

### `notification-service`
- Internal endpoints for send job execution
- Delivery status query (optional admin use)

## 4) Event Catalog

| Event | Producer | Consumers | Purpose |
|---|---|---|---|
| `ApplicationCreated` | `job-service` | `pipeline-service` | Initialize stage context |
| `ApplicationStatusChanged` | `pipeline-service` | `notification-service` | Trigger status change email |
| `ApplicationStalled` | `pipeline-service` | `notification-service` | Trigger stale reminder |
| `PreferenceUpdated` | `user-service` | `notification-service` | Respect opt-out settings |
| `NotificationSent` | `notification-service` | (optional analytics) | Track successful sends |
| `NotificationFailed` | `notification-service` | (ops review) | Track retries/failures |

## 5) Error Contract

All services return a normalized error payload:

```json
{
  "error": "ERROR_CODE",
  "message": "Human readable summary",
  "trace_id": "uuid",
  "details": {}
}
```

Gateway passes through `trace_id` and maps unexpected exceptions to a safe generic code.

## 6) Versioning and Compatibility Policy

- API prefix stays `/api/v1` for all non-breaking changes.
- Breaking changes require new OpenAPI major version and coordinated gateway rollout.
- Event schema changes:
  - Additive fields: backward-compatible, minor version bump.
  - Field removals/type changes: breaking, major version bump.
- Consumers must tolerate unknown additive fields.

## 7) Contract Quality Gates

- OpenAPI diff check on every service PR against pinned contract.
- Event schema validation tests in producer and consumer repos.
- Gateway integration smoke tests for critical paths:
  - auth login/refresh
  - create application
  - transition status
  - dashboard read

## 8) Rollout Notes

- Maintain temporary compatibility endpoints if monolith-to-service extraction happens gradually.
- Prefer introducing gateway mappings first, then re-point routes to extracted services.
- Keep Flutter API client pinned to released `job-tracker-shared-libs` versions only.

