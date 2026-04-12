# Story E015-S002: Application list + detail views

Status: done

## Story

As an **authenticated mobile user**,
I want to browse my applications and open a detail view for each,
So that I can quickly review status and context while on the go.

## Acceptance Criteria

1. Authenticated state renders an application list sourced from dashboard API data.
2. List supports search/filter behavior consistent with current dashboard patterns.
3. Tapping a list item opens an application detail view.
4. Detail view presents core metadata and retains feature sections (interview schedule, integrations, notes/comments).
5. Existing dashboard list/search/filter/export/recommendation behavior is not regressed.
6. Widget tests cover list rendering, search narrowing, navigation to detail, and section availability.
7. `flutter analyze` and `flutter test` pass.

## Technical Notes

- Scope is Flutter UI/controller behavior over existing dashboard API contract.
- Detail view remains aligned with current placeholder/section composition for this story.
- Future stories can evolve API-backed deep detail/timeline without breaking list navigation.

## Dependencies

- E015-S001 — done
- E012-S001..S004 dashboard/feature baseline — done

## Definition of Done

- Acceptance criteria pass
- `flutter analyze` passes
- `flutter test` passes
- Story status set to `done`

