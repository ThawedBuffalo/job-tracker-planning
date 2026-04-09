# Story E001-S004: Flutter auth flow integration (register/login/logout)

Status: done

## Story

As an end user,
I want Flutter auth screens integrated with gateway auth APIs,
so that I can sign up, sign in, and sign out reliably.

## Acceptance Criteria

1. Flutter app integrates register/login/logout with `/api/v1/auth/*` endpoints.
2. Access token/session state is handled consistently for protected screens.
3. Auth failures display user-friendly messages mapped from `ErrorEnvelope`.
4. Successful login redirects to the dashboard entry flow.
5. Logout clears local auth state and returns to public route.
6. Widget/integration tests cover auth happy path and common failures.

## Tasks / Subtasks

- [x] Implement auth API client methods from pinned contract version
- [x] Build/update register/login forms with validation and submit states
- [x] Implement auth state management and route guards
- [x] Implement logout action and state reset
- [x] Map backend error codes to UI messages
- [x] Add widget/integration tests for critical auth flows

## Dev Notes

- Keep token handling consistent with gateway/user-service contract.
- Do not hardcode endpoint URLs; use environment/config abstraction.
- Reuse shared error handling patterns for future features.

### Project Structure Notes

- Target repo: `job-tracker-flutter`
- Depends on: `E001-S003`

### References

- `_bmad-output/planning-artifacts/STORY-MAP.md`
- `_bmad-output/planning-artifacts/api-contract-index.md`
- `_bmad-output/planning-artifacts/repo-map.md`

## Dev Agent Record

### Agent Model Used

GitHub Copilot (April 6, 2026)

### Debug Log References

- `flutter create --project-name job_tracker_flutter --platforms=web .`
- `flutter analyze`
- `flutter test`

### Completion Notes List

- Bootstrapped the missing Flutter app scaffold in `job-tracker-flutter` because the repo previously contained planning artifacts only.
- Added a gateway-backed auth API client with environment-based `API_BASE_URL` configuration.
- Implemented register/login/logout UI, in-memory auth state, protected dashboard entry flow, and logout reset behavior.
- Mapped backend `ErrorEnvelope` codes to user-friendly UI copy.
- Added widget tests covering unauthenticated guard, login success, login failure, register success, and logout.
- Verified the implementation with `flutter analyze` and `flutter test`.

### File List

- `job-tracker-flutter/pubspec.yaml`
- `job-tracker-flutter/lib/main.dart`
- `job-tracker-flutter/lib/src/app.dart`
- `job-tracker-flutter/lib/src/config/app_config.dart`
- `job-tracker-flutter/lib/src/auth/auth_api.dart`
- `job-tracker-flutter/lib/src/auth/auth_controller.dart`
- `job-tracker-flutter/lib/src/auth/auth_views.dart`
- `job-tracker-flutter/lib/src/auth/models.dart`
- `job-tracker-flutter/test/widget_test.dart`
- `job-tracker-flutter/README.md`
- `_bmad-output/implementation-artifacts/E001-S004-flutter-auth-flow-integration.md`

