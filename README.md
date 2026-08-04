# Purplebricks (purplebricks)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Purplebricks is the United Kingdom's largest online (hybrid) estate agency, founded in 2014 by Michael and Kenny Bruce and David Shepherd, selling and letting residential property across England, Wales and Scotland through a fixed-fee, remote model staffed by local property experts rather than a high-street branch network. After a 2023 sale to rival online agent Strike for a nominal one pound the combined business was rebranded back under the Purplebricks name, and in 2026 the founding Bruce brothers returned alongside Sir Charles Dunstone's Freston Ventures. It sits on the brokerage rung of the value chain, originating the seller relationship and the listing and then distributing that stock to Rightmove, Zoopla, OnTheMarket and PrimeLocation through the UK's portal feed arrangements, because the UK has no MLS and therefore no shared cooperative data exchange. Its API posture is best described as published-but-undeclared: there is no developer portal, no partner programme, no signup and no documentation site, yet the estate serves sixteen real OpenAPI 3.0.1 documents and thirteen live public Swagger UIs across api.purplebricks.co.uk and www.purplebricks.co.uk, covering property, account, valuations, viewings, messaging, lettings, branch/portal syndication, communications, conveyancing, agent, feedback, workflow and Outlook calendar sync. Every operation is Bearer-token protected with no public key issuance, errors are ASP.NET ProblemDetails (RFC 7807), and the branch service exposes the Rightmove, Zoopla and OnTheMarket syndication seam directly. RESO is absent, as expected outside North America, and the genuinely open UK property data layer it consumes for its sold-prices tool belongs to government (HM Land Registry), not to Purplebricks.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/purplebricks/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/purplebricks/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- Property Listings
- Online Estate Agency
- Rentals
- Lettings
- PropTech
- Mortgage
- Conveyancing
- Land Registry
- OpenAPI
- Microservices
- Swagger
- Azure

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

**Correction (2026-07-26 enrichment round).** The first-round review recorded "no public API,
zero specs harvested". That was wrong, and the correction is the headline finding of this repo.
Purplebricks serves **sixteen real OpenAPI 3.0.1 documents** and **thirteen live public Swagger
UIs** from `api.purplebricks.co.uk` (mirrored on `www.purplebricks.co.uk`), covering **194 paths
and 205 operations**. The first round probed only `/swagger`, `/swagger/v1/swagger.json` and
`/openapi.json` at the *host root* — which 404s behind Azure Front Door. The specs live one level
down, per microservice, at `https://api.purplebricks.co.uk/<service>-api/swagger/v1/swagger.json`.
The thread that led there was the single robots.txt disclosure, `/outlook-api/v1/agent/outlook-sync`:
its sibling `/outlook-api/swagger/v1/swagger.json` returns a parseable OpenAPI document, and the
same pattern holds for twelve more services.

| Service | Paths | Swagger UI |
|---|---|---|
| Property (v1 + v2) | 97 | https://api.purplebricks.co.uk/property-api/swagger/index.html |
| Valuations (v1 + v2) | 22 | https://api.purplebricks.co.uk/valuations-api/swagger/index.html |
| Account | 15 | https://api.purplebricks.co.uk/account-api/swagger/index.html |
| Viewings | 14 | https://api.purplebricks.co.uk/viewings-api/swagger/index.html |
| Branch (Rightmove / Zoopla / OnTheMarket) | 12 | https://api.purplebricks.co.uk/branch-api/swagger/index.html |
| Messaging (v1 + v2) | 11 | https://api.purplebricks.co.uk/messaging-api/swagger/index.html |
| Lettings | 9 | https://api.purplebricks.co.uk/lettings-api/swagger/index.html |
| Communications | 4 | https://api.purplebricks.co.uk/communications-api/swagger/index.html |
| Conveyancing | 3 | https://api.purplebricks.co.uk/conveyancing-api/swagger/index.html |
| Workflow | 3 | https://api.purplebricks.co.uk/workflow-api/swagger/index.html |
| Agent | 2 | https://api.purplebricks.co.uk/agent-api/swagger/index.html |
| Feedback | 1 | https://api.purplebricks.co.uk/feedback-api/swagger/index.html |
| Outlook Sync | 1 | https://api.purplebricks.co.uk/outlook-api/swagger/index.html |

