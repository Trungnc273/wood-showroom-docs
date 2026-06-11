# REVIEW_FIXES_v7.md

## Purpose

Records the language-policy fix after realizing the website must be Vietnamese.

## Added

### 1. LANGUAGE_POLICY.md

Locks Phase 1 user-facing language to Vietnamese.

Covers:

- Public UI
- Admin UI
- Form labels
- Validation messages
- Toast messages
- Error messages shown to users
- SEO metadata
- Button labels
- Product badges
- 3D/AR fallback copy

### 2. TASKS.md

Added:

```text
FE-LANG-01 Implement Vietnamese UI language mapping
```

### 3. AGENTS.md

Added Vietnamese UI Language Rule.

### 4. Agent Rules

Updated:

- frontend.cursorrules
- backend.cursorrules
- review-agent.cursorrules
- docs-agent.cursorrules

### 5. Prompts

Updated scaffolding/review prompts to include `LANGUAGE_POLICY.md`.

## Decision

Code/API/internal identifiers remain English.

Rendered UI and user-facing messages are Vietnamese.
