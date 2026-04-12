# Story E013-S004: Two-factor authentication (2FA)

Status: done

## Story

As an **authenticated user**,
I want to enable two-factor authentication for login,
So that account access requires both password and a second verification factor.

## Acceptance Criteria

1. User-service supports storing and reading a per-user `two_factor_enabled` setting.
2. Authenticated endpoints exist to read/update a user's 2FA setting.
3. Login for 2FA-enabled users returns a challenge response instead of immediate tokens.
4. A dedicated 2FA verification endpoint exchanges a valid challenge/code for normal auth tokens.
5. Invalid/expired/malformed 2FA verification attempts return a standard error envelope.
6. Existing non-2FA login behavior remains unchanged for users without 2FA enabled.
7. Integration tests cover enablement, challenge flow, invalid code, and successful verification.

## Technical Notes

- Scope is user-service implementation with dev-friendly challenge code configuration.
- Challenge state is in-process for this slice.
- Stronger TOTP secret provisioning and backup codes can follow in a later story.

## Dependencies

- E013-S001 — done
- E013-S002 — done
- E013-S003 — done

## Definition of Done

- Acceptance criteria pass
- `npm test` (or targeted integration tests) passes
- Story status reflects progress accurately


