---
name: Work a Purplebricks message thread
description: List threads, read and page messages, post replies and mark threads read using the Purplebricks
  Messaging API cursor pagination.
api: openapi/purplebricks-messaging-v1-openapi.yml
base_url: https://api.purplebricks.co.uk/messaging-api
auth: 'Authorization: Bearer <token> (no public issuance)'
operations:
- getV1Threads
- getV1ThreadsThreadId
- getV1ThreadsThreadIdMessages
- patchV1ThreadsThreadIdRead
- postV1Messages
- getV1MessagesCount
generated: '2026-07-26'
method: generated
---

# Work a Purplebricks message thread

> **Access reality check.** Every Purplebricks operation below is protected by a global
> `Authorization: Bearer <token>` requirement. Purplebricks runs no developer programme, no
> signup, no key issuance and no OAuth token endpoint, so an agent can only execute this skill
> with a token issued inside Purplebricks. Unauthenticated calls return `401` with an ASP.NET
> `ProblemDetails` body. Do not attempt to obtain a token by any other means.

**Conventions that apply to every step**
- Base URL: `https://api.purplebricks.co.uk/messaging-api`
- Auth: `Authorization: Bearer <token>` (declared as an apiKey-in-header scheme in the spec).
- Errors: RFC 7807 `ProblemDetails` (`type`, `title`, `status`, `detail`, `instance`), served as
  `application/json`. See `errors/purplebricks-problem-types.yml`.
- **No idempotency key exists anywhere in the estate** — never blind-retry a `POST`. A duplicate
  create surfaces as `409 Conflict`; treat `409` as "already done", not as a failure to retry.
- Rate limiting is signalled only by `429`; no quota headers are published. Back off on `429`.
- The upstream documents declare almost no `operationId`s. The ids used below are the ones this
  repo's overlay (`overlays/purplebricks-messaging-v1-overlay.yaml`) assigns; the method + path is
  the authoritative binding.

## What this does
Threaded messaging between a seller or buyer and their local property expert.

## Steps
1. **List threads.** `GET /v1/threads` (`getV1Threads`). Pagination here is **cursor-based**:
   pass `limit` and the returned `cursor`; do not use `page`/`pageSize` on this service (those belong
   to the Property and Valuations APIs). `includeSystemMessages` controls whether platform-generated
   messages appear — default to excluding them when summarising for a human.
2. **Open a thread.** `GET /v1/threads/{threadId}` (`getV1ThreadsThreadId`), then
   `GET /v1/threads/{threadId}/messages` (`getV1ThreadsThreadIdMessages`) with the same cursor
   discipline. `.../messages/count` gives an unread/total count without pulling bodies.
3. **Reply.** `POST /v1/messages` (`postV1Messages`). Not idempotent — a repeated POST posts a second
   message. On timeout, re-read the thread before resending.
4. **Mark read.** `PATCH /v1/threads/{threadId}/read` (`patchV1ThreadsThreadIdRead`) — the one PATCH
   in the estate, and safely repeatable.
5. **Badge counts.** `GET /v1/messages/count` (`getV1MessagesCount`), filtered by `isRead`.
6. Agent-side annotations live on `/v1/messages/{messageId}/notes`; these are internal and must not be
   surfaced to a customer.

## Version note
`v2` serves a narrower `Threads` surface at the same base URL. Prefer `v1` unless you have a specific
reason for the `v2` shape; both are live.
