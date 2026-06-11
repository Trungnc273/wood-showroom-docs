# API_SPEC.md — Human-readable API Contract

## 1. Source of Truth

`openapi.yaml` is the legal API contract and source of truth between FE and BE.

This file explains the API in human-readable form. If this file conflicts with `openapi.yaml`, stop and mark the task as BLOCKED.

## 2. Base Path

All APIs use:

```text
/api/v1
```

Browser code calls same-origin paths only:

```text
/api/v1/*
```

Do not call Railway backend URL directly from browser code.

## 3. Security Schemes

Use names exactly as defined in `openapi.yaml`:

```text
cookieAuth
csrfHeader
```

Admin state-changing endpoints require:

```yaml
security:
  - cookieAuth: []
    csrfHeader: []
```

Admin read-only endpoints require:

```yaml
security:
  - cookieAuth: []
```

## 4. Public APIs

```http
GET  /api/v1/products
GET  /api/v1/products/:slug
GET  /api/v1/categories
POST /api/v1/contact-inquiries
```

## 5. Admin Auth APIs

```http
GET  /api/v1/admin/auth/csrf
POST /api/v1/admin/auth/login
GET  /api/v1/admin/auth/me
POST /api/v1/admin/auth/logout
```

`/admin/auth/me` is required so the frontend can check the current admin session after page reload.

## 6. Admin Category APIs

```http
GET    /api/v1/admin/categories
POST   /api/v1/admin/categories
PATCH  /api/v1/admin/categories/:id
DELETE /api/v1/admin/categories/:id
```

Delete means deactivate/soft delete by default.

## 7. Admin Product APIs

```http
GET    /api/v1/admin/products
POST   /api/v1/admin/products
GET    /api/v1/admin/products/:id
PATCH  /api/v1/admin/products/:id
DELETE /api/v1/admin/products/:id
```

`PATCH` is a partial update. `UpdateProductRequest` fields are optional, but every provided field must pass validation.

Slug conflict must return:

```json
{
  "error": {
    "code": "SLUG_ALREADY_EXISTS",
    "message": "Product with this slug already exists",
    "details": {
      "slug": "ban-go-soi"
    }
  }
}
```

## 8. Media Upload APIs

Use presigned upload + confirm flow.

```http
POST   /api/v1/admin/media/presigned-upload
POST   /api/v1/admin/media/confirm
PATCH  /api/v1/admin/products/:id/media/reorder
DELETE /api/v1/admin/media/:id
```

Deprecated:

```http
POST /api/v1/admin/products/:id/media
```

Do not implement deprecated direct attach endpoint in Phase 1.

## 9. Admin Inquiry APIs

```http
GET   /api/v1/admin/contact-inquiries
PATCH /api/v1/admin/contact-inquiries/:id/status
```

Inquiry list must include pagination meta.

## 10. Product Specs DTO Validation

`specsJson` in product create/update must follow:

```json
{
  "width_cm": 120,
  "height_cm": 75,
  "depth_cm": 60,
  "seat_height_cm": 45,
  "weight_kg": 18,
  "finish": "Sơn PU mờ",
  "color_note": "Nâu óc chó / vàng gỗ tự nhiên"
}
```

Reject invalid specs with:

```json
{
  "error": {
    "code": "INVALID_PRODUCT_SPECS",
    "message": "Product specs format is invalid",
    "details": {}
  }
}
```

## 11. Standard Error Response

All errors use:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

Required error codes include:

```text
INVALID_PRODUCT_SPECS
UNAUTHORIZED
CSRF_TOKEN_INVALID
PRODUCT_NOT_FOUND
CATEGORY_NOT_FOUND
MEDIA_NOT_FOUND
SLUG_ALREADY_EXISTS
FILE_TOO_LARGE
UNSUPPORTED_MEDIA_TYPE
RATE_LIMITED
STORAGE_TEMPORARILY_UNAVAILABLE
```

## 12. OpenAPI Contract Hardening Rules

Required:

- `ProductSpecs` must be strict with `additionalProperties: false`.
- Admin state-changing routes must require `cookieAuth` and `csrfHeader`.
- Common `ErrorResponse` must be used for all error responses.
- Presigned upload response must include `uploadId`.
- Media confirm endpoint must exist.
- Media reorder endpoint must exist.
- Admin category CRUD endpoints must exist.
- `/admin/auth/me` must exist.


## 13. Error Message Language

Error codes remain English and stable for machines.

Error messages that may be displayed to users should be Vietnamese.

Example:

```json
{
  "error": {
    "code": "SLUG_ALREADY_EXISTS",
    "message": "Slug này đã được sử dụng cho sản phẩm khác.",
    "details": {
      "slug": "ban-go-soi"
    }
  }
}
```

Follow `LANGUAGE_POLICY.md`.
