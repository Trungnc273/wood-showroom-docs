# ARCHITECTURE.md — Wood Showroom 3D/AR

## 1. Architecture Decision

Use:

```text
Multi-repo + Backend modular monolith
```

Repositories:

```text
wood-showroom-docs
wood-showroom-web
wood-showroom-api
wood-showroom-infra later if needed
```

Backend architecture:

```text
NestJS modular monolith
```

Do not use microservices in Phase 1.

## 2. High-level System

```text
User Mobile/Desktop
        ↓
CDN / Edge Cache
        ↓
Next.js Frontend
        ↓
NestJS Backend API
        ↓
PostgreSQL
```

Media flow:

```text
Admin selects file
        ↓
Frontend validates metadata
        ↓
Backend validates and creates signed upload URL
        ↓
Frontend uploads directly to Cloudflare R2
        ↓
Backend saves media metadata in PostgreSQL
        ↓
Public site loads media through CDN/R2
```

## 3. Frontend Repo

Repository:

```text
wood-showroom-web
```

Structure:

```text
src/
├── app/
│   ├── (public)/
│   └── (admin)/
├── features/
│   ├── public-products/
│   ├── admin-products/
│   ├── media-manager/
│   ├── contact/
│   └── 3d-ar/
└── shared/
    ├── ui/
    ├── hooks/
    ├── lib/
    ├── api/
    ├── config/
    └── types/
```

Rules:

```text
app → can import features and shared
features → can import shared only
shared → cannot import features or app
features → cannot import other features
```

## 4. Admin UI Decision

Admin UI stays inside `wood-showroom-web` in Phase 1.

Use Next.js route groups:

```text
src/app/(public)/
src/app/(admin)/
```

A separate `wood-showroom-admin` repo may be created later if needed.

## 5. Backend Repo

Repository:

```text
wood-showroom-api
```

Structure:

```text
src/
├── modules/
│   ├── admin-auth/
│   ├── products/
│   ├── categories/
│   ├── media/
│   ├── inquiries/
│   └── site-settings/
├── common/
└── main.ts
```

Products module should separate public/admin controllers:

```text
public-products.controller.ts
admin-products.controller.ts
products.service.ts
products.repository.ts
```

## 6. API Versioning

Use:

```text
/api/v1
```

Reason:

- FE/BE deploy separately.
- Supports backward compatibility.
- Reduces release risk in multi-repo setup.

## 7. API Contract

Use:

```text
openapi.yaml
```

`API_SPEC.md` explains API in human-readable format, but `openapi.yaml` is the primary contract.

## 8. Media Rules

- Do not store image/model binaries in PostgreSQL.
- Do not use local production storage.
- Use Cloudflare R2.
- Backend does not stream large product media.
- Product cards never load GLB/USDZ.
- 3D/AR loads on user click only.

## 9. Redis Decision

Phase 1 does not use Redis.

Prepare abstractions:

```text
CacheService
RateLimitService
Future Queue abstraction
```

Add Redis later only if needed.

## 10. Edge Protection

Use:

```text
Cloudflare WAF / Rate Limiting
```

as primary protection.

NestJS in-memory rate limit is secondary.

## 11. PostgreSQL Connection Strategy

- Use managed PostgreSQL.
- PrismaClient singleton in NestJS.
- Tune Prisma pool size.
- Use pooled URL/PgBouncer if provider supports.
- Use direct DB URL for migrations if required.

## 12. Deployment

Phase 1:

```text
Frontend: Vercel
Backend: Railway
Database: Managed PostgreSQL
Storage: Cloudflare R2
Edge protection: Cloudflare
```

## 13. Scaling Strategy

First scale by:

1. CDN and R2 for assets.
2. Lazy-load 3D/AR.
3. Cache public product reads.
4. Proper PostgreSQL indexes.
5. Connection pooling.
6. Redis only when there is clear need.


## 14. Same-origin API Proxy Strategy

Because the frontend is deployed on Vercel and the backend is deployed on Railway, browser requests must not call the Railway domain directly in production.

Required rule:

```text
Browser → https://frontend-domain.com/api/v1/*
        → Next.js rewrite/proxy
        → Railway backend
```

This makes admin auth cookies work as same-origin cookies from the browser perspective.

Frontend code must call:

```text
/api/v1/products
/api/v1/admin/auth/login
/api/v1/admin/products
```

Frontend code must not call:

```text
https://wood-showroom-api.up.railway.app/api/v1/*
```

`next.config.ts` must include a rewrite from `/api/v1/:path*` to the backend API base URL.

Backend CORS should still be configured defensively, but the primary production flow is same-origin proxy through Next.js.

## 15. Presigned Upload Confirmation Strategy

Media upload must not rely on a happy-path-only flow.

Required flow:

```text
1. Admin requests presigned upload URL.
2. Backend creates uploadId and pending storage key.
3. Frontend uploads file directly to R2.
4. Frontend calls POST /api/v1/admin/media/confirm with uploadId and product/media metadata.
5. Backend verifies the pending upload metadata and creates product_media DB record.
```

If upload succeeds but confirm fails, the object is considered orphan/pending.

Cleanup strategy:

```text
Phase 1:
- Store pending uploads under a pending/ prefix.
- Document R2 lifecycle cleanup or scheduled cleanup for pending objects older than 24h.

Phase 2:
- Add automated cleanup job if storage volume grows.
```

Do not create product_media metadata before upload confirmation succeeds.
