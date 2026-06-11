# SPEC.md — Wood Showroom 3D/AR MVP

## 1. Product Summary

Wood Showroom 3D/AR is a Vietnamese digital showroom/catalogue website for wooden furniture, wood decor, and handcrafted wood products.

Customers browse products, view images/specs, view 3D models, try AR room preview on mobile, and contact the workshop owner through Zalo, Facebook/Messenger, phone, or a simple inquiry form.

This is not a full e-commerce platform in Phase 1.

## 2. Phase 1 Goals

The MVP must support:

- Product discovery
- Product details
- 3D product viewing
- AR room preview
- Direct contact to workshop
- Admin product/media/category management
- Inquiry management
- Basic SEO

## 3. Phase 1 Non-goals

Do not build:

- Buyer account/login
- Shopping cart
- Online payment
- Checkout
- Order tracking
- Microservices
- Separate admin repository
- Redis
- Marketplace/multi-vendor
- Complex realtime chat system
- Review/rating system
- Recommendation AI
- Product color variant API/table
- Site settings admin API/table

## 4. Product Types

Phase 1 supports only two product types.

### 4.1 Mẫu đặt làm

Internal value:

```text
made_to_order
```

Recommended fields:

```text
product_type = made_to_order
is_customizable = true
production_lead_time = text
stock_quantity = null
```

### 4.2 Hàng có sẵn

Internal value:

```text
ready_stock
```

Recommended fields:

```text
product_type = ready_stock
stock_quantity = number
is_unique = true/false
is_customizable = true/false
```

### 4.3 Độc bản is not a product type

Use:

```text
is_unique = true
stock_quantity = 1
```

### 4.4 Đặt theo yêu cầu is not a product type

Use inquiry flow:

```text
inquiry_type = custom_order
```

## 5. Customer-facing Requirements

### 5.1 Homepage

Must include:

- Premium hero
- Product value proposition
- 3D/AR highlight
- Featured products
- Product type sections/tabs
- Custom order CTA
- Workshop trust section
- Contact CTA

### 5.2 Catalogue

Must include:

- Product list/grid
- Product type filter
- Category filter
- Product card
- CTA to product detail

### 5.3 Product Detail

Must include:

- Image gallery
- Product name
- Product type badge
- Category
- Material
- Specs/dimensions from `specsJson`
- Finish/color note from `specsJson.finish` and `specsJson.color_note`
- Price or contact label
- Stock/status
- 3D viewer entry
- AR preview entry
- Contact buttons
- Fallback if 3D/AR fails

### 5.4 Contact

Must support:

- Zalo
- Facebook/Messenger
- Phone
- Optional inquiry form

## 6. Admin Requirements

Admin must be able to:

- Login
- Check current session via `/api/v1/admin/auth/me`
- Logout
- Create/edit/archive products
- Publish/hide products via product status
- Manage categories with basic CRUD
- Upload images
- Upload GLB
- Upload USDZ if needed
- Confirm media upload
- Reorder media
- Delete media
- Set primary image
- View inquiries
- Update inquiry status

## 7. Deferred Phase 2 Admin Requirements

Do not implement in Phase 1 unless explicitly re-scoped:

- Product color variant table/API
- Site settings table/API
- Admin-managed homepage content
- Admin-managed contact info
- Advanced media processing queue

Phase 1 uses static/env/config for contact info and `specsJson` for finish/color notes.

## 8. 3D/AR Requirements

- Product cards must not load GLB/USDZ.
- Product detail shows images first.
- 3D viewer loads only when user clicks “Xem 3D”.
- AR starts only when user clicks “Thử AR”.
- Use `ModelViewerBoundary`.
- Loading timeout is exactly `15s`.
- No infinite loading.
- Fallback to product image if model fails.
- Show light toast/message on failure.

## 9. Frontend State Requirements

- Public data: Next.js Server Components/fetch/cache where appropriate.
- Admin server-state: TanStack Query.
- Client UI state: Zustand.
- Do not use one global store for everything.
- Do not use Redux Toolkit in Phase 1.

## 10. Performance Requirements

- Mobile-first.
- Product cards use thumbnails only.
- Lazy-load below-the-fold images.
- Lazy-load 3D/AR.
- Serve media via Cloudflare R2/CDN.
- Backend must not stream large media.
- Avoid heavy mobile animation.
- No horizontal scroll on mobile.

## 11. Security Requirements

- Admin auth uses JWT in HttpOnly Secure SameSite cookie.
- Do not store admin JWT in localStorage.
- CSRF Double Submit Cookie required for admin state-changing requests.
- R2 secrets server-side only.
- File upload must validate extension, MIME, size, and admin permission.
- Browser calls same-origin `/api/v1/*` through Next.js rewrite/proxy.

## 12. Acceptance Criteria

MVP is acceptable when:

- Customers can browse products.
- Customers can open product details.
- Customers can view product images/specs/category.
- Customers can open 3D viewer where model exists.
- Customers can try AR where asset/device supports it.
- Customers can contact workshop.
- 3D/AR errors fallback gracefully after 15s timeout.
- Admin can manage products/media/categories/inquiries.
- Media is stored in R2, not DB/local production.
- Mobile UX is usable.
- SEO basics are implemented.
- No buyer login/cart/payment exists in Phase 1.


## 13. Language Requirements

The website must be Vietnamese-first in Phase 1.

All user-facing copy must be Vietnamese:

- Public customer UI
- Admin UI
- Form labels
- Validation messages
- Toast/error messages
- SEO title/description
- Product badges
- Button labels
- 3D/AR fallback messages

Technical/internal identifiers remain English:

- Code identifiers
- API fields
- Enum values
- Database columns
- File names
- Task IDs

Follow `LANGUAGE_POLICY.md`.

Examples:

```text
made_to_order → Mẫu đặt làm
ready_stock → Hàng có sẵn
published → Đang hiển thị
hidden → Đang ẩn
archived → Đã lưu trữ
```


## 14. Responsive UI Requirements

The website must support:

```text
Mobile phone
Tablet
Laptop
Desktop PC
```

Follow `MOBILE_RESPONSIVE_REQUIREMENTS.md`.

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

Requirements:

- Mobile-first implementation.
- No horizontal scroll on supported viewports.
- Product catalogue adapts: 1 column mobile, 2 columns tablet, 3-4 columns desktop.
- Product detail adapts: stacked layout mobile, two-column layout tablet/desktop.
- Admin UI must not be desktop-only.
- 3D/AR fallback must work on mobile and tablet.
- CTA buttons must remain reachable on mobile.


## 15. Android & iOS Mobile Web Requirements

The website must work well on mobile web for:

```text
Android Chrome
iOS Safari
```

Follow `MOBILE_BROWSER_COMPATIBILITY.md`.

Requirements:

- Do not assume desktop browser behavior on mobile.
- iOS Safari safe-area and viewport behavior must be handled.
- Android Chrome back button behavior for modals/drawers must be acceptable.
- Mobile upload must work through normal file picker.
- Admin UI must be usable on Android/iOS browsers.
- 3D/AR must have Android/iOS-specific fallback behavior.
