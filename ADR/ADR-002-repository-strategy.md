# ADR-002 — Repository Strategy

## Status

Accepted

## Decision

Use multi-repo:

```text
wood-showroom-docs
wood-showroom-web
wood-showroom-api
```

Optional later:

```text
wood-showroom-infra
wood-showroom-admin
```

## Reason

FE/BE need separate Git, Docker, CI/CD, and ownership. Docs repo is the source of truth.
