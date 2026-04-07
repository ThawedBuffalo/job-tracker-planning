# Story E006-S002: Implement notification-service event consumers and SMTP sender

Status: ready-for-dev

## Story

As a user,
I want notification emails when key application events happen,
so that I do not miss important follow-ups.

## Acceptance Criteria

1. `notification-service` consumes `ApplicationStatusChanged` and `PreferenceUpdated` events.
2. Service applies user notification preference checks before sending.
3. SMTP sender dispatches templated emails with required context fields.
4. Success/failure outcomes are recorded with correlation/event identifiers.
5. Errors are retried according to policy hooks (full policy in `E006-S003`).
6. Integration tests cover consumer-to-send flow.

## Tasks / Subtasks

- [ ] Implement event consumer handlers and payload validation
- [ ] Implement preference lookup/decision logic
- [ ] Implement SMTP adapter and templated message builder
- [ ] Persist send attempt records and delivery outcomes
- [ ] Add integration tests for status-change notification flow
- [ ] Document required env vars for SMTP configuration

## Dev Notes

- Keep provider-specific logic isolated behind adapter interface.
- Use idempotency keys to avoid duplicate sends for reprocessed events.
- Respect opt-out defaults and per-type preferences.

### Project Structure Notes

- Target repo: `job-tracker-notification-service`
- Depends on: `E006-S001`, `E004-S002`, `E001-S002`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E006-S002-implement-notification-consumers-smtp-sender.md`

