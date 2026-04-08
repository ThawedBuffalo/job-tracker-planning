# Story E011-S004: Notification digest (daily/weekly email summaries)

Status: done

## Story

As a platform engineer,
I want notification-service to generate periodic digest emails,
so that users receive a concise daily/weekly summary of their recent notification activity.

## Acceptance Criteria

1. `notification-service` supports digest generation for `daily` and `weekly` frequencies.
2. Digest generation respects user preference (`digest_enabled`, `digest_frequency`) and email channel enablement.
3. Digest emails summarize sent notification activity in the cadence window.
4. Repeated runs inside cadence window do not emit duplicate digest emails.
5. Digest runs are exposed via an authenticated internal endpoint for scheduler/manual execution.
6. Integration tests cover successful digest generation, validation errors, preference opt-out, and cadence behavior.
7. Infra/docs include digest-related environment variables.

## Tasks / Subtasks

- [x] Add digest config (`NOTIFY_DIGEST_DAILY_CADENCE_MS`, `NOTIFY_DIGEST_WEEKLY_CADENCE_MS`)
- [x] Add digest batch module for daily/weekly generation
- [x] Add internal run endpoint (`POST /notifications/digests/run`)
- [x] Respect preferences and cadence cursor to prevent duplicate digest sends
- [x] Add digest integration tests
- [x] Update infra/docs

## Dev Agent Record

### Agent Model Used

GitHub Copilot (GPT-4.1), 2026-04-08

### Debug Log References

### Completion Notes List

- Added daily/weekly digest generation module with cadence windows and user preference checks.
- Added authenticated digest run endpoint (`POST /notifications/digests/run`).
- Added digest integration tests for success, validation, opt-out, and cadence duplicate suppression.
- Notification service integration suite passes (`28/28`).
- Infra env + compose + docs updated for digest cadence variables.

### File List

- `job-tracker-notification-service/src/digest.js`
- `job-tracker-notification-service/src/app.js`
- `job-tracker-notification-service/src/store.js`
- `job-tracker-notification-service/src/config.js`
- `job-tracker-notification-service/src/mailer.js`
- `job-tracker-notification-service/test/notification.int.test.js`
- `job-tracker-notification-service/README.md`
- `job-tracker-infra/.env.example`
- `job-tracker-infra/docker-compose.yml`
- `job-tracker-infra/README.md`

