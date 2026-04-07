# Story E004-S003: Implement timeline and next-step guidance query

Status: ready-for-dev

## Story

As a user,
I want to view full status history and guidance for what to do next,
so that I can move each application forward confidently.

## Acceptance Criteria

1. `pipeline-service` implements `GET /applications/{id}/timeline` per contract.
2. Timeline response returns ordered append-only events with event metadata.
3. Response includes current `next_step_prompt` (or equivalent contract field).
4. Endpoint enforces user ownership and token validation.
5. Not-found and forbidden cases return normalized `ErrorEnvelope`.
6. Tests verify ordering, guidance output, and ownership controls.

## Tasks / Subtasks

- [ ] Implement timeline query endpoint and service
- [ ] Implement event ordering and pagination behavior if configured
- [ ] Implement next-step guidance resolver from latest status context
- [ ] Add authorization and ownership checks
- [ ] Add tests for timeline shape and prompt generation
- [ ] Document response semantics for frontend consumers

## Dev Notes

- Guidance logic should be deterministic and tied to status enum.
- Avoid expensive joins; use pipeline-owned data and approved service calls.
- Keep response contract stable for dashboard/detail pages.

### Project Structure Notes

- Target repo: `job-tracker-pipeline-service`
- Depends on: `E004-S002`

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

- `_bmad-output/implementation-artifacts/E004-S003-implement-timeline-next-step-guidance-query.md`

