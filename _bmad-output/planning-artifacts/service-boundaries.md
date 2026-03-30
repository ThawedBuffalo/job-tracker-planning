---
workflowType: architecture-support
project_name: job-tracker
author: JC
date: 2026-03-30
status: Draft
---

# Service Boundaries

## 1) Domain Ownership Matrix

| Service | Owns | Can Write | Reads Via | Emits Events | Consumes Events |
|---|---|---|---|---|---|
| `user-service` | Identity, credentials, profile, notification preferences | `user_svc.*` | Own DB | `UserRegistered`, `PreferenceUpdated` | None (v1) |
| `job-service` | Job application records, source metadata, optional fields | `job_svc.*` | Own DB + APIs | `ApplicationCreated`, `ApplicationUpdated`, `ApplicationDeleted` | None (v1) |
| `pipeline-service` | Status timeline, transition rules, next-step prompts | `pipeline_svc.*` | Own DB + `job-service` API | `ApplicationStatusChanged`, `ApplicationStalled` | `ApplicationCreated` |
| `notification-service` | Delivery jobs, retry schedule, send history | `notification_svc.*` | Own DB + APIs | `NotificationSent`, `NotificationFailed` | `ApplicationStatusChanged`, `ApplicationStalled`, `PreferenceUpdated` |
| `api-gateway` | Edge routing, auth enforcement, response envelope | None | Service APIs only | None | None |

## 2) Hard Boundary Rules

1. A service writes only its own PostgreSQL schema.
2. No service performs cross-schema joins in application code.
3. Cross-service data access happens only via published API or domain events.
4. Gateway is the only public ingress from Flutter.
5. Any breaking contract change must be versioned and approved in `job-tracker-shared-libs`.

## 3) Job vs Pipeline Anti-Corruption Rules

- `job-service` owns application core fields (title, company, URL, notes, location, compensation, follow-up date).
- `pipeline-service` owns stage state machine and immutable timeline.
- `job-service` cannot mutate stage directly.
- `pipeline-service` cannot mutate job core fields.
- Stage transition requests from UI route through gateway to `pipeline-service`.
- `pipeline-service` validates allowed transitions and records append-only events.

## 4) Synchronous Contracts (v1)

- `api-gateway -> user-service`: auth/account/preferences
- `api-gateway -> job-service`: CRUD/search/capture
- `api-gateway -> pipeline-service`: transition commands, timeline query
- `api-gateway -> notification-service`: optional admin/support endpoints only
- `pipeline-service -> job-service`: read job metadata needed for prompts or notification payload composition

## 5) Asynchronous Event Contracts (v1)

- `job-service` publishes `ApplicationCreated`.
- `pipeline-service` publishes `ApplicationStatusChanged` and `ApplicationStalled`.
- `notification-service` consumes status/stalled/preference events and handles email delivery.

All events include:
- `event_id` (UUID)
- `event_type`
- `occurred_at` (UTC)
- `user_id`
- `application_id` (when applicable)
- `schema_version`

## 6) Data Classification and Security

- Password hashes and refresh tokens remain in `user-service` boundary only.
- JWT claims carry minimum identity/scope needed (`sub`, `email`, `roles`, `iat`, `exp`).
- Every data query enforces user scoping by `user_id`.
- Security-relevant events are audit logged with trace IDs.

## 7) Boundary Violation Checks (CI)

- Reject PRs that introduce DB clients to non-owned schemas.
- Reject PRs with endpoint changes that are not reflected in OpenAPI contract diffs.
- Reject event payload changes without schema version bump and compatibility notes.

## 8) Transitional Guidance from Monolith Baseline

- Start by preserving endpoint shapes from current monolith docs to keep Flutter changes minimal.
- Extract notification responsibilities first if capacity is limited.
- Keep gateway facade stable while internal service routes evolve.

