# Story E013-S002: Data export + deletion (GDPR right to be forgotten)

Status: done

## Story

As an **authenticated user**,
I want to export my account data and permanently delete my account,
So that I can exercise GDPR data portability and deletion rights.

## Acceptance Criteria

1. Authenticated `GET /privacy/export` endpoint returns current user account profile and session-related data.
2. Authenticated `DELETE /privacy/account` endpoint deletes the current user account and related session records.
3. Export and deletion endpoints require bearer authentication and return standard error envelopes when unauthorized.
4. Deletion invalidates future login/refresh access for the deleted account.
5. User-service audit events include success/failure coverage for export and deletion operations.
6. Existing auth endpoint behavior and contracts are not regressed.
7. Integration tests cover export success/auth guard, deletion behavior, and audit success events.

## Technical Notes

- Scope is user-service local data ownership only (users + sessions).
- Cross-service erasure orchestration can be handled in a follow-up story.
- Reuse existing trace and audit structures from E013-S001.

## Dependencies

- E013-S001 — done

## Definition of Done

- Acceptance criteria pass
- `npm test` (or targeted integration tests) passes
- Story status reflects progress accurately

