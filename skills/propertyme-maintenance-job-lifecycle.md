---
name: Run a PropertyMe maintenance job end to end
description: Raise a maintenance work order against a property, collect and update supplier quotations, drive it through approve, assign, complete, reject and reopen, and attach documents and images — using the v2 job-task shape where it exists.
api: openapi/propertyme-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - AddJobV2Requestjobtasks_Post
  - AddJobRequestjobtasks_Post
  - ChangedJobTaskV2Requestjobtasks_Get
  - SearchJobTaskV2Requestjobtaskssearch_Get
  - JobTaskV2RequestjobtasksId_Get
  - UpdateJobV2RequestjobtasksId_Post
  - AddJobTasksQuotationRequestjobtasksIdquotation_Post
  - GetJobTasksQuotationRequestjobtasksIdquotations_Get
  - UpdateJobTasksQuotationRequestjobtasksIdquotationQuoteId_Post
  - AddJobTasksQuotationDocumentRequestjobtasksIdquotationQuoteIddocuments_Post
  - ApproveJobTaskRequestjobtasksIdapprove_Post
  - AssignTaskRequestjobtasksIdassign_Post
  - CompleteJobTaskRequestjobtasksIdcomplete_Post
  - RejectJobTaskRequestjobtasksIdreject_Post
  - ReopenJobTaskRequestjobtasksIdreopen_Post
  - AddJobCommentRequestjobtasksIdcomments_Post
  - AddJobDocumentsRequestjobtasksIddocuments_Post
  - AddJobImagesRequestjobtasksIdimages_Post
  - GetJobDocumentsjobtasksIddocuments_Get
  - GetJobImagesjobtasksIdimages_Get
  - JobTaskManagerRequestjobtasksIdmembers_Get
---

# Run a PropertyMe maintenance job end to end

Job tasks are PropertyMe's maintenance work orders and the largest write surface in the API. Every
operation here needs `activity:read`, and every state transition needs `activity:write`.

## Which version to use

Job tasks are the one resource with a v2 shape, and both generations are live with neither
deprecated. Use v2 for the core read/write path:

- `AddJobV2Requestjobtasks_Post` — `POST /v2/jobtasks`
- `ChangedJobTaskV2Requestjobtasks_Get` — `GET /v2/jobtasks?Timestamp=`
- `SearchJobTaskV2Requestjobtaskssearch_Get` — `GET /v2/jobtasks/search`
- `JobTaskV2RequestjobtasksId_Get` — `GET /v2/jobtasks/{Id}`
- `UpdateJobV2RequestjobtasksId_Post` — `POST /v2/jobtasks/{Id}`

The v1 equivalents (`AddJobRequestjobtasks_Post`, `JobTaskRequestjobtasksId_Get`,
`UpdateJobRequestjobtasksId_Post`, `SearchJobTaskRequestjobtaskssearch_Get`,
`ChangedJobTaskRequestjobtasks_Get`) still work. Everything else — transitions, quotations,
attachments — exists only under `/v1/jobtasks/{Id}/…` and is shared by both shapes.

## 1. Raise the job

`POST /v2/jobtasks` with the lot the work is against and the job detail.

**There is no idempotency contract.** No `Idempotency-Key` header exists on this or any other
PropertyMe write. If the call times out, do **not** blind-retry — re-read with
`SearchJobTaskV2Requestjobtaskssearch_Get` filtered on `DateCreatedMin` / `DateCreatedMax` and
confirm whether the job landed before trying again.

## 2. Quote it

- `AddJobTasksQuotationRequestjobtasksIdquotation_Post` — `POST /v1/jobtasks/{Id}/quotation` —
  record a supplier's quote.
- `GetJobTasksQuotationRequestjobtasksIdquotations_Get` — `GET /v1/jobtasks/{Id}/quotations` — read
  the quotes on the job.
- `UpdateJobTasksQuotationRequestjobtasksIdquotationQuoteId_Post` —
  `POST /v1/jobtasks/{Id}/quotation/{QuoteId}` — revise one.
- `AddJobTasksQuotationDocumentRequestjobtasksIdquotationQuoteIddocuments_Post` —
  `POST /v1/jobtasks/{Id}/quotation/{QuoteId}/documents` — attach the supplier's quote document.

Suppliers are contacts. Resolve them with `QuerySuppliersRequestcontactssuppliers_Get`
(`GET /v1/contacts/suppliers`, `contact:read`).

## 3. Drive the state machine

All of these are `POST /v1/jobtasks/{Id}/<action>` and all need `activity:write`:

| Action | Operation |
|---|---|
| approve | `ApproveJobTaskRequestjobtasksIdapprove_Post` |
| assign | `AssignTaskRequestjobtasksIdassign_Post` |
| complete | `CompleteJobTaskRequestjobtasksIdcomplete_Post` |
| reject | `RejectJobTaskRequestjobtasksIdreject_Post` |
| reopen | `ReopenJobTaskRequestjobtasksIdreopen_Post` |

The contract does not publish the permitted transition graph. Read current state with
`JobTaskV2RequestjobtasksId_Get` before transitioning, and treat a `400` ("Invalid job id" /
"Invalid job request") as "that transition is not valid from here" rather than retrying it.

`JobTaskManagerRequestjobtasksIdmembers_Get` (`GET /v1/jobtasks/{Id}/members`) tells you which agency
member owns the job before you assign or escalate.

## 4. Attach evidence

- `AddJobDocumentsRequestjobtasksIddocuments_Post` — `POST /v1/jobtasks/{Id}/documents`
- `AddJobImagesRequestjobtasksIdimages_Post` — `POST /v1/jobtasks/{Id}/images`
- `AddJobCommentRequestjobtasksIdcomments_Post` — `POST /v1/jobtasks/{Id}/comments`
- Read them back with `GetJobDocumentsjobtasksIddocuments_Get` and `GetJobImagesjobtasksIdimages_Get`.

## 5. Find work to do

`SearchJobTaskV2Requestjobtaskssearch_Get` accepts `DueDateMin`, `DueDateMax`, `DateCreatedMin`,
`DateCreatedMax`, `Status`, `Priority`, `Offset` and `Limit`. This is one of the twelve operations
that pages: there is no total count and no next link, so page until a short page returns.

## Errors

`400` on these operations means an invalid job id or an invalid request body — fix it, do not retry.
`500` is "An internal error has occurred" — back off and retry. Nothing here returns
`application/problem+json`; read the status code.

## Billing the work

Posting the supplier's invoice against the trust ledger is a separate scope and a separate skill —
see `propertyme-post-supplier-bill.md`.
