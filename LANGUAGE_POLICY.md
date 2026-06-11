# LANGUAGE_POLICY.md — Vietnamese UI & Content Policy

## 1. Decision

The Wood Showroom 3D/AR website is a Vietnamese-first website.

All user-facing website text must be in Vietnamese in Phase 1.

This includes:

- Public customer website
- Admin UI
- Form labels
- Validation messages
- Toast messages
- Empty states
- Error messages shown to users
- SEO title/description
- Button labels
- Product badges
- 3D/AR fallback messages
- Contact CTA copy

## 2. Internal Language Rules

Use English for technical/internal identifiers:

```text
Code identifiers
API paths
API field names
Database field names
Enum values
File names
Git branches
Environment variable names
OpenAPI schema names
Task IDs
```

Examples:

```text
productType = made_to_order
inquiryType = product_consultation
status = published
specsJson.color_note
```

But UI display must be Vietnamese:

```text
made_to_order → Mẫu đặt làm
ready_stock → Hàng có sẵn
published → Đang hiển thị
hidden → Đang ẩn
archived → Đã lưu trữ
```

## 3. Public UI Copy

Common labels:

```text
Trang chủ
Sản phẩm
Mẫu đặt làm
Hàng có sẵn
Đồ độc bản
Chất liệu
Kích thước
Màu hoàn thiện
Tình trạng
Liên hệ báo giá
Xem chi tiết
Xem 3D
Thử AR trong phòng
Liên hệ tư vấn
Tư vấn đặt làm
Giữ hàng
```

Product type labels:

```text
made_to_order → Mẫu đặt làm
ready_stock → Hàng có sẵn
```

Price labels:

```text
contact → Liên hệ báo giá
from → Từ
fixed → Giá bán
```

## 4. Admin UI Copy

Admin labels must also be Vietnamese.

Examples:

```text
Đăng nhập quản trị
Tổng quan
Quản lý sản phẩm
Thêm sản phẩm
Chỉnh sửa sản phẩm
Quản lý danh mục
Quản lý media
Yêu cầu liên hệ
Trạng thái
Đang nháp
Đang hiển thị
Đang ẩn
Đã lưu trữ
Lưu thay đổi
Huỷ
Xoá
Lưu trữ
```

## 5. Validation Messages

Validation messages shown in UI must be Vietnamese.

Examples:

```text
Vui lòng nhập tên sản phẩm.
Slug không hợp lệ.
Vui lòng chọn loại sản phẩm.
Kích thước phải là số lớn hơn hoặc bằng 0.
File GLB không được vượt quá 30MB.
File USDZ không được vượt quá 50MB.
Ảnh không được vượt quá 5MB.
Phiên đăng nhập đã hết hạn, vui lòng đăng nhập lại.
```

## 6. API Error Rule

Error `code` remains English and stable for machines.

Error `message` should be Vietnamese when it may be shown directly to the user.

Example:

```json
{
  "error": {
    "code": "SLUG_ALREADY_EXISTS",
    "message": "Slug này đã được sử dụng cho sản phẩm khác.",
    "details": {
      "slug": "ban-go-soi"
    }
  }
}
```

## 7. SEO Language

SEO metadata must be Vietnamese.

Examples:

```text
Bàn trà gỗ sồi tự nhiên | Wood Showroom
Xem mẫu bàn trà gỗ sồi tự nhiên, hỗ trợ xem 3D và thử AR trong phòng.
```

`slug` should be lowercase, no Vietnamese accents, kebab-case:

```text
ban-tra-go-soi-tu-nhien
tu-go-oc-cho
ke-trang-tri-go-tu-nhien
```

## 8. Locale Formatting

Use Vietnamese locale formatting where applicable:

```text
Locale: vi-VN
Currency: VND
Date format: dd/MM/yyyy
Time format: HH:mm
```

Price example:

```text
12.500.000 ₫
```

## 9. 3D/AR Vietnamese Fallback Copy

3D load failure:

```text
Không thể tải mô hình 3D, bạn vẫn có thể xem ảnh sản phẩm.
```

AR unsupported:

```text
Thiết bị hiện không hỗ trợ AR. Bạn vẫn có thể xem ảnh và mô hình 3D của sản phẩm.
```

Loading:

```text
Đang tải mô hình 3D...
```

Timeout:

```text
Mô hình 3D tải quá lâu. Vui lòng thử lại sau.
```

## 10. Agent Rules

Frontend Agent must not create English UI labels unless:

- The string is a technical internal value.
- The string is a brand/product model name.
- Human explicitly requests English copy.

Backend Agent must return stable English error codes but Vietnamese default messages for user-facing errors.

Review Agent must block English UI copy in Phase 1 unless justified.
