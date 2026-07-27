---
name: Resolve a Purplebricks property to its portal branches
description: Look up the Rightmove, Zoopla and OnTheMarket branch a Purplebricks property syndicates through,
  and search the branch registries.
api: openapi/purplebricks-branch-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/branch-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- getV1PropertyPropertyIdRightmovebranch
- getV1PropertyPropertyIdZooplabranch
- getV1PropertyPropertyIdOnthemarketbranch
- getV1RightmovebranchBranchId
- getV1RightmovebranchSearch
generated: '2026-07-26'
method: generated
---

# Resolve a Purplebricks property to its portal branches

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/branch-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-branch-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
The UK has no MLS, so a listing reaches buyers by being syndicated into the portals. The Branch API is
the machine-readable trace of that seam: it maps a Purplebricks property onto the Rightmove, Zoopla and
OnTheMarket branch it is advertised under.

## Steps
1. **Property to branch, per portal.**
   - `GET /v1/property/{propertyId}/rightmovebranch` (`getV1PropertyPropertyIdRightmovebranch`)
   - `GET /v1/property/{propertyId}/zooplabranch` (`getV1PropertyPropertyIdZooplabranch`)
   - `GET /v1/property/{propertyId}/onthemarketbranch` (`getV1PropertyPropertyIdOnthemarketbranch`)
   A `404` here means the property is not syndicated to that portal — it is a meaningful business
   answer, not an error to retry.
2. **Branch registries.** `GET /v1/rightmovebranch`, `/v1/zooplabranch`, `/v1/onthemarketbranch` list
   them; `/{branchId}` (`getV1RightmovebranchBranchId`) fetches one; `/search`
   (`getV1RightmovebranchSearch`) queries by `town` / `postCode`.
3. Every operation on this service declares `401` and `403` on all twelve paths — a `403` means the
   token is valid but not scoped to that branch. Do not retry a `403`.

## Context worth carrying
Portal-side data access is licensed by Rightmove, Zoopla and OnTheMarket, not by Purplebricks. This API
resolves the relationship; it does not grant any right to portal listing data.
