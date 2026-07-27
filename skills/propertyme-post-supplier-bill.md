---
name: Post a supplier bill to the PropertyMe trust ledger
description: Create a bill with its supporting document against a connected portfolio's trust accounting ledger — the single transaction-writing operation in the published contract — safely, given that PropertyMe offers no idempotency key.
api: openapi/propertyme-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - AddBillRequestbills_Post
  - QuerySuppliersRequestcontactssuppliers_Get
  - ChangedLotRequestlots_Get
  - LotRequestlotsId_Get
  - GetJobTasksQuotationRequestjobtasksIdquotations_Get
  - TransactionDashboardRequestdashboardstransactionsType_Get
  - AddLotFolioDocumentRequestlotsIdfoliodocuments_Post
---

# Post a supplier bill to the PropertyMe trust ledger

`AddBillRequestbills_Post` — `POST /v1/bills` — is the **only** transaction write in PropertyMe's
published contract. It requires `transaction:write`. Everything else in the API reads the ledger or
writes non-financial records. Treat this operation with more care than any other.

## Read this first: there is no idempotency key

PropertyMe publishes no `Idempotency-Key` header, no client-supplied reference and no documented
dedupe window — not on this operation and not anywhere else in the API. A retry after a timeout can
create a **second bill against the trust account**.

Consequences you must design around:

1. **Never blind-retry `POST /v1/bills`.** If the call fails without a definitive response, stop.
2. Generate your own external reference before you post and carry it in a field you can search on,
   so you can recognise your own bill later.
3. Reconcile before retrying: read the transaction dashboard with
   `TransactionDashboardRequestdashboardstransactionsType_Get`
   (`GET /v1/dashboards/transactions/{Type}`, `transaction:read`) and confirm whether the bill
   landed.
4. If you cannot determine the outcome, surface it to a human. Do not resolve trust-account
   ambiguity automatically.

## 1. Resolve the parties

- Supplier: `QuerySuppliersRequestcontactssuppliers_Get` — `GET /v1/contacts/suppliers`
  (`contact:read`, pages with `Offset` / `Limit`).
- Property: `ChangedLotRequestlots_Get` (`GET /v1/lots?Timestamp=0`) for the mirror, or
  `LotRequestlotsId_Get` (`GET /v1/lots/{Id}`) for one lot (`property:read`).
- If the bill is settling maintenance work, pull the agreed figure from
  `GetJobTasksQuotationRequestjobtasksIdquotations_Get` (`GET /v1/jobtasks/{Id}/quotations`,
  `activity:read`) rather than re-keying it.

## 2. Post the bill

`POST /v1/bills` carries the bill and its supporting document together — the document is part of the
bill request, not a follow-up attachment call. Send `Accept: application/json` and
`Authorization: Bearer <access_token>` as on every operation.

## 3. Read the response honestly

There is no `application/problem+json`. The error body is shaped like the success body, so branch on
the HTTP status:

- `200` — accepted.
- `400` — "Invalid bill request". A defect in your payload. Fix it; do not retry unchanged.
- `500` — "An internal error has occurred". **Outcome unknown.** Reconcile before doing anything
  else; do not retry on a timer.

There is no request-id or correlation header in the contract, so there is nothing to quote back to
PropertyMe support beyond the timestamp and the payload you sent — log both.

## 4. Attach anything further to the folio

If more documentation needs to sit against the owner or tenant folio afterwards, use
`AddLotFolioDocumentRequestlotsIdfoliodocuments_Post` — `POST /v1/lots/{Id}/folio/documents`
(`property:write`).

## Scope discipline

Ask for `transaction:write` only in integrations that genuinely post bills. It is the highest-
consequence scope PropertyMe issues, it is separate from `transaction:read`, and the agency can see
and revoke your connection at any time. If your integration only reads money, request
`transaction:read` alone — see `scopes/propertyme-scopes.yml`.
