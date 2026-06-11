# DESIGN_TOKENS.md — Wood Showroom 3D/AR

## 1. Purpose

This file locks the visual direction for `wood-showroom-web` so the Frontend Agent does not hallucinate random colors, fonts, spacing, or component styles.

The brand should feel:

```text
premium
warm
natural
wood-crafted
mobile-first
trustworthy
simple
```

Avoid:

```text
cheap e-commerce UI
overly colorful UI
gaming/neon style
heavy glassmorphism everywhere
complex animations on mobile
```

---

## 2. Brand Concept

Core message:

> Từ gỗ tự nhiên đến không gian sống của bạn.

Visual inspiration:

- Natural wood grain
- Warm workshop lighting
- Handcrafted furniture
- Calm interior space
- Premium but not luxury-expensive
- Clean catalogue, not marketplace clutter

---

## 3. Color Tokens

Use warm neutral colors inspired by wood, cream paper, and dark walnut.

### 3.1 Core Palette

```css
:root {
  --color-bg: #FAF7F0;
  --color-surface: #FFFFFF;
  --color-surface-warm: #F3E8D8;

  --color-text: #2B2118;
  --color-text-muted: #6F6257;
  --color-text-subtle: #95887C;

  --color-brand: #7A4E2D;
  --color-brand-hover: #5F3B22;
  --color-brand-soft: #D9B98F;

  --color-accent: #B8793E;
  --color-accent-soft: #E8CDA8;

  --color-border: #E2D3C1;
  --color-border-strong: #C7AA8A;

  --color-success: #2F7D4F;
  --color-warning: #B7791F;
  --color-error: #B42318;

  --color-overlay: rgba(43, 33, 24, 0.56);
}
```

### 3.2 Tailwind Token Names

Recommended Tailwind names:

```text
background
surface
surface-warm
foreground
muted
subtle
brand
brand-hover
brand-soft
accent
accent-soft
border
border-strong
success
warning
error
overlay
```

### 3.3 Usage Rules

- Page background: `background`
- Card background: `surface`
- Soft section background: `surface-warm`
- Primary CTA: `brand`
- Hover CTA: `brand-hover`
- Highlights/badges: `accent`
- Text: `foreground`
- Secondary text: `muted`
- Borders: `border`

Do not use random blues/purples/neon colors unless explicitly required by a feature.

---

## 4. Typography

### 4.1 Font Stack

Use Vietnamese-friendly sans-serif font stack:

```css
font-family:
  Inter,
  "Be Vietnam Pro",
  system-ui,
  -apple-system,
  BlinkMacSystemFont,
  "Segoe UI",
  sans-serif;
```

If using Google Fonts, prefer:

```text
Be Vietnam Pro
Inter
```

### 4.2 Type Scale

Mobile-first type scale:

```text
display: 40px / 48px / 700
h1:      32px / 40px / 700
h2:      26px / 34px / 700
h3:      22px / 30px / 600
body:    16px / 26px / 400
small:   14px / 22px / 400
caption: 12px / 18px / 500
```

Desktop can scale up:

```text
display: 56px / 64px
h1:      44px / 54px
h2:      34px / 44px
```

### 4.3 Text Rules

- Avoid long uppercase Vietnamese headings.
- Use sentence case for headings.
- Keep product card text short.
- Product descriptions should be readable, not dense.

---

## 5. Spacing

Use 4px-based spacing.

```text
xs: 4px
sm: 8px
md: 16px
lg: 24px
xl: 32px
2xl: 48px
3xl: 64px
4xl: 96px
```

Mobile section padding:

```text
padding-x: 16px
padding-y: 48px
```

Desktop section padding:

```text
padding-x: 64px to 96px
padding-y: 80px to 120px
```

---

## 6. Radius & Shadow

Radius:

```text
sm: 8px
md: 12px
lg: 18px
xl: 24px
full: 999px
```

Shadow:

```css
--shadow-card: 0 10px 30px rgba(43, 33, 24, 0.08);
--shadow-card-hover: 0 18px 45px rgba(43, 33, 24, 0.14);
--shadow-modal: 0 24px 70px rgba(43, 33, 24, 0.24);
```

Avoid heavy dark shadows on mobile.

---

## 7. Component Style Rules

### 7.1 Button

Primary:

```text
background: brand
text: white
radius: full or lg
height: 44-48px mobile
```

Secondary:

```text
background: transparent or surface
border: border-strong
text: brand
```

Ghost:

```text
background: transparent
text: foreground/muted
```

CTA copy examples:

