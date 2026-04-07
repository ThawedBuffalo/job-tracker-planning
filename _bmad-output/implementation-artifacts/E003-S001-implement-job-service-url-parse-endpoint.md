# Story E003-S001: Implement URL parse endpoint in job-service

Status: ready-for-dev

## Story

As a user,
I want to paste a job URL and receive prefilled fields,
so that data entry is faster.

## Acceptance Criteria

1. `job-service` exposes URL parse endpoint per contract (`/applications/parse-url` or agreed equivalent).
2. Endpoint extracts at minimum title, company, and source URL when available.
3. Partial parse success is supported and returns available fields without failing entire request.
4. Invalid/unreachable URLs return clear structured errors.
5. Parser execution meets baseline response target from planning docs.
6. Tests cover common job board pages, partial failures, and invalid URLs.

## Tasks / Subtasks

- [ ] Implement URL parse handler using configured parser library
- [ ] Add parsing strategies/selectors and fallback logic
- [ ] Implement timeout and error handling behavior
- [ ] Map parse results to contract response schema
- [ ] Add test fixtures for representative source pages
- [ ] Document known parser limitations and extension points

## Dev Notes

- Keep parser deterministic and safe; avoid executing remote scripts.
- Preserve raw input URL in response for traceability.
- This endpoint should not persist application records directly.

### Project Structure Notes

- Target repo: `job-tracker-job-service`
- Depends on: `E002-S002`

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

- `_bmad-output/implementation-artifacts/E003-S001-implement-job-service-url-parse-endpoint.md`

