# Story E012-S002: Notes & comments per application (collaboration)

Status: done

## Story

As an **authenticated user**,
I want to add notes and comment threads on each application,
So that I can keep interview context, follow-up details, and collaboration history in one place.

## Acceptance Criteria

1. `ApplicationComment` model exists with `id`, `applicationId`, `authorLabel`, `message`, and `createdAt` fields.
2. Dashboard detail view shows a `Notes & Comments` section for the selected application.
3. User can submit a non-empty comment from the UI; empty/whitespace-only submission is blocked.
4. Newly submitted comments render immediately in the thread without requiring full page refresh.
5. Thread renders newest comment first with timestamp and author label.
6. Existing dashboard list/search/filter/loading/error/empty states are not regressed.
7. Widget tests cover: section render, comment submit, empty comment validation, ordering.
8. `flutter analyze` reports no issues.

## Technical Notes

- Scope for this story is Flutter-side collaboration UX with in-memory persistence in controller.
- API persistence can be wired in a later story; keep controller API composable.
- Keep all current dashboard behaviors and test contracts intact.

## Dependencies

- E012-S001 — done

## Definition of Done

- All acceptance criteria pass
- `flutter test` passes
- `flutter analyze` passes
- Story status set to `done`
