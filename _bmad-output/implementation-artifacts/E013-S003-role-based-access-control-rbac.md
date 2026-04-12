# Story E013-S003: Role-based access control (RBAC)

Status: done

## Story

As a **platform operator**,
I want role-aware authentication and authorization for sensitive endpoints,
So that only permitted roles can access privileged capabilities.

## Acceptance Criteria

1. Users carry a role (`candidate`, `recruiter`, or `admin`) in persisted profile data.
2. Access tokens include a role claim generated at login/refresh time.
3. Authorization middleware enforces required roles and returns `403 FORBIDDEN` envelopes for disallowed roles.
4. Admin-only endpoint(s) are protected by RBAC middleware.
5. Forbidden RBAC attempts emit structured audit events with role context.
6. Existing auth and privacy endpoint behavior is not regressed.
7. Integration tests cover role claim presence, admin access success, and non-admin forbidden behavior.

## Technical Notes

- Default role is `candidate`; admin bootstrap is email-seeded via config.
- Scope covers user-service RBAC foundation only.
- Cross-service RBAC propagation can follow in later stories.

## Dependencies

- E013-S001 — done
- E013-S002 — done

## Definition of Done

- Acceptance criteria pass
- `npm test` (or targeted integration tests) passes
- Story status reflects progress accurately

