# PropertyMe

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

PropertyMe is an Australian cloud property management and trust accounting platform for residential real estate agencies, founded in 2013 and operated by MePay Holdings Pty Ltd (AFCA member ID 81095, AFS licence no. 528836), with roughly 1.7 million properties under management across Australia and New Zealand. In the Australian property value chain it sits on the PROPERTY MANAGEMENT rail rather than the listing or settlement rails — it does not operate a portal like REA Group's realestate.com.au or Domain, and it is not a PEXA conveyancing participant; it is the system of record for the rental portfolio, holding lots, tenancies, owners, tenants, suppliers, trust transactions, inspections, maintenance jobs and documents, plus its own MePay payments product and the Grow CRM and AiMe assistant products. Its API posture is unusually honest for this sector — the machine-readable contract is genuinely open while the credentials are not. A Swagger 2.0 document describing 75 paths, 86 operations and 296 definitions is served anonymously with no login at https://app.propertyme.com/api/openapi.json, rendered by a public Swagger UI at https://app.propertyme.com/api/swagger-ui/, and the OpenID Connect discovery document at https://login.propertyme.com is also served anonymously and advertises the full scope list. But no self-serve developer signup, app registration route or public client-credential issuance path exists anywhere on propertyme.com.au, app.propertyme.com or any developer/developers/docs/api subdomain (none of which resolve); a developer must approach PropertyMe to be issued an OAuth client_id and client_secret, and every call is additionally scoped to one customer's portfolio that the agency itself connects and can disconnect. RESO is absent — PropertyMe does not appear in the RESO certification directory, there is no OData service, no $metadata document and no Universal Property Identifier, which is the expected Australian answer because RESO is a North American NAR/MLS construct with no Australian counterpart. PropertyMe publishes no open data.

**Areas:** Real Estate · Australia · Property Management · Rentals · PropTech · Tenancy · Trust Accounting · Inspection · Maintenance · Documents · Payments · New Zealand

## PropertyMe on APIs.io

Everything profiled for PropertyMe is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/propertyme/](https://apis.io/providers/propertyme/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [PropertyMe Connection API](https://apis.io/apis/propertyme/propertyme-connection-api/) — The Connection API from PropertyMe — 1 operation(s) for connection.
- [PropertyMe Scope: Activities API](https://apis.io/apis/propertyme/propertyme-scope-activities-api/) — The Scope: Activities API from PropertyMe — 43 operation(s) for scope: activities.
- [PropertyMe Scope: Bills API](https://apis.io/apis/propertyme/propertyme-scope-bills-api/) — The Scope: Bills API from PropertyMe — 2 operation(s) for scope: bills.
- [PropertyMe Scope: Contacts API](https://apis.io/apis/propertyme/propertyme-scope-contacts-api/) — The Scope: Contacts API from PropertyMe — 11 operation(s) for scope: contacts.
- [PropertyMe Scope: Messages API](https://apis.io/apis/propertyme/propertyme-scope-messages-api/) — The Scope: Messages API from PropertyMe — 1 operation(s) for scope: messages.
- [PropertyMe Scope: Properties API](https://apis.io/apis/propertyme/propertyme-scope-properties-api/) — The Scope: Properties API from PropertyMe — 17 operation(s) for scope: properties.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (3)

Multi-step workflows that chain operations into a business task.

- [Connect a PropertyMe portfolio and run a change-since sync](https://apis.io/arazzos/propertyme/propertyme-connect-and-sync/)
- [Schedule, conduct, report and close a PropertyMe inspection](https://apis.io/arazzos/propertyme/propertyme-inspection-cycle/)
- [Raise, quote, approve and complete a PropertyMe maintenance job](https://apis.io/arazzos/propertyme/propertyme-maintenance-job/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [propertyme](https://apis.io/servers/propertyme/propertyme/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Propertyme Agentic Access](https://apis.io/agentic-access/propertyme/propertyme-agentic-access/)

#### Collections (1)

Postman and Open Collection request collections.

- [PropertyMe](https://apis.io/collections/propertyme/open-propertyme/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Propertyme Openapi Overlay](https://apis.io/overlays/propertyme/propertyme-openapi-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Propertyme Authentication](https://apis.io/security/propertyme/propertyme-authentication/)
- [Propertyme Domain Security](https://apis.io/security/propertyme/propertyme-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Propertyme Scopes](https://apis.io/scopes/propertyme/propertyme-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.propertyme.com.au/)
- [Documentation](https://app.propertyme.com/api/swagger-ui/)
- [API reference](https://app.propertyme.com/api/swagger-ui/)
- [Pricing](https://www.propertyme.com.au/pricing)
- [Sign up](https://www.propertyme.com.au/request-a-demo)
- [Support](https://support.propertyme.com/hc/en-us)
- [Blog](https://www.propertyme.com.au/blog)
- [Security](https://www.propertyme.com.au/security)
- [Terms of service](https://www.propertyme.com.au/terms)
- [Privacy policy](https://www.propertyme.com.au/privacy)
- [GitHub](https://github.com/PropertyMe)
- [Status](https://status.propertyme.com/)

## API Evangelist rating

**Kin Score 41.4** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 49.1 |
| Contract governance | 4.5 |
| Operational transparency | 28.9 |
| Developer ergonomics | 37.5 |
| Access clarity | 44.7 |

**Agent readiness 22.6** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | — |

**Regulatory alignment 31.3** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/propertyme/](https://apis.io/providers/propertyme/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for PropertyMe: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/propertyme/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
