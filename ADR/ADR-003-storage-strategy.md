# ADR-003 — Storage Strategy

## Status

Accepted

## Decision

Use Cloudflare R2 for production media.

Do not store media binaries in PostgreSQL.

Do not store production media on local server disk.

## Upload Flow

Use presigned upload with explicit confirmation.

```text
1. Admin selects file.
2. Frontend performs pre-upload validation.
3. Frontend requests presigned URL from backend.
4. Backend creates uploadId and pending storage key.
5. Frontend uploads directly to R2.
6. Frontend calls POST /api/v1/admin/media/confirm with uploadId.
7. Backend creates product_media metadata only after confirm succeeds.
```

## Pending / Orphan Cleanup

If R2 upload succeeds but metadata confirmation fails, the object becomes pending/orphaned.

Phase 1 cleanup strategy:

```text
- Store pending uploads under pending/{uploadId}/...
- Add documentation for deleting pending objects older than 24h using R2 lifecycle/prefix cleanup if available.
- If lifecycle cleanup is not configured, add a manual/scheduled cleanup task before production.
```

Phase 2 option:

```text
- Automated cleanup worker
- Redis/BullMQ queue if media processing becomes complex
```

## Reason

The project is media-heavy. Object storage + CDN is required for images, GLB, and USDZ.

Explicit confirmation prevents database rows from pointing to files that were never uploaded and gives the system a cleanup path for orphaned files.
