# Revert (revert-api)

Revert is an open-source unified API for building product integrations. A single set of REST endpoints normalizes third-party CRMs (Salesforce, HubSpot, Pipedrive, Zoho, Close), chat/messaging (Slack, Discord, MS Teams), ticketing (Jira, Trello, Linear, ClickUp), accounting, and ATS providers into unified data models, while Revert manages OAuth connections, token refresh, retries, and a passthrough proxy for provider-native calls.

## Access model (read this first)

- **Open source, self-hostable (live):** The core Revert platform is **AGPL-3.0** and self-hostable via Docker Compose — [github.com/revertinc/revert](https://github.com/revertinc/revert). The self-hosted backend serves the same REST API described here on `SERVER_PORT` (default `4001`). This is the live, community-supported path today.
- **Hosted Revert Cloud (RETIRED):** The managed service at `https://api.revert.dev` (plus `docs.revert.dev` and the app) has been **shut down**. **Revert has joined Ampersand** — the repository README directs new users to [docs.withampersand.com](https://docs.withampersand.com/). As of **2026-07-12**, `revert.dev`, `www.revert.dev`, `api.revert.dev`, and `docs.revert.dev` return **no DNS A records** (the apex has only an SOA at Namecheap default nameservers; subdomains are NXDOMAIN). The hosted API and hosted docs are therefore offline.

This catalog entry documents the API **from the open-source repository**, which remains available. Every path, method, header, and unified field below was read directly from the repo's Fern API definition (`fern/definition/`) and the Express route registration (`packages/backend/routes/index.ts`). Response payload schemas in the OpenAPI are modeled from the unified type definitions and simplified where noted (flagged in `review.yml`).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/revert-api/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/revert-api/refs/heads/main/apis.yml)

## Authentication

- `x-revert-api-token` — your Revert API key (private token) for the environment. **Required** on every request.
- `x-revert-t-id` — the tenant / customer id of the linked connection. Required on unified data endpoints and connection endpoints.
- `x-api-version` — optional Revert API version selector; defaults to latest.

Revert does not use OAuth bearer tokens for its own API. End-user OAuth to the downstream provider is handled by Revert during the connect flow (the frontend connect SDK uses a separate **public token**), and Revert transparently refreshes those tokens.

## Base URL

- `https://api.revert.dev` — Revert Cloud (hosted) — **RETIRED, does not resolve as of 2026-07-12**.
- `http://localhost:4001` — self-hosted default (`SERVER_PORT=4001`).

## WebSocket

**No.** Revert exposes REST over HTTPS plus HTTP **Server-Sent Events** (via `better-sse`) for OAuth connect status (`/connection/integration-status/{revertPublicToken}` and per-module `integration-status` endpoints), and Svix **webhooks** for connection events. SSE is one-way HTTP streaming, not WebSocket. No `ws://`/`wss://` endpoint exists. See `review.yml`.

## Tags

- Unified API
- Embedded iPaaS
- Integrations
- Product Integrations
- Open Source
- CRM Integrations
- Connectors
- API Integration

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Revert Unified CRM API

Unified CRM data models across Salesforce, HubSpot, Pipedrive, Zoho, and Close — contacts, leads, companies, deals, notes, events, tasks, and users — each with list, get, create, update, and unified search operations under `/crm`.

- **Human URL:** [https://github.com/revertinc/revert](https://github.com/revertinc/revert)
- **Base URL:** `https://api.revert.dev`

### Revert Unified Chat API

Unified messaging models for Slack, Discord, and Microsoft Teams — list channels (`GET /chat/channels`), list users (`GET /chat/users`), and post a message (`POST /chat/message`).

- **Base URL:** `https://api.revert.dev`

### Revert Unified Ticketing API

Unified ticketing models for Jira, Trello, Linear, and ClickUp — tasks, users, comments, and collections under `/ticket`, plus a ticketing passthrough proxy.

- **Base URL:** `https://api.revert.dev`

### Revert Connection Management API

Manage linked third-party connections per tenant — get a connection (`GET /connection`), list all (`GET /connection/all`), delete, connection webhooks (`/connection/webhook`), bulk import (`/connection/import`), and SSE connect status.

- **Base URL:** `https://api.revert.dev`

### Revert Metadata API

Retrieve the integrations/app configuration available for an environment from its public token — `GET /metadata/crms`.

- **Base URL:** `https://api.revert.dev`

### Revert Passthrough Proxy API

Forward requests to the underlying provider's native API using Revert's managed connection and refreshed OAuth token — `POST /crm/proxy`, `POST /ticket/proxy`, `POST /accounting/proxy`, `POST /ats/proxy`.

- **Base URL:** `https://api.revert.dev`

### Revert Unified Accounting API

Unified accounting models (accounts, expenses, vendors) and a passthrough proxy under `/accounting` — an additional integration category in the open-source backend.

- **Base URL:** `https://api.revert.dev`

### Revert Unified ATS API

Unified applicant-tracking models (departments, candidates, offers, jobs) and a passthrough proxy under `/ats`.

- **Base URL:** `https://api.revert.dev`

## Common Properties

- [GitHub Organization](https://github.com/revertinc)
- [LinkedIn](https://www.linkedin.com/company/revertinc)
- [Website](https://revert.dev) (domain no longer resolves)
- [Documentation](https://github.com/revertinc/revert)
- [Authentication](authentication/revert-api-authentication.yml)
- [Domain Security](security/revert-api-domain-security.yml)
- [Plans](plans/revert-api-plans-pricing.yml)
- [Rate Limits](rate-limits/revert-api-rate-limits.yml)
- [Fin Ops](finops/revert-api-finops.yml)

## Artifacts

- [OpenAPI](openapi/revert-api-openapi.yml)
- [Postman Collection](collections/revert-api.postman_collection.json)
- [Open Collection](collections/revert-api.opencollection.json)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
