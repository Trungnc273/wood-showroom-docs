# AGENTS.md — AI Agent Rules

## 1. Mission

Build Phase 1 of Wood Showroom 3D/AR without over-engineering.

## 2. Stack Lock

Do not change without ADR:

- Next.js 15 + App Router + TypeScript + Tailwind
- NestJS + TypeScript
- PostgreSQL + Prisma
- Cloudflare R2
- `model-viewer` + GLB/USDZ
- TanStack Query for admin server-state
- Zustand for lightweight UI state
- `/api/v1` + `openapi.yaml`

## 3. Scope Lock

Do not implement in Phase 1:

- Buyer login
- Cart
- Payment
- Checkout
- Microservices
- Redis
- Separate admin repo
- Product color variant table/API
- Site settings admin API/table

## 4. FE Rules

- Use `src/app` only for routing/layout/pages.
- Put business code in `src/features`.
- Put common code in `src/shared`.
- Features must not import other features.
- Product cards must never load GLB/USDZ.
- 3D/AR must load only on user action.
- Use `ModelViewerBoundary` with exactly 15s timeout.
- Use same-origin `/api/v1/*`; never call Railway backend URL directly.
- Use admin API client for CSRF and 401 handling.

## 5. BE Rules

- Modular monolith.
- Public/admin controllers separated.
- PrismaClient singleton.
- Validate DTOs before Prisma.
- Standard error response only.
- Use HttpOnly cookie auth for admin.
- Use CSRF Double Submit Cookie for admin mutations.
- No local production media storage.
- R2 credentials server-side only.
- Use uploadId + media confirm flow.

## 6. API Rules

- `openapi.yaml` is the legal API contract and source of truth.
- Update `openapi.yaml` before implementation.
- Do not implement request/response shapes not present in OpenAPI.
- Use exact security scheme names: `cookieAuth` and `csrfHeader`.
- Use standard `ErrorResponse`.

## 7. Working Process

Before code:

- Read relevant docs only.
- Produce Shadow Plan:
  - files to create/edit
  - docs/ADR referenced
  - risks
  - assumptions
  - commands/tests

After code:

- List changed files.
- Explain changes.
- Report tests/checks.
- State known limitations.

## 8. Conflict Resolution

When encountering conflicting documentation:

1. Treat `openapi.yaml` as the highest authority for API contract.
2. Treat `SPEC.md` as the highest authority for product requirements.
3. Treat `ARCHITECTURE.md` and ADRs as the highest authority for architectural decisions.
4. If conflict cannot be resolved using these rules, mark the task as `BLOCKED` and ask Human.
5. Do not guess ambiguous requirements.

## 9. Frontend Import Guardrails

```text
src/app      → may import src/features and src/shared
src/features → may import src/shared only
src/shared   → must not import src/features or src/app
features     → must not import other features
```

If multiple features need shared logic/component, move it to:

```text
src/shared/
```

## 10. Agent Context Isolation

Frontend Agent reads:

```text
SPEC.md
ARCHITECTURE.md frontend sections
API_SPEC.md
openapi.yaml
DESIGN_TOKENS.md
LANGUAGE_POLICY.md
MOBILE_RESPONSIVE_REQUIREMENTS.md
MOBILE_BROWSER_COMPATIBILITY.md
SEO_STRATEGY.md
PERFORMANCE_BUDGET.md
AGENTS.md
TASKS.md relevant FE tasks
```

Backend Agent reads:

```text
SPEC.md
ARCHITECTURE.md backend sections
DATA_MODEL.md
API_SPEC.md
openapi.yaml
ASSET_STORAGE_STRATEGY.md
AGENTS.md
TASKS.md relevant BE tasks
```

DevOps Agent reads:

```text
DEPLOYMENT.md
CI_CD.md
ARCHITECTURE.md deployment sections
AGENTS.md
TASKS.md relevant DevOps tasks
```

Docs Agent reads all docs but must report inconsistencies before modifying.

## 11. OpenAPI Contract Guardrails

Backend Agent must:

- Implement endpoints exactly as specified in `openapi.yaml`.
- Generate/implement DTOs from strict schemas.
- Validate DTOs according to schema; reject invalid `specsJson`.
- Return standard `ErrorResponse`.
- Enforce `cookieAuth` and `csrfHeader` on admin state-changing endpoints.
- Use `uploadId` + confirm flow for R2 media.

Frontend Agent must:

- Generate or manually follow TypeScript types from `openapi.yaml`.
- Use same-origin `/api/v1/*`; never call Railway backend URL directly.
- Attach `X-CSRF-Token` header for admin POST/PATCH/DELETE requests.
- Handle common `ErrorResponse` consistently.
- Never assume arbitrary JSON in `specsJson`.

Review Agent must block code that diverges from `openapi.yaml`.

## 12. Definition of Done

- Matches docs.
- Builds.
- Passes lint/typecheck where available.
- Does not violate scope.
- Does not load heavy media unnecessarily.
- Has fallback/error handling for 3D/AR.
- Security rules are not bypassed.


## 13. Vietnamese UI Language Rule

Phase 1 UI language is Vietnamese.

Frontend Agent must write all user-facing UI copy in Vietnamese:

- Public website
- Admin UI
- Form labels
- Validation messages
- Toast messages
- Empty states
- SEO metadata
- 3D/AR fallback messages

Backend Agent must keep machine error codes in English but use Vietnamese default error messages when messages may be shown to users.

Code identifiers, API fields, enum values, filenames, and task IDs remain English.

Follow `LANGUAGE_POLICY.md`.

Review Agent must block English UI labels unless explicitly approved by Human.


## 14. Responsive UI Rule

Phase 1 must support mobile phone, tablet, laptop, and desktop PC.

Frontend Agent must implement mobile-first responsive layouts.

Minimum test widths:

```text
360px
390px
430px
768px
1024px
1280px
1440px
```

Review Agent must block code if:

- UI only works on laptop/desktop.
- Mobile or tablet has broken layout.
- Horizontal scroll appears on supported viewports.
- Admin UI is desktop-only.
- Product detail CTA is hard to reach on mobile.


## 15. Android & iOS Mobile Browser Rule

Phase 1 must work on Android Chrome and iOS Safari.

Frontend Agent must consider:

- iOS safe-area inset.
- iOS input font-size minimum 16px.
- Android back button behavior for modals/drawers.
- Mobile file picker upload.
- No hover-only critical actions.
- AR fallback differences between Android and iOS.
- USDZ requirement for reliable iOS AR.

Review Agent must block code if mobile web works only on desktop responsive emulator but fails Android/iOS browser rules.
