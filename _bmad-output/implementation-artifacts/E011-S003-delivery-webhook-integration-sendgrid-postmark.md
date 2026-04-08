# Story E011-S003: Delivery webhook integration (SendGrid, Postmark)

Status: done

## Story

As a platform engineer,
I want notification-service to ingest provider delivery webhooks,
so that delivery outcomes are reconciled into internal notification status records.

## Acceptance Criteria

1. `notification-service` exposes provider webhook endpoints for SendGrid and Postmark.
2. Webhook endpoints verify request signature via shared secret.
3. Valid delivery/bounce events reconcile to internal records by `provider_message_id`.
4. Invalid signatures are rejected (`401`) without side effects.
5. Duplicate/replayed webhook events are safe and idempotent.
6. Integration tests cover success and signature failure flows.
7. Infra/docs include webhook env vars and routing notes.

## Tasks / Subtasks

- [x] Add webhook config (`SENDGRID_WEBHOOK_ENABLED`, `SENDGRID_WEBHOOK_SECRET`, `POSTMARK_WEBHOOK_ENABLED`, `POSTMARK_WEBHOOK_SECRET`)
- [x] Add provider webhook endpoints in notification-service app
- [x] Add provider event normalization and signature verification helper
- [x] Add provider message id lookup and reconciliation helper in store
- [x] Add/extend integration tests for webhook success/failure
- [x] Update infra/docs for webhook env vars

## Dev Agent Record

### Agent Model Used

GitHub Copilot (GPT-4.1), 2026-04-07

### Debug Log References

### Completion Notes List

- SendGrid/Postmark webhook endpoints added and signature-validated.
- Provider outcomes reconciled by `provider_message_id` in store.
- Added replay-safety and invalid-signature no-side-effect test coverage.
- Notification integration suite passes (`24/24`).

### File List

- `job-tracker-notification-service/src/webhook.js`
- `job-tracker-notification-service/src/app.js`
- `job-tracker-notification-service/src/config.js`
- `job-tracker-notification-service/src/store.js`
- `job-tracker-notification-service/test/notification.int.test.js`
- `job-tracker-notification-service/README.md`
- `job-tracker-infra/.env.example`
- `job-tracker-infra/docker-compose.yml`
- `job-tracker-infra/README.md`

