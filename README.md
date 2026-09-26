# nibble-platform-contracts

Shared wire contracts: protobuf/gRPC (ingest v1 + v2) and OpenAPI (compare HTTP).

- `proto/ingest/` — gRPC ingest (`buf generate`)
- `openapi/compare/v1/openapi.yaml` — `POST /v1/compare` request/response (source of truth for HTTP compare)

**Why this repo exists:** `nibble-data-acquisition` and `nibble-api-engine` must agree on bytes on the wire without sharing domain types or SQL. Contracts are versioned independently so a collector can upgrade the API shape on a release tag, not by importing `nibble-go-data-model`.

This is transport language, not business language. Handlers in `nibble-api-engine` map these messages into domain entities.
