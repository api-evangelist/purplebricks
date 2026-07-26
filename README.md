# Purplebricks (purplebricks)

Purplebricks is the United Kingdom's largest online (hybrid) estate agency, founded in 2014 by Michael and Kenny Bruce and David Shepherd, selling and letting residential property across England, Wales and Scotland through a fixed-fee, remote model staffed by local property experts rather than a high-street branch network. After a 2023 sale to rival online agent Strike for a nominal £1 the combined business was rebranded back under the Purplebricks name, and in 2026 the founding Bruce brothers returned alongside Sir Charles Dunstone's Freston Ventures. It sits on the brokerage rung of the value chain — it originates the seller relationship and the listing, then distributes that stock to Rightmove, Zoopla, OnTheMarket and PrimeLocation through the UK's portal feed arrangements, because the UK has no MLS and therefore no shared cooperative data exchange. API posture, stated honestly, is none-published: no developer, partner or API portal exists, developer./developers./docs. do not resolve, api.purplebricks.co.uk answers only with an Azure Front Door 404, and no OpenAPI, Swagger, GraphQL or OData $metadata contract is served anywhere on the estate. The one API path that is publicly discoverable — /outlook-api/v1/agent/outlook-sync, disallowed in robots.txt — returns 401 and is an internal agent-facing calendar sync, not a developer product. RESO is absent, as expected outside North America. The only machine-readable public surface is schema.org JSON-LD embedded in its pages, and the genuinely open UK property data layer it consumes for its sold-prices tool belongs to government (HM Land Registry), not to Purplebricks.

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

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

No public APIs are documented by Purplebricks. See `review.yml` for the full probe record.

## RESO Posture

**No RESO reference found.** Purplebricks holds no RESO Web API certification and no RESO Data Dictionary certification, publishes no OData endpoint, serves no `$metadata` document, and uses no Universal Property Identifier (UPI). RESO is a North American standard carried by MLSs and their vendors; the United Kingdom has no MLS, so there is no cooperative endpoint to certify.

## Access Gate

**none-published.** There is nothing for a developer to sign up for, apply to, join, or licence. No developer programme, no partner API programme, no sandbox, no keys. Purplebricks listing data reaches third parties only by way of the portals it syndicates to (Rightmove, Zoopla, OnTheMarket, PrimeLocation), and any data licence for that is with the portal, not with Purplebricks.

## Open Data

Purplebricks publishes **no** open data. It **consumes** the UK's open government layer: its sold-prices tool at `/house-prices` is explicitly attributed to HM Land Registry. In the UK the open, callable property data is public sector (HM Land Registry, Ordnance Survey) while the private market layer is a closed portal duopoly — Purplebricks sits entirely on the closed side.

## Auth Model

None published. No API keys, no OAuth2, no OpenID Connect discovery document (`/.well-known/openid-configuration` → 404). The single discoverable API path, `/outlook-api/v1/agent/outlook-sync`, returns HTTP 401 — an authenticated internal agent endpoint, undocumented and not a developer surface.

## Common Properties

- [Website](https://www.purplebricks.co.uk/)
- [Property Search — For Sale](https://www.purplebricks.co.uk/search/property-for-sale)
- [Property Search — To Rent](https://www.purplebricks.co.uk/search/property-to-rent)
- [House Prices (HM Land Registry data)](https://www.purplebricks.co.uk/house-prices)
- [Estate Agents](https://www.purplebricks.co.uk/estate-agents)
- [Landlords](https://www.purplebricks.co.uk/landlords)
- [Where We Advertise (portal distribution)](https://www.purplebricks.co.uk/where-we-advertise)
- [Pricing / Packages](https://www.purplebricks.co.uk/services/our-packages)
- [Purplebricks Mortgages](https://www.purplebricksmortgages.co.uk/)
- [Terms](https://www.purplebricks.co.uk/terms)
- [Blog](https://www.purplebricks.co.uk/blog)
- [Tech Blog](https://purplebricks.io/)
- [GitHub Organization](https://github.com/purplebricks)
- [Careers](https://purplebricks.bamboohr.com/careers)
- [LinkedIn](https://www.linkedin.com/company/purplebricks-uk/)
- [Twitter / X](https://x.com/purplebricksuk)
- [Facebook](https://www.facebook.com/purplebricksUK)
- [Instagram](https://www.instagram.com/purplebricksuk)
- [YouTube](https://www.youtube.com/channel/UCXD3FPBFjPuMA4pCs-_KBJA)
- [TikTok](https://www.tiktok.com/@purplebricksuk)

## Maintainers

- Kin Lane — kin@apievangelist.com
