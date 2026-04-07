# Story E006-S003: Implement notification retry/backoff and delivery log

Status: ready-for-dev

## Story

As a platform developer,
I want robust retry and delivery logging for email sends,
so that transient failures do not silently drop notifications.

## Acceptance Criteria

1. Failed sends are retried with exponential backoff up to configured max window.
2. Delivery log tracks attempts, status, timestamps, and failure reasons.
3. Retry scheduling is idempotent and prevents duplicate concurrent sends.
4. Terminal failure state is recorded when retry budget is exhausted.
5. Operational metrics/log fields support troubleshooting.
6. Tests cover retry success, retry exhaustion, and duplicate-event handling.

## Tasks / Subtasks

- [ ] Implement retry policy config and scheduler/executor
- [ ] Implement persistent delivery attempt log model
- [ ] Implement idempotency key checks for send jobs
- [ ] Add telemetry/logging for retry lifecycle events
- [ ] Add tests for backoff behavior and max-attempt handling
- [ ] Document retry tuning environment variables

## Dev Notes

- Keep retry policy configurable by environment.
- Avoid tight retry loops; enforce jitter/backoff interval.
- Preserve full audit trail for each send lifecycle.

### Project Structure Notes

- Target repo: `job-tracker-notification-service`
- Depends on: `E006-S002`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`
- `_bmad-output/planning-artifacts/epics.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E006-S003-implement-notification-retry-backoff-delivery-log.md`

