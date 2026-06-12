# TASKS.md — MVP Task Breakdown for AI Agents

## Status Values

```text
TODO
IN_PROGRESS
BLOCKED
REVIEW
DONE
```

## Owner Values

```text
Human
Frontend Agent
Backend Agent
DevOps Agent
Docs Agent
Review Agent
Unassigned
```

## Rules

- Each task must be small enough for one focused AI Agent pass.
- Each task must reference relevant docs/ADR.
- Each task must have acceptance criteria.
- AI Agent must produce a Shadow Plan before editing.
- A task can move to DONE only when acceptance criteria are satisfied.

---

## Phase A — Documentation Lock

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| DOC-01 | Validate documentation pack | docs | All docs | None | Required docs exist; review history is marked historical only | DONE | Docs Agent |
| DOC-02 | Validate OpenAPI syntax | docs | openapi.yaml | DOC-01 | `openapi.yaml` parses with YAML/OpenAPI validator | DONE | Docs Agent |
| DOC-03 | Freeze Phase 1 scope | docs | SPEC.md, AGENTS.md | DOC-01 | Non-goals are explicit; Phase 2 deferred items are clear | DONE | Human |
| DOC-04 | Harden and validate OpenAPI contract | docs | openapi.yaml, API_SPEC.md, ADR-005, ADR-003 | DOC-02 | ProductSpecs strict; cookieAuth/csrfHeader exist; ErrorResponse standard; media confirm/reorder/category/me endpoints exist | DONE | Docs Agent |

---

## Phase B — Repository Scaffolding

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| WEB-SETUP-01 | Scaffold Next.js 15 App Router project | web | ADR-001, ARCHITECTURE.md | DOC-04 | Project runs locally; TS + Tailwind configured | DONE | Frontend Agent |
| WEB-SETUP-02 | Create FE folder structure | web | ARCHITECTURE.md, AGENTS.md | WEB-SETUP-01 | `src/app`, `src/features`, `src/shared` exist; no full FSD | DONE | Frontend Agent |
| WEB-SETUP-03 | Add Next.js API proxy rewrite | web | ADR-005, DEPLOYMENT.md | WEB-SETUP-01 | `/api/v1/*` rewrites to backend URL; browser code does not call Railway directly | DONE | Frontend Agent |
| API-SETUP-01 | Scaffold NestJS API project | api | ADR-001, ARCHITECTURE.md | DOC-04 | Project runs locally | DONE | Backend Agent |
| API-SETUP-02 | Create BE modular structure | api | ARCHITECTURE.md, AGENTS.md | API-SETUP-01 | Modules/common structure exists | DONE | Backend Agent |
| API-SETUP-03 | Setup Docker and env examples | web/api | DEPLOYMENT.md | WEB-SETUP-01, API-SETUP-01 | Each repo has Dockerfile and `.env.example` | DONE | DevOps Agent |

---

## Phase C — Backend Database Foundation

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| BE-DB-01 | Setup Prisma and PostgreSQL connection | api | DATA_MODEL.md, ADR-001 | API-SETUP-01 | Prisma connects; PrismaClient singleton provider exists | DONE | Backend Agent |
| BE-DB-02 | Define enum types in Prisma schema | api | DATA_MODEL.md | BE-DB-01 | ProductType only has `made_to_order`, `ready_stock`; other enums match docs | DONE | Backend Agent |
| BE-DB-03 | Define Category and Product schema | api | DATA_MODEL.md | BE-DB-02 | Product has no `dimensions`; material indexed; specs_json exists; category FK exists | DONE | Backend Agent |
| BE-DB-04 | Define ProductMedia schema | api | DATA_MODEL.md, ADR-003 | BE-DB-03 | Media metadata table exists; no binary fields | DONE | Backend Agent |
| BE-DB-05 | Define AdminUser and Inquiry schema | api | DATA_MODEL.md | BE-DB-03 | Admin and inquiry tables exist | DONE | Backend Agent |
| BE-DB-06 | Create and run initial migration | api | CI_CD.md | BE-DB-05 | Migration runs locally; `prisma migrate deploy` path documented | DONE | Backend Agent |
| BE-DB-07 | Create development seed script | api | DATA_MODEL.md | BE-DB-06 | `npx prisma db seed` creates 1 admin, 3-5 categories, sample products for both product types | DONE | Backend Agent |

