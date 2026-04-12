# Story E014-S004: Debug logging mode + request playback

Status: done

## Story

As a **platform operator**,
I want a temporary debug logging mode and request replay tooling in the gateway,
So that I can investigate production issues without changing normal request behavior.

## Acceptance Criteria

1. API gateway supports an opt-in debug mode controlled by configuration.
2. When enabled, proxied requests/responses are captured in a bounded in-memory buffer.
3. Captured entries redact sensitive headers/body fields before operator retrieval.
4. Operator endpoints exist to list captures, inspect a capture, and replay a captured request.
5. Debug endpoints require a dedicated debug token and are unavailable when debug mode is disabled.
6. Existing auth, tracing, and metrics behavior are not regressed.
7. Integration tests cover disabled mode, redaction/capture, replay success, and buffer eviction behavior.

## Technical Notes

- Scope is gateway-only and in-memory.
- Replay uses the original upstream target and sanitized captured payload.
- This mode is for temporary debugging, not durable audit retention.

## Dependencies

- E014-S001 — done
- E014-S002 — done
- E014-S003 — done

## Definition of Done

- Acceptance criteria pass
- Gateway tests pass
- Story status reflects progress accurately

