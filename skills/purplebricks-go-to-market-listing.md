---
name: Take a Purplebricks property to market
description: Drive a property from instruction through market price agreement, media and copy approval
  to a live advert using the Purplebricks Property API go-to-market surface.
api: openapi/purplebricks-property-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/property-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- getV1PropertyPropertyId
- getV1GotomarketPropertyIdProgress
- getV1GotomarketPropertyIdMarketprice
- postV1GotomarketPropertyIdMarketpriceAccept
- postV1GotomarketPropertyIdApproveall
- getV1GotomarketPropertyIdAdvertsummary
- getV1ExposurereportPropertyId
generated: '2026-07-26'
method: generated
---

# Take a Purplebricks property to market

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/property-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-property-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
`GoToMarket` is the launch pipeline for a listing: agree the price, review ownership, gather
floorplan/description/key features and images, approve, and go live. It is the highest-value flow in
the 86-path Property API.

## Steps
1. **Load the property.** `GET /v1/property/{propertyId}` (`getV1PropertyPropertyId`), and
   `GET /v1/property/{propertyId}/address` for the resolved address.
2. **Read pipeline state first.** `GET /v1/gotomarket/{propertyId}/progress`
   (`getV1GotomarketPropertyIdProgress`). Every subsequent write is state-dependent and returns
   `409 Conflict` when taken out of order — always read progress before writing.
3. **Agree the market price.** `GET /v1/gotomarket/{propertyId}/marketprice`
   (`getV1GotomarketPropertyIdMarketprice`), then exactly one of
   `POST /v1/gotomarket/{propertyId}/marketprice/accept` (`postV1GotomarketPropertyIdMarketpriceAccept`)
   or `.../marketprice/reject`.
4. **Clear the ownership review.** `GET /v1/gotomarket/{propertyId}/ownershipreview`, then
   `.../ownershipreview/accept` or `.../ownershipreview/reject`.
5. **Assemble the advert.** `GET .../floorplan`, `GET .../propertydescription`,
   `GET .../propertykeyfeatures`; upload images with `GET .../getimageuploaddetails` followed by
   `POST .../saveimagedetails`. Preferences for board and key safe go through
   `POST .../boardandkeysafepreference`; council tax band through `POST .../propertytaxband`.
6. **Approve.** `POST /v1/gotomarket/{propertyId}/approveall` (`postV1GotomarketPropertyIdApproveall`)
   — a single irreversible gate. Confirm with `GET .../advertsummary`
   (`getV1GotomarketPropertyIdAdvertsummary`).
7. **Measure.** `GET /v1/exposurereport/{propertyId}` (`getV1ExposurereportPropertyId`) for exposure,
   `POST /v1/exposurereport/{propertyId}/generate` to force a refresh, and
   `GET /v1/property/getpropertyhistory` for the listing timeline.

## Do not
- Do not call the `InternalPerformanceReport` operations (`/v1/performancereport/internal/*`) for a
  seller — they are internal agent-performance analytics.
- `POST /v1/property/resetproperty/{propertyId}/{userId}` is destructive. Never call it as part of a
  go-to-market flow.
