# REVIEW_FIXES_v9.md

## Purpose

Records Android/iOS mobile web compatibility update.

## Added

### 1. MOBILE_BROWSER_COMPATIBILITY.md

Locks mobile browser requirements for:

```text
Android Chrome
iOS Safari
Samsung Internet optional
iOS Chrome/Edge as WebKit-based secondary targets
```

### 2. TASKS.md

Added:

```text
FE-MOB-01 Implement Android/iOS mobile browser compatibility foundation
QA-MOB-01 Run Android/iOS mobile browser QA
```

### 3. SPEC.md

Added Android/iOS mobile web requirements.

### 4. TEST_PLAN.md

Added Android/iOS browser QA.

### 5. Agent Rules

Updated Frontend/Review/Docs agent rules to block Android/iOS mobile issues.

### 6. Prompts

Updated scaffold/review prompts to include Android/iOS compatibility.

## Key Requirements

- iOS safe-area support.
- iOS mobile input font-size >= 16px.
- Android back button behavior for modals/drawers.
- Mobile file picker upload.
- No hover-only critical actions.
- Android/iOS AR fallback behavior.
