# ADR-009 — Phase 1 Scope Refinements

## Status

Accepted

## Decision

The following refinements are accepted for Phase 1:

1. Admin Category CRUD is included.
2. Product color variant table/API is deferred.
3. Site settings table/API is deferred.
4. Product update uses partial `PATCH`, not full-replace `PUT`.
5. Media reorder has explicit endpoint.
6. Admin session check endpoint `/api/v1/admin/auth/me` is required.

## Reason

These decisions remove ambiguity before AI Agent execution.

Admin category CRUD is operationally useful in Phase 1. Product color variants and site settings can wait because they increase API/UI complexity without being necessary for MVP validation.
