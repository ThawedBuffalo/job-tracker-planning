# Story E006-S001: Define Notification API/Event Schemas v1

Status: ready-for-dev

## Story

As a platform developer,
I want `openapi/notification-service.v1.yaml` and notification event schemas,
so that notification delivery behavior is contract-defined before service implementation.

## Acceptance Criteria

1. `openapi/notification-service.v1.yaml` exists with v1 metadata and internal/admin endpoints documented.
2. Event schemas exist in `events/notification-events.v1.json` and include `NotificationRequested`, `NotificationSent`, and `NotificationFailed`.
3. Event envelope fields include `event_id`, `event_type`, `occurred_at`, `schema_version`, and correlation metadata.
4. Contract defines SMTP delivery outcome payload fields (`recipient`, `template`, `provider_message_id`, `attempt_count`, `failure_reason`).
5. Error responses follow normalized `ErrorEnvelope`.
6. OpenAPI and schema validation checks pass in CI.

## Tasks / Subtasks

- [ ] Author `openapi/notification-service.v1.yaml` with tags, operation IDs, and response codes
- [ ] Author `events/notification-events.v1.json` with required event definitions
- [ ] Add JSON schema examples for success/failure payloads
- [ ] Document versioning rules and backward compatibility expectations
- [ ] Ensure lint and compatibility checks are wired in CI
- [ ] Update shared-libs README contract index

## Dev Notes

- Keep this story contract-first only; SMTP implementation is in `E006-S002`.
- Ensure event fields align with outbox and idempotency rules from architecture.
- Avoid coupling notification schemas to database models.

### Project Structure Notes

- Target repo: `job-tracker-shared-libs`
- Primary artifacts: `openapi/notification-service.v1.yaml`, `events/notification-events.v1.json`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`
- `_bmad-output/planning-artifacts/repo-map.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E006-S001-define-notification-api-event-schemas-v1.md`

