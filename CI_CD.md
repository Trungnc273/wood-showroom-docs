# CI_CD.md

## 1. Tooling

Use:

```text
GitHub Actions
Vercel Git Integration
Railway GitHub Deployment
```

## 2. Branch Strategy

```text
main      → production
develop   → staging
feature/* → PR into develop
```

## 3. wood-showroom-docs

Checks:

- Markdown lint if needed.
- Validate OpenAPI if configured.

No deploy.

## 4. wood-showroom-web

Pull Request checks:

- Install dependencies.
- Lint.
- Typecheck.
- Build.
- Optional unit tests.
- Vercel Preview Deployment.

Merge `develop`:

- Deploy staging frontend.

Merge `main`:

- Deploy production frontend.

## 5. wood-showroom-api

Pull Request checks:

- Install dependencies.
- Lint.
- Typecheck.
- Unit tests.
- Build.
- Prisma schema validation.
- Migration check if possible.

Merge `develop`:

- Deploy staging backend.

Merge `main`:

- Deploy production backend.

## 6. Preview Environments

Phase 1:

- Vercel Preview for FE PRs.
- Railway PR environment optional.
- If no BE preview, FE preview may use staging API.

## 7. Multi-repo Release Safety

- API changes must be backward-compatible.
- `openapi.yaml` must be updated before FE/BE implementation.
- BE deploy first for additive changes.
- FE deploy after BE is ready.


## 8. Backend Prisma Migration Deploy

Backend deployment must include Prisma migration.

Recommended backend deploy sequence:

```text
1. Checkout code
2. Setup Node.js
3. Install dependencies
4. Run lint/typecheck/test
5. Run npx prisma generate
6. Run npx prisma migrate deploy
7. Trigger/start Railway deployment
```

Environment variables:

```text
DATABASE_URL        = pooled runtime URL if provider supports it
DIRECT_DATABASE_URL = direct database URL for migration if required
```

Rules:

- Migration must run before the new backend version serves traffic.
- Do not use `prisma migrate dev` in production/staging deploy.
- Use `prisma migrate deploy` for staging/production.
- If using PgBouncer/pooled URL, verify whether migrations require `DIRECT_DATABASE_URL`.
- A backend build that changes Prisma schema without migration is invalid.
