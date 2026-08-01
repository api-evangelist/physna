---
name: Ingest a 3D model and find geometric matches
description: Upload a 3D model to Physna, wait for it to finish processing, then find geometrically similar parts.
api: openapi/physna-openapi-original.json
operations: [CreateModels, GetModel, GetModelStateReport, GetPartMatches, GetModelMatches]
---

# Ingest a 3D model and find geometric matches

Use the Physna Public API (base `https://api.physna.com/v2`) to ingest a 3D
model and run geometric search once it is processed.

## Auth
Obtain an OAuth 2.0 access token from Physna's Okta authorization server using
the `client_credentials` grant (`scope=tenantApp roles`). Send it as
`Authorization: Bearer <token>`. Include `X-PHYSNA-TENANTID` where required.
See `conventions/physna-conventions.yml` and `authentication/physna-authentication.yml`.

## Steps
1. **Ingest the model** — call `CreateModels` (`POST /models`) with the model
   file reference and target folder. Capture the returned model id.
2. **Wait for processing** — poll `GetModel` (`GET /models/{id}`) or
   `GetModelStateReport` (`GET /models/state-report`) until the model state is
   ready. Match and file endpoints return `503` while a model is still
   processing (see `errors/physna-problem-types.yml`), so retry on 503.
3. **Find part matches** — call `GetPartMatches`
   (`GET /models/{id}/part-matches`) to retrieve geometrically similar parts,
   or `GetModelMatches` (`GET /models/{id}/matches`) for the combined match set.
4. **Page results** — use `page` (1-based) and `perPage` query params; read the
   `PageData` block to iterate.

## Notes
- Prefer `GetPartMatches` over the deprecated `GetPartToPartMatches`.
- Errors are `application/json` `ErrorResponse` objects, not RFC 9457.
