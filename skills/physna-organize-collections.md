---
name: Organize models into collections and search within them
description: Create a Physna collection, add models to it, list its contents, and run geometric search scoped to the collection.
api: openapi/physna-openapi-original.json
operations: [CreateCollection, AddToCollection, GetCollectionContent, GetCollectionMatches]
---

# Organize models into collections and search within them

Group related 3D models into a collection so search and reporting can be scoped
to a curated set.

## Auth
OAuth 2.0 Bearer token (Okta client_credentials). See
`authentication/physna-authentication.yml`.

## Steps
1. **Create the collection** — call `CreateCollection` (`POST /collections`)
   with a name (and optional tag). Capture the collection id.
2. **Add models** — call `AddToCollection` (`POST /collections/{id}/add`) with
   the model ids to include.
3. **Verify contents** — call `GetCollectionContent`
   (`GET /collections/{id}/content`) to list the models now in the collection.
4. **Search within the collection** — call `GetCollectionMatches`
   (`GET /collections/{id}/matches`) to find geometric matches scoped to the
   collection.

## Notes
- Use `page` / `perPage` to page collection contents and matches.
- Retry on `503` while a collection is still reprocessing.
