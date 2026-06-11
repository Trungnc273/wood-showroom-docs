# PERFORMANCE_BUDGET.md

## 1. Goal

The website must feel fast on mobile. Heavy media must not block the first usable page.

## 2. Core Rules

- Product cards do not load GLB/USDZ.
- Homepage does not auto-load 3D models.
- 3D loads only after user clicks “Xem 3D”.
- AR starts only after user clicks “Thử AR”.
- Product cards use thumbnails.
- Lazy-load below-the-fold images.
- Use `ModelViewerBoundary`.
- No infinite loading.
- Fallback to 2D image when 3D/AR fails.

## 3. Product Card

Allowed:

- Thumbnail
- Product name
- Badge
- Material
- Price/contact label
- `has3D`/`hasAR` badge

Not allowed:

- Loading model-viewer
- Loading GLB/USDZ
- Heavy animation

## 4. Product Detail

Initial load:

- Product text
- Gallery images
- CTA buttons

Lazy action:

- 3D viewer
- AR launcher

Timeout:

```text
3D loading timeout: 15 seconds
```

After timeout, show fallback.

## 5. Backend Performance

- Do not stream media through backend.
- Use indexes for product listing.
- Use PrismaClient singleton.
- Use connection pool sizing.
- Use Cloudflare WAF rate limiting.
- NestJS in-memory rate limit is secondary only.

## 6. Scale Consideration

If 10,000 users load a 10MB GLB:

```text
10,000 × 10MB = 100GB bandwidth
```

Therefore models must be loaded only on demand.


## 8. Mobile And Tablet Performance

Mobile and tablet performance are priority.

Rules:

- Do not auto-load GLB/USDZ on mobile/tablet.
- Do not use heavy scroll effects on mobile.
- Use responsive images.
- Product cards use thumbnails only.
- Product detail loads 3D viewer only after user click.
- Admin pages should remain usable on mobile, but heavy admin tables should become card/list layouts.


## 9. Android & iOS Performance

Android and iOS devices vary in performance.

Rules:

- Do not auto-load model-viewer on mobile.
- Do not auto-load GLB/USDZ.
- Reduce heavy animations on mobile.
- Use responsive images for mobile Safari/Chrome.
- Avoid scroll-jacking.
- 3D/AR must be opt-in.
- Fallback must show quickly if unsupported.