## Access posture — published-but-undeclared

These are production first-party services that are publicly reachable and publicly
self-documenting, **not** a developer product. Every operation carries a global
`Authorization: Bearer <token>` requirement and Purplebricks issues no tokens to third parties:
no developer portal, no partner programme, no signup, no key issuance, no OAuth token endpoint,
no `/.well-known/` layer, no status page, no SLA, no deprecation policy, no changelog, no client
SDK in any language, no MCP server, no GraphQL. Treat any integration as unsupported.

## What the contracts reveal

- **Portal syndication is modelled directly.** `branch-api` exposes `RightmoveBranch`,
  `ZooplaBranch` and `OnTheMarketBranch` registries plus property-to-branch resolution — the
  machine-readable trace of the UK no-MLS seam.
- **HM Land Registry sits inside the product.** `property-api` carries a `LandRegistry` tag and a
  `/v1/landregistry/transactions` operation, confirming the consumer sold-prices tool is backed by
  the open government layer.
- **Lettings runs on street.co.uk.** `lettings-api` creates valuation, viewing and offer enquiries
  in street.co.uk and receives its listing webhooks — the only webhook surface in the estate, and
  it is inbound.
- **UK regulatory plumbing is visible.** `communications-api` exposes a TPS (Telephone Preference
  Service) status check.
- **The persistence model leaks.** Component schemas are named with full .NET namespaces, several
  of them literal `Purplebricks.CosmosDb.Documents.*` document types.
- **No idempotency anywhere.** Across 205 operations the only header parameter declared in the
  entire estate is `Authorization`.

## RESO Posture

**No RESO reference found.** No RESO Web API or Data Dictionary certification, no OData endpoint,
no `$metadata` document (404), no UPI. RESO is a North American MLS standard; the UK has no MLS.
Purplebricks models the portal duopoly directly instead — see `branch-api`.

## Open Data

Purplebricks publishes **no** open data. It **consumes** the UK's open government layer: its
sold-prices tool at `/house-prices` is attributed to HM Land Registry, and `property-api` exposes a
Land Registry transactions operation. In the UK the open, callable property data is public sector
(HM Land Registry, Ordnance Survey) while the private market layer is a closed portal duopoly.

## Auth Model

A single scheme, applied globally in all sixteen documents: `Authorization: Bearer <token>`,
declared as an apiKey-in-header scheme (the ASP.NET Swashbuckle convention). No OAuth 2.0, no
OpenID Connect discovery (404 on every host), no scopes, no public key issuance. Errors are
ASP.NET `ProblemDetails` (RFC 7807).

## Artifacts in this repo

- `openapi/` — 16 harvested OpenAPI 3.0.1 documents (originals kept verbatim in `openapi/_original/`)
- `overlays/` — 16 OpenAPI Overlay 1.0.0 documents adding provenance, verified `servers[]` and 203 derived operationIds
- `skills/` — 6 packaged Agent Skills grounded in real method+path bindings
- `agentic-access/` — recommended `x-agentic-access` contracts for all 205 operations
- `authentication/`, `conventions/`, `errors/`, `lifecycle/`, `conformance/`, `data-model/`
- `packages/` — 18 first-party NuGet packages (Pat.* Azure Service Bus libraries, PB.ITOps.AspNetCore.Versioning); **none is an API client SDK**
- `security/` — TLS/HSTS/SPF/DMARC/DNSSEC probe results
- `well-known/` — every `/.well-known/` probe, all 404
- `asyncapi/` — inbound street.co.uk webhook receivers; no outbound event surface
- `mcp/` — candidate MCP tool set derived from the specs; Purplebricks operates no MCP server
- `llms/` — generated llms.txt

