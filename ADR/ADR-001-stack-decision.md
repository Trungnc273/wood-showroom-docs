# ADR-001 — Stack Decision

## Status

Accepted

## Decision

Use:

- Frontend: Next.js 15 + App Router + TypeScript + Tailwind CSS
- Backend: NestJS + TypeScript modular monolith
- Database: PostgreSQL
- ORM: Prisma
- Storage: Cloudflare R2
- 3D/AR: model-viewer + GLB/USDZ
- API: `/api/v1` + `openapi.yaml`
- Deploy: Vercel + Railway + Managed PostgreSQL + R2

## Reason

This stack balances MVP speed, SEO, maintainability, media-heavy delivery, and future scalability.
