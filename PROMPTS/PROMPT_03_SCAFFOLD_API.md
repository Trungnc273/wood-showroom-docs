# PROMPT_03_SCAFFOLD_API.md

Use this prompt with Codex or Claude Sonnet for `wood-showroom-api`.

---

You are the Backend Agent for Wood Showroom 3D/AR.

Goal:

Execute Phase B backend scaffolding tasks:

- API-SETUP-01
- API-SETUP-02
- API-SETUP-03

Read only these docs first:

- SPEC.md
- ARCHITECTURE.md backend sections
- DATA_MODEL.md
- API_SPEC.md
- LANGUAGE_POLICY.md
- openapi.yaml
- ASSET_STORAGE_STRATEGY.md
- AGENTS.md
- TASKS.md
- agent-rules/backend.cursorrules

Hard constraints:

- User-facing default error messages should be Vietnamese; error codes remain English.

- NestJS + TypeScript.
- Modular monolith.
- Public/admin controllers must be separated later.
- Use `/api/v1`.
- Do not implement business endpoints yet unless needed for healthcheck.
- Do not add Redis.
- Do not add microservices.
- Do not add product_colors/product_specs/site_settings tables in Phase 1.
- Prepare structure for Prisma but do not invent schema beyond DATA_MODEL.md.
- Standard error shape must be prepared:
  `{ error: { code, message, details } }`.

Before editing, produce a Shadow Plan:

1. Files to create/edit.
2. Commands to run.
3. Risks/assumptions.
4. How you will verify.

Expected output:

- NestJS app scaffolded.
- Module structure exists:
  - `src/modules/admin-auth`
  - `src/modules/products`
  - `src/modules/categories`
  - `src/modules/media`
  - `src/modules/inquiries`
  - `src/common`
- Global prefix `/api/v1` configured.
- Health endpoint or root smoke endpoint exists.
- Dockerfile and docker-compose.dev.yml exist.
- `.env.example` includes database, R2, auth/cookie, CORS/proxy settings.
- Project builds/tests.
