---
name: Find 3D models from a 2D image
description: Upload a 2D image to Physna and retrieve the 3D models that visually match it.
api: openapi/physna-openapi-original.json
operations: [GetSignedUploadUrlForImage, GetImageMatches]
---

# Find 3D models from a 2D image

Use Physna Image Search to go from a photograph or rendering to matching 3D
models in your tenant.

## Auth
OAuth 2.0 Bearer token (Okta client_credentials). See
`authentication/physna-authentication.yml`.

## Steps
1. **Request a signed upload URL** — call `GetSignedUploadUrlForImage`
   (`POST /images`) to obtain a signed URL, then upload the image bytes to it.
2. **Retrieve image matches** — call `GetImageMatches`
   (`GET /images/model-matches`) with the uploaded image reference to get the
   ranked list of 3D models that match the image.
3. **Page results** — iterate with `page` / `perPage`.

## Notes
- Image processing is asynchronous; retry on `503` until results are ready.
