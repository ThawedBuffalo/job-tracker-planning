# Story E014-S003: Alert rules + PagerDuty integration

Status: done

## Story

As a **platform operator**,
I want alerting rules and PagerDuty-ready routing for gateway health/error signals,
So that production-impacting incidents can trigger actionable notifications.

## Acceptance Criteria

1. Infrastructure repo includes an optional observability compose overlay with Prometheus and Alertmanager.
2. Prometheus config scrapes the API gateway `/metrics` endpoint and loads alert rule files.
3. Alert rules cover gateway down, sustained 5xx ratio, and low-traffic warning scenarios.
4. Alertmanager config includes a PagerDuty-ready critical receiver path.
5. `.env.example` and operator-facing docs describe observability ports and PagerDuty routing key setup.
6. CI/local compose validation covers the observability overlay config.

## Technical Notes

- Scope is config/documentation/validation only; live PagerDuty credentials remain placeholder-based.
- Alert rules depend on gateway metrics introduced in E014-S002.
- Additional service-specific alerts can follow in later stories.

## Dependencies

- E014-S001 — done
- E014-S002 — done

## Definition of Done

- Acceptance criteria pass
- Compose config validation succeeds
- Story status reflects progress accurately


