---
name: Share and embed a Plausible dashboard
description: Create a shared link through the Sites API, grant scoped guest access, and embed a live dashboard in a client-facing page.
api: openapi/plausible-sharedlinks-api-openapi.yml
operations: [upsertSharedLink, listGuests, upsertGuest, deleteGuest, getSite]
generated: '2026-08-13'
method: generated
source: openapi/plausible-sharedlinks-api-openapi.yml, openapi/plausible-guests-api-openapi.yml, components/plausible-components.yml, https://plausible.io/docs/embed-dashboard, https://plausible.io/docs/sites-api
---

# Share and embed a Plausible dashboard

Two different ways to give someone else the numbers, with different security properties.
Pick deliberately.

- **Shared link** — a tokenised public URL. Anyone holding the URL sees the dashboard. No
  Plausible account needed. This is the only option that can be **embedded**.
- **Guest access** — a named person, by email, with a `viewer` or `editor` role. Requires
  them to have a Plausible account. Revocable per person. Cannot be embedded.

Both require a **Sites API key** (Enterprise plan) against
`https://plausible.io/api/v1`.

## Shared link and embed

1. **Call `upsertSharedLink`** — `PUT /api/v1/sites/shared-links` with `site_id` and a
   `name`. It is find-or-create keyed on the name, so re-running it returns the existing
   link rather than minting a second one. That makes it safe to run on every deploy.

2. **Leave `password` blank if you intend to embed.** A password-protected dashboard
   **cannot** be embedded — the browser refuses the frame with a "refused to connect"
   error. This is not configurable. Decide up front: password protection *or* embedding,
   never both.

3. **Build the embed.** The shared link has the form
   `https://plausible.io/share/yourdomain.com?auth=TOKEN`. Render it in an iframe and load
   the host script `https://plausible.io/js/embed.host.js` on the surrounding page.
   Optional parameters: theme (`light`, `dark`, `system`), a custom background colour, and
   `&width=manual` to disable Plausible's default `max-width: 1024px; margin: 0 auto`
   rules so your own layout controls the width. The embed-code generator in Website
   settings > Visibility produces the exact markup.

4. **Treat the token as a credential.** It is a bearer secret in a query string. It will
   land in referrer headers, browser history, and any log that records full URLs. Rotate
   it by creating a new named link and retiring the old one.

## Guest access

1. **Call `upsertGuest`** — `PUT /api/v1/sites/guests` with `site_id`, `email` and `role`
   (`viewer` or `editor`). Find-or-create, so re-invitations are safe.
2. **Audit with `listGuests`** (`GET /api/v1/sites/guests`) before and after — it returns
   both accepted guests and pending invitations.
3. **Revoke with `deleteGuest`** (`DELETE /api/v1/sites/guests/{email}`). The email is the
   path segment; URL-encode it. This removes a pending invitation as well as an accepted
   guest.

## Rules that will bite you

- **Offboarding is two jobs, not one.** Removing a guest does nothing to shared links. If
  you are cutting off a departed client or contractor, you must revoke the shared link too
  — anyone who copied the URL still has full read access.
- `role` is `viewer` or `editor` only. There is no per-report or per-metric scoping.
- Errors are flat `{"error": "prose"}` JSON. `401` covers a bad key, the wrong key type,
  and no access to the site.
- No `Idempotency-Key` exists provider-wide; the safety here comes from these specific
  operations being find-or-create, not from an idempotency contract.
