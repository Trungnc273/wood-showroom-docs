# MOBILE_RESPONSIVE_REQUIREMENTS.md — Mobile, Tablet & Desktop Requirements

## 1. Decision

The website must be **mobile-first** and fully responsive.

Phase 1 must support:

```text
Mobile phone
Tablet
Laptop
Desktop PC
```

Do not design only for laptop/PC and then shrink it later.

The primary customer browsing experience is expected to happen on mobile phones, especially through Zalo/Facebook links.

---

## 2. Target Viewports

The UI must be tested at minimum on these widths:

```text
360px  - small Android phone
390px  - common iPhone/Android phone
430px  - large phone
768px  - tablet portrait
1024px - tablet landscape / small laptop
1280px - laptop
1440px - desktop PC
```

Acceptance rule:

```text
No horizontal scroll at any target viewport.
No broken layout at any target viewport.
Primary CTA remains visible and usable.
```

---

## 3. Breakpoints

Use Tailwind-style breakpoints:

```text
base: <640px   mobile
sm:   640px
md:   768px    tablet portrait
lg:   1024px   tablet landscape / laptop
xl:   1280px   laptop / desktop
2xl:  1536px   large desktop
```

Implementation rule:

```text
Base CSS/classes = mobile
md/lg/xl = enhancements for larger screens
```

Do not write desktop-first layouts that require many overrides for mobile.

---

## 4. Public Customer UI Requirements

### 4.1 Header / Navigation

Mobile:

```text
Logo left
Menu button right
Drawer/bottom sheet/mobile menu
CTA visible or easy to access
```

Tablet/Desktop:

```text
Logo left
Navigation visible
Contact CTA visible
```

Rules:

- Header must not wrap awkwardly at 390px.
- Mobile menu must be keyboard/touch usable.
- No tiny tap targets.

### 4.2 Homepage

Mobile order:

```text
Hero
Primary CTA
Trust/product highlight
Featured products
Product type sections
Workshop story
Contact CTA
```

Tablet/Desktop may use richer two-column layouts.

Rules:

- Hero text must not overflow.
- CTA must appear within first screen or very near it.
- Heavy visual effects must not block reading on mobile.

### 4.3 Product Catalogue

Mobile:

```text
1-column product grid
Filter button/sheet
Large product image
Clear product type badge
```

Tablet:

```text
2-column product grid
Filter sidebar or collapsible filter
```

Desktop:

```text
3-4 column product grid
Filter sidebar/top filter
```

Rules:

- Product cards must use thumbnails only.
- No GLB/USDZ/model-viewer in cards.
- Filter UI must be usable with touch.

### 4.4 Product Detail

Mobile order:

```text
Image gallery
Product name + badges
Price/contact label
Primary CTA
Specs
3D/AR buttons
Description/story
Related products
```

Tablet/Desktop:

```text
Left: gallery
Right: product info + CTA
Below: specs, 3D/AR, story, related products
```

Rules:

- CTA must remain easy to reach on mobile.
- 3D/AR must be opt-in, not auto-load.
- Image gallery must support touch swipe or simple thumbnails.
- Product specs table must not overflow horizontally.

### 4.5 Contact CTA

Mobile:

```text
Sticky or clearly repeated CTA is allowed.
Zalo/Phone buttons must be large enough to tap.
```

Minimum tap target:

```text
44px height/width
```

---

## 5. Admin UI Requirements

Admin must also be responsive.

Mobile admin is not required to be as comfortable as desktop, but it must not break.

Mobile admin:

```text
Single-column forms
Tables become cards or horizontally safe lists
Sticky save button allowed
Media upload controls remain usable
```

Tablet/Desktop admin:

```text
Two-column forms where useful
Tables visible with actions
Sidebar/navigation can be visible
```

Rules:

- Admin product form must be usable at 390px.
- Category management must be usable at 390px.
- Inquiry list must be readable at 390px.
- No critical admin action should require desktop only.
- If a complex table is used, provide mobile card layout instead of forcing horizontal scroll.

---

## 6. 3D/AR Responsive Requirements

Mobile:

- 3D/AR buttons must be visible but not auto-load.
- Viewer opens in modal/fullscreen-like area.
- 15s timeout applies.
- Fallback image + contact CTA must fit on mobile.
- AR unsupported message must be Vietnamese and readable.

Tablet/Desktop:

- Viewer may be inline or modal.
- Product information and contact CTA remain accessible.

Rules:

```text
Never block contact CTA because 3D/AR failed.
Never show infinite loading.
Never load model-viewer inside product cards.
```

---

## 7. Image & Media Requirements

Mobile:

```text
Use responsive images
Avoid loading desktop-size hero image on small screens
Use thumbnails for cards
```

Tablet/Desktop:

```text
Use larger responsive image sizes
Do not stretch low-res thumbnails
```

Rules:

- Product image aspect ratio should be stable to avoid layout shift.
- Alt text is required.
- Avoid autoplay video on mobile.

---

## 8. Typography Requirements

Mobile:

```text
Body text must be readable at 16px.
Line height must be comfortable.
Headings must wrap naturally in Vietnamese.
```

Rules:

- Avoid long all-caps Vietnamese text.
- Avoid font sizes below 14px for important text.
- Product specs must be readable without zooming.

---

## 9. Performance Requirements by Device

Mobile performance is priority.

Rules:

- No GLB/USDZ auto-load.
- No product card model loading.
- Lazy-load below-the-fold images.
- Lazy-load 3D viewer.
- Reduce animation on mobile.
- Avoid heavy scroll effects on mobile.
- Respect `prefers-reduced-motion`.

---

## 10. QA Checklist

Before marking responsive tasks DONE, test:

```text
360px
390px
430px
768px
1024px
1280px
1440px
```

For each viewport:

- No horizontal scroll.
- Header usable.
- Product grid layout correct.
- Product detail CTA reachable.
- Contact buttons tappable/clickable.
- Admin form usable.
- Admin list/table usable.
- 3D/AR fallback does not break layout.
- Vietnamese text does not overflow badly.

---

## 11. Agent Rules

Frontend Agent must implement mobile-first layouts.

Review Agent must block code if:

- UI only works on laptop/desktop.
- Mobile has horizontal scroll.
- Tablet layout is broken.
- Admin UI is desktop-only.
- Product detail CTA is hard to reach on mobile.
- Product cards load heavy 3D/AR assets.


## 12. Android & iOS Browser Compatibility

Responsive layout must be tested on mobile browsers, not only resized desktop browser.

Follow:

```text
MOBILE_BROWSER_COMPATIBILITY.md
```

Required targets:

```text
Android Chrome
iOS Safari
```

Key requirements:

- iOS safe-area inset support.
- iOS input font-size at least 16px to avoid zoom.
- Android back button behavior for modals/drawers.
- File upload must work through mobile file picker.
- AR must have Android/iOS-specific fallback.
