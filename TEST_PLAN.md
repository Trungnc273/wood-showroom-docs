# TEST_PLAN.md

## 1. Customer Manual Tests

- Open homepage on mobile.
- Open homepage on desktop.
- Open catalogue.
- Filter by product type.
- Open product detail.
- View image gallery.
- Click “Xem 3D”.
- Click “Thử AR”.
- Test 3D fallback when model fails.
- Test AR unsupported message.
- Click Zalo/Facebook/Phone CTA.
- Submit contact form if enabled.

## 2. Admin Manual Tests

- Login.
- Logout.
- Create product.
- Edit product.
- Publish product.
- Hide product.
- Archive product.
- Upload image.
- Upload GLB.
- Upload USDZ if used.
- Try uploading file over limit and confirm blocked.
- Reorder media.
- Set primary image.
- View inquiry.
- Update inquiry status.

## 3. Device Tests

- Android Chrome.
- iPhone Safari.
- Desktop Chrome.
- Mobile width 390px.
- Mobile width 430px.

## 4. Performance Tests

- Homepage does not load GLB.
- Catalogue cards do not load GLB/USDZ.
- Product detail loads 3D only after click.
- 3D loading has timeout.
- No horizontal scroll on mobile.
- CTA is accessible on mobile.

## 5. Security Tests

- Admin routes require auth.
- JWT is not stored in localStorage.
- State-changing admin requests require CSRF.
- File upload requires admin.
- Invalid file type is rejected.
- Oversized GLB/USDZ is rejected.


## 6. Responsive QA Matrix

Test these viewport widths:

```text
360px
390px
430px
768px
1024px
1280px
1440px
```

For each viewport, verify:

- No horizontal scroll.
- Header/menu usable.
- Product catalogue grid adapts correctly.
- Product detail CTA reachable.
- Product specs do not overflow.
- Contact buttons are tappable/clickable.
- Admin login usable.
- Admin product form usable.
- Admin category UI usable.
- Admin inquiry UI usable.
- 3D/AR fallback does not break layout.


## 7. Android & iOS Browser QA

Test on real devices if possible.

Minimum:

```text
Android Chrome
iOS Safari
```

Check:

- Header/menu works.
- Android back button closes modal/drawer predictably.
- iOS input fields do not zoom unexpectedly.
- Sticky bottom CTA respects iPhone safe area.
- Product card actions are not hover-only.
- Product detail gallery works with touch.
- 3D modal opens and closes.
- AR unsupported fallback works.
- Admin login works.
- Admin file upload works with mobile file picker.
- Admin forms remain usable.
- Toasts do not cover important actions.
