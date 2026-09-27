# nibble-platform-contracts

Shared wire contracts: protobuf/gRPC (ingest v1 + v2) and OpenAPI (HTTP).

- `proto/ingest/` — gRPC ingest (`buf generate`)
- `openapi/http/v1/openapi.yaml` — combined REST index (health, storefront, home, sponsored, source menu, compare)
- `openapi/health/v1/openapi.yaml` — `GET /health`
- `openapi/storefront/v1/openapi.yaml` — `GET /v1/storefront` (camelCase JSON for the web app), `PUT /v1/menu-items/{itemId}/image`
- `openapi/home/v1/openapi.yaml` — `GET /v1/home` (home feed: banners, sponsored / deals / popular / recommended rails)
- `openapi/sponsored/v1/openapi.yaml` — `POST /v1/sponsored/events` (sponsored impression / click logging)
- `openapi/compare/v1/openapi.yaml` — `POST /v1/compare` and compare sessions
- `openapi/source/v1/openapi.yaml` — `GET /v1/source-stores/{storeId}/menu`

**Why this repo exists:** `nibble-data-acquisition` and `nibble-api-engine` must agree on bytes on the wire without sharing domain types or SQL. Contracts are versioned independently so a collector can upgrade the API shape on a release tag, not by importing `nibble-go-data-model`.

This is transport language, not business language. Handlers in `nibble-api-engine` map these messages into domain entities.
