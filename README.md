# PropertyMe (propertyme)

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

PropertyMe is an Australian cloud property management and trust accounting platform for residential real estate agencies, founded in 2013 and operated by MePay Holdings Pty Ltd (AFCA member ID 81095, AFS licence no. 528836), with roughly 1.7 million properties under management across Australia and New Zealand. In the Australian property value chain it sits on the PROPERTY MANAGEMENT rail rather than the listing or settlement rails — it does not operate a portal like REA Group's realestate.com.au or Domain, and it is not a PEXA conveyancing participant; it is the system of record for the rental portfolio, holding lots, tenancies, owners, tenants, suppliers, trust transactions, inspections, maintenance jobs and documents, plus its own MePay payments product and the Grow CRM and AiMe assistant products. Its API posture is unusually honest for this sector — the machine-readable contract is genuinely open while the credentials are not. A Swagger 2.0 document describing 75 paths, 86 operations and 296 definitions is served anonymously with no login at https://app.propertyme.com/api/openapi.json, rendered by a public Swagger UI at https://app.propertyme.com/api/swagger-ui/, and the OpenID Connect discovery document at https://login.propertyme.com is also served anonymously and advertises the full scope list. But no self-serve developer signup, app registration route or public client-credential issuance path exists anywhere on propertyme.com.au, app.propertyme.com or any developer/developers/docs/api subdomain (none of which resolve); a developer must approach PropertyMe to be issued an OAuth client_id and client_secret, and every call is additionally scoped to one customer's portfolio that the agency itself connects and can disconnect. RESO is absent — PropertyMe does not appear in the RESO certification directory, there is no OData service, no `$metadata` document and no Universal Property Identifier, which is the expected Australian answer because RESO is a North American NAR/MLS construct with no Australian counterpart. PropertyMe publishes no open data.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/propertyme/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/propertyme/refs/heads/main/apis.yml)

## Tags

- Real Estate
- Australia
- Property Management
- Rentals
- PropTech
- Tenancy
- Trust Accounting
- Inspections
- Maintenance
- Documents
- Payments
- New Zealand

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### PropertyMe Contacts API

Read the contact records in a connected PropertyMe portfolio — owners, tenants, suppliers and the agency contact itself — with change-since-timestamp polling, contact alerts by type, images, and write access for comments and documents attached to a contact. Requires the `contact:read` scope; comment and document creation require `contact:write`.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Contacts
- Owners
- Tenants
- Suppliers

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Properties API

The lot (property) record in a connected PropertyMe portfolio — list all lots changed since a timestamp, or filter to rentals, active sales, vacancies and archived lots, retrieve lot detail, contact alerts, images and the managing member, and attach comments and documents to a lot or to an owner or tenant folio. Requires the `property:read` scope; writes require `property:write`.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Properties
- Lots
- Rentals
- Vacancy

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Tenancies API

Read-only access to the tenancies in a connected PropertyMe portfolio and to tenancy balances, including a single tenancy balance record by id. This is the lettings ledger surface that most listing-oriented real estate APIs omit entirely. Requires the `property:read` scope.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Tenancies
- Leases
- Balances

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Inspections API

The full routine and entry/exit inspection lifecycle — create, query, search by contact or lot, filter by status, and drive an inspection through schedule, reschedule, inspect, close and reopen transitions, plus inspection reports, comments, documents, images and the assigned inspection manager. Requires `activity:read` and, for the state transitions, `activity:write`.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Inspections
- Reports
- Compliance

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Job Tasks API

Maintenance work orders, in both a v1 and a newer v2 shape. Create and update jobs, search by due date, created date and status, move a job through approve, assign, complete, reject and reopen, manage supplier quotations and quotation documents, and attach comments, documents and images. The largest write surface in the API. Requires `activity:read` and `activity:write`.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Maintenance
- Jobs
- Work Orders
- Quotations
- Suppliers

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Tasks API

General property management tasks distinct from maintenance jobs — list tasks changed since a timestamp, create and update a task, find a task by id, read the assigned task manager, and attach comments, documents and images. Requires `activity:read` and `activity:write`.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Tasks
- Workflow

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Bills API

Create a new bill in a connected PropertyMe portfolio, including the supporting document, against the trust accounting ledger. This is the only transaction-writing operation exposed in the published contract and it requires the `transaction:write` scope.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Bills
- Trust Accounting
- Transactions

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Dashboards API

Typed dashboard aggregates over a connected portfolio — activities, communications, lots and transactions, each retrieved by dashboard item type. The read model behind the PropertyMe dashboard, drawing on the activity, communication, property and transaction read scopes.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Dashboards
- Reporting
- Analytics

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Documents and Images API

The file surface, exposed as sub-resources rather than a standalone collection — create documents against contacts, lots, owner and tenant folios, inspections, tasks and jobs, and list images for contacts, lots, inspections, tasks and jobs. Document creation is governed by the write scope of the parent resource.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Documents
- Images
- Files

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Members API

The agency staff directory inside a connected portfolio — list all members, and resolve the responsible member for a lot, a task, a job or an inspection. Requires the `contact:read` scope.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Members
- Property Managers
- Staff

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

### PropertyMe Portfolio Connection API

The consent seam. A single `DELETE /v1/portfolios/disconnect` operation severs the integration's connection to the customer's current portfolio. Every other operation in the API is implicitly scoped to the one portfolio the agency connected, which is what makes this a per-customer, revocable integration rather than market-wide data access.

- **Human URL:** [https://app.propertyme.com/api/swagger-ui/](https://app.propertyme.com/api/swagger-ui/)
- **Base URL:** `https://app.propertyme.com/api`

#### Tags

- Connection
- Portfolios
- Consent

#### Properties

- [OpenAPI](openapi/propertyme-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [Authentication](authentication/propertyme-openid-configuration.json)

## Common Properties

- [Website](https://www.propertyme.com.au/)
- [About](https://www.propertyme.com.au/about)
- [Documentation](https://app.propertyme.com/api/swagger-ui/)
- [API Reference](https://app.propertyme.com/api/swagger-ui/)
- [OpenAPI](https://app.propertyme.com/api/openapi.json)
- [Authentication](authentication/propertyme-openid-configuration.json)
- [Sign Up](https://www.propertyme.com.au/request-a-demo)
- [Pricing](https://www.propertyme.com.au/pricing)
- [Security](https://www.propertyme.com.au/security)
- [Terms of Service](https://www.propertyme.com.au/terms)
- [Privacy Policy](https://www.propertyme.com.au/privacy)
- [Support](https://support.propertyme.com/hc/en-us)
- [Status Page](https://status.propertyme.com/)
- [Blog](https://www.propertyme.com.au/blog)
- [Blog RSS](https://www.propertyme.com.au/feed)
- [Integrations](https://www.propertyme.com.au/integrations)
- [Partners](https://www.propertyme.com.au/partner-directory)
- [GitHub Organization](https://github.com/PropertyMe)
- [Sample Code](https://github.com/PropertyMe/HelloPropertyMe.NET)
- [LinkedIn](https://www.linkedin.com/company/propertyme)
- [YouTube](https://www.youtube.com/channel/UC99HN1NFPAYyXvyKyRhHkJQ)
- [Contact](https://www.propertyme.com.au/contact)

## Maintainers

- **Kin Lane** — kin@apievangelist.com
