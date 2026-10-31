# Datavant

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

Datavant is a United States health-data logistics company, formed from the 2021 merger of Datavant and Ciox Health, that connects and de-identifies healthcare data across a "network of networks" spanning 350+ real-world data partners, 80,000+ hospitals and clinics, and a majority of the largest US health systems. Its core capabilities are privacy-preserving record linkage using Datavant tokens, HIPAA Expert Determination and de-identification, medical record retrieval / release of information, and real-world evidence generation for life sciences, payers, providers, and government. Datavant publishes a real machine-readable contract for the retrieval side of that business: an OpenAPI 3.1.0 document for the Datavant REST API (36 paths, 54 operations, OAuth 2.0 client credentials, base https://api.datavant.io/v2) is served anonymously at https://developer.datavant.com/openapi.json, even though that host's root returns HTTP 404. Access is still enterprise and contract-gated - there is no self-serve sign-up, no published pricing beyond a single AWS Marketplace listing, no published scopes or rate limits, and no FHIR CapabilityStatement. Home market is the United States.

**Areas:** Healthcare · United States · Interoperability · Health Data · De-Identification · Tokenization · Real-World Data · Record Retrieval · Data Connectivity · Life Sciences · HIPAA · Medical Records

## Datavant on APIs.io

Everything profiled for Datavant is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/datavant/](https://apis.io/providers/datavant/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Datavant Configuration API](https://apis.io/apis/datavant/datavant-configuration-api/) — This is an object representing account settings and default order preferences associated with your account.
- [Datavant Documents API](https://apis.io/apis/datavant/datavant-documents-api/) — Interact with the various documents that are collected and/or generated by Datavant through the identified patient record retrieval process including…
- [Datavant Order Queries API](https://apis.io/apis/datavant/datavant-order-queries-api/) — An order query is a request for medical records for a specific patient, but the exact encounter criteria are unknown and/or are flexible. It's possible,…
- [Datavant Orders API](https://apis.io/apis/datavant/datavant-orders-api/) — An order is the parent container for incoming queries. An order contains one or more queries along with other identifying information (e.g., type, reason, etc.)
- [Datavant Prematch API](https://apis.io/apis/datavant/datavant-prematch-api/) — The Prematch API from Datavant — 4 operation(s) for prematch.
- [Datavant Projects API](https://apis.io/apis/datavant/datavant-projects-api/) — A project is an optional entity that allows you to group multiple orders together and apply consistent configuration to all nested orders.
- [Datavant Visits API](https://apis.io/apis/datavant/datavant-visits-api/) — A visit is a record of the actions performed by Datavant to obtain charts from a specific clinic within the specified date ranges for a specific patient.
- [Datavant Oauth2 API](https://apis.io/apis/datavant/datavant-oauth2-api/) — The Oauth2 API from Datavant — 1 operation(s) for oauth2.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [datavant](https://apis.io/servers/datavant/datavant/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Datavant Agentic Access](https://apis.io/agentic-access/datavant/datavant-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Datavant Rest Api Overlay](https://apis.io/overlays/datavant/datavant-rest-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Datavant Authentication](https://apis.io/security/datavant/datavant-authentication/)
- [Datavant Domain Security](https://apis.io/security/datavant/datavant-domain-security/)
- [Datavant Trust Center](https://apis.io/security/datavant/datavant-trust-center/)
- [Datavant Vulnerability Disclosure](https://apis.io/security/datavant/datavant-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Datavant Scopes](https://apis.io/scopes/datavant/datavant-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Datavant Plans Pricing](https://apis.io/plans/datavant/datavant-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Datavant Rate Limits](https://apis.io/rate-limits/datavant/datavant-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.datavant.com/)
- [Documentation](https://developer.datavant.com/docs)
- [API reference](https://developer.datavant.com/docs)
- [Pricing](https://aws.amazon.com/marketplace/pp/prodview-eolcfagze2ihw)
- [Blog](https://www.datavant.com/blog)
- [Security](https://www.datavant.com/report-vulnerabilities)
- [Compliance](https://www.datavant.com/about/privacy-compliance)
- [Trust centre](https://trust.datavant.com/)
- [Terms of service](https://www.datavant.com/terms-of-use)
- [Privacy policy](https://www.datavant.com/privacy-policy)
- [GitHub](https://github.com/datavant)
- [Status](https://status.datavant.com/)

## API Evangelist rating

**Kin Score 56.5** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 50.3 |
| Contract governance | 18.2 |
| Operational transparency | 44.7 |
| Developer ergonomics | 54.2 |
| Access clarity | 67.1 |

**Agent readiness 34.5** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | negotiable (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
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
| Pagination Ratio | partial |
| Operation Distinctiveness | mostly (partial credit) |

**Regulatory alignment 46.1** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/datavant/](https://apis.io/providers/datavant/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Datavant: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/datavant/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
