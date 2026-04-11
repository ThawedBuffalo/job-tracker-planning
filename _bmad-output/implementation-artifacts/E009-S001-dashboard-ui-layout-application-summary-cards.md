# Story E009-S001: Dashboard UI layout + application summary cards

Status: done

## Story

As a user,
I want a dashboard with clear summary cards and a structured layout,
so that I can quickly understand my job-search pipeline at a glance.

## Acceptance Criteria

1. Flutter app includes a dashboard screen with a stable layout for summary + list sections.
2. Summary cards show at least: total applications, active applications, interview-stage count, and offer count.
3. Dashboard reads from existing v1 gateway/dashboard endpoints already used by Wave 1 features.
4. Loading, empty, and error states are handled with user-friendly messaging.
5. Pull-to-refresh (or explicit refresh action) re-fetches dashboard data.
6. Widget tests cover success, empty, and API-error rendering paths.

## Tasks / Subtasks

- [ ] Define dashboard screen composition and reusable card widgets
- [ ] Add view-model/state wiring for dashboard data fetch
- [ ] Render summary metrics with accessible labels and stable formatting
- [ ] Implement loading/empty/error UI states and retry action
- [ ] Add refresh trigger and state update behavior
- [ ] Add/extend widget tests for the three key render states

## Dev Notes

- Reuse existing auth/session wiring from `E001-S004`.
- Avoid introducing new backend contracts in this story.
- Keep UI responsive for desktop first, but avoid hardcoding window-specific dimensions.

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Target story path: `_bmad/stories/E009-S001-dashboard-ui-layout-application-summary-cards.md`
- Depends on: `E005-S001`, `E005-S002`

### References

- `WAVE2.md` (E009 overview and story list)
- `_bmad-output/implementation-artifacts/sprint-status.yaml`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

To be filled by dev agent.

### Debug Log References

### Completion Notes List

### File List

- `_bmad-output/implementation-artifacts/E009-S001-dashboard-ui-layout-application-summary-cards.md`
