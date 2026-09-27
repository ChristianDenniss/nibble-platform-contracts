# Provider catalog v1

Acquisition submits `ingest.v2.IngestService/RecordSourceSnapshot` with
`content_type = application/vnd.nibble.catalog.v1+json`, `raw_json` containing
UTF-8 JSON matching `ingest.schema.json`, and a SHA-256 identifier/checksum.
This is a private service-to-service channel, not a public upload endpoint.
The API validates provider identity, currency, prices, and provenance before
an atomic database import. Replaying the same bytes is idempotent. Maximum
message size is 16 MiB. Each included provider supplies its entire retained
Fredericton directory, including last-good menus when retrieval fails.

`GET /v1/catalog` is the public read endpoint (`/api/v1/catalog` through the web
proxy). Its response is `{ city, restaurants: [...] }`. A restaurant contains
`id`, `name`, `address`, optional `rating` and `image`, `artworkKey`, `categories`,
`providers`, `sources` and `items`. Source entries contain `provider`, `url`,
`address`, `menuAvailable`. Items contain `id`, `name`, `section`, and `offers`.

Each offer contains:

- `provider`, `sourceUrl`, `currency` (CAD), `amountCents` (integer or null).
- `status`: `priced`, `options_required`, or `unavailable`.
- `priceKind`: `base`, `from`, or `options`; optional `priceNote`.
- `fulfillmentMode`: `unspecified` unless explicitly established by the source.
- `deliveryCents`, `serviceCents`: nullable; unknown never means zero.
- `promotions`: array of `{title, conditions?}`; eligibility is not assumed.

`unavailable` means that restaurant has a verified provider listing, but no
unambiguous comparable item price. It does not establish that the provider
sells the item. Never rank an unavailable price as cheapest. Starting prices
are not basket totals. Matching preserves quantity, size and meal variants.

The public response deliberately excludes collection times, crawl ages,
schedules, raw HTML, and collector status. They remain internal provenance.
The catalog is a browsing read model; it does not imply delivery serviceability,
a checkout quote, or a verified promotion for a user's basket/address.
