---
name: Size a Datavant retrieval with prematch, and manage tenant configuration
description: Run a prematch order to estimate how many records Datavant can find before committing to a full retrieval, and manage the tenant configuration and supporting documents that every order inherits.
api: openapi/datavant-rest-api-openapi.yml
base_url: https://api.datavant.io/v2
operations:
  - create_prematch_order_orders_prematch_post
  - list_prematch_orders_orders_prematch_all_get
  - get_prematch_result_counts_orders_prematch__order_id__counts_get
  - delete_prematch_orders_prematch__order_uuid__delete
  - get_configuration_configuration__get
  - put_configuration_configuration__put
  - get_letter_of_representation_configuration_supporting_documents_letter_of_representation_get
  - update_letter_of_representation_configuration_supporting_documents_letter_of_representation_put
  - get_health_plan_letter_configuration_supporting_documents_health_plan_letter_get
  - update_health_plan_letter_configuration_supporting_documents_health_plan_letter_put
generated: '2026-08-14'
method: generated
source: openapi/datavant-rest-api-openapi.yml
---

# Size a retrieval with prematch, and manage tenant configuration

Two setup flows that belong before any real order: find out what is findable, and make
sure the defaults every order inherits are the ones you want.

## Prematch — estimate before you commit

A prematch order asks Datavant how many records it expects to find for a patient
population, without dispatching real record requests. Prematch orders are kept
separate from normal orders: they are **excluded** from `GET /orders/`.

1. `create_prematch_order_orders_prematch_post` (`POST /orders/prematch`) —
   `multipart/form-data`. Submit the population you want sized.
2. `get_prematch_result_counts_orders_prematch__order_id__counts_get`
   (`GET /orders/prematch/{order_id}/counts`) — the result counts. Poll this; there is
   no event or webhook to tell you it finished.
3. `list_prematch_orders_orders_prematch_all_get` (`GET /orders/prematch/all`) — list
   everything you have run.
4. `delete_prematch_orders_prematch__order_uuid__delete`
   (`DELETE /orders/prematch/{order_uuid}`) — clean up. Note this one takes the
   **UUID**, while the counts operation takes the external **id**. Mixing them up
   returns a 404, not a helpful message.

Prematch is the cheap step. Run it before `submit_order_orders__order_id__submit_post`,
which is where real requests go out to providers.

## Tenant configuration — what every order inherits

`get_configuration_configuration__get` (`GET /configuration/`) returns the single
configuration object for your tenant. There is no id in the path: one tenant, one
configuration. `put_configuration_configuration__put` (`PUT /configuration/`) replaces
it.

It carries the account defaults orders inherit: `DeliveryLocation`, `DeliveryFormat`,
`DeliveryFrequency` and `DeliveryTiming`, coding options (`CodingConfiguration`),
chart-finder and allow/block settings (`ChartFinderConfiguration`,
`RetrievalAllowBlockConfiguration`, `RetrievalSources`), and extraction/report
configuration.

**`PUT` here is a whole-object replace, not a patch, and it is tenant-wide.** Read the
current configuration, modify the fields you mean to change, and send the whole object
back. An agent should treat this operation as requiring human confirmation — changing it
silently changes the behaviour of every order the account submits afterwards.

## Supporting documents on the configuration

Two account-level PDFs sit alongside the configuration, each with get / update / delete:

- **Letter of representation** —
  `get_letter_of_representation_configuration_supporting_documents_letter_of_representation_get`,
  `update_letter_of_representation_configuration_supporting_documents_letter_of_representation_put`,
  `delete_letter_of_representation_configuration_supporting_documents_letter_of_representation_delete`
- **Health plan letter** —
  `get_health_plan_letter_configuration_supporting_documents_health_plan_letter_get`,
  `update_health_plan_letter_configuration_supporting_documents_health_plan_letter_put`,
  `delete_health_plan_letter_configuration_supporting_documents_health_plan_letter_delete`

Both uploads are `multipart/form-data` and both are validated to `application/pdf` —
anything else returns **422 "The file is not mime type application/pdf"**. A missing
configuration or missing letter returns **404** with a message that names which of the
two is absent, so read `errors[].message`, not just the status.

Project-level supporting documents are separate:
`upload_supporting_document_projects__project_id__supporting_documents__document_type__put`
takes a `document_type` path parameter.

## Reminders

- Bearer token from `POST /oauth2/token`, 7200 seconds, no scopes.
- No idempotency key exists; `PUT` operations are naturally repeatable, `POST /orders/prematch` is not.
- Errors are `{"errors": [{code, message, params}]}` arrays.