---

## Phase D — Backend Product, Category & Validation

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| BE-PROD-01A | Implement Product DTOs | api | API_SPEC.md, DATA_MODEL.md | BE-DB-06 | DTO validates productType, status, material, price fields | DONE | Backend Agent |
| BE-PROD-01B | Implement strict `specsJson` validation | api | API_SPEC.md, DATA_MODEL.md | BE-PROD-01A | Invalid specs return 400 `INVALID_PRODUCT_SPECS`; nested unknown objects rejected | DONE | Backend Agent |
| BE-PROD-01C | Implement ProductsRepository | api | ARCHITECTURE.md | BE-PROD-01B | Repository contains DB access only | DONE | Backend Agent |
| BE-PROD-01D | Implement ProductsService | api | ARCHITECTURE.md | BE-PROD-01C | Public only returns published products; slug conflict returns `SLUG_ALREADY_EXISTS` | DONE | Backend Agent |
| BE-PROD-01E | Implement public products controller | api | API_SPEC.md, openapi.yaml | BE-PROD-01D | `GET /api/v1/products` and `GET /api/v1/products/:slug` work | DONE | Backend Agent |
| BE-PROD-01F | Implement admin products controller | api | API_SPEC.md, openapi.yaml | BE-PROD-01D | Admin create/PATCH/archive product works and validates DTOs | DONE | Backend Agent |
| BE-CAT-01A | Implement Category DTOs | api | API_SPEC.md, openapi.yaml | BE-DB-06 | Create/update category DTOs validate name, slug, sortOrder, isActive | DONE | Backend Agent |
| BE-CAT-01B | Implement CategoriesRepository/Service | api | DATA_MODEL.md | BE-CAT-01A | Active categories returned publicly; admin can manage all categories | DONE | Backend Agent |
| BE-CAT-01C | Implement public/admin category controllers | api | API_SPEC.md, openapi.yaml | BE-CAT-01B | Public GET categories and admin CRUD endpoints work | DONE | Backend Agent |
| BE-INQ-01 | Implement public inquiry creation API | api | API_SPEC.md | BE-DB-06 | `POST /contact-inquiries` validates message/inquiryType/contact info | DONE | Backend Agent |
| BE-INQ-02 | Implement admin inquiry list/status APIs | api | API_SPEC.md, openapi.yaml | BE-INQ-01 | Admin can list inquiries with pagination and PATCH status with CSRF | DONE | Backend Agent |

---

## Phase E — Backend Auth & Security

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| BE-AUTH-01 | Implement CSRF bootstrap endpoint | api | ADR-005, openapi.yaml | API-SETUP-02 | `GET /admin/auth/csrf` sets readable `csrf-token` cookie | DONE | Backend Agent |
| BE-AUTH-02 | Implement admin login with HttpOnly cookie | api | ADR-005 | BE-AUTH-01, BE-DB-07 | Login sets HttpOnly Secure SameSite cookie; no token in response body | DONE | Backend Agent |
| BE-AUTH-03 | Implement current admin endpoint | api | API_SPEC.md, openapi.yaml | BE-AUTH-02 | `GET /admin/auth/me` returns current user or 401 | DONE | Backend Agent |
| BE-AUTH-04 | Implement logout and admin auth guard | api | ADR-005 | BE-AUTH-03 | Admin APIs reject unauthenticated requests; logout clears cookie | DONE | Backend Agent |
| BE-AUTH-05 | Implement CSRF validation for mutations | api | ADR-005 | BE-AUTH-04 | Admin POST/PATCH/DELETE require valid `X-CSRF-Token` | DONE | Backend Agent |
| BE-SEC-01 | Implement standard error response | api | AGENTS.md, API_SPEC.md | API-SETUP-02 | All errors follow `{ error: { code, message, details } }` | DONE | Backend Agent |
| BE-SEC-02 | Add in-memory secondary rate limit | api | ADR-006, ADR-008 | BE-AUTH-02 | Login/contact/upload endpoints have basic in-memory protection | TODO | Backend Agent |

---

