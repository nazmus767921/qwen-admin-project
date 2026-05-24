# AdminKit — Drizzle Database Schema

> This document defines the exact database schema required to implement the 15 feature modules in AdminKit using Drizzle ORM.

## Core Tables

### 1. Admin Users (`admin_users`)
Handles admin authentication and roles.
- `id`: uuid (primary key)
- `email`: varchar (unique, not null)
- `password_hash`: text (not null)
- `name`: varchar
- `role`: enum ('superadmin', 'admin', 'staff') default 'staff'
- `avatar_url`: text
- `created_at`: timestamp
- `updated_at`: timestamp

### 2. Users / Customers (`users`)
Customer directory.
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `email`: varchar (unique)
- `phone`: varchar (unique)
- `avatar_url`: text
- `created_at`: timestamp
- `updated_at`: timestamp

### 3. Brands (`brands`)
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `slug`: varchar (unique, not null)
- `logo_url`: text
- `is_featured`: boolean (default false)
- `created_at`: timestamp
- `updated_at`: timestamp

### 4. Products (`products`)
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `slug`: varchar (unique, not null)
- `description`: text
- `brand_id`: uuid (foreign key to `brands(id)`, nullable)
- `gender`: enum ('Men', 'Women', 'Unisex') (nullable)
- `primary_image`: text
- `gallery_images`: jsonb (array of strings)
- `tags`: jsonb (array of strings)
- `notes`: jsonb `{ top: string[], middle: string[], base: string[] }`
- `is_featured`: boolean (default false)
- `is_new_arrival`: boolean (default false)
- `is_trending`: boolean (default false)
- `is_hot`: boolean (default false)
- `deleted_at`: timestamp (soft delete)
- `created_at`: timestamp
- `updated_at`: timestamp

### 5. Product Variants (`product_variants`)
- `id`: uuid (primary key)
- `product_id`: uuid (foreign key to `products(id)`, cascade)
- `label`: varchar (e.g., "50ml", "100ml") (not null)
- `price`: integer (in cents/poisha) (not null)
- `discount_price`: integer (nullable)
- `stock_quantity`: integer (default 0)
- `sku`: varchar (unique, nullable)
- `created_at`: timestamp
- `updated_at`: timestamp

### 6. Orders (`orders`)
- `id`: uuid (primary key)
- `order_number`: varchar (unique, not null)
- `user_id`: uuid (foreign key to `users(id)`, nullable)
- `customer_info`: jsonb `{ name, email, phone }`
- `shipping_address`: jsonb `{ label, recipient_name, phone, address_line1, city, zone, zip }`
- `items_subtotal`: integer
- `delivery_charge`: integer
- `discount_total`: integer
- `total`: integer
- `payment_method`: enum ('cod', 'bkash', 'nagad', 'card')
- `payment_status`: enum ('pending', 'paid', 'failed', 'refunded') default 'pending'
- `order_status`: enum ('pending', 'confirmed', 'packed', 'shipped', 'out-for-delivery', 'delivered', 'cancelled', 'returned') default 'pending'
- `tracking_number`: varchar
- `tracking_provider`: varchar
- `placed_at`: timestamp
- `updated_at`: timestamp

### 7. Order Items (`order_items`)
- `id`: uuid (primary key)
- `order_id`: uuid (foreign key to `orders(id)`, cascade)
- `variant_id`: uuid (foreign key to `product_variants(id)`, nullable)
- `combo_id`: uuid (foreign key to `combos(id)`, nullable)
- `product_name`: varchar (snapshot of name at time of order)
- `variant_label`: varchar (snapshot of variant)
- `unit_price`: integer (snapshot of price)
- `quantity`: integer (not null)
- `line_total`: integer (not null)

### 8. Reviews (`reviews`)
- `id`: uuid (primary key)
- `product_id`: uuid (foreign key to `products(id)`, cascade)
- `user_id`: uuid (foreign key to `users(id)`, cascade)
- `rating`: integer (1-5)
- `title`: varchar
- `body`: text
- `is_published`: boolean (default true)
- `created_at`: timestamp
- `updated_at`: timestamp

