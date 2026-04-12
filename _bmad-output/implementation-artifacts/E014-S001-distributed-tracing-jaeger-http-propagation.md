# Story E014-S001: Distributed tracing (Jaeger) HTTP propagation

Status: done

## Story

As a **platform operator**,
I want gateway requests to carry distributed tracing context to downstream services,
So that cross-service request flows can be correlated in observability tooling.

## Acceptance Criteria

1. API gateway reads inbound `traceparent` header and forwards it upstream.
2. API gateway generates a valid `traceparent` header when inbound requests do not provide one.
3. Gateway response includes the effective `traceparent` value for client-side correlation.
4. Existing `x-correlation-id` behavior remains unchanged.
5. Gateway integration tests cover forwarding and generation behavior for tracing context.
6. Existing auth/ownership gateway tests are not regressed.

## Technical Notes

- This slice is HTTP propagation only; deeper auto-instrumentation can follow.
- W3C trace format: `00-<trace-id>-<parent-id>-01`.
- Keep compatibility with existing correlation ID contract.

## Dependencies

- E013-S004 — done

## Definition of Done

- Acceptance criteria pass
- `npm test` (or targeted gateway integration tests) passes
- Story status reflects progress accurately


