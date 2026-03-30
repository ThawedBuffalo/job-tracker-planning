---
workflowType: architecture
project_name: job-tracker
author: JC
date: 2026-03-30
status: Draft - Microservices Baseline
supersedes:
  - _bmad-output/planning-artifacts/architecture.md
inputs:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/epics.md
  - design-artifacts/A-Product-Brief/platform-requirements.md
---

# Architecture Document - Polyrepo Microservices

## 1) Intent and Constraints

This document re-baselines `job-tracker` from monolith-first to a pragmatic microservices topology while preserving the original delivery constraints:

- Flutter Web frontend
- Spring Boot services (Java 21)
- PostgreSQL 16
- Single VPS deployment via Docker Compose
- Solo engineer delivery

The architecture optimizes for clear service boundaries and contract discipline, not horizontal scale on day one.

## 2) Target Topology

**Client**
- `job-tracker-flutter` (Flutter Web)

**Backend services**
- `job-tracker-api-gateway` (BFF + edge policies)
- `job-tracker-user-service` (auth, account, notification preferences)
- `job-tracker-job-service` (application capture, CRUD, search)
- `job-tracker-pipeline-service` (status transitions, append-only status events, next-step guidance)
- `job-tracker-notification-service` (email dispatch, retry, stale checks)

**Support repos**
- `job-tracker-shared-libs` (OpenAPI specs, event schemas, shared Java libs)
- `job-tracker-infra` (Compose, Nginx, migrations, backup jobs)
- `job-tracker-planning` (PRD, architecture, epics, story map)

## 3) ADRs (Microservices Baseline)

### ADR-M1: Repository strategy
**Decision:** Polyrepo with eight repos and a planning repo as source of truth.

**Rationale:** Matches BMad story handoff model and keeps per-service CI/CD simple.

### ADR-M2: Deployment model
**Decision:** All services run on one VPS via Docker Compose behind Nginx TLS.

**Rationale:** Preserves low ops overhead while enabling service separation.

### ADR-M3: API style
**Decision:** REST/JSON only (`/api/v1/...`) with OpenAPI as contract source.

**Rationale:** Fits Flutter Web client and existing PRD assumptions.

### ADR-M4: Authentication and session
**Decision:** `user-service` issues JWT access tokens (15 min) and refresh tokens (7 days, HttpOnly cookie). `api-gateway` enforces auth for protected routes.

**Rationale:** Maintains current security model while centralizing identity concerns.

### ADR-M5: Data ownership
**Decision:** Single PostgreSQL instance, schema-per-service ownership.

**Schema plan:**
- `user_svc`
- `job_svc`
- `pipeline_svc`
- `notification_svc`

**Rule:** A service writes only its own schema. Cross-service reads happen through APIs/events, never direct table joins.

### ADR-M6: Async integration strategy
**Decision:** PostgreSQL outbox + poller workers in each service (no broker on day one).

**Rationale:** Reliable event publication without adding RabbitMQ/Kafka operational load for v1.

### ADR-M7: Notification triggering
**Decision:** `notification-service` reacts to domain events and schedules retries/backoff for failed sends.

**Rationale:** Keeps email delivery concerns isolated and scalable first for extraction.

### ADR-M8: Migration ownership
**Decision:** All Flyway migrations live in `job-tracker-infra`, organized by service schema.

**Rationale:** Maintains one migration authority and avoids drift across repos.

### ADR-M9: Backward-compatible rollout
**Decision:** Microservices are introduced in phases with gateway facade preserving stable client contracts.

**Rationale:** Allows incremental extraction without breaking Flutter flows.

## 4) Runtime Interaction Model

### Synchronous path (request/response)
1. Flutter calls `api-gateway`.
2. Gateway validates token and routes request.
3. Target service executes business logic in its owned schema.
4. Gateway returns normalized error envelope to client.

### Asynchronous path (domain events)
1. Service writes business transaction + outbox event atomically.
2. Poller publishes event to subscribed service endpoint/queue table.
3. Consumer records idempotency key and processes exactly-once effect.
4. `notification-service` sends email and retries with exponential backoff up to 24h.

## 5) Quality and Security Guardrails

- All protected routes require JWT and user scoping (`user_id`) checks.
- Structured JSON error format enforced at gateway and services.
- Security headers and TLS termination at Nginx.
- Secrets only via environment variables or secret files mounted by Compose.
- Audit events are append-only in `pipeline_svc.application_events`.
- Daily PostgreSQL backups with 7-day retention managed by `job-tracker-infra`.

## 6) Observability Minimum

- Correlation ID generated at gateway and passed downstream.
- JSON logs in all services with `service`, `trace_id`, `user_id` (when present), and `event_id`.
- Health endpoints: `/actuator/health` per service.
- Basic uptime/error dashboard from container logs and health probes.

## 7) Phased Rollout Plan

### Phase 0: Contract-first prep (planning + shared-libs)
- Freeze OpenAPI baseline per service in `job-tracker-shared-libs`.
- Define event schemas and idempotency conventions.
- Create `STORY-MAP.md` in planning repo with repo tags.

### Phase 1: Edge + identity extraction
- Stand up `api-gateway` and `user-service` first.
- Keep job/pipeline logic in transitional service if needed.
- Route auth/account flows through gateway.

### Phase 2: Domain extraction
- Extract `job-service` and `pipeline-service` with clear ownership.
- Introduce outbox-driven events for status changes.
- Verify no cross-schema writes.

### Phase 3: Notification extraction and hardening
- Move all email logic into `notification-service`.
- Enable retry/backoff and stale reminder scheduling.
- Add contract compatibility checks in CI.

## 8) Exit Criteria to Start Story Generation (`*sm`)

- Service boundaries approved (`service-boundaries.md`).
- Repo map approved (`repo-map.md`).
- API/event catalog approved (`api-contract-index.md`).
- Story IDs tagged with target repo and dependency order.

