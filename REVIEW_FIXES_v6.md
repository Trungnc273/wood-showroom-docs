# REVIEW_FIXES_v6.md

## Purpose

Records v6 updates after v5 was accepted as production-ready for Agent execution.

## Added

### 1. DESIGN_TOKENS.md

Locks frontend visual direction:

- Color palette
- Typography
- Spacing
- Radius/shadow
- Product card rules
- Admin UI rules
- 3D/AR UI fallback copy
- Accessibility rules

### 2. DOCS_VALIDATION.md

Adds validation commands for:

- OpenAPI syntax
- Duplicate task IDs
- Security naming
- Deprecated endpoint check
- Deferred scope check

### 3. PROMPTS/

Added execution prompts:

- PROMPT_01_VALIDATE_DOCS.md
- PROMPT_02_SCAFFOLD_WEB.md
- PROMPT_03_SCAFFOLD_API.md
- PROMPT_04_REVIEW_SCAFFOLD.md

### 4. Agent rules update

Frontend Agent now explicitly reads:

- DESIGN_TOKENS.md
- SEO_STRATEGY.md

### 5. TASKS.md update

FE-UI-01 now references DESIGN_TOKENS.md.
