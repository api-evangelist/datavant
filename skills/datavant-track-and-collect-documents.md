---
name: Track a Datavant order and collect the retrieved documents
description: Page through orders, inspect per-query visit activity, then list and download the medical record documents Datavant has retrieved.
api: openapi/datavant-rest-api-openapi.yml
base_url: https://api.datavant.io/v2
operations:
  - list_orders_orders__get
  - get_order_orders__order_id__get
  - get_order_retrievals_reporting_data_orders__order_id__reporting_retrievals_get
  - get_query_visits_projects__project_id__queries__query_id__visits_get
  - get_visit_details_projects__project_id__queries__query_id__visits__visit_uuid__get
  - list_documents_documents_get
  - download_document_documents__document_uuid__get
generated: '2026-08-14'
method: generated
source: openapi/datavant-rest-api-openapi.yml
---

# Track a Datavant order and collect the retrieved documents

Read-only follow-through on an order that is already submitted. Everything here returns
PHI — treat every response body as protected data and do not log it.

## 1. Find the order

`list_orders_orders__get` (`GET /orders/`) is paginated with `limit` (default **50**,
maximum **100**) and `offset`. Responses are wrapped in the `JsonApiPage` envelope:

```
{ "data": [...], "total": N, "limit": 50, "offset": 0,
  "unfiltered_total": M, "links": { "first": ..., "next": ..., "prev": ..., "last": ..., "self": ... } }
```

Follow `links.next` until it is absent; do not compute offsets yourself. Filter with
`status`, `order_name`, `project_name`, `secondary_reason`; sort with `sort_by` and
`sort_dir`.

Prematch orders are **excluded** from this list — use
`list_prematch_orders_orders_prematch_all_get` (`GET /orders/prematch/all`) for those.

`get_order_orders__order_id__get` (`GET /orders/{order_id}`) fetches one by your own
external id. `get_order_by_specific_id_orders_order_get` (`GET /orders/order`) takes the
identifier as a query parameter instead, which is the form to use when you hold a UUID.

## 2. See what retrieval actually did

- `get_order_retrievals_reporting_data_orders__order_id__reporting_retrievals_get`
  (`GET /orders/{order_id}/reporting/retrievals`) — reporting data for the order,
  with a `breakdown` query parameter.
- `get_query_visits_projects__project_id__queries__query_id__visits_get`
  (`GET /projects/{project_id}/queries/{query_id}/visits`) — every visit for a query. A
  *visit* is the work Datavant did against one clinic for one patient over a date range.
- `get_visit_details_projects__project_id__queries__query_id__visits__visit_uuid__get`
  — one visit in full, including `VisitStatus`, `VisitHistoryEntry` and
  `VisitRetrievalDetails` (in-network vs out-of-network `retrieval_id`).

Visits are the honest progress signal. Order status tells you the container moved;
visits tell you whether records are actually coming back.

## 3. List the documents

`list_documents_documents_get` (`GET /documents`) lists documents associated with an
order or an order query — filter with `order_id`, `order_uuid`, `query_id`,
`query_uuid`, `chase_uuid`. Same `JsonApiPage` envelope, same `limit`/`offset` rules.

Each `DocumentResponse` carries a `DocumentType` and an indicator of whether the chart
has already been downloaded — use it to make your collection loop resumable instead of
re-pulling every document each run.

## 4. Download

`download_document_documents__document_uuid__get`
(`GET /documents/{document_uuid}`) returns the document itself.

- **400** — the UUID is malformed. Fix the input; do not retry.
- **404** — the document does not exist *for this tenant*. Retrying will not change it.
- **501** — "Document exists in an unsupported document store." This is not transient
  either; the docs direct you to contact support. Record it and move on rather than
  looping.

## Pull vs push

You do not have to poll. An order's delivery configuration (`AllDeliveryConfig`) can push
results to SFTP, S3, email or portal download on a schedule — daily, weekly, biweekly,
monthly, by due date, or per document. There is **no webhook and no event stream**: if
you want push, configure delivery on the order rather than waiting for a callback that
does not exist.

## Cautions

- No rate limits are published and no `429` is defined. Keep concurrency low and space
  out polling; there is no `Retry-After` to tell you when to come back.
- The `version-datavant` header pins the API version; omitted it defaults to
  `2023-04-01`.
- Errors are `{"errors": [ ... ]}` arrays — read every entry.