## Phase F — Backend R2 Media Flow

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| BE-MEDIA-01A | Setup R2 client service | api | ADR-003, ASSET_STORAGE_STRATEGY.md | API-SETUP-02 | R2 client configured with env only; no secrets exposed | DONE | Backend Agent |
| BE-MEDIA-01B | Implement media file validation service | api | ASSET_STORAGE_STRATEGY.md | BE-MEDIA-01A | Validates extension, MIME, size, mediaType | DONE | Backend Agent |
| BE-MEDIA-01C | Implement presigned upload API with retry | api | ASSET_STORAGE_STRATEGY.md | BE-MEDIA-01B | Retries transient R2 errors 3 times; returns 503 on failure | DONE | Backend Agent |
| BE-MEDIA-01D | Implement pending upload ID flow | api | ADR-003 | BE-MEDIA-01C | Presigned response includes uploadId and pending storageKey | DONE | Backend Agent |
| BE-MEDIA-01E | Implement media confirm API | api | ADR-003, API_SPEC.md | BE-MEDIA-01D | Confirm creates product_media metadata only after upload success | DONE | Backend Agent |
| BE-MEDIA-01F | Implement media reorder API | api | API_SPEC.md, openapi.yaml | BE-MEDIA-01E | `PATCH /admin/products/:id/media/reorder` updates sortOrder safely | DONE | Backend Agent |
| BE-MEDIA-01G | Implement media delete/archive API | api | API_SPEC.md | BE-MEDIA-01E | DB metadata delete/archive works; R2 cleanup behavior documented | DONE | Backend Agent |

---

## Phase G — Frontend Public UX

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| FE-UI-01 | Implement design tokens and base UI | web | DESIGN_TOKENS.md, SPEC.md | WEB-SETUP-02 | Tailwind brand palette, Vietnamese-friendly font stack, spacing/radius/shadow tokens, button/input/card components exist and are reviewed by Human | TODO | Frontend Agent |
| FE-LANG-01 | Implement Vietnamese UI language mapping | web | LANGUAGE_POLICY.md, SPEC.md | FE-UI-01 | Product type/status/price labels, validation messages, CTA copy, admin labels, and 3D/AR fallback copy are Vietnamese | TODO | Frontend Agent |
| FE-RESP-01 | Implement mobile-first responsive foundation | web | MOBILE_RESPONSIVE_REQUIREMENTS.md, DESIGN_TOKENS.md | FE-LANG-01 | Base layout supports 360/390/430/768/1024/1280/1440px; no horizontal scroll; spacing/container/grid utilities are mobile-first | TODO | Frontend Agent |
| FE-MOB-01 | Implement Android/iOS mobile browser compatibility foundation | web | MOBILE_BROWSER_COMPATIBILITY.md, MOBILE_RESPONSIVE_REQUIREMENTS.md | FE-RESP-01 | iOS safe-area utilities, 16px mobile input font, mobile file picker compatibility, non-hover actions, modal/back behavior guidance implemented | TODO | Frontend Agent |
| FE-PUBLIC-01 | Implement public layout/header/footer | web | SPEC.md, LANGUAGE_POLICY.md, MOBILE_RESPONSIVE_REQUIREMENTS.md, MOBILE_BROWSER_COMPATIBILITY.md | FE-MOB-01 | Mobile-first layout; no admin imports | TODO | Frontend Agent |
| FE-PUBLIC-02A | Implement homepage hero section | web | SPEC.md | FE-PUBLIC-01 | Premium hero with CTA exists; responsive mobile/desktop | TODO | Frontend Agent |
| FE-PUBLIC-02B | Implement homepage featured products section | web | SPEC.md, API_SPEC.md | FE-PROD-01 | Featured products use product card; no GLB | TODO | Frontend Agent |
| FE-PUBLIC-02C | Implement homepage product type sections | web | SPEC.md | FE-PUBLIC-02B | Sections/tabs for `made_to_order` and `ready_stock` exist | TODO | Frontend Agent |
| FE-PUBLIC-02D | Implement homepage trust/workshop section | web | SPEC.md | FE-PUBLIC-02A | Workshop intro, trust signals, contact CTA exist | TODO | Frontend Agent |
| FE-PROD-01 | Implement product card | web | PERFORMANCE_BUDGET.md, LANGUAGE_POLICY.md, MOBILE_RESPONSIVE_REQUIREMENTS.md, MOBILE_BROWSER_COMPATIBILITY.md | FE-MOB-01 | Card uses thumbnail only; no model-viewer/GLB/USDZ | TODO | Frontend Agent |
| FE-PROD-02 | Implement catalogue page | web | API_SPEC.md | FE-PROD-01 | Filter by product type/category works | TODO | Frontend Agent |
| FE-PROD-03 | Implement product detail page | web | SPEC.md, API_SPEC.md | FE-PROD-02 | Gallery/specs/category/contact show; no 3D until click | TODO | Frontend Agent |
| FE-CONTACT-01 | Implement contact CTA/form | web | SPEC.md, API_SPEC.md | FE-PROD-03 | Zalo/Facebook/Phone/form flow works | TODO | Frontend Agent |

