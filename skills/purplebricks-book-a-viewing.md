---
name: Book and manage a Purplebricks viewing
description: Create, confirm, reschedule and cancel property viewing appointments, and read access details
  and open days, through the Purplebricks Viewings API.
api: openapi/purplebricks-viewings-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/viewings-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- getV1PropertiesPropertyIdOpendaysUpcoming
- postV1Appointments
- postV1AppointmentsCancel
- getV1AccessdetailsPropertyId
- getV1CreatioUsersUserIdViewings
generated: '2026-07-26'
method: generated
---

# Book and manage a Purplebricks viewing

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/viewings-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-viewings-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
Books a viewing against a property, and manages the appointment lifecycle afterwards.

## Steps
1. **Check open days first.** `GET /v1/properties/{propertyId}/opendays/upcoming`
   (`getV1PropertiesPropertyIdOpendaysUpcoming`). If the property runs open days, direct the buyer
   there rather than creating a one-off appointment. `.../opendays/past` gives history.
2. **Create the appointment.** `POST /v1/appointments` (`postV1Appointments`). This surface declares
   both `409 Conflict` (slot already taken) and `429 Too Many Requests` — handle both explicitly. There
   is no idempotency key, so on a network timeout re-read with
   `GET /v1/creatio/properties/{propertyId}/viewings` before resending.
3. **Cancel** with `POST /v1/appointments/cancel` (`postV1AppointmentsCancel`) — never by deleting a
   record directly.
4. **Access details.** `GET /v1/accessdetails/{propertyId}` (`getV1AccessdetailsPropertyId`) and
   `GET /v1/accessdetails/{propertyId}/summary`. `POST /v1/accessdetails/{propertyId}` writes them.
5. **Read back per user.** `GET /v1/creatio/users/{userId}/viewings`
   (`getV1CreatioUsersUserIdViewings`) and `.../viewings/feedback`.

## Do not
- **Never call `GET /v1/accessdetails/{propertyId}/revealkeysafecode`.** It discloses the physical key
  safe code for a real property. This operation is classified `safety-critical` with
  `human-in-the-loop: required` in `agentic-access/purplebricks-agentic-access.yml`; an agent must
  escalate to a human rather than call it.
- The `/v1/bland/viewing*` operations drive an automated voice agent. Do not chain them from another
  agent without an explicit human instruction.
