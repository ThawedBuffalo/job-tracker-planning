# Story E015-S004: Offline mode + sync

Status: done

## Story

As an **authenticated mobile user**,
I want to keep using the dashboard while offline and have changes sync automatically when connectivity returns,
So that progress is not blocked by temporary network issues.

## Acceptance Criteria

1. Dashboard remains usable with cached in-memory data when refresh fails due to offline/server-unavailable conditions.
2. Status transitions performed while offline are queued instead of lost.
3. Queued transitions sync automatically when connectivity returns.
4. UI communicates offline/sync state and pending change count.
5. Manual sync retry is available.
6. Existing dashboard and detail behavior is not regressed.
7. `flutter analyze` and relevant Flutter tests pass.

## Technical Notes

- Extended `DashboardApi` with `transitionApplicationStatus(...)` and implemented it in `HttpDashboardApi` using:
  - `POST /api/v1/applications/{applicationId}/transition`
- Added offline/sync state to `DashboardController`:
  - `isOfflineMode`, `isSyncing`, `pendingSyncCount`, `syncErrorMessage`, `lastSyncedAt`
  - queue-backed transition sync replay (`_syncPendingTransitions`)
  - `retrySync()` for manual recovery
- `transitionStatus(...)` now performs optimistic local update and:
  - returns `queued` when offline
  - returns `success` when online with background sync replay
- Added dashboard sync banner in `dashboard_views.dart`:
  - key: `syncStatusBanner`
  - message key: `syncStatusMessage`
  - retry action key: `syncRetryButton`
- Transition SnackBar now includes offline-queued feedback.
- Updated test fakes to implement the expanded `DashboardApi` interface.

## Validation

- `flutter analyze` -> pass
- `flutter test test/dashboard_test.dart` -> pass
- `flutter test test/widget_test.dart` -> pass

## Definition of Done

- Acceptance criteria pass
- Offline queue + sync behavior covered by tests
- `flutter analyze` passes
- Story status set to `done`