---

## Phase G.5 — Frontend SEO

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| FE-SEO-01 | Implement product page metadata | web | SEO_STRATEGY.md, SPEC.md | FE-PROD-03 | Product detail renders title, meta description, OG image from product data | TODO | Frontend Agent |
| FE-SEO-02 | Implement sitemap.xml | web | SEO_STRATEGY.md | FE-PROD-02 | `/sitemap.xml` includes published product URLs `/san-pham/{slug}` | TODO | Frontend Agent |
| FE-SEO-03 | Implement robots.txt | web | SEO_STRATEGY.md | WEB-SETUP-01 | `/robots.txt` disallows `/admin/`, allows public pages, references sitemap | TODO | Frontend Agent |
| FE-SEO-04 | Verify server-side rendering for product pages | web | SEO_STRATEGY.md | FE-PROD-03 | Product detail HTML includes product name/description/specs in initial response | TODO | Frontend Agent |

---

## Phase H — Frontend 3D/AR

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| FE-3D-01 | Add `model-viewer` integration shell | web | ADR-004 | FE-PROD-03 | Viewer is dynamically/lazily loaded only after user click | TODO | Frontend Agent |
| FE-3D-02 | Implement `ModelViewerBoundary` | web | SPEC.md, PERFORMANCE_BUDGET.md, ADR-004 | FE-3D-01 | Wraps model-viewer; timeout exactly 15s; fallback 2D image + contact CTA; no infinite loading | TODO | Frontend Agent |
| FE-3D-03 | Implement AR launcher/fallback | web | ADR-004 | FE-3D-02 | AR opens if supported; unsupported devices see friendly fallback | TODO | Frontend Agent |
| FE-3D-04 | Add 3D/AR client state with Zustand | web | ADR-007 | FE-3D-02 | Modal/viewer/gallery state is local/lightweight; no Redux | TODO | Frontend Agent |

---

## Phase I — Frontend Admin

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| FE-ADMIN-01 | Implement admin route group/layout | web | ARCHITECTURE.md, LANGUAGE_POLICY.md, MOBILE_RESPONSIVE_REQUIREMENTS.md, MOBILE_BROWSER_COMPATIBILITY.md | FE-MOB-01 | `(admin)` routes and protected layout exist | TODO | Frontend Agent |
| FE-ADMIN-02 | Implement shared admin API client | web | ADR-005, ADR-007 | FE-ADMIN-01 | Same-origin `/api/v1` calls; CSRF header injection; global 401 redirect | TODO | Frontend Agent |
| FE-ADMIN-03 | Implement login/session check | web | ADR-005, API_SPEC.md | FE-ADMIN-02 | Login works; `/admin/auth/me` used on reload; no localStorage token | TODO | Frontend Agent |
| FE-ADMIN-04 | Setup TanStack Query for admin | web | ADR-007 | FE-ADMIN-02 | QueryClientProvider exists; admin data cached/refetched | TODO | Frontend Agent |
| FE-ADMIN-05 | Implement admin product list | web | API_SPEC.md | FE-ADMIN-04 | List/filter product admin view works | TODO | Frontend Agent |
| FE-ADMIN-06A | Implement admin product base form | web | DATA_MODEL.md | FE-ADMIN-05 | Supports name, slug, type, category, status, material, price | TODO | Frontend Agent |
| FE-ADMIN-06B | Implement strict specs sub-form | web | DATA_MODEL.md | FE-ADMIN-06A | Supports numeric specs + finish/color_note; invalid values blocked | TODO | Frontend Agent |
| FE-ADMIN-07 | Implement product create/PATCH mutations | web | API_SPEC.md | FE-ADMIN-06B | Uses TanStack mutations; handles validation/slug conflict errors | TODO | Frontend Agent |
| FE-ADMIN-08 | Implement admin category management UI | web | API_SPEC.md | FE-ADMIN-04 | Admin can list/create/update/deactivate categories | TODO | Frontend Agent |
| FE-ADMIN-09 | Implement admin inquiry list | web | API_SPEC.md | FE-ADMIN-04 | Admin can view inquiry list with status/date/message preview | TODO | Frontend Agent |
| FE-ADMIN-10 | Implement admin inquiry status update | web | API_SPEC.md | FE-ADMIN-09 | Admin can change inquiry status using mutation + CSRF | TODO | Frontend Agent |
| FE-MEDIA-01 | Implement pre-upload client validation | web | ASSET_STORAGE_STRATEGY.md | FE-ADMIN-06A | Checks extension and size before presigned call; GLB <30MB, USDZ <50MB, image <5MB; toast and abort on fail | TODO | Frontend Agent |
| FE-MEDIA-02 | Implement media upload with presigned URL | web | ADR-003, API_SPEC.md | FE-MEDIA-01 | Uploads directly to R2 using presigned URL | TODO | Frontend Agent |
| FE-MEDIA-03 | Implement media confirm call | web | ADR-003, API_SPEC.md | FE-MEDIA-02 | Calls `/api/v1/admin/media/confirm` after upload success | TODO | Frontend Agent |
| FE-MEDIA-04 | Implement media manager UI | web | SPEC.md | FE-MEDIA-03 | Delete/set primary image works | TODO | Frontend Agent |
| FE-MEDIA-05 | Implement media reorder UI | web | API_SPEC.md | FE-MEDIA-04 | Drag/reorder or manual reorder calls reorder endpoint | TODO | Frontend Agent |

