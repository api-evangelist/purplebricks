---
name: Generate a Purplebricks conveyancing quote
description: Create and retrieve conveyancing quotes with their fee groups for the legal leg of a Purplebricks
  sale.
api: openapi/purplebricks-conveyancing-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/conveyancing-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- postV1ConveyancingquoteQuote
- getV1ConveyancingquoteQuotes
- getV1ConveyancingquoteQuoteQuoteId
generated: '2026-07-26'
method: generated
---

# Generate a Purplebricks conveyancing quote

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/conveyancing-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-conveyancing-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
Produces the conveyancing quote bundled into a Purplebricks selling package — the legal leg of the
transaction, priced per property, tenure and contact set.

## Steps
1. **Create the quote.** `POST /v1/conveyancingquote/quote` (`postV1ConveyancingquoteQuote`) with a
   `CreateQuoteRequestModel`: the property (`CreateQuotePropertyRequestModel`, carrying
   `TenureTypeModel` and `PropertyQuoteTypeModel`) and the contacts
   (`CreateQuoteContactRequestModel` with `ContactTypeModel` and `PositionTypeModel`). Returns `201`.
2. **Read it back.** `GET /v1/conveyancingquote/quote/{quoteId}`
   (`getV1ConveyancingquoteQuoteQuoteId`). The response carries `ConveyancingQuoteFeeGroupModel`
   entries, each holding `ConveyancingQuoteFeeModel` line items with a `FeeTypeModel`. Present fees
   **grouped**, never as a single flattened number.
3. **List** with `GET /v1/conveyancingquote/quotes` (`getV1ConveyancingquoteQuotes`) and check
   `QuoteStatusTypeModel` before treating a quote as current.

## Do not
- Do not re-`POST` on a timeout. There is no idempotency key; re-read the list first, since a second
  quote for the same property is a real duplicate and a customer-visible pricing error.
