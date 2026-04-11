# Story E009-S003: Recommendation engine (follow-up suggestions)

Status: done

## Story

As a user,
I want clear follow-up recommendations on my dashboard,
so that I can take the next best action for each application.

## Acceptance Criteria

1. Dashboard surfaces recommendation suggestions derived from current application state.
2. Recommendations include at least one actionable suggestion for stalled or inactive applications.
3. Recommendation text is deterministic and stable for the same input data.
4. No-data and error states show safe fallback messaging without breaking existing layout.
5. Existing dashboard list/search/filter/metric behavior remains unchanged.
6. Widget tests validate recommendation rendering and fallback behavior.

## Tasks / Subtasks

- [ ] Define v1 recommendation rules based on status and recency
- [ ] Add recommendation model/formatter helpers in dashboard layer
- [ ] Render recommendation section/cards with accessible labels
- [ ] Add fallback copy for no recommendations available
- [ ] Preserve existing dashboard interactions and metric cards
- [ ] Add/extend widget tests for recommendation success + fallback

## Dev Notes

- Keep this story UI-first and rule-based; avoid introducing ML or backend contracts in v1.
- Reuse existing `ApplicationSummary` fields (`status`, `lastStatusChangedAt`) for heuristics.
- Keep recommendations short and action-oriented (for example: follow up, prep interview, negotiate offer).

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Target story path: `_bmad/stories/E009-S003-recommendation-engine-follow-up-suggestions.md`
- Depends on: `E009-S002`

### References

- `WAVE2.md` (E009-S003)
- `_bmad-output/implementation-artifacts/sprint-status.yaml`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E009-S003-recommendation-engine-follow-up-suggestions.md`