## Marketing Modules

### 9. Collections (`collections`)
- `id`: uuid (primary key)
- `title`: varchar (not null)
- `subtitle`: varchar
- `type`: enum ('gender', 'tag', 'brand', 'custom')
- `filter_value`: varchar
- `image_url`: text
- `display_order`: integer
- `is_active`: boolean (default true)
- `created_at`: timestamp

### 10. Coupons (`coupons`)
- `id`: uuid (primary key)
- `code`: varchar (unique, not null)
- `type`: enum ('percent', 'fixed', 'free_delivery') (not null)
- `value`: integer (not null)
- `min_order`: integer (default 0)
- `max_uses`: integer (nullable)
- `uses_count`: integer (default 0)
- `is_active`: boolean (default true)
- `expires_at`: timestamp (nullable)
- `created_at`: timestamp

### 11. Flash Sales (`flash_sales`)
- `id`: uuid (primary key)
- `slug`: varchar (unique, not null)
- `title`: varchar (not null)
- `starts_at`: timestamp (not null)
- `ends_at`: timestamp (not null)
- `is_active`: boolean (default true)
- `created_at`: timestamp

### 12. Flash Sale Items (`flash_sale_items`)
- `id`: uuid (primary key)
- `flash_sale_id`: uuid (foreign key to `flash_sales(id)`, cascade)
- `variant_id`: uuid (foreign key to `product_variants(id)`, cascade)
- `sale_price`: integer (not null)
- `stock_limit`: integer (nullable)

### 13. Combos (`combos`)
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `slug`: varchar (unique, not null)
- `combo_price`: integer (not null)
- `image_url`: text
- `tag`: varchar
- `is_active`: boolean (default true)
- `created_at`: timestamp

### 14. Combo Items (`combo_items`)
- `id`: uuid (primary key)
- `combo_id`: uuid (foreign key to `combos(id)`, cascade)
- `variant_id`: uuid (foreign key to `product_variants(id)`, cascade)
- `quantity`: integer (default 1)

### 15. Slides (`slides`)
- `id`: uuid (primary key)
- `image_url`: text (not null)
- `headline`: varchar
- `subheadline`: varchar
- `link_url`: varchar
- `display_order`: integer
- `is_active`: boolean (default true)

### 16. Announcement Items (`announcement_items`)
- `id`: uuid (primary key)
- `type`: enum ('info', 'offer', 'promo', 'event', 'warning', 'notice')
- `content`: text (not null)
- `link_url`: varchar
- `emoji`: varchar
- `is_active`: boolean (default true)
- `sort_order`: integer

## Config Modules

### 17. Site Settings (`site_settings`)
Singleton table (enforced id = 1).
- `id`: integer (primary key, default 1)
- `site_title`: varchar
- `font_body`: varchar
- `font_heading`: varchar
- `footer_address`: text
- `footer_phone`: varchar
- `footer_email`: varchar
- `whatsapp_number`: varchar
- `whatsapp_message`: varchar
- `variant_display_style`: enum ('pills', 'cards') default 'pills'
- `search_display_style`: enum ('dropdown', 'overlay') default 'dropdown'
- `is_maintenance`: boolean (default false)
- `social_links`: jsonb `{ facebook, instagram, youtube, tiktok }`
- `bkash_number`: varchar
- `bkash_qr_url`: varchar
- `payment_credentials`: jsonb
- `courier_credentials`: jsonb

### 18. Payment Gateways (`payment_gateways`)
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `slug`: varchar (unique, not null)
- `is_active`: boolean (default true)

### 19. Couriers (`couriers`)
- `id`: uuid (primary key)
- `name`: varchar (not null)
- `slug`: varchar (unique, not null)
- `is_active`: boolean (default true)
