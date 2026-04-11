# Story E004-S002: Implement pipeline transition validation and append-only events

Status: done

## Story

As a user,
I want valid stage transitions with immutable history,
so that pipeline progression is trustworthy and auditable.

## Acceptance Criteria

1. `pipeline-service` implements `POST /applications/{id}/transition` per contract.
2. Transition rules reject invalid/disallowed status changes with structured errors.
3. Every accepted transition writes an immutable timeline event.
4. Transition response includes new status, event timestamp, and next-step prompt.
5. `ApplicationStatusChanged` event is published via outbox-compatible flow.
6. Tests cover valid transitions, invalid transitions, and idempotency/duplicate handling.

## Tasks / Subtasks

- [ ] Implement transition command endpoint and service logic
- [ ] Implement status transition validator and policy matrix
- [ ] Persist append-only events in pipeline schema
- [ ] Emit `ApplicationStatusChanged` event with idempotency key
- [ ] Add error mapping for invalid transition and not-found cases
- [ ] Add unit/integration tests for command/event behavior

## Dev Notes

- Pipeline service owns transition rules and timeline persistence.
- Do not mutate historical events once written.
- Ensure ownership checks are enforced before transition.

### Project Structure Notes

- Target repo: `job-tracker-pipeline-service`
- Depends on: `E004-S001`, `E002-S002`, `E008-S003`

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

- `_bmad-output/implementation-artifacts/E004-S002-implement-pipeline-transition-validation-events.md`
