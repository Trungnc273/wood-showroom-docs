# Cloudflare R2 Storage — Integration & Smoke Checklist

Phase M2. Covers the media upload pipeline (presigned PUT → confirm → public
serve → delete) backed by Cloudflare R2, and the runtime configuration the API
needs to read R2 credentials.

## Required API environment variables

All five must be present in `wood-showroom-api/.env` (git-ignored; never commit):

```text
CLOUDFLARE_R2_BUCKET
CLOUDFLARE_R2_ACCESS_KEY_ID
CLOUDFLARE_R2_SECRET_ACCESS_KEY
CLOUDFLARE_R2_ENDPOINT
CLOUDFLARE_R2_PUBLIC_URL
```

`.env.example` documents these names (without values).

The API builds an S3-compatible client from `CLOUDFLARE_R2_ENDPOINT` +
credentials, reads the bucket from `CLOUDFLARE_R2_BUCKET`, and generates public
URLs as `${CLOUDFLARE_R2_PUBLIC_URL}/${storageKey}`. If endpoint/credentials are
missing the client stays null and uploads fall back to a non-functional mock URL.

## Docker: the API container must load the R2 env

The dockerized API does **not** bake `.env` into the image, so the
`wood-showroom-api` service in the root `docker-compose.yml` must reference it
via `env_file`; otherwise `process.env.CLOUDFLARE_R2_*` is empty inside the
container and uploads silently produce mock URLs:

```yaml
  wood-showroom-api:
    env_file:
      - ./wood-showroom-api/.env
    environment:
      # docker-network overrides take precedence over env_file
      DATABASE_URL: postgresql://postgres:postgres@postgres:5432/wood_showroom?schema=public
      # ...
```

After changing env, recreate the container so it re-reads variables:

```bash
docker compose up -d wood-showroom-api
# verify (prints SET/EMPTY only, never values):
docker exec wood-showroom-api node -e "['CLOUDFLARE_R2_ENDPOINT','CLOUDFLARE_R2_BUCKET','CLOUDFLARE_R2_PUBLIC_URL','CLOUDFLARE_R2_ACCESS_KEY_ID','CLOUDFLARE_R2_SECRET_ACCESS_KEY'].forEach(k=>console.log(k+'='+(process.env[k]?'SET':'EMPTY')))"
```

> Note: the root `docker-compose.yml` is a local runtime/orchestration file and
> is not currently tracked by any of the three repos. Keep the `env_file` entry
> when reproducing this environment.

## Cloudflare R2 bucket CORS (required for browser uploads)

Server-side PUT (Node) does not need CORS. But the admin UI uploads **directly
from the browser** to the R2 signed URL, which is cross-origin — so the bucket
needs a CORS policy allowing `PUT` from the admin origins. Paste this into the
R2 bucket → Settings → CORS Policy (adjust origins to the live ngrok URL):

```json
[
  {
    "AllowedOrigins": [
      "https://cunningly-finespun-torie.ngrok-free.dev",
      "http://localhost:3002",
      "http://localhost:3000"
    ],
    "AllowedMethods": ["GET", "PUT", "HEAD"],
    "AllowedHeaders": ["*"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3600
  }
]
```

The ngrok hostname changes per session on the free plan — update
`AllowedOrigins` whenever it changes.

## Upload pipeline (how it works)

1. `POST /admin/media/presigned-upload` `{ fileName, mimeType, sizeBytes, mediaType }`
   → validates type/size, returns `{ uploadId, uploadUrl, storageKey, publicUrl, expiresAt }`.
   `uploadUrl` is a short-lived (1h) signed PUT; it is **not persisted**.
2. Browser `PUT`s the file bytes to `uploadUrl` (Content-Type + Content-Length
   are signed and must match).
3. `POST /admin/media/confirm` `{ uploadId, productId, mediaType, altText?, sortOrder?, isPrimary? }`
   → moves the object `pending/{uploadId}/…` → `products/{productId}/{mediaId}/…`,
   reads real size/mime via HEAD, and stores **only** safe metadata
   (`storageKey`, public `url`, mime, size, flags). Sets `has3d`/`hasAr` for 3D/AR.
4. `DELETE /admin/media/:id` → removes the DB row (resets `has3d`/`hasAr` if last
   one) and deletes the R2 object **by stored `storageKey`** (never a
   client-supplied key).

## Validation policy

| mediaType  | extensions                | MIME                                                   | max size |
| ---------- | ------------------------- | ------------------------------------------------------ | -------- |
| image      | jpg, jpeg, png, webp, gif | image/jpeg, image/png, image/webp, image/gif           | 5 MB     |
| model_3d   | glb                       | model/gltf-binary                                      | 30 MB    |
| ar_usdz    | usdz                      | model/vnd.usdz+zip, model/usdz+zip, application/octet-stream | 50 MB |
| video      | mp4, webm                 | video/mp4, video/webm                                  | 100 MB   |

## Smoke checklist

- [ ] All five R2 env vars present in `.env`; `.env` git-ignored.
- [ ] Container Node process sees R2 vars as SET (command above).
- [ ] Admin login over HTTPS (ngrok) works (Secure cookies require HTTPS).
- [ ] `presigned-upload` returns a `*.r2.cloudflarestorage.com` signed URL.
- [ ] Browser/HTTP `PUT` to the signed URL returns 200 + ETag.
- [ ] `confirm` returns a `${CLOUDFLARE_R2_PUBLIC_URL}/…` public URL.
- [ ] Public URL loads (200, correct content-type/size).
- [ ] Media appears in product detail/gallery.
- [ ] `DELETE` removes the DB row and the R2 object (public URL → 404).
- [ ] No secret in API responses, logs, frontend bundle, or git.

## 3D/AR note

The upload/validation path supports `model_3d` (.glb) and `ar_usdz` (.usdz) with
the limits above. Use **real furniture** models only — never external demo
assets (e.g. model-viewer's Astronaut).
