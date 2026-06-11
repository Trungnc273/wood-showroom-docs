# ADR-006 — Redis Decision

## Status

Accepted

## Decision

Do not use Redis in Phase 1.

Use Cloudflare WAF/rate limiting at edge and in-memory rate limit as secondary.

Prepare abstractions for cache/rate-limit/queue.

## Reason

Phase 1 does not need buyer sessions, realtime chat, queues, or distributed cache.
