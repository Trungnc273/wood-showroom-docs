# ASSET_STORAGE_STRATEGY.md — Images, GLB, USDZ

## 1. Decision

Use Cloudflare R2 for production media storage.

Do not store media binaries in PostgreSQL.

Do not store production media on local server disk.

## 2. Asset Types

- Product images
- Product thumbnails
- Hero images
- GLB model files
- USDZ AR files
- Preview videos if needed

## 3. Storage Layout

```text
products/{productId}/images/{mediaId}.webp
products/{productId}/thumbnails/{mediaId}.webp
products/{productId}/models/{mediaId}.glb
products/{productId}/ar/{mediaId}.usdz
site/hero/{fileName}
site/brand/{fileName}
```

## 4. Upload Flow

```text
Admin chooses file
→ Frontend pre-validates file
→ Backend validates file metadata and admin permission
→ Backend creates signed upload URL
→ Frontend uploads directly to R2
→ Backend saves metadata in PostgreSQL
```

## 5. Validation

Frontend and backend both validate:

- Extension
- MIME type
- File size
- Media type
- Admin permission

Do not trust frontend validation alone.

## 6. Size Limits

GLB:

```text
Target: < 5MB
Warning: > 10MB
Reject by default: > 30MB
```

USDZ:

```text
Target: < 20MB
Warning: > 30MB
Reject by default: > 50MB
```

Images:

```text
Product image max: 5MB
Thumbnail target: < 1MB
```

## 7. GLB Optimization

Phase 1 does not optimize server-side automatically.

Asset preparer/admin must optimize before upload.

Recommended tools:

- Blender export settings
- gltf-transform
- Texture compression
- Mesh simplification
- Draco/mesh compression if suitable

Possible scripts later:

```text
npm run glb:inspect
npm run glb:optimize
```

## 8. Phase 2 Option

Later add:

```text
BullMQ + Redis
Server-side media processing
Automatic GLB optimization
Automatic image derivatives
Automatic thumbnail generation
```


## 9. R2 Presigned URL Resilience

When backend requests or prepares a presigned upload URL and R2/S3-compatible client fails with a transient 5xx/network error, the system must not crash.

Required behavior:

```text
Retry max: 3 attempts
Strategy: exponential backoff
Example delays: 300ms, 900ms, 2700ms
```

If still failing after retries, backend returns:

```http
503 Service Unavailable
```

Response:

```json
{
  "error": {
    "code": "STORAGE_TEMPORARILY_UNAVAILABLE",
    "message": "Hệ thống lưu trữ đang gián đoạn, vui lòng thử lại sau",
    "details": {}
  }
}
```

Rules:

- Do not crash NestJS process.
- Do not create media metadata if upload URL was not created.
- Admin UI must show a friendly error.
- Admin can retry manually.


## 10. Edge Cases

### Concurrent uploads

Multiple uploads can be in-flight simultaneously.

Rules:

- Each upload receives a unique `uploadId`.
- Each upload uses a unique `pending/{uploadId}/...` storage key.
- Confirm calls are independent.
- Upload order must not determine media sort order.

### Abandoned uploads

If browser closes after R2 upload but before confirm:

- Object remains under `pending/`.
- No `product_media` DB row is created.
- Pending objects older than 24h are removed by lifecycle/manual/scheduled cleanup.
- Admin UI should warn if user navigates away during upload.

### Duplicate file names

Duplicate original file names are allowed.

Storage keys must not rely only on original file name. Use:

```text
pending/{uploadId}/{safeFileName}
products/{productId}/{mediaId}/{safeFileName}
```

### Confirm failure

If confirm fails:

- Show friendly admin error.
- Do not create product_media row.
- File remains pending and cleanup handles it later.
