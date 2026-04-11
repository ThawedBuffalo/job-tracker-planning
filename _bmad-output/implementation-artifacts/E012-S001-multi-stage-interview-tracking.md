# Story E012-S001: Multi-stage interview tracking

Status: done

## Story

As an **authenticated user**,
I want to track which stage of the interview process each application is in (phone screen → technical screen → onsite → final round),
So that I can see my interview pipeline at a glance and know exactly where each conversation stands.

## Acceptance Criteria

1. **Interview sub-stages defined** — An `InterviewStage` enum exists with values: `phone_screen`, `technical_screen`, `onsite`, `final_round`. Each has a display label and an ordered sequence index.
2. **ApplicationSummary carries interview stage** — `ApplicationSummary` has an optional `interviewStage` field (nullable; only set when `status == interview`).
3. **fromJson parses interview_stage** — `ApplicationSummary.fromJson` correctly parses `"interview_stage"` from JSON (null-safe).
4. **Dashboard interview cards show stage** — When an application has `status == interview`, the dashboard card shows the sub-stage chip below the status chip (e.g. "Phone Screen").
5. **Interview stage progression UI** — A horizontal stepper widget (`InterviewStagestepper`) renders the 4 stages in order; the current stage is highlighted; completed stages show a check.
6. **Stage advancement** — The stepper lets users advance to the next stage (button disabled on final round). Advancement triggers an `onStageAdvanced(InterviewStage)` callback (controller handles persistence).
7. **Controller exposes stage update** — `DashboardController.setInterviewStage(String applicationId, InterviewStage stage)` updates the in-memory application list so the UI reflects the change without a full reload.
8. **Empty state not regressed** — Existing empty, loading, and error states continue to pass all existing tests.
9. **Widget tests pass** — New tests cover: stage chip renders, stepper advances, controller update propagates to dashboard card.
10. **No static-analysis issues** — `flutter analyze` returns no issues.

## Technical Notes

- `InterviewStage` is client-side only for v1; the field is read from the API JSON when present and written back via a PATCH call (stub — no real network call in tests).
- `InterviewStageStepper` is a stateless widget driven by the current stage value.
- Stage ordering: `phone_screen(0)` → `technical_screen(1)` → `onsite(2)` → `final_round(3)`.
- Keep existing `ApplicationStatus.interview` unchanged; `interviewStage` is additive.
- Target file changes: `models.dart`, `dashboard_controller.dart`, `dashboard_views.dart`, `dashboard_test.dart`.

## Dependencies

- E009-S001 (dashboard layout) — done ✅
- E009-S003 (recommendations) — done ✅

## Definition of Done

- All 10 acceptance criteria satisfied
- `flutter test` — all tests pass
- `flutter analyze` — no issues
- Story status set to `done`

