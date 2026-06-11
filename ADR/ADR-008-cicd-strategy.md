# ADR-008 — CI/CD Strategy

## Status

Accepted

## Decision

Use:

- GitHub Actions for checks.
- Vercel Git integration for frontend deployment.
- Railway GitHub deployment for backend.

Branches:

```text
main -> production
develop -> staging
feature/* -> PR into develop
```

## Backend Deploy Sequence

Backend deployment must include Prisma migration.

Recommended deploy sequence:

```text
1. Checkout code
2. Setup Node.js
3. Install dependencies
4. Run lint/typecheck/test
5. Run npx prisma generate
6. Run npx prisma migrate deploy
7. Start/trigger Railway backend deployment
```

Environment variables:

```text
DATABASE_URL        = pooled runtime URL if provider supports it
DIRECT_DATABASE_URL = direct database URL for migration if required
```

Rules:

- Do not use `prisma migrate dev` in staging/production.
- Use `prisma migrate deploy` for staging/production.
- Backend schema changes without migrations are invalid.
- BE must deploy backward-compatible changes before FE uses them.

## Reason

This is simple, fast, and suitable for MVP multi-repo development.

Running migrations before serving new backend code prevents runtime crashes caused by missing tables/columns.
