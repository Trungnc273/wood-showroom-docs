# MOBILE_BROWSER_COMPATIBILITY.md — Android & iOS Mobile Web Compatibility

## 1. Decision

The website must work well on mobile web for both:

```text
Android Chrome
iOS Safari
```

Secondary supported mobile browsers:

```text
Android Samsung Internet
iOS Chrome
iOS Edge
```

Important note:

On iOS, Chrome/Edge still use WebKit under the hood. Therefore, iOS Safari compatibility is the main iOS target.

---

## 2. Minimum Browser Targets

Phase 1 must test at minimum:

```text
Android Chrome - latest stable
iOS Safari - latest stable
iOS Safari - previous major version if available
Samsung Internet - latest stable if available
```

Do not assume a feature works on iPhone just because it works on Android Chrome.

---

## 3. iOS Safari Rules

### 3.1 Viewport Height

Avoid relying only on `100vh` for full-screen sections/modals because iOS Safari browser chrome changes viewport height.

Use safer units where possible:

```css
min-height: 100dvh;
```

Fallback:

```css
min-height: 100vh;
```

For full-screen mobile drawers/modals, test with Safari address bar expanded and collapsed.

### 3.2 Safe Area Insets

Support iPhone notch/home indicator safe areas.

Use:

```css
padding-bottom: env(safe-area-inset-bottom);
padding-top: env(safe-area-inset-top);
```

Apply especially to:

- Sticky bottom CTA
- Mobile bottom navigation
- Fullscreen 3D/AR modal
- Cookie/toast area if near bottom

### 3.3 Input Zoom

iOS Safari may zoom when input font size is below 16px.

Rule:

```text
All form inputs, selects, and textareas must use at least 16px font-size on mobile.
```

### 3.4 Sticky / Fixed Elements

Test sticky bottom CTA and fixed header on iOS Safari.

Rules:

- Fixed bottom CTA must not cover form buttons.
- Fixed bottom CTA must respect safe-area inset.
- Header must not jitter badly when scrolling.
- Avoid nested fixed elements inside transformed parents.

### 3.5 File Upload

iOS file picker behavior differs from desktop and Android.

Rules:

- Admin upload UI must accept files using normal `<input type="file">`.
- Do not depend on drag-and-drop only.
- Show selected file name, size, and validation result clearly.
- Client validation must run after file selection.

### 3.6 Hover

iOS has no real hover.

Rules:

- Do not hide important actions behind hover.
- Product card CTA must be visible/tappable.
- Tooltips must not be required for understanding.

---

## 4. Android Chrome Rules

### 4.1 Viewport and Address Bar

Android Chrome also changes viewport height when browser chrome expands/collapses.

Use responsive layout and avoid critical content being placed only at the very bottom without spacing.

### 4.2 Back Button Behavior

Android users often use the system back button.

Rules:

- Modals/drawers should close predictably.
- Product image gallery or 3D modal should not trap the user.
- Navigation should not create confusing history loops.

### 4.3 File Upload

Android supports file picker/camera sources.

Rules:

- Upload validation must handle files from camera/gallery/file manager.
- MIME type may be missing or inconsistent; backend must validate extension and content metadata defensively.
- UI must show file size errors before presigned upload.

### 4.4 Performance

Android devices vary widely.

Rules:

- Do not auto-load GLB/USDZ.
- Avoid heavy animation on mobile.
- Use thumbnails and lazy loading.
- Keep product listing lightweight.

---

## 5. 3D/AR Compatibility

### 5.1 Android

Android AR usually depends on:

```text
model-viewer
WebXR / Scene Viewer support
Chrome compatibility
Device AR capability
```

Rules:

- Show `Thử AR trong phòng` only when AR is likely supported, or show fallback clearly.
- If AR launch fails, show Vietnamese fallback message.
- Product detail must remain usable without AR.

### 5.2 iOS

iOS AR generally uses:

```text
USDZ
AR Quick Look
Safari / iOS WebKit behavior
```

Rules:

- If product has AR on iOS, USDZ asset should exist.
- If USDZ does not exist, hide/disable AR button or show fallback.
- GLB alone is not enough for reliable iOS AR.
- Do not assume Android AR behavior works on iOS.

### 5.3 Required Fallbacks

Vietnamese fallback messages:

```text
Thiết bị hiện không hỗ trợ AR. Bạn vẫn có thể xem ảnh và mô hình 3D của sản phẩm.
Không thể mở AR trên thiết bị này. Vui lòng thử xem ảnh sản phẩm hoặc liên hệ tư vấn.
Không thể tải mô hình 3D, bạn vẫn có thể xem ảnh sản phẩm.
```

---

## 6. Touch Interaction Rules

Minimum tap target:

```text
44px × 44px
```

Rules:

- Buttons must be easy to tap with thumb.
- Product card should not require precise tapping.
- Spacing between action buttons should be at least 8px.
- Avoid tiny icon-only actions in public UI.
- Admin icon actions must have labels or accessible names.

---

## 7. Mobile Forms

Rules:

- Inputs use 16px font size minimum.
- Labels visible, not placeholder-only.
- Validation messages appear close to fields.
- Keyboard type should match field:
  - phone → `tel`
  - email if added later → `email`
  - number specs → numeric/decimal where appropriate
- Submit button remains reachable.
- Avoid form actions hidden below sticky CTA.

---

## 8. Mobile Admin Compatibility

Admin UI must work on Android/iOS browsers.

Requirements:

- Login form usable on iOS Safari and Android Chrome.
- Product form usable at 390px.
- Category management usable at 390px.
- Inquiry list usable at 390px.
- Media upload works with file picker.
- Do not require drag-and-drop.
- Tables become cards/lists on mobile.
- Sticky save/delete actions respect safe area.

---

## 9. Browser Feature Fallback Rules

Do not depend on unsupported APIs without fallback.

Check before use:

```text
WebXR
Share API
Clipboard API
IntersectionObserver
ResizeObserver
dialog element behavior
```

If using newer APIs, provide fallback or simple behavior.

---

## 10. QA Checklist

Test at minimum:

```text
Android Chrome 390px / 430px
iOS Safari 390px / 430px
Tablet Safari/Chrome 768px
Tablet landscape 1024px
```

Check:

- Header/menu opens/closes.
- No horizontal scroll.
- Sticky CTA respects safe area.
- Inputs do not zoom on iOS.
- Product card actions are tappable.
- Product detail gallery works with touch.
- 3D viewer opens and can be closed.
- AR fallback works when unsupported.
- Admin login works.
- Admin upload works via file picker.
- Admin forms can be saved.
- Toasts do not cover critical buttons.
- Back button behavior is acceptable on Android.

---

## 11. Agent Rules

Frontend Agent must not implement desktop-only or Android-only behavior.

Review Agent must block code if:

- iOS Safari breaks layout.
- Android Chrome back button traps user in modal.
- Inputs zoom on iOS because font-size is too small.
- Sticky CTA ignores safe-area inset.
- Admin upload requires drag-and-drop only.
- AR has no iOS/Android fallback.
