# REVIEW_FIXES_v3.md

## Purpose

Records fixes applied after the second architecture/docs review.

## Applied Fixes

### 1. TASKS.md Granularity

Rewrote TASKS.md into fine-grained AI-friendly tasks.

Examples:

```text
BE-PROD-01A DTOs
BE-PROD-01B strict specsJson validation
BE-PROD-01C repository
BE-PROD-01D service
BE-PROD-01E public controller
BE-PROD-01F admin controller
```

### 2. ModelViewerBoundary Task

Added explicit tasks:

```text
FE-3D-02 Implement ModelViewerBoundary
```

Acceptance criteria:

- Wrap model-viewer
- 15s timeout
- 2D fallback
- Contact CTA remains available
- No infinite loading

### 3. Pre-upload Validation Task

Added explicit tasks:

```text
FE-MEDIA-01 Client pre-upload validation
BE-MEDIA-01B Backend media validation service
```

### 4. Infra WAF Task

Added:

```text
INFRA-01 Configure Cloudflare WAF/rate limit rules
```

### 5. Cross-domain Cookie Fix

Updated ADR-005, DEPLOYMENT.md, and ARCHITECTURE.md:

```text
Browser calls same-origin /api/v1/*
Next.js rewrites/proxies to Railway backend
```

### 6. CSRF Protocol

Updated ADR-005:

```text
Double Submit Cookie
csrf-token cookie
X-CSRF-Token header
```

### 7. R2 Orphan File Cleanup

Updated ADR-003 and ARCHITECTURE.md:

```text
uploadId
pending storage key
POST /api/v1/admin/media/confirm
pending/orphan cleanup after 24h
```

### 8. Global Admin 401 Handling

Updated ADR-007:

```text
401 → clear admin client state/query cache → redirect /admin/login → toast
```

### 9. Agent Rules

Added:

```text
agent-rules/frontend.cursorrules
agent-rules/backend.cursorrules
agent-rules/review-agent.cursorrules
```
