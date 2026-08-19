---
name: Query Plausible site traffic
description: Run a Stats API v2 query against a Plausible site to get metrics, break them down by dimension, filter them, and page through large result sets.
api: openapi/plausible-query-api-openapi.yml
operations: [runQuery]
generated: '2026-08-13'
method: generated
source: openapi/plausible-query-api-openapi.yml, conventions/plausible-conventions.yml, errors/plausible-problem-types.yml, https://plausible.io/docs/stats-api
---

# Query Plausible site traffic

Plausible's Stats API is a single endpoint. Everything — aggregates, time series,
breakdowns, funnels, comparisons — is one `runQuery` call with a different body. There is
no `/aggregate`, no `/breakdown`, no `/timeseries` in v2. If you are looking for those,
you are reading the legacy v1 docs.

## Before you start

- You need a **Stats API key**, created in Account settings > API Keys. It requires a
  **Business or Enterprise** plan. There is no test key and no test mode — every call
  reads production data.
- `site_id` is the **domain** registered in Plausible (`example.com`), not an opaque id.

## Steps

1. **Authenticate.** Send `Authorization: Bearer YOUR-KEY` on every request. Omitting it
   returns `401 {"error":"Missing API key. ..."}`.

2. **Call `runQuery`** — `POST https://plausible.io/api/v2/query` with
   `Content-Type: application/json`. The body requires three fields:
   - `site_id` — the domain.
   - `metrics` — an array from: `visitors`, `visits`, `pageviews`, `views_per_visit`,
     `bounce_rate`, `visit_duration`, `events`, `scroll_depth`, `percentage`,
     `conversion_rate`, `group_conversion_rate`, `average_revenue`, `total_revenue`.
   - `date_range` — either a keyword (`day`, `7d`, `28d`, `30d`, `91d`, `month`, `6mo`,
     `12mo`, `year`, `all`) or a two-element array of ISO date-times.

3. **Shape the result with `dimensions`.** Omit it for a single aggregate row. Add a time
   dimension (`time:day`, `time:week`, `time:month`) for a time series, or a property
   dimension (`event:page`, `visit:source`, `visit:country`, `visit:device`,
   `visit:browser`, `visit:os`, `visit:utm_*`) for a breakdown. You can combine them.

4. **Narrow with `filters` and sort with `order_by`.** Both are arrays of arrays. Filter
   before you paginate — filtering server-side is always cheaper than pulling rows and
   discarding them, and it costs you fewer of your 600 hourly requests.

5. **Paginate.** Set `pagination.limit` (default 10000) and `pagination.offset`
   (default 0). To learn how many rows exist, set `include.total_rows = true` and read
   `meta.total_rows` from the response. Do not page blindly.

6. **Read the response.** `results[]` is an array of `{metrics: [...], dimensions: [...]}`
   where the arrays are positionally aligned with the `metrics` and `dimensions` you
   asked for. `query` echoes the resolved query — use it to confirm the server
   interpreted your date range the way you intended.

## Rules that will bite you

- **Rate limit: 600 requests per hour per key, and you cannot see your remaining budget.**
  Plausible returns no `X-RateLimit-*`, no `RateLimit-*` and no `Retry-After` header. On
  `429`, back off exponentially; the window is rolling. Batch dimensions into one query
  instead of issuing one query per dimension.
- **`401` is ambiguous.** `{"error":"Invalid API key or site ID. ..."}` means a bad key,
  the wrong key *type*, a plan that does not include the Stats API, *or* a site the key
  cannot reach. Check all four before retrying.
- **Errors are not RFC 9457.** You get `{"error": "prose"}`. There is no machine-readable
  code — branch on HTTP status, not on message text.
- **No idempotency keys.** `runQuery` is a read, so retries are safe by nature, but do not
  assume the same contract on any write endpoint.
- **Custom properties must be registered first.** You cannot break down on an
  `event:props:*` dimension until that property exists on the site — see the
  *Provision a Plausible site* skill.
- Record the `x-request-id` response header. It is on every response and it is what
  Plausible support will ask you for.
