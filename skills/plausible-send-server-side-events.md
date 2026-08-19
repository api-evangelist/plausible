---
name: Send server-side events to Plausible
description: Record pageviews and custom events (including ecommerce revenue) from a server or non-browser client, and verify they were actually accepted rather than silently dropped.
api: openapi/plausible-events-api-openapi.yml
operations: [recordEvent]
generated: '2026-08-13'
method: generated
source: openapi/plausible-events-api-openapi.yml, conventions/plausible-conventions.yml, errors/plausible-problem-types.yml, https://plausible.io/docs/events-api
---

# Send server-side events to Plausible

The Events API is how you get data into Plausible without the JavaScript tracker. It is
available on **every plan**, including trials, and it takes **no API key**. That makes it
the easiest Plausible surface to call and the easiest one to get silently wrong.

**The single most important fact: this endpoint always returns `202`.** It returns `202`
when the event is stored and it returns `202` when the event is thrown away. If you treat
`202` as success you will ship an integration that reports zero events and no errors.

## Steps

1. **Call `recordEvent`** — `POST https://plausible.io/api/event` with
   `Content-Type: application/json` (`text/plain` is also accepted and parsed as JSON).

2. **Set `User-Agent`.** It is **required**. Plausible derives the privacy-preserving
   visitor identifier from it. HTTP clients that send no User-Agent, or a generic one
   shared across all your traffic, will collapse every visitor into one.

3. **Set `X-Forwarded-For` to the real end-user IP.** This is the step server-side
   integrations skip. Without it Plausible sees *your server's* IP for every event,
   attributes all traffic to one visitor in your datacentre's country, and will usually
   drop the events as bot traffic.

4. **Send the body.** Required: `domain` (the site's registered domain), `name`
   (`pageview`, or any other string for a custom event), and `url` (the page URL — UTM
   parameters in it are extracted automatically). Optional: `referrer`, `props` (max **30**
   key/value pairs), `revenue` (`{currency, amount}` with an ISO 4217 code), and
   `interactive` (defaults true; set false for events that should not affect bounce rate).

5. **Verify acceptance — do not skip this.** Read the response headers:
   - `x-plausible-dropped: 1` present → your event was **discarded** (bot heuristics, or a
     `domain` that is not registered on the account).
   - Header absent → accepted.
   Send `X-Debug-Request: true` to get back `200` with the IP address Plausible actually
   used for visitor counting. That is the supported way to prove your `X-Forwarded-For`
   plumbing works behind a proxy or CDN.

6. **Log `x-request-id`** from the response. It is present on every response and it is the
   only handle support has on your request.

## Rules that will bite you

- **`202` is "received", never "stored".** Assert on `x-plausible-dropped`, not on status.
  A test against an unregistered domain returns `202` with body `ok` and
  `x-plausible-dropped: 1` — useful for exercising your request shape without polluting a
  real site, useless as a signal that ingestion works.
- **No idempotency.** There is no `Idempotency-Key` header and no deduplication. A retried
  POST is a second pageview. Do not retry on timeouts unless double-counting is acceptable;
  prefer fire-and-forget with a short timeout.
- **`props` keys must be registered** as custom properties on the site before they can be
  broken down on in the Stats API. Sending an unregistered prop is not an error — the value
  is simply not queryable. Register them with the *Provision a Plausible site* skill.
- **Revenue tracking requires it enabled on the site** (`revenue_tracking` in the site's
  tracker configuration) and a goal configured for the event name.
- `400` is the only error status: a body that cannot be parsed. Everything else is `202`.
