# Story E011-S001: Message queue integration for notification dispatch

Status: done

## Story

As a platform engineer,
I want notification dispatch to run through a durable queue,
so that API/pipeline request paths are not blocked by SMTP latency and notification jobs survive worker restarts.

## Acceptance Criteria

1. `notification-service` enqueues dispatch jobs and does not perform SMTP send inline on request path.
2. A worker process consumes queued jobs and executes existing preference checks plus retry/backoff logic.
3. Queue payload preserves `event_id`, `correlation_id`, and enough context for idempotent processing.
4. Duplicate/replayed jobs are suppressed via existing correlation/idempotency behavior.
5. Redis queue failure is handled safely (service-level fail-open where applicable; retries/logging retained).
6. Integration tests cover producer -> queue -> worker -> delivery outcome (`sent`/`failed`).
7. Infra/docs define required queue env vars and runtime topology.

## Tasks / Subtasks

- [x] Add queue configuration (`QUEUE_ENABLED`, Redis URL/DB, queue name, worker concurrency)
- [x] Add queue producer path in `notification-service` for incoming notification events
- [x] Add queue worker module in `notification-service` and integrate retry/policy path
- [x] Wire `pipeline-service` producers for status-change and stalled events to queued dispatch contract
- [x] Ensure duplicate suppression on `event_id`/`correlation_id`
- [x] Add/extend integration tests for enqueue, success, retry, and duplicate replay
- [x] Update infra/docs for queue runtime and ops notes

## Dev Notes

- Queue backend target: Redis (BullMQ-compatible pattern) to reuse existing Wave 2 Redis infrastructure.
- Preserve existing event contracts from v1 (`ApplicationStatusChanged`, `ApplicationStalled`, `PreferenceUpdated`).
- Keep SMTP transport isolated; queue worker orchestrates retries and delivery logging.

### Project Structure Notes

- Target repos: `job-tracker-notification-service`, `job-tracker-pipeline-service`, `job-tracker-infra`
- Depends on: `E010-S001`, `E010-S002`, `E006-S003`
- Blocks: `E011-S002`, `E011-S003`

### References

- `WAVE2.md`
- `job-tracker-planning/_bmad-output/implementation-artifacts/sprint-status.yaml`
- `job-tracker-notification-service/.bmad/stories/E006-S003-implement-notification-retry-backoff-delivery-log.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `job-tracker-planning/_bmad-output/implementation-artifacts/E011-S001-message-queue-integration-notification-dispatch.md`

