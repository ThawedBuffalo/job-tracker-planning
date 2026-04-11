# Story E009-S002: Pipeline metrics (conversion rate, time-to-offer)

Status: done

## Story

As a user,
I want pipeline performance metrics on my dashboard,
so that I can understand progress quality and where to improve.

## Acceptance Criteria

1. Dashboard displays at least these metrics: conversion rate and time-to-offer.
2. Conversion rate is defined/documented and rendered with a stable percentage format.
3. Time-to-offer is defined/documented and rendered in days.
4. Metrics render correctly for loading, populated, and no-data states.
5. Existing dashboard list/search/filter behavior remains unchanged.
6. Widget tests validate metric rendering logic and no-data handling.

## Tasks / Subtasks

- [ ] Define metric formulas for v1 dashboard usage
- [ ] Add metric view-model fields and formatting helpers
- [ ] Render metric cards in dashboard layout with accessible labels
- [ ] Add no-data fallback text for metric values
- [ ] Ensure existing dashboard interactions still work
- [ ] Add/extend widget tests for metrics success + no-data paths

## Dev Notes

- Keep this story UI-first and avoid introducing contract changes unless required.
- If backend aggregate metric endpoint is unavailable, compute from current loaded items with clear caveats.
- Preserve existing key names and interactions added in E009-S001 unless intentionally versioned.

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Target story path: `_bmad/stories/E009-S002-pipeline-metrics-conversion-time-to-offer.md`
- Depends on: `E009-S001`

### References

- `WAVE2.md` (E009-S002)
- `_bmad-output/implementation-artifacts/sprint-status.yaml`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E009-S002-pipeline-metrics-conversion-time-to-offer.md`
