# DATA_MODEL.md — PostgreSQL Data Model

## 1. Database

Use PostgreSQL.

## 2. Phase 1 Tables

```text
admin_users
categories
products
product_media
contact_inquiries
```

## 3. Deferred Phase 2 Tables

Do not create these tables in the Phase 1 migration unless explicitly re-scoped:

```text
product_colors
product_specs
site_settings
```

Phase 1 uses:

- `products.specs_json.finish`
- `products.specs_json.color_note`
- static/env/config for contact settings

## 4. Enums

### ProductType

```text
made_to_order
ready_stock
```

### ProductStatus

```text
draft
published
hidden
archived
```

### PriceType

```text
from
fixed
contact
```

### MediaType

```text
image
thumbnail
video
model_3d
ar_usdz
```

### InquiryType

```text
product_consultation
custom_order
general
```

### InquiryStatus

```text
new
contacted
resolved
spam
```

## 5. admin_users

```text
id uuid primary key
email varchar unique not null
password_hash text not null
display_name varchar
role varchar default 'admin'
is_active boolean default true
created_at timestamp
updated_at timestamp
```

## 6. categories

```text
id uuid primary key
name varchar not null
slug varchar unique not null
description text
sort_order integer default 0
is_active boolean default true
created_at timestamp
updated_at timestamp
```

## 7. products

```text
id uuid primary key
name varchar not null
slug varchar unique not null
short_description text
description text
product_type varchar not null
status varchar default 'draft'
category_id uuid references categories(id)
material text
price_type varchar default 'contact'
price numeric nullable
price_label varchar nullable
stock_quantity integer nullable
is_unique boolean default false
is_customizable boolean default false
production_lead_time varchar nullable
has_3d boolean default false
has_ar boolean default false
specs_json jsonb nullable
seo_title varchar nullable
seo_description text nullable
created_by uuid references admin_users(id)
created_at timestamp
updated_at timestamp
deleted_at timestamp nullable
```

Indexes:

```text
unique(slug)
index(status)
index(product_type)
index(category_id)
index(material)
index(created_at)
```

## 8. Product Specs JSON Schema

`products.specs_json` is not arbitrary JSON.

Allowed Phase 1 schema:

```json
{
  "width_cm": 120,
  "height_cm": 75,
  "depth_cm": 60,
  "seat_height_cm": 45,
  "weight_kg": 18,
  "finish": "Sơn PU mờ",
  "color_note": "Nâu óc chó / vàng gỗ tự nhiên"
}
```

Rules:

```text
width_cm: optional number
height_cm: optional number
depth_cm: optional number
seat_height_cm: optional number
weight_kg: optional number
finish: optional string
color_note: optional string
```

Nested unknown objects are not allowed in Phase 1.

Do not use a free-text `dimensions` column.

## 9. product_media

```text
id uuid primary key
product_id uuid references products(id) not null
media_type varchar not null
url text not null
storage_key text not null
thumbnail_url text nullable
mime_type varchar not null
size_bytes bigint
width integer nullable
height integer nullable
alt_text varchar nullable
sort_order integer default 0
is_primary boolean default false
created_at timestamp
updated_at timestamp
```

Indexes:

```text
index(product_id)
index(media_type)
index(product_id, sort_order)
```

## 10. contact_inquiries

```text
id uuid primary key
product_id uuid references products(id) nullable
name varchar nullable
phone varchar nullable
zalo varchar nullable
facebook varchar nullable
message text not null
inquiry_type varchar default 'general'
status varchar default 'new'
source_page text nullable
created_at timestamp
updated_at timestamp
```

## 11. Seed Strategy

Development seed must create:

- 1 admin user
- 3-5 categories
- 2-3 sample products per product type
- Sample product media metadata if needed

Seed command:

```text
npx prisma db seed
```
