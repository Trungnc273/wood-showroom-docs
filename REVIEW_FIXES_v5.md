# REVIEW_FIXES_v5.md

## Purpose

Records fixes applied after the v4 documentation review.

## Accepted Decisions

- Admin Category CRUD: included in Phase 1.
- Product Colors API/table: deferred to Phase 2.
- Site Settings API/table: deferred to Phase 2.
- Product update: use partial `PATCH`.
- Media reorder: explicit endpoint required.

## Fixed Critical Issues

### CRT-01 — Duplicate DOC-04

TASKS.md now has only one `DOC-04`.

### CRT-02 — Duplicate AGENTS.md section 11

AGENTS.md now has one `OpenAPI Contract Guardrails` section.

### CRT-03 — Missing admin category CRUD

Added to OpenAPI, API_SPEC, SPEC, PLAN, and TASKS.

### CRT-04 — Missing SEO tasks

Added FE-SEO-01 to FE-SEO-04.

### CRT-05 — Attach Media conflict

Deprecated direct attach flow. Phase 1 uses:

```text
presigned-upload → R2 upload → media/confirm
```

### CRT-06 — Security scheme naming mismatch

Standardized all docs to:

```text
cookieAuth
csrfHeader
```

## Additional Fixes

- Added `/api/v1/admin/auth/me`.
- Added media reorder endpoint.
- Added pagination meta to inquiry list.
- Changed update product from PUT full replace to PATCH partial update.
- Added seed task.
- Added admin inquiry UI tasks.
- Added docs-agent.cursorrules.
- Added conflict resolution rules for agents.
- Unified 3D timeout to exactly 15 seconds.
- Added `SLUG_ALREADY_EXISTS` error code.
- Marked `product_colors`, `product_specs`, and `site_settings` as Phase 2/deferred.
