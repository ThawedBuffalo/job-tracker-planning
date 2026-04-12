# Story E014-S002: Prometheus metrics + Grafana dashboards

Status: done

## Story

As a **platform operator**,
I want gateway request metrics exposed in Prometheus format,
So that dashboards and alerts can monitor traffic volume and error trends.

## Acceptance Criteria

1. API gateway exposes a public `GET /metrics` endpoint with Prometheus-compatible output.
2. Gateway records request totals using low-cardinality labels for HTTP method, route template, and status code.
3. Metrics exclude self-observability noise from `/health` and `/metrics` requests.
4. Existing auth/ownership route behavior is not regressed.
5. Integration tests verify metrics exposure and counter increments for success/error paths.
6. Operator-facing docs mention the new metrics endpoint and intended scrape behavior.

## Technical Notes

- Scope is gateway-first metrics only.
- Use low-cardinality route templates rather than raw IDs or query strings.
- Grafana dashboard assets can follow after scrape targets are stable.

## Dependencies

- E014-S001 — done

## Definition of Done

- Acceptance criteria pass
- Gateway tests pass
- Story status reflects progress accurately

