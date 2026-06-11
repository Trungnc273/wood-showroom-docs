# ADR-007 — Frontend State Management

## Status

Accepted

## Decision

Use:

- Next.js Server Components/fetch for public data.
- TanStack Query for admin server-state.
- Zustand for lightweight client UI state.

Do not use Redux Toolkit in Phase 1.

## Global API Error Handling

Admin API client must centralize unauthorized handling.

Rule:

```text
If any admin API request returns HTTP 401:
1. Clear client-side admin UI state where applicable.
2. Invalidate/clear TanStack Query admin cache.
3. Redirect user to /admin/login.
4. Show a lightweight toast: "Phiên đăng nhập đã hết hạn, vui lòng đăng nhập lại."
```

Do not duplicate ad-hoc 401 `try/catch` logic in every component.

## API Client Rule

Admin screens must use a shared admin API client under:

```text
src/shared/api/
```

This client handles:

- credentials/cookie behavior
- CSRF header injection
- global 401 handling
- standard error mapping

## Reason

This matches Next.js App Router and avoids unnecessary global state complexity.

Centralized unauthorized handling prevents duplicated error logic and makes admin auth behavior predictable.
