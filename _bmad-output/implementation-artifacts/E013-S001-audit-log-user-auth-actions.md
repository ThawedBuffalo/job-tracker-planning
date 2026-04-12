# Story E013-S001: Audit log (user auth actions)

Status: done

## Story

As a **platform operator**,
I want authentication outcomes to emit structured audit events with correlation IDs,
So that security-sensitive user actions can be traced and reviewed reliably.

## Acceptance Criteria

1. User service emits structured audit events for register/login/refresh/logout success and failure paths.
2. Audit events include a stable timestamp, `event` name, service label, and `trace_id`/correlation ID.
3. If an audit sink fails, authentication API responses still complete without regression.
4. Request middleware reuses incoming `x-correlation-id` when present; otherwise generates a new trace ID.
5. Existing auth endpoint behavior and response contracts are not regressed.
6. Integration tests cover forwarded correlation ID and non-blocking audit-failure behavior.

## Technical Notes

- This story is the first security-hardening slice for audit foundations.
- Current implementation uses pluggable in-process audit writer with structured stdout fallback.
- Durable database-backed audit persistence can follow in a subsequent story slice.

## Dependencies

- Wave 1 auth flow stories — done

## Definition of Done

- Acceptance criteria pass
- `npm test` (or targeted auth integration tests) passes
- Story status reflects progress accurately

