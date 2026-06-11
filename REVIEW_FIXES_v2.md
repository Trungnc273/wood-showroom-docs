# REVIEW_FIXES_v2.md

## Purpose

This file records the fixes applied after architecture/docs review.

## Applied Fixes

### 1. SPEC.md and TASKS.md

They already exist in the documentation pack.

Files:

```text
SPEC.md
TASKS.md
```

### 2. AGENTS.md Guardrails

Added:

- FE import rules
- Feature import ban
- Context isolation guidance for FE/BE/DevOps Agents

### 3. DATA_MODEL.md / API_SPEC.md / openapi.yaml

Added strict schema for `specs_json` / `specsJson`.

Allowed shape:

```json
{
  "width_cm": 120,
  "height_cm": 75,
  "depth_cm": 60,
  "seat_height_cm": 45,
  "weight_kg": 18,
  "finish": "Sơn PU mờ"
}
```

Unknown nested objects are not allowed in Phase 1.

### 4. CI_CD.md

Added Prisma migration deploy flow:

```text
npx prisma generate
npx prisma migrate deploy
```

Also documents `DIRECT_DATABASE_URL`.

### 5. ASSET_STORAGE_STRATEGY.md

Added R2 presigned URL retry/resilience behavior:

```text
max 3 retries
exponential backoff
return 503 if storage remains unavailable
```

### 6. TASKS.md

Added task status and owner tracking fields.

Allowed statuses:

```text
TODO
IN_PROGRESS
BLOCKED
REVIEW
DONE
```
