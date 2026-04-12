# Story E015-S003: Transition UI + notifications

Status: done

## Story

As an **authenticated mobile user**,
I want to transition application statuses from the mobile detail flow and receive immediate feedback,
So that I can keep my pipeline up to date while on the go.

## Acceptance Criteria

1. Detail view exposes status transition actions based on allowed workflow transitions.
2. Transition rules are enforced (invalid status jumps are blocked).
3. Successful transitions update local dashboard state immediately (list + detail).
4. Transition outcomes show clear in-app notifications.
5. Terminal statuses show no transition actions and communicate final-state behavior.
6. Existing list/detail/search/filter/export/recommendation behavior remains intact.
7. `flutter analyze` and dashboard widget/controller tests pass.

## Technical Notes

- Added transition matrix helpers to `ApplicationStatus` in `models.dart`.
- Added `DashboardController.transitionStatus(...)` with deterministic results:
  - `success`
  - `noChange`
  - `invalid`
  - `notFound`
- Updated `ApplicationDetailPlaceholder` to listen to controller updates so status changes render immediately.
- Added `_StatusTransitionSection` with action chips and SnackBar notifications.
- Extended `dashboard_test.dart` with an E015-S003 group and adjusted schedule tests for new scroll depth.

## Validation

- `flutter analyze` -> pass
- `flutter test test/dashboard_test.dart` -> pass

## Definition of Done

- Acceptance criteria pass
- `flutter analyze` passes
- Story tests pass
- Story status set to `done`

