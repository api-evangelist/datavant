---
name: Submit a Datavant medical record retrieval order
description: Authenticate with client credentials, create a project and an order, attach patient queries and their authorization documents, check validation, then submit and dispatch the order for retrieval.
api: openapi/datavant-rest-api-openapi.yml
base_url: https://api.datavant.io/v2
operations:
  - get_configuration_configuration__get
  - create_project_projects__post
  - create_order_orders__post
  - add_query_to_order_orders__order_id__queries_post
  - create_patient_authorization_orders__order_id__queries__query_id__supporting_documents_patient_authorizations_post
  - get_order_validation_results_orders__order_id__validation_stats_get
  - get_order_errors_and_warnings_orders__order_uuid__error_and_warning_counts_get
  - submit_order_orders__order_id__submit_post
  - dispatch_order_orders__order_id__dispatch_post
generated: '2026-08-14'
method: generated
source: openapi/datavant-rest-api-openapi.yml
---

# Submit a Datavant medical record retrieval order

This API moves identified patient medical records. Every operation below touches PHI.
Before using it in an agent, read `agentic-access/datavant-agentic-access.yml` — the
submit and dispatch steps are classified **safety-critical** and are marked
human-in-the-loop required.

## 1. Get a token

`POST https://api.datavant.io/v2/oauth2/token` with a JSON body of
`grant_type=client_credentials`, `client_id`, `client_secret`. The response is
`{access_token, token_type: "bearer", expires_in: 7200}`. Send it as
`Authorization: Bearer <access_token>` on every call and refresh at 7200 seconds.

There are **no scopes** — one token grants all 54 operations. Do not hand this token to
a general-purpose agent loop without an external policy gate.

Optionally pin the API version with the `version-datavant` request header. If you omit
it the API uses `2023-04-01`.

## 2. Read your tenant configuration first

`get_configuration_configuration__get` (`GET /configuration/`) returns the account-level
defaults every order inherits — delivery location, delivery format, coding and QA
options. Read it before creating anything so you know what the order will do by default
rather than discovering it after dispatch.

## 3. (Optional) Create a project

`create_project_projects__post` (`POST /projects/`). A project groups orders and applies
one configuration across all of them. Use it when you are submitting a related batch;
skip it for a one-off order. The `project_id` you supply is your own external id — a
repeat of an existing id returns **409**, not an update.

## 4. Create the order

`create_order_orders__post` (`POST /orders/`). An order is the parent container for one
or more queries. You supply `order_id` as your own external id; Datavant returns an
`order_uuid` as well, and later operations are split between the two keys — read the
`operationId` carefully, some take `{order_id}` and some take `{order_uuid}`.

A new order lands in `draft` status. Everything in the next two steps only works while
it is in `draft`: mutating a non-draft order returns **422 "Order is not in 'draft'
status."**

## 5. Add one query per patient

`add_query_to_order_orders__order_id__queries_post`
(`POST /orders/{order_id}/queries`). A query is a request for records for one patient
where the exact encounter criteria may be flexible. One query can fan out into several
patient searches and chases.

For a large batch, `attach_roster_orders__order_id__roster_post`
(`POST /orders/{order_id}/roster`) uploads a roster file instead of adding queries one
at a time.

## 6. Attach the patient authorization

`create_patient_authorization_orders__order_id__queries__query_id__supporting_documents_patient_authorizations_post`
(`POST /orders/{order_id}/queries/{query_id}/supporting-documents/patient-authorizations`).

- `multipart/form-data`, and the file **must** be `application/pdf` — anything else
  returns **422 "The file is not mime type application/pdf"**.
- A duplicate filename on the same query returns **409**.
- List what is attached with
  `list_patient_authorization_filenames_orders__order_id__queries__query_id__supporting_documents_patient_authorizations_get`.

## 7. Check validation before you submit

- `get_order_validation_results_orders__order_id__validation_stats_get`
  (`GET /orders/{order_id}/validation/stats`) returns counts: `valid`, `failed`,
  `copied`, `in_progress`. `in_progress` is `-1` when the count is not yet known.
- `get_order_errors_and_warnings_orders__order_uuid__error_and_warning_counts_get`
  (`GET /orders/{order_uuid}/error-and-warning-counts`) — note this one takes the
  **UUID**.
- `get_order_validation_report_orders__order_uuid__validation_report_get` returns the
  full report.

Do not submit while `failed > 0` unless a human has reviewed the report.

## 8. Submit, then dispatch

`submit_order_orders__order_id__submit_post` (`POST /orders/{order_id}/submit`), then
`dispatch_order_orders__order_id__dispatch_post` (`POST /orders/{order_id}/dispatch`).
`dispatch_instructions_orders__order_id__start_record_retrieval_put`
(`PUT /orders/{order_id}/start-record-retrieval`) begins retrieval.

**These are not idempotent.** The API defines no `Idempotency-Key` header and no
retry-safety contract. A timeout on submit or dispatch is ambiguous: re-issuing it may
duplicate real-world record requests against providers. On a timeout, do **not** retry
blindly — re-read the order with `get_order_orders__order_id__get` and branch on its
status.

## Errors

Every 4xx/5xx returns `application/json` shaped
`{"errors": [{"code": "...", "message": "...", "params": [...]}]}` — an **array**, so
read all of them, not just the first. See `errors/datavant-problem-types.yml`.

- **400** bad request · **404** not found · **409** id already exists ·
  **422** validation or wrong order status · **501** document in an unsupported store.
- No **429** is defined and no rate-limit headers are returned, so there is no published
  backoff signal. Rate-limit yourself conservatively.

## Backing out

`cancel_order_orders__order_uuid__cancel_post` (by UUID) cancels;
`close_order_orders__order_id__close_request_post` closes a request.
