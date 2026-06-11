# PROMPT_01_VALIDATE_DOCS.md

Use this prompt with Claude Opus or a Review Agent.

---

You are the Docs/Review Agent for the Wood Showroom 3D/AR project.

Read the documentation pack first:

- README.md
- SPEC.md
- ARCHITECTURE.md
- DATA_MODEL.md
- API_SPEC.md
- openapi.yaml
- TASKS.md
- AGENTS.md
- DESIGN_TOKENS.md
- LANGUAGE_POLICY.md
- MOBILE_RESPONSIVE_REQUIREMENTS.md
- MOBILE_BROWSER_COMPATIBILITY.md
- DOCS_VALIDATION.md
- ADR/*.md
- agent-rules/*.cursorrules

Task:

Run DOC-01 to DOC-04 from TASKS.md.

Check:

0. LANGUAGE_POLICY.md exists
0b. MOBILE_RESPONSIVE_REQUIREMENTS.md exists
0c. MOBILE_BROWSER_COMPATIBILITY.md exists and Android/iOS QA tasks are present and responsive tasks are present and Vietnamese UI requirements are enforced.

1. Every Phase 1 feature in SPEC.md has task coverage in TASKS.md.
2. Every API in API_SPEC.md exists in openapi.yaml.
3. Every Phase 1 table in DATA_MODEL.md has DB task coverage.
4. Deferred Phase 2 items are not accidentally assigned Phase 1 tasks.
5. No duplicate task IDs.
6. Security scheme names are only `cookieAuth` and `csrfHeader`.
7. openapi.yaml parses and follows the required contract.
8. Section numbering is not confusing.
9. `REVIEW_FIXES_v*.md` are treated as history only.

Output:

- PASS/FAIL
- Issues found
- Suggested fixes
- Whether Phase B scaffolding can start
