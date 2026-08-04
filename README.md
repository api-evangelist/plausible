# Plausible (plausible)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Plausible is an open source, privacy-friendly web analytics platform designed as a lightweight alternative to Google Analytics. It provides essential website traffic metrics without using cookies or collecting personal data, making it compliant with GDPR, CCPA, and other privacy regulations out of the box. It can be self-hosted or used as a cloud service.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/plausible/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/plausible/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Analytics
- Cookie-Free
- GDPR
- Open Source
- Privacy
- Web Analytics

## Timestamps

- **Created:** 2026-03-26
- **Modified:** 2026-05-19

## APIs

### Plausible Stats API

The Plausible Stats API provides programmatic access to website analytics data including aggregate metrics, time-series data, and breakdowns by various dimensions such as pages, sources, countries, devices, and browsers. It enables developers to retrieve visitor counts, pageviews, bounce rates, visit durations, and custom event data for building external dashboards and integrating analytics into other applications.

- **Human URL:** [https://plausible.io/docs/stats-api](https://plausible.io/docs/stats-api)
- **Base URL:** `https://plausible.io/api/v2`

#### Tags

- Analytics
- Metrics
- Reporting
- Statistics

#### Properties

- [Documentation](https://plausible.io/docs/stats-api)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/plausible/refs/heads/main/openapi/plausible-stats-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/plausible-events.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-events.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-sites.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-sites.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-stats.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-stats.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Plausible Events API

The Plausible Events API allows developers to send pageview and custom events to Plausible from server-side applications, mobile apps, or any environment where the standard JavaScript snippet cannot be used. It supports recording pageviews, custom events with properties, and revenue tracking data while maintaining Plausible's privacy-first approach.

- **Human URL:** [https://plausible.io/docs/events-api](https://plausible.io/docs/events-api)
- **Base URL:** `https://plausible.io/api`

#### Tags

- Analytics
- Events
- Pageviews
- Tracking

#### Properties

- [Documentation](https://plausible.io/docs/events-api)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/plausible/refs/heads/main/openapi/plausible-events-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/plausible-events.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-events.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-sites.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-sites.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-stats.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-stats.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Plausible Sites API

The Plausible Sites API enables developers to programmatically manage sites within their Plausible account. It supports creating new sites, deleting existing sites, retrieving site information, and managing shared links and goals. This API is useful for agencies and platforms that need to automate site provisioning and configuration.

- **Human URL:** [https://plausible.io/docs/sites-api](https://plausible.io/docs/sites-api)
- **Base URL:** `https://plausible.io/api/v1/sites`

#### Tags

- Analytics
- Management
- Provisioning
- Sites

#### Properties

- [Documentation](https://plausible.io/docs/sites-api)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/plausible/refs/heads/main/openapi/plausible-sites-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/plausible-events.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-events.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-sites.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-sites.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/plausible-stats.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/plausible-stats.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/plausible-analytics)
- [Website](https://plausible.io)
- [Documentation](https://plausible.io/docs)
- [A P I Documentation](https://plausible.io/docs/stats-api)
- [Getting Started](https://plausible.io/docs/add-website)
- [Blog](https://plausible.io/blog)
- [Pricing](https://plausible.io/pricing)
- [Git Hub](https://github.com/plausible/analytics)
- [Login](https://plausible.io/login)
- [Sign Up](https://plausible.io/register)
- [Support](https://plausible.io/contact)
- [Self Hosted](https://plausible.io/self-hosted-web-analytics)
- [Changelog](https://github.com/plausible/analytics/releases)
- [Terms of Service](https://plausible.io/terms)
- [Privacy Policy](https://plausible.io/privacy)
- [Data Policy](https://plausible.io/data-policy)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
