---
name: Review PropertyMe tenancies and arrears
description: Read the lettings ledger — tenancies, tenancy balances and the tenant, owner and lot records behind them — to produce an arrears or occupancy view over a connected portfolio.
api: openapi/propertyme-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - TenancyRequesttenancies_Get
  - TenancyBalanceRequesttenanciesbalances_Get
  - SingleTenancyBalanceRequesttenanciesbalancesid_Get
  - ChangedLotRequestlots_Get
  - LotRentalsRequestlotsrentals_Get
  - LotVacantSalesRequestlotsvacancy_Get
  - LotDetailRequestlotsIddetail_Get
  - ContactTenantsRequestcontactstenants_Get
  - ContactOwnershipsRequestcontactsownerships_Get
  - ContactRequestcontactsId_Get
  - GetContactAlertsRequestcontactsIdalertsType_Get
  - LotManagerRequestlotsIdmembers_Get
  - TransactionDashboardRequestdashboardstransactionsType_Get
---

# Review PropertyMe tenancies and arrears

This is the lettings ledger surface that listing-oriented real estate APIs omit entirely. It is
**read-only** — the public contract exposes no tenancy writes. Needs `property:read`, plus
`contact:read` for the people and `transaction:read` for the transaction dashboard.

## 1. The tenancies

`TenancyRequesttenancies_Get` — `GET /v1/tenancies` — takes no change-since timestamp. Filters:

- `ContactId` — tenancies for one contact
- `LotId` — tenancies against one property
- `HasOwnership` — whether the tenancy has an ownership attached
- `IncludeClosed` — include closed tenancies (default excludes them)

It returns the whole matching set with no `Offset` / `Limit`. Size your timeouts for a full portfolio.

## 2. The balances

- `TenancyBalanceRequesttenanciesbalances_Get` — `GET /v1/tenancies/balances` — the balance records
  across the portfolio. This is your arrears source.
- `SingleTenancyBalanceRequesttenanciesbalancesid_Get` — `GET /v1/tenancies/balances/{id}` — one
  balance by id, for drill-down.

There is no arrears endpoint and no arrears flag: arrears is a computation you perform over the
balance records against the rent-paid-to date on the tenancy. Do the arithmetic on your side, and
say so in your output rather than implying PropertyMe classified it.

## 3. Join to the property and the people

Relationships are flat GUIDs — there is no `expand` or `include`.

- Property: `ChangedLotRequestlots_Get` (`GET /v1/lots?Timestamp=0`) for the mirror,
  `LotRentalsRequestlotsrentals_Get` (`GET /v1/lots/rentals`) for just the rental book,
  `LotVacantSalesRequestlotsvacancy_Get` (`GET /v1/lots/vacancy`) for vacancies, and
  `LotDetailRequestlotsIddetail_Get` (`GET /v1/lots/{Id}/detail`) for the fuller record. The three
  filtered lot reads page with `Offset` / `Limit`.
- Tenants: `ContactTenantsRequestcontactstenants_Get` (`GET /v1/contacts/tenants`, pages).
- Owners: `ContactOwnershipsRequestcontactsownerships_Get` (`GET /v1/contacts/ownerships`, pages).
- One person: `ContactRequestcontactsId_Get` (`GET /v1/contacts/{Id}`).
- Warnings on a person: `GetContactAlertsRequestcontactsIdalertsType_Get`
  (`GET /v1/contacts/{Id}/alerts/{Type}`) — read these before you act on an arrears case; the
  equivalent for a property is `GetContactAlertsRequestlotsIdalertsType_Get`
  (`GET /v1/lots/{Id}/alerts/{Type}`).
- Who manages it: `LotManagerRequestlotsIdmembers_Get` (`GET /v1/lots/{Id}/members`).

## 4. The aggregate view

`TransactionDashboardRequestdashboardstransactionsType_Get` —
`GET /v1/dashboards/transactions/{Type}` — the pre-aggregated transaction dashboard, keyed by
dashboard item type. Cheaper than reconstructing totals from balances when you only need the
headline. Requires `transaction:read`.

Note the trap on all four dashboard operations: a missing required query parameter comes back as
`502`, not `400`. Treat a `502` from `/v1/dashboards/*` as a client-side request defect, not a
gateway failure to retry.

## 5. Handling this data

Balances, arrears and tenant alerts are money and tenancy data about identifiable people, held in a
trust-accounting system under an Australian AFS licence. Do not write it anywhere the agency has not
asked you to, do not aggregate it across agencies, and remember the agency can sever your connection
at any time — see `propertyme-connect-and-sync-portfolio.md`.

Writing anything back against the ledger is a different scope entirely — see
`propertyme-post-supplier-bill.md`.
