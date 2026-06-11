# PROMPT_02_SCAFFOLD_WEB.md

Use this prompt with Codex or Claude Sonnet for `wood-showroom-web`.

---

You are the Frontend Agent for Wood Showroom 3D/AR.

Goal:

Execute Phase B frontend scaffolding tasks:

- WEB-SETUP-01
- WEB-SETUP-02
- WEB-SETUP-03

Read only these docs first:

- SPEC.md
- ARCHITECTURE.md frontend sections
- API_SPEC.md
- openapi.yaml
- DESIGN_TOKENS.md
- LANGUAGE_POLICY.md
- MOBILE_RESPONSIVE_REQUIREMENTS.md
- MOBILE_BROWSER_COMPATIBILITY.md
- SEO_STRATEGY.md
- PERFORMANCE_BUDGET.md
- AGENTS.md
- TASKS.md
- agent-rules/frontend.cursorrules

Hard constraints:

- UI language must be Vietnamese in Phase 1.
- UI must be mobile-first and support phone/tablet/laptop/desktop.
- UI must consider Android Chrome and iOS Safari mobile web behavior.

- Next.js 15 App Router + TypeScript + Tailwind.
- Use `src/app`, `src/features`, `src/shared`.
- Features must not import other features.
- Browser code calls same-origin `/api/v1/*` only.
- Add Next.js rewrite for `/api/v1/:path*` to backend URL.
- Do not implement product pages yet.
- Do not implement 3D/AR yet.
- Do not load GLB/USDZ anywhere.
- Do not store JWT in localStorage.

Before editing, produce a Shadow Plan:

1. Files to create/edit.
2. Commands to run.
3. Risks/assumptions.
4. How you will verify.

Expected output:

- Next.js project scaffolded.
- Tailwind configured with tokens from DESIGN_TOKENS.md.
- Responsive foundation supports 360/390/430/768/1024/1280/1440px without horizontal scroll.
- Mobile browser foundation covers iOS safe-area, 16px inputs, non-hover actions, and mobile file picker.
- Folder structure exists:
  - `src/app/(public)`
  - `src/app/(admin)`
  - `src/features`
  - `src/shared/ui`
  - `src/shared/api`
  - `src/shared/lib`
  - `src/shared/hooks`
  - `src/shared/types`
- `next.config.ts` has rewrite for `/api/v1/:path*`.
- `.env.example` includes `BACKEND_API_URL`.
- Project builds/typechecks.
