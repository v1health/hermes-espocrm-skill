# Data Contracts for Dashboards and Queues

For each displayed value, document:

| Field | Required content |
|---|---|
| Metric name | What the UI claims to measure |
| Entity / source | API or repository returning the data |
| Filters | Exact fields, operators and statuses |
| Scope | Global / specific Business Unit / current API user |
| Freshness | Timestamp source, timezone and stale window |
| Completeness | Pagination/caps, duplicate elimination and aggregation |
| Error semantics | Loading, success zero, unknown, stale, denied, partial |
| Update triggers | First load, refresh, filter changes, background sync |

## State model

- `Loading`: request pending; no success styling.
- `0`: source confirmed valid and complete, count is genuinely zero.
- `Unknown`: invalid API, missing field, request failure, permissions or incomplete data.
- `Stale`: last valid timestamp outside documented age window.
- `Recent activity`: evidence of an event, **not** proof that the integration is healthy.
- `Healthy`: a real, service-specific health check passed.

Never infer `Active` or `Healthy` from the mere existence of an object or an unrelated authenticated endpoint.

## Accounting example

A queue called "ready for accountant" needs an explicit status *and* send flag predicate confirmed against the business process. Using a UI label as a database status can produce phantom queues. Null send flags should not be silently interpreted as unsent. Cross-check with a known-good server-side count before publishing metrics.

## Shared caches

When multiple UI modules need the same count, share results or promises rather than doing extra API calls. Attach downstream updates to promise completion (`finally`/`allSettled` when appropriate); avoid arbitrary setTimeout polling. Re-run dependent UI updates after data refresh and when relevant filters change. If a cache is per-BU, don't label it as a global count.