---

## Phase J — Infra, CI/CD, Deployment

| ID | Task | Repo | Docs/ADR | Dependencies | Acceptance Criteria | Status | Owner |
|---|---|---|---|---|---|---|---|
| INFRA-01 | Configure Cloudflare WAF/rate limit rules | infra/docs | ADR-006, DEPLOYMENT.md | BE-SEC-02 | Rules documented for admin/login/contact/upload endpoints | TODO | DevOps Agent |
| INFRA-02 | Configure public media cache rules | infra/docs | ASSET_STORAGE_STRATEGY.md | BE-MEDIA-01E | Cache rules documented for R2/public media assets | TODO | DevOps Agent |
| CI-01 | Add web GitHub Actions | web | CI_CD.md | WEB-SETUP-01 | PR lint/typecheck/build passes | TODO | DevOps Agent |
| CI-02 | Add api GitHub Actions | api | CI_CD.md | API-SETUP-01 | PR lint/typecheck/test/build passes | TODO | DevOps Agent |
| CI-03 | Add Prisma migration deploy step | api | CI_CD.md, ADR-008 | BE-DB-06 | Deploy workflow runs `prisma generate` and `prisma migrate deploy` | TODO | DevOps Agent |
| DEPLOY-01 | Deploy frontend staging | web | DEPLOYMENT.md | CI-01 | Vercel staging URL works | TODO | DevOps Agent |
| DEPLOY-02 | Deploy backend staging | api | DEPLOYMENT.md | CI-02, CI-03 | Railway staging URL works | TODO | DevOps Agent |
| QA-01 | Run full manual QA | all | TEST_PLAN.md | DEPLOY-01, DEPLOY-02 | Customer/admin/mobile/3D/AR/upload tests completed | TODO | Review Agent |
| QA-RESP-01 | Run responsive QA matrix | web | MOBILE_RESPONSIVE_REQUIREMENTS.md | QA-01 | 360/390/430/768/1024/1280/1440px checked; no horizontal scroll; public/admin/3D fallback layouts pass | TODO | Review Agent |
| QA-MOB-01 | Run Android/iOS mobile browser QA | web | MOBILE_BROWSER_COMPATIBILITY.md | QA-RESP-01 | Android Chrome and iOS Safari checked; safe-area/input zoom/file picker/back button/AR fallback pass | TODO | Review Agent |
| QA-02 | Architecture compliance review | all | ARCHITECTURE.md, AGENTS.md | QA-01 | No feature import violations; no GLB in cards; auth/csrf follows ADR | TODO | Review Agent |
