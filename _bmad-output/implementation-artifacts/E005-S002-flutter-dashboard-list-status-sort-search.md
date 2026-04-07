# Story E005-S002: Flutter dashboard list with status, sort, and search

Status: ready-for-dev

## Story

As a user,
I want a dashboard list with filtering and search,
so that I can quickly find and prioritize applications.

## Acceptance Criteria

1. Dashboard fetches data from `/api/v1/dashboard` and/or `/api/v1/applications` per gateway contract.
2. UI supports status filtering, search by title/company, and default recency sort.
3. Stalled applications are visually distinguishable.
4. Empty, loading, and error states are implemented and accessible.
5. User can navigate from list row to application detail flow.
6. Widget/integration tests cover filter/search/sort behavior.

## Tasks / Subtasks

- [ ] Build dashboard data models and API integration layer
- [ ] Implement list UI with filter/search controls
- [ ] Implement default sort and stalled-state indicators
- [ ] Implement loading/empty/error UX states
- [ ] Add navigation hooks to detail/timeline views
- [ ] Add tests for list rendering and interactions

## Dev Notes

- Keep list state reactive and debounce search requests.
- Use contract enums/fields directly to reduce mapping drift.
- Ensure responsive behavior for desktop and mobile widths.

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Depends on: `E005-S001`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/epics.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E005-S002-flutter-dashboard-list-status-sort-search.md`

