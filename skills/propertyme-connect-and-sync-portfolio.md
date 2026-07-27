---
name: Connect a PropertyMe portfolio and keep it in sync
description: Obtain an OAuth token for one agency's connected PropertyMe portfolio, seed a full read of contacts, lots and tenancies, then keep the mirror current with change-since polling — and disconnect cleanly when the agency revokes.
api: openapi/propertyme-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - ChangedContactsRequestcontacts_Get
  - ChangedLotRequestlots_Get
  - ChangedInspectionsRequestinspections_Get
  - ChangedJobTaskRequestjobtasks_Get
  - ChangedTasksRequesttasks_Get
  - TenancyRequesttenancies_Get
  - MembersRequestmembers_Get
  - AgencyRequestcontactsagency_Get
  - DisconnectPortfolioRequestportfoliosdisconnect_Delete
---

# Connect a PropertyMe portfolio and keep it in sync

PropertyMe has no webhooks. Synchronisation is polling against a change-since cursor. This skill is
the foundation every other PropertyMe skill assumes.

## Before you start

- You need a `client_id` and `client_secret` issued by PropertyMe. There is no self-serve signup and
  no developer portal — request them from PropertyMe directly.
- Base URL: `https://app.propertyme.com/api`
- Identity: `https://login.propertyme.com` (OpenID Connect discovery at
  `/.well-known/openid-configuration`).
- The token is scoped to **one** customer portfolio the agency connected. There is no portfolio path
  parameter. If you serve many agencies, you hold one token per agency.

## 1. Get a token

Run the authorization-code flow with PKCE (`S256`) against
`https://login.propertyme.com/connect/authorize`, exchange at
`https://login.propertyme.com/connect/token`. Request `offline_access` alongside your data scopes so
you get a `refresh_token` and can keep polling without dragging the agency back through consent.

Ask only for what you need: `contact:read`, `property:read`, `activity:read` for a read-only mirror;
add the matching `:write` scopes only when you actually write. See
`scopes/propertyme-scopes.yml` for what each scope unlocks.

Send the token as `Authorization: Bearer <access_token>`, and always send `Accept: application/json`
— it is a **required** parameter on every operation in the contract, not an optional courtesy.

## 2. Seed the mirror

Take a starting cursor of `0` and read each collection once:

- `ChangedContactsRequestcontacts_Get` — `GET /v1/contacts?Timestamp=0` — owners, tenants, suppliers.
- `ChangedLotRequestlots_Get` — `GET /v1/lots?Timestamp=0` — the properties.
- `TenancyRequesttenancies_Get` — `GET /v1/tenancies` — the leases. This one takes no timestamp.
- `MembersRequestmembers_Get` — `GET /v1/members` — the agency staff directory.
- `AgencyRequestcontactsagency_Get` — `GET /v1/contacts/agency` — the agency's own contact record.

Store the highest changed-timestamp you observe per collection as that collection's cursor.

## 3. Poll for changes

On each cycle, re-call the change-since collections with your stored cursor:

| Operation | Path | Scope |
|---|---|---|
| `ChangedContactsRequestcontacts_Get` | `GET /v1/contacts` | `contact:read` |
| `ChangedLotRequestlots_Get` | `GET /v1/lots` | `property:read` |
| `ChangedInspectionsRequestinspections_Get` | `GET /v1/inspections` | `activity:read` |
| `ChangedJobTaskRequestjobtasks_Get` | `GET /v1/jobtasks` | `activity:read` |
| `ChangedTasksRequesttasks_Get` | `GET /v1/tasks` | `activity:read` |

`Timestamp` is a **required** int64 and the semantics are strictly greater-than. Advance the cursor
only after you have durably persisted the batch — if you advance first and then fail, those records
are gone from your view until they change again.

Prefer `ChangedJobTaskV2Requestjobtasks_Get` (`GET /v2/jobtasks`) over the v1 shape for new work:
both are live and neither is deprecated, but v2 is the newer job-task model.

## 4. Resolve the graph yourself

PropertyMe returns flat GUID references — `LotId`, `ContactId`, `FolioId`, `ManagerMemberId` — and
supports no `expand`, `include` or `fields` parameter. Join on your side from the mirror rather than
fanning out per-record reads. When you do need the fuller property record, use
`LotDetailRequestlotsIddetail_Get` (`GET /v1/lots/{Id}/detail`) rather than the summary
`LotRequestlotsId_Get`.

## 5. Page where paging exists

Only twelve operations accept `Offset` and `Limit` — the lot filters
(`LotRentalsRequestlotsrentals_Get`, `LotActiveSalesRequestlotssales_Get`,
`LotVacantSalesRequestlotsvacancy_Get`, `LotArchivedRequestlotsarchived_Get`), the contact filters
(`ContactTenantsRequestcontactstenants_Get`, `QuerySuppliersRequestcontactssuppliers_Get`,
`ContactOwnershipsRequestcontactsownerships_Get`) and the inspection and job searches. There is no
total count, no cursor and no next link: page until a page comes back shorter than `Limit`. Every
other list operation returns the whole set unbounded — size your timeouts for that.

## 6. Handle errors and back off

Errors do not use `application/problem+json`; a 4xx or 5xx comes back shaped like the success
response. Read the HTTP status, not the body shape.

- `400` — bad or unknown id, or an invalid request body. Do not retry unchanged.
- `500` — "An internal error has occurred". Retry with exponential backoff.
- `502` — on the dashboard and task collections this means a **required query parameter was not
  supplied**, not a gateway failure. Fix the request.

No rate-limit policy or `RateLimit` headers are published and no `429` is declared. Be conservative:
serialise your polling, keep a modest cycle interval, and back off on any 5xx.

## 7. Disconnect

When the agency revokes, or when you are tearing down the integration, call
`DisconnectPortfolioRequestportfoliosdisconnect_Delete` — `DELETE /v1/portfolios/disconnect`. This
cannot be undone: the current access token is invalidated and the portfolio user has to choose to
reconnect. Do not call it as part of error handling.
