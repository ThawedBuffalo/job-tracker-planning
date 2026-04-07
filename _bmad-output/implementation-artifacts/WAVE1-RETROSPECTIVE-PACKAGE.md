# Wave 1 Retrospective Package

**Date:** April 6, 2026  
**Scope:** Completed Wave 1 backend/platform delivery  
**Status:** finalized

## Included Epic Retrospectives

Completed epics with formal retrospective artifacts:

- [`epic-E002-retro-2026-04-06.md`](./epic-E002-retro-2026-04-06.md)
- [`epic-E003-retro-2026-04-06.md`](./epic-E003-retro-2026-04-06.md)
- [`epic-E004-retro-2026-04-06.md`](./epic-E004-retro-2026-04-06.md)
- [`epic-E006-retro-2026-04-06.md`](./epic-E006-retro-2026-04-06.md)
- [`epic-E007-retro-2026-04-06.md`](./epic-E007-retro-2026-04-06.md)

## Deferred / Partial Epics

The following epics are **not retro-closed** because scope remains incomplete or deferred:

- `E005` — `E005-S002` still `ready-for-dev`

These should receive follow-up retrospectives after remaining scope is formally completed or descoped.

## Cross-Epic Wins

1. **Contract-first development reduced integration friction**
   - Shared OpenAPI and event schemas kept service boundaries coherent.
2. **Thin gateway + strong downstream ownership checks worked well**
   - Security enforcement stayed understandable and testable.
3. **Integration-heavy testing strategy paid off**
   - Real route and flow tests caught problems earlier than unit-only coverage would have.
4. **Correlation IDs improved operability**
   - Cross-service error tracing became practical even with a small stack.

## Cross-Epic Pain Points

1. **In-memory persistence shortcuts hide production realities**
   - Multiple services still need durable storage patterns.
2. **Terminology drift across stories/contracts/code**
   - Example: terminal state naming in pipeline flow.
3. **Wave boundaries mixed “done” work with deferred UX/mobile items**
   - Epic-level completion needs clearer descoping rules.
4. **Performance concerns surfaced late**
   - Query/indexing and async work should move earlier in future waves.

## Carry-Forward Action List

### Highest Priority

- Start Wave 2 with performance baselining/load testing before major feature expansion.
- Introduce durable persistence for pipeline outbox/timeline and notification delivery logs.
- Plan security hardening around row-level security, denial metrics, and rate limiting.

### Team / Process

- Run weekly cross-service sync for shared contract and migration changes.
- Add schema review to pull request checklist for persistence changes.
- Keep retrospective checkpoints at epic close, not only at wave end.

## Recommended Wave 2 Sequencing Adjustments

Based on retrospective evidence, prioritize:

1. `E010-S003` / performance baseline work earlier
2. `E010-S001` connection pooling / DB hardening
3. `E011-S001` async notification queueing
4. `E013-S001` audit + security policy groundwork

## Decision Log

- Formal retrospective closure applied only to epics fully complete in `sprint-status.yaml`.
- Partial epics remain open to avoid masking deferred scope as complete.
- Wave 2 planning docs should reference this package as the authoritative retro artifact.

