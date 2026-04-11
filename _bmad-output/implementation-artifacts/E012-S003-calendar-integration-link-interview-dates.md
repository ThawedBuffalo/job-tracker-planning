# Story E012-S003: Calendar integration (link interview dates)

Status: done

## Story

As an **authenticated user**,
I want to attach interview date/time details to an application and open them in my calendar,
So that I do not miss scheduled phone screens, technical interviews, or onsite rounds.

## Acceptance Criteria

1. `InterviewSchedule` model exists with `applicationId`, `title`, `startsAt`, `endsAt`, `location`, and `notes` fields.
2. Application detail view includes an `Interview Schedule` section with date/time input controls.
3. User can save a valid schedule entry (start < end) for an application.
4. Validation blocks invalid entries (empty title, missing start/end, end before/equal start).
5. Saved schedule is shown immediately in detail view without full dashboard refresh.
6. `Add to Calendar` action is present and generates a calendar-launch payload/URI for the saved schedule.
7. Existing dashboard list/search/filter/export/recommendations states are not regressed.
8. Widget tests cover section render, validation, save, and add-to-calendar action visibility.
9. `flutter analyze` reports no issues.

## Technical Notes

- Scope is Flutter-first scheduling UX with controller-managed in-memory state.
- External calendar provider wiring can stay behind an interface for follow-up work.
- Keep current detail and dashboard interaction patterns stable.

## Dependencies

- E012-S001 — done
- E012-S002 — done

## Definition of Done

- All acceptance criteria pass
- `flutter test` passes
- `flutter analyze` passes
- Story status set to `done`
