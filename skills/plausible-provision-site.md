---
name: Provision a Plausible site
description: Create a site through the Sites API and configure it end to end — tracker options, conversion goals, and the custom properties that make custom events queryable.
api: openapi/plausible-sites-api-openapi.yml
operations: [listTeams, listSites, createSite, getSite, updateSite, deleteSite, listGoals, upsertGoal, deleteGoal, listCustomProps, createCustomProp, deleteCustomProp]
generated: '2026-08-13'
method: generated
source: openapi/plausible-sites-api-openapi.yml, openapi/plausible-goals-api-openapi.yml, openapi/plausible-customprops-api-openapi.yml, openapi/plausible-teams-api-openapi.yml, conventions/plausible-conventions.yml, https://plausible.io/docs/sites-api
---

# Provision a Plausible site

This is the agency and platform flow: stand up a Plausible site for a customer and get it
into a usable state without anyone touching the dashboard.

## Before you start

- You need a **Sites API key** — a different key type from the Stats API key, created in
  Account settings > API Keys. It requires an **Enterprise** plan.
- **This key is unscoped.** There is no read-only mode and no per-site restriction. A
  Sites API key can delete every site on the account. Treat it accordingly: store it
  where a Stats-only integration cannot reach it, and never hand it to a client-side
  process.
- Base URL is `https://plausible.io/api/v1` — note the Sites API is still **v1** while the
  Stats API is on v2.

## Steps

1. **Orient with `listTeams`** (`GET /api/v1/sites/teams`) to confirm which account the key
   is scoped to before you write anything.

2. **Check for an existing site with `listSites`** (`GET /api/v1/sites`). It is cursor
   paginated: pass `limit` (default 100) and follow `meta.after` from the response,
   stopping when the cursor comes back `null`. Do this before creating — `createSite` is
   the one write in this flow that is **not** find-or-create, so a blind retry after a
   timeout can leave you with a duplicate.

3. **Create the site with `createSite`** (`POST /api/v1/sites`). Required: `domain`. Send
   `timezone` as an IANA identifier (`Etc/UTC`, `Europe/Berlin`) — reports are bucketed by
   it and changing it later reinterprets the data. Optionally send
   `tracker_script_configuration` in the same call.

4. **Configure the tracker with `updateSite`** (`PUT /api/v1/sites/{site_id}`) if you did
   not do it at creation. `tracker_script_configuration` accepts `outbound_links`,
   `file_downloads`, `form_submissions`, `track_404_pages`, `revenue_tracking` and
   `hash_based_routing`. Turn on `revenue_tracking` **now** if the customer sells
   anything — revenue events sent before it is enabled are not backfilled.

5. **Register custom properties with `createCustomProp`**
   (`PUT /api/v1/sites/custom-props`) for every `props` key your events will carry. This
   step is what makes custom event properties appear as dimensions in the Stats API. Skip
   it and your events are ingested, accepted, and permanently unqueryable. Verify with
   `listCustomProps`.

6. **Create conversion goals with `upsertGoal`** (`PUT /api/v1/sites/goals`). Set
   `goal_type` to `event` and supply `event_name`, or to `page` and supply `page_path`.
   Verify with `listGoals`. Remove with `deleteGoal` (`DELETE /api/v1/sites/goals/{goal_id}`).

7. **Confirm with `getSite`** (`GET /api/v1/sites/{site_id}`) and hand the tracker snippet
   to the customer.

`deleteSite` (`DELETE /api/v1/sites/{site_id}`) removes the site and its data. There is no
undo and no soft delete. Gate it behind an explicit human confirmation.

## Rules that will bite you

- **Retry safety is per-operation, not provider-wide.** There is no `Idempotency-Key`
  header anywhere in Plausible. `upsertGoal`, `createCustomProp` and `upsertGuest` are
  find-or-create, so retrying them converges. `createSite` is not — check with `listSites`
  first, or you will create duplicates.
- **`site_id` is the domain**, and it is also the path segment. URL-encode it.
- **`401` conflates everything**: bad key, Stats key used on a Sites route, plan without
  the Sites API, or no access to that site. Confirm the key *type* first — this is the
  most common cause.
- Errors are flat `{"error": "prose"}` JSON, not RFC 9457. Branch on status code.
- Log the `x-request-id` response header on every write.
