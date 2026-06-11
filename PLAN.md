# PLAN.md — Implementation Plan

The phase mapping below matches `TASKS.md`.

## Phase A — Documentation Lock

- Validate required docs.
- Validate OpenAPI.
- Freeze scope.
- Resolve duplicate IDs/sections before coding.

## Phase B — Repository Scaffolding

- Create `wood-showroom-web`.
- Create `wood-showroom-api`.
- Add Docker/env examples.
- Add frontend API proxy rewrite.

## Phase C — Backend Database Foundation

- Prisma setup.
- PostgreSQL schema.
- Initial migration.
- Development seed script.

## Phase D — Backend Product, Category & Validation

- Product DTOs.
- Strict `specsJson` validation.
- Products repository/service/controllers.
- Category admin CRUD.
- Public categories API.

## Phase E — Backend Auth & Security

- Admin login.
- `/admin/auth/me`.
- Logout.
- CSRF Double Submit Cookie.
- Admin guard.
- Standard errors.
- Basic rate limiting.

## Phase F — Backend R2 Media Flow

- R2 client.
- Media validation.
- Presigned upload.
- `uploadId` pending flow.
- Media confirm.
- Media reorder.
- Media delete/archive.

## Phase G — Frontend Public UX

- Design tokens.
- Public layout.
- Homepage sections.
- Product card.
- Catalogue.
- Product detail.
- Contact CTA/form.

## Phase G.5 — Frontend SEO

- Product metadata.
- Sitemap.
- Robots.
- SSR verification.

## Phase H — Frontend 3D/AR

- Lazy `model-viewer`.
- `ModelViewerBoundary`.
- AR fallback.
- Zustand for lightweight UI state.

## Phase I — Frontend Admin

- Admin layout.
- Admin API client.
- Login/session.
- Product/category/media/inquiry UI.
- TanStack Query.

## Phase J — Infra, CI/CD, Deployment Hardening

- Cloudflare WAF/rate limit.
- Public media cache rules.
- GitHub Actions.
- Prisma migration deploy.
- Vercel/Railway staging.
- Manual QA.
- Architecture compliance review.