```text
Xem sản phẩm
Xem 3D
Thử AR trong phòng
Liên hệ tư vấn
Giữ hàng
Tư vấn đặt làm
```

### 7.2 Product Card

Must include:

- Thumbnail image only
- Product name
- Product type badge
- Category/material if available
- Price/contact label
- 3D/AR capability badge only as text/icon

Must not include:

- GLB
- USDZ
- `<model-viewer>`
- Heavy video autoplay

Card style:

```text
surface background
border
soft shadow on hover only
rounded corners
image ratio 4:3 or 1:1 depending layout
```

### 7.3 Product Detail

Recommended layout:

Mobile:

```text
Gallery
Product info
CTA sticky bottom or clear CTA block
Specs
3D/AR actions
Related products
```

Desktop:

```text
Left: gallery
Right: product info + CTA
Below: specs + 3D/AR/story sections
```

### 7.4 Admin UI

Admin should be clean and functional, not decorative.

Admin style:

```text
white/surface background
clear table/form layout
brand color for primary actions
error/warning states clear
no heavy animation
```

---

## 8. Layout Breakpoints

Recommended:

```text
sm: 640px
md: 768px
lg: 1024px
xl: 1280px
2xl: 1536px
```

Priority device widths to test:

```text
390px
430px
768px
1440px
```

No horizontal scroll on mobile.

---

## 9. Motion Rules

Use subtle motion only.

Allowed:

- Fade in
- Slight translate-y
- Image hover scale 1.02
- Button hover
- Drawer/modal transition

Avoid:

- Long scroll-jacking
- Heavy 3D animation on mobile
- Multiple simultaneous parallax effects
- Infinite animations that distract from product

Respect reduced motion if possible:

```css
@media (prefers-reduced-motion: reduce) {
  /* reduce animation */
}
```

---

## 10. 3D/AR UI Rules

3D viewer entry should be explicit:

```text
Button: Xem 3D
Button: Thử AR trong phòng
```

Before click:

- Show product image.
- Do not preload model.

After click:

- Show loading state.
- Timeout exactly 15s.
- If fail, fallback to image + contact CTA.

Fallback copy:

```text
Không thể tải mô hình 3D, bạn vẫn có thể xem ảnh sản phẩm.
```

Unsupported AR copy:

```text
Thiết bị hiện không hỗ trợ AR. Bạn vẫn có thể xem ảnh và mô hình 3D của sản phẩm.
```

---

## 11. Image Rules

Product card images:

```text
Use thumbnail
Object-fit cover
Alt text required
```

Product detail images:

```text
Use responsive image sizes
Do not block initial text render
```

Hero image:

```text
Warm wood/interior/workshop feel
Do not overpower CTA
```

---

## 12. Accessibility Rules

Minimum:

- Buttons have accessible labels.
- Images have alt text.
- Form inputs have labels.
- Color contrast must be readable.
- Keyboard focus visible.
- Dialog/modal can close with Escape.
- Admin forms show validation errors near fields.

---

## 13. Human Review Checkpoint

Before implementing all pages, Human should approve:

- Brand palette
- Button style
- Product card style
- Product detail layout
- Admin form style

This should happen after task:

```text
FE-UI-01
```


## 14. Vietnamese Copy Rules

The interface language is Vietnamese.

Use short, natural Vietnamese labels.

Good:

```text
Xem 3D
Thử AR trong phòng
Liên hệ tư vấn
Mẫu đặt làm
Hàng có sẵn
Quản lý sản phẩm
Lưu thay đổi
```

Avoid English UI copy:

```text
View details
Add product
Save changes
Upload media
Contact us
```

Technical values may stay English in code, but rendered labels must be Vietnamese.


## 15. Responsive Layout Rules

Design must be mobile-first.

Target viewports:

```text
360px
390px
430px
768px
1024px
1280px
1440px
```

Product grid:

```text
mobile: 1 column
tablet: 2 columns
desktop: 3-4 columns
```

Product detail:

```text
mobile: stacked layout
tablet/desktop: gallery + info two-column layout
```

Admin:

```text
mobile: single-column form, card-like lists
tablet/desktop: tables and multi-column forms where appropriate
```

No horizontal scroll on mobile/tablet.


## 16. iOS/Android Mobile UI Rules

Mobile UI must work on Android Chrome and iOS Safari.

Rules:

- Tap targets minimum 44px.
- Mobile input font size minimum 16px.
- Sticky bottom CTA must use safe-area spacing.
- Important actions cannot be hover-only.
- Mobile admin upload cannot depend on drag-and-drop.
- 3D/AR modal must fit small screens and be closable.
