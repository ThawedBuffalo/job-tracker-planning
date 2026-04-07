# Story E006-S004: Implement stalled-application event producer and reminder loop

Status: ready-for-dev

## Story

As a user,
I want reminders when applications stall,
so that I can follow up before opportunities go cold.

## Acceptance Criteria

1. Pipeline logic detects stalled applications per configured inactivity threshold.
2. `ApplicationStalled` events are emitted for eligible non-terminal applications.
3. Notification service consumes stalled events and sends reminder emails.
4. Reminder cadence prevents duplicate reminders within the configured window.
5. User preference opt-outs are respected for stalled reminders.
6. Tests cover detection, event emission, and reminder dispatch behavior.

## Tasks / Subtasks

- [ ] Implement stalled detection query/job in pipeline service
- [ ] Emit `ApplicationStalled` events with required metadata
- [ ] Implement reminder handling path in notification service
- [ ] Add dedupe/cadence controls for recurring reminders
- [ ] Add tests for threshold and terminal-state exclusions
- [ ] Document scheduler configuration and operational notes

## Dev Notes

- Keep detection logic in pipeline boundary; notification only consumes events.
- Ensure timezone handling is UTC-consistent for threshold calculations.
- Use explicit terminal status guardrails (`rejected`, `closed`).

### Project Structure Notes

- Target repo: `job-tracker-pipeline-service`
- Depends on: `E004-S002`, `E006-S002`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/service-boundaries.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E006-S004-implement-stalled-application-reminder-loop.md`

