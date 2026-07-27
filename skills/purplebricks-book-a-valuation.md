---
name: Book a Purplebricks free valuation
description: Capture a seller lead, check postcode eligibility, request valuation availability and schedule
  the follow-up through the Purplebricks Valuations API.
api: openapi/purplebricks-valuations-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/valuations-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- postV1Leadcapture
- getV1PostcodeIsintrialregion
- getV1ValuationintentShoulduseinstantvaluationflow
- postV1ValuationintentRequestvaluationavailability
- postV1ValuationintentBookinguserdata
- postV1Followup
- getV1ValuationreportId
generated: '2026-07-26'
method: generated
---

# Book a Purplebricks free valuation

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/valuations-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-valuations-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
The free valuation is the front door of the whole Purplebricks model — it is where a seller
relationship starts before any listing exists. This skill walks the lead from capture to a booked
valuation appointment and its follow-up.

## Steps
1. **Check the postcode is in scope.** `GET /v1/postcode/isintrialregion` (`getV1PostcodeIsintrialregion`).
   Some journeys are region-gated; `GET /v1/salesforcediarypilot/isindiarymanagementtrial/{postcode}`
   tells you whether diary management runs through the Salesforce pilot for that postcode.
2. **Decide the flow.** `GET /v1/valuationintent/shoulduseinstantvaluationflow`
   (`getV1ValuationintentShoulduseinstantvaluationflow`) returns whether this lead takes the instant
   valuation path or the booked-appointment path. Branch on it — do not assume.
3. **Capture the lead.** `POST /v1/leadcapture` (`postV1Leadcapture`). Validation failures come back
   as `400` with FluentValidation `ValidationFailure` entries naming the offending property; fix and
   resend rather than retrying unchanged.
4. **Request availability.** `POST /v1/valuationintent/requestvaluationavailability`
   (`postV1ValuationintentRequestvaluationavailability`).
5. **Persist the booking user data.** `POST /v1/valuationintent/bookinguserdata`
   (`postV1ValuationintentBookinguserdata`). This is not idempotent — a repeat submission is a new
   record, so record the response before any retry.
6. **Schedule the follow-up.** `POST /v1/followup` (`postV1Followup`). To move an existing follow-up
   use `POST /v1/followup/reschedule`; to re-temperature a lead use `POST /v1/followup/updateleadtemp`.
7. **Read the valuation report** once produced: `GET /v1/valuationreport/{id}` (`getV1ValuationreportId`).

## Do not
- Do not call `GET /v1/followup/toaction` or `/v1/followup/lpplist` on behalf of a consumer — those
  are agent worklists, not customer data.
