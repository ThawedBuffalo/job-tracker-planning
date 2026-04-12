# Story E015-S001: Flutter setup + authentication (mobile auth foundation)

Status: done

## Story

As an **authenticated mobile user**,
I want the Flutter app auth flow to support the platform's current login requirements,
So that I can sign in successfully even when two-factor verification is enabled.

## Acceptance Criteria

1. Flutter auth models parse current account metadata including role and `two_factor_enabled`.
2. Flutter auth API supports login responses that either return a session or request second-factor verification.
3. Flutter auth UI renders a dedicated verification step when login requires 2FA.
4. Successful 2FA verification continues into the protected dashboard flow.
5. Invalid 2FA codes display a user-friendly mapped error message.
6. Existing register/login/logout behavior for non-2FA users is not regressed.
7. Widget tests cover the 2FA challenge success/failure flow.
8. `flutter analyze` and `flutter test` pass.

## Technical Notes

- Scope is Flutter-side auth flow adaptation only.
- Challenge state remains in-memory in the controller for this slice.
- Cookie/refresh persistence can be expanded in a later mobile story.

## Dependencies

- E001-S004 — done
- E013-S004 — done

## Definition of Done

- Acceptance criteria pass
- `flutter analyze` passes
- `flutter test` passes
- Story status set to `done`

