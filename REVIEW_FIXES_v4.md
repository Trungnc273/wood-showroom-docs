# REVIEW_FIXES_v4.md

## Purpose

Records OpenAPI hardening fixes after review.

## Applied Fixes

### 1. Strict ProductSpecs

`openapi.yaml` now defines:

```yaml
ProductSpecs:
  type: object
  additionalProperties: false
  properties:
    width_cm:
      type: number
    height_cm:
      type: number
    depth_cm:
      type: number
    seat_height_cm:
      type: number
    weight_kg:
      type: number
    finish:
      type: string
```

`CreateProductRequest.specsJson` and `ProductDetail.specsJson` reference `ProductSpecs`.

### 2. Security Schemes

Added:

```yaml
cookieAuth:
  type: apiKey
  in: cookie
  name: admin_session

csrfHeader:
  type: apiKey
  in: header
  name: X-CSRF-Token
```

Admin state-changing endpoints require both `cookieAuth` and `csrfHeader`.

### 3. CSRF Bootstrap

Added:

```http
GET /api/v1/admin/auth/csrf
```

This endpoint can set/refresh the readable `csrf-token` cookie before login or admin mutations.

### 4. Media Confirm Flow

Added/strengthened:

```http
POST /api/v1/admin/media/presigned-upload
POST /api/v1/admin/media/confirm
```

Presigned upload response now includes:

```json
{
  "uploadId": "uuid",
  "uploadUrl": "...",
  "storageKey": "pending/...",
  "publicUrl": "...",
  "expiresAt": "..."
}
```

Metadata is created only after confirm.

### 5. Common ErrorResponse

Added reusable error schema and common responses:

- 400 ValidationError
- 401 Unauthorized
- 403 Forbidden
- 404 NotFound
- 409 Conflict
- 413 FileTooLarge
- 415 UnsupportedMediaType
- 429 RateLimited
- 503 StorageUnavailable
- default DefaultError

### 6. Same-origin API Contract

OpenAPI server descriptions clarify that production browser calls should go through same-origin `/api/v1/*` proxy, not Railway directly.

### 7. Agent Guardrails

Updated `AGENTS.md` and `API_SPEC.md` to force FE/BE agents to follow OpenAPI strictly.
