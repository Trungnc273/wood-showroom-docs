# ADR-005 — Admin Auth Strategy

## Status

Accepted

## Decision

Use JWT stored in HttpOnly Secure cookie for admin authentication.

Do not store JWT in localStorage.

Use CSRF protection for admin state-changing requests.

## Cookie Domain Strategy

Frontend is deployed on Vercel and backend is deployed on Railway, so direct browser calls to the Railway backend domain would create cross-domain cookie problems.

Required production strategy:

```text
Browser calls same-origin frontend URL:
/api/v1/*

Next.js rewrites/proxies request to Railway backend.
```

Frontend code must call same-origin API paths only:

```text
/api/v1/admin/auth/login
/api/v1/admin/products
/api/v1/products
```

Frontend code must not call the Railway backend URL directly from the browser.

Cookie settings:

```text
HttpOnly: true
Secure: true in staging/production
SameSite: Strict by default for same-origin proxy flow
Path: /
```

If a later deployment mode requires true cross-site requests, this ADR must be revisited before changing cookie behavior.

## CSRF Pattern

Use Double Submit Cookie pattern.

Backend behavior:

```text
1. On login or CSRF bootstrap, backend sets a non-HttpOnly cookie named csrf-token.
2. For state-changing requests, backend validates the request header X-CSRF-Token.
3. The header value must match the csrf-token cookie value or a signed/HMAC-equivalent value depending on implementation.
```

Frontend behavior:

```text
1. Read csrf-token cookie from browser.
2. Send it as X-CSRF-Token for POST, PUT, PATCH, DELETE admin requests.
```

State-changing admin requests include:

```text
POST
PUT
PATCH
DELETE
```

## Why

- HttpOnly cookie reduces token theft risk from XSS.
- Same-origin proxy avoids cross-site cookie issues on Vercel/Railway.
- Double Submit Cookie gives frontend/backend an explicit CSRF protocol.
