# Wood Showroom 3D/AR — Documentation Pack

**Current version:** v9  
**Date:** 2026-06-12  
**Status:** Ready for repository scaffolding after final human review.

> Review history files (`REVIEW_FIXES_v*.md`) are historical records only.  
> Do not treat review files as active instructions. All accepted fixes have been incorporated into the main documentation files.

## Repositories

```text
wood-showroom-docs
wood-showroom-web
wood-showroom-api
wood-showroom-infra later if needed
```

## Phase 1 Stack

- Frontend: Next.js 15 + App Router + TypeScript + Tailwind CSS
- Backend: NestJS modular monolith + TypeScript
- Database: PostgreSQL
- ORM: Prisma
- Storage: Cloudflare R2
- 3D/AR: `model-viewer` + GLB/USDZ
- Frontend state: Next.js Server Components/fetch + TanStack Query + Zustand
- API: `/api/v1` + `openapi.yaml`
- Auth: JWT in HttpOnly Secure SameSite cookie + CSRF Double Submit Cookie
- Rate limit: Cloudflare WAF primary + NestJS in-memory secondary
- CI/CD: GitHub Actions + Vercel + Railway

## Phase 1 Product Types

```text
made_to_order
ready_stock
```

Do not implement `one_of_a_kind` or `custom_order` as product types.

Use:

```text
is_unique
is_customizable
inquiry_type = custom_order
```

## Phase 1 Decisions Added in v5

- Admin Category CRUD is included in Phase 1.
- Product colors table/API is deferred to Phase 2; use `specsJson.color_note` / `finish` in Phase 1.
- Site settings table/API is deferred to Phase 2; use env/config/static contact settings in Phase 1.
- Product update uses `PATCH /api/v1/admin/products/:id` with partial update.
- Media reorder has explicit endpoint: `PATCH /api/v1/admin/products/:id/media/reorder`.
- Admin session check endpoint exists: `GET /api/v1/admin/auth/me`.
- SEO tasks are explicitly included in `TASKS.md`.
- Documentation and agent guardrails were deduplicated and normalized.


## v6 Additions

- Added `DESIGN_TOKENS.md` for frontend visual consistency.
- Added `DOCS_VALIDATION.md` for pre-coding validation commands.
- Added scaffolding prompts under `PROMPTS/`.
- Updated agent rules to include design tokens and SEO strategy.


## v7 Additions

- Added `LANGUAGE_POLICY.md`.
- Locked Phase 1 UI language to Vietnamese.
- Updated SPEC, AGENTS, API_SPEC, DESIGN_TOKENS, TASKS, prompts, and agent rules to enforce Vietnamese UI copy.


## v8 Additions

- Added `MOBILE_RESPONSIVE_REQUIREMENTS.md`.
- Locked responsive requirements for mobile phone, tablet, laptop, and desktop PC.
- Added responsive QA tasks and agent rules.
- Updated scaffold/review prompts to require mobile-first implementation.


## v9 Additions

- Added `MOBILE_BROWSER_COMPATIBILITY.md`.
- Locked Android Chrome and iOS Safari mobile web compatibility requirements.
- Added Android/iOS-specific QA tasks and agent rules.
- Updated prompts to include mobile browser compatibility checks.
