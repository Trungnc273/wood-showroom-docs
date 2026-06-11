# REVIEW_FIXES_v8.md

## Purpose

Records the responsive/mobile-tablet requirement update.

## Added

### 1. MOBILE_RESPONSIVE_REQUIREMENTS.md

Locks requirement that the website must support:

```text
Mobile phone
Tablet
Laptop
Desktop PC
```

Minimum viewport test targets:

```text
360px
390px
430px
768px
1024px
1280px
1440px
```

### 2. TASKS.md

Added:

```text
FE-RESP-01 Implement mobile-first responsive foundation
QA-RESP-01 Run responsive QA matrix
```

### 3. SPEC.md

Added responsive UI requirements.

### 4. TEST_PLAN.md

Added responsive QA matrix.

### 5. Agent Rules

Updated Frontend/Review/Docs agent rules to block desktop-only UI.

### 6. Prompts

Updated scaffold/review prompts to include responsive requirements.

## Decision

The project is not laptop/desktop-only.

Phase 1 must be mobile-first and responsive across phone, tablet, laptop, and desktop.