## Common Properties

- [DomainSecurity](security/purplebricks-domain-security.yml)
- [AgenticAccess](agentic-access/purplebricks-agentic-access.yml)
- [Website](https://www.purplebricks.co.uk/)
- [PropertySearch](https://www.purplebricks.co.uk/search/property-for-sale)
- [PropertySearch](https://www.purplebricks.co.uk/search/property-to-rent)
- [HousePrices](https://www.purplebricks.co.uk/house-prices)
- [Agents](https://www.purplebricks.co.uk/estate-agents)
- [Landlords](https://www.purplebricks.co.uk/landlords)
- [PortalDistribution](https://www.purplebricks.co.uk/where-we-advertise)
- [Pricing](https://www.purplebricks.co.uk/services/our-packages)
- [Mortgages](https://www.purplebricksmortgages.co.uk/)
- [TermsOfService](https://www.purplebricks.co.uk/terms)
- [Blog](https://www.purplebricks.co.uk/blog)
- [TechBlog](https://purplebricks.io/)
- [GitHubOrganization](https://github.com/purplebricks)
- [Careers](https://purplebricks.bamboohr.com/careers)
- [LinkedIn](https://www.linkedin.com/company/purplebricks-uk/)
- [Twitter](https://x.com/purplebricksuk)
- [Facebook](https://www.facebook.com/purplebricksUK)
- [Instagram](https://www.instagram.com/purplebricksuk)
- [YouTube](https://www.youtube.com/channel/UCXD3FPBFjPuMA4pCs-_KBJA)
- [TikTok](https://www.tiktok.com/@purplebricksuk)
- [Authentication](authentication/purplebricks-authentication.yml)
- [Conventions](conventions/purplebricks-conventions.yml)
- [ErrorCatalog](errors/purplebricks-problem-types.yml)
- [Lifecycle](lifecycle/purplebricks-lifecycle.yml)
- [Conformance](conformance/purplebricks-conformance.yml)
- [DataModel](data-model/purplebricks-data-model.yml)
- [Packages](packages/purplebricks-packages.yml)
- [LLMsTxt](llms/purplebricks-llms.txt)
- [APIReference](https://api.purplebricks.co.uk/property-api/swagger/index.html)
- [AgentSkill](skills/_index.yml)
- [Book a Purplebricks free valuation](skills/purplebricks-book-a-valuation.md)
- [Take a Purplebricks property to market](skills/purplebricks-go-to-market-listing.md)
- [Book and manage a Purplebricks viewing](skills/purplebricks-book-a-viewing.md)
- [Work a Purplebricks message thread](skills/purplebricks-messaging-thread.md)
- [Generate a Purplebricks conveyancing quote](skills/purplebricks-conveyancing-quote.md)
- [Resolve a Purplebricks property to its portal branches](skills/purplebricks-portal-branch-lookup.md)
- [PrivacyPolicy](https://www.purplebricks.co.uk/terms/privacy-policy)
- [TermsOfUse](https://www.purplebricks.co.uk/terms/terms-of-use)
- [SignUp](https://www.purplebricks.co.uk/register)
- [Login](https://www.purplebricks.co.uk/account/login)
- [Support](https://www.purplebricks.co.uk/contact-us)
- [HelpCenter](https://www.purplebricks.co.uk/faqs)
- [Robots](https://www.purplebricks.co.uk/robots.txt)
- [Sitemap](https://www.purplebricks.co.uk/sitemap.xml)
- [/.well-known/ probe results (all 404 — no discovery layer published)](well-known/purplebricks-well-known.yml)
- [Inbound street.co.uk webhook receivers only — no outbound event surface](asyncapi/purplebricks-webhooks.yml)
- [Derived candidate MCP tool set — Purplebricks operates no MCP server](mcp/purplebricks-mcp.yml)

## Maintainers

- Kin Lane — kin@apievangelist.com
