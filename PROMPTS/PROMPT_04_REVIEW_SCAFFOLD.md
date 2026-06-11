# PROMPT_04_REVIEW_SCAFFOLD.md

Use this prompt with Claude Opus or a Review Agent after scaffolding.

---

You are the Review Agent for Wood Showroom 3D/AR.

Review the scaffolded repositories:

- wood-showroom-web
- wood-showroom-api

Read:

- AGENTS.md
- TASKS.md
- ARCHITECTURE.md
- DESIGN_TOKENS.md
- LANGUAGE_POLICY.md
- MOBILE_RESPONSIVE_REQUIREMENTS.md
- MOBILE_BROWSER_COMPATIBILITY.md
- API_SPEC.md
- openapi.yaml
- agent-rules/review-agent.cursorrules

Check:

- UI copy is Vietnamese for public/admin surfaces.
- Mobile/tablet/laptop/desktop responsive requirements are satisfied.
- Android Chrome and iOS Safari compatibility rules are satisfied.

## Frontend

- Next.js App Router exists.
- `src/app`, `src/features`, `src/shared` structure exists.
- No feature imports another feature.
- Tailwind tokens follow DESIGN_TOKENS.md.
- Browser API calls use `/api/v1/*`, not Railway URL.
- `next.config.ts` has API proxy rewrite.
- No JWT localStorage usage.
- No GLB/USDZ/model-viewer loaded in scaffold.
- No horizontal scroll at 360/390/430/768/1024px.
- iOS safe-area/input zoom and Android modal/back behavior are considered.

## Backend

- NestJS app runs.
- `/api/v1` prefix configured.
- Modular structure exists.
- No microservice/Redis added.
- Standard error structure prepared.
- Docker/env examples exist.

Output:

- PASS/FAIL
- Violations
- Required fixes
- Whether Phase C can start
