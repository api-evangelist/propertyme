---
name: Schedule, conduct and close a PropertyMe inspection
description: Create a routine or entry/exit inspection against a property, move it through schedule, reschedule, inspect, close and reopen, produce and update the inspection report, and attach comments, documents and images.
api: openapi/propertyme-openapi.json
generated: '2026-07-26'
method: generated
operations:
  - AddInspectionRequestinspections_Post
  - ChangedInspectionsRequestinspections_Get
  - GetInspectionRequestinspectionsId_Get
  - UpdateInspectionRequestinspectionsid_Post
  - DeleteInspectionRequestinspectionsid_Delete
  - SearchInspectionsRequestinspectionssearch_Get
  - QueryInspectionRequestinspectionsquery_Get
  - GetInspectionsRequestinspectionsstatusStatus_Get
  - ScheduleInspectionRequestinspectionsidschedule_Post
  - RescheduleInspectionRequestinspectionsidreschedule_Post
  - InspectInspectionRequestinspectionsidinspect_Post
  - CloseInspectionRequestinspectionsidclose_Post
  - ReopenInspectionRequestinspectionsidreopen_Post
  - CreateInspectionReportRequestinspectionsIdreport_Post
  - UpdateInspectionReportRequestinspectionsIdreport_Create
  - GetInspectionReportRequestinspectionsreportsId_Get
  - GetInspectionManagerRequestinspectionsMembersId_Get
  - AddInspectionCommentRequestinspectionsIdcomments_Post
  - AddInspectionDocumentRequestinspectionsIddocuments_Post
  - GetInspectionsImagesinspectionsIdimages_Get
---

# Schedule, conduct and close a PropertyMe inspection

Inspections are the routine and entry/exit inspection lifecycle for a managed property. Reads need
`activity:read`; every state transition and write needs `activity:write`.

## 1. Create

`AddInspectionRequestinspections_Post` — `POST /v1/inspections` — against the lot being inspected.

There is no idempotency key on this write. If the POST times out, find out whether it landed with
`SearchInspectionsRequestinspectionssearch_Get` (`GET /v1/inspections/search?LotId=…`) before
retrying.

## 2. Drive the lifecycle

All transitions are `POST /v1/inspections/{id}/<action>`:

| Action | Operation | Meaning |
|---|---|---|
| schedule | `ScheduleInspectionRequestinspectionsidschedule_Post` | Book the inspection in |
| reschedule | `RescheduleInspectionRequestinspectionsidreschedule_Post` | Move an already-scheduled inspection |
| inspect | `InspectInspectionRequestinspectionsidinspect_Post` | Mark it conducted |
| close | `CloseInspectionRequestinspectionsidclose_Post` | Finalise |
| reopen | `ReopenInspectionRequestinspectionsidreopen_Post` | Undo a close |

The permitted transition graph is not published. Read current state with
`GetInspectionRequestinspectionsId_Get` (`GET /v1/inspections/{Id}`) first and treat a `400`
("Invalid inspection id" / "Invalid inspection request") as an illegal transition, not a transient
failure.

`UpdateInspectionRequestinspectionsid_Post` (`POST /v1/inspections/{id}`) edits the inspection
itself; `DeleteInspectionRequestinspectionsid_Delete` (`DELETE /v1/inspections/{id}`) removes it.

## 3. The report

- `CreateInspectionReportRequestinspectionsIdreport_Post` — `POST /v1/inspections/{Id}/report`
- `UpdateInspectionReportRequestinspectionsIdreport_Create` — `PUT /v1/inspections/{Id}/report`
  (note the operationId says `_Create` but the method is `PUT`)
- `GetInspectionReportRequestinspectionsreportsId_Get` — `GET /v1/inspections/reports/{Id}` — the
  report is fetched by **report** id on its own path, not under the inspection.

## 4. Find inspections

- `ChangedInspectionsRequestinspections_Get` — `GET /v1/inspections?Timestamp=` — the change-since
  feed for your sync. Required int64 `Timestamp`, strictly greater-than.
- `SearchInspectionsRequestinspectionssearch_Get` — `GET /v1/inspections/search` — by `ContactId` or
  `LotId`, with `Offset` / `Limit`.
- `QueryInspectionRequestinspectionsquery_Get` — `GET /v1/inspections/query` — the rich filter:
  `DueDateMin`, `DueDateMax`, `Type`, `Status`, `Published`, `MemberOrTeam`, `TaskMemberOrTeam`,
  `Label`, `Offset`, `Limit`.
- `GetInspectionsRequestinspectionsstatusStatus_Get` — `GET /v1/inspections/status/{Status}` — one
  status bucket at a time.

All three filtered reads page with `Offset` / `Limit` and return no total and no next link — page
until a page comes back short.

## 5. Attachments and ownership

- `AddInspectionCommentRequestinspectionsIdcomments_Post` — `POST /v1/inspections/{Id}/comments`
- `AddInspectionDocumentRequestinspectionsIddocuments_Post` — `POST /v1/inspections/{Id}/documents`
- `GetInspectionsImagesinspectionsIdimages_Get` — `GET /v1/inspections/{Id}/images`
- `GetInspectionManagerRequestinspectionsMembersId_Get` — `GET /v1/inspections/Members/{Id}` — the
  agency member responsible. Note the capitalised `Members` segment.

## Follow-on work

An inspection that finds a defect usually becomes a maintenance job — see
`propertyme-maintenance-job-lifecycle.md`.
