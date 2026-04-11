# Story E009-S004: Export to CSV / PDF reports

Status: review

## Story

As a user,
I want to export my dashboard data as CSV and PDF,
so that I can review or share my job-search progress offline.

## Acceptance Criteria

1. Dashboard provides an export action for CSV and PDF report generation.
2. CSV export includes current dashboard application data with stable column ordering.
3. PDF export includes at minimum summary metrics and visible recommendations in a readable layout.
4. Export behavior handles empty-data states safely with clear user feedback.
5. Existing dashboard list/search/filter/metric/recommendation behavior remains unchanged.
6. Widget or unit tests validate export formatting logic and export-action state handling.

## Tasks / Subtasks

- [x] Define v1 export payload/column set for CSV and PDF content sections
- [x] Add dashboard export helpers/builders for CSV and printable report content
- [x] Add export action UI to dashboard with accessible labels
- [x] Handle empty-data export attempts with clear fallback messaging
- [x] Preserve existing dashboard interactions and layout behavior
- [x] Add/extend tests for export formatting and empty-data handling

## Dev Notes

- Keep v1 export client-side from currently loaded dashboard data; avoid backend contract changes.
- Prefer simple, deterministic exports over advanced styling.
- If full native file save/share integration is unavailable, generate content and stage it behind testable helpers first.

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Target story path: `_bmad/stories/E009-S004-export-csv-pdf-reports.md`
- Depends on: `E009-S003`

### References

- `WAVE2.md` (E009-S004)
- `_bmad-output/implementation-artifacts/sprint-status.yaml`
- `_bmad-output/planning-artifacts/architecture-polyrepo.md`

## Dev Agent Record

### Agent Model Used

GitHub Copilot

### Debug Log References

- `flutter analyze`
- `flutter test test/dashboard_test.dart`
- `flutter test`

### Completion Notes List

- Added deterministic client-side CSV export formatting for dashboard application data.
- Added readable PDF-style report generation including summary metrics, recommendations, and applications.
- Added dashboard export actions with safe empty-state feedback and preview dialogs.
- Extended widget coverage for CSV preview, PDF preview, and no-data export behavior.

### File List

- `lib/src/dashboard/export_formatter.dart`
- `lib/src/dashboard/dashboard_controller.dart`
- `lib/src/dashboard/dashboard_views.dart`
- `test/dashboard_test.dart`
- `_bmad-output/implementation-artifacts/E009-S004-export-csv-pdf-reports.md`
