# Story E012-S004: Third-party integrations (LinkedIn, Greenhouse)

Status: done

## Story

As an **authenticated user**,
I want to attach third-party job posting integration details to an application and open provider links,
So that I can jump back to LinkedIn or Greenhouse sources while tracking progress.

## Acceptance Criteria

1. `ThirdPartyIntegration` model exists with `applicationId`, `provider`, `postingUrl`, `externalId`, `notes`, and `updatedAt` fields.
2. Application detail view includes a `Third-Party Integrations` section with provider, URL, and external ID inputs.
3. User can save a valid integration entry for an application and see it immediately in detail view without full dashboard refresh.
4. Validation blocks invalid entries where both URL and external ID are empty, or URL is not a valid HTTP/HTTPS URI.
5. `Open Integration` action is shown for saved entries and generates a provider launch payload/URI.
6. Existing dashboard list/search/filter/export/recommendations/interview schedule/comments states are not regressed.
7. Widget tests cover section render, validation, save, and open-integration action visibility.
8. `flutter analyze` reports no issues.

## Technical Notes

- Scope is Flutter-side integration UX with controller-managed in-memory state.
- Provider wiring is URI/payload based and does not require network/OAuth in this story.
- Keep current detail and dashboard interaction patterns stable.

## Dependencies

- E012-S001 — done
- E012-S002 — done
- E012-S003 — done

## Definition of Done

- All acceptance criteria pass
- `flutter test` passes
- `flutter analyze` passes
- Story status set to `done`

