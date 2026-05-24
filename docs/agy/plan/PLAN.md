# AdminKit — Implementation Plan

> A plug-and-play, production-ready admin panel template for Next.js 16 projects.
> Clone → Configure → Deploy. Full e-commerce admin in minutes.

---

## 1. Project Overview

### Vision
AdminKit is a **standalone Next.js 16 template repository** that provides a complete, feature-rich admin dashboard. It is designed to be cloned and integrated into any new or existing Next.js project with minimal configuration. Think of it as "Shopify Admin, but you own the code."

### Key Principles
- **Plug-and-Play**: Clone the repo, set env vars, run `pnpm db:setup`, and you have a working admin panel.
- **Portable**: Uses Drizzle ORM (works with any Postgres), Auth.js v5 (framework-native auth), and S3-compatible storage via UploadThing.
- **Configurable**: A type-safe `admin.config.ts` controls enabled modules, branding, currency, locale, and feature flags.
- **Production-Ready**: Error boundaries, loading states, RBAC, Zod validation, and accessible UI out of the box.
- **Modern Stack**: Next.js 16, React 19, Tailwind CSS v4, shadcn/ui, Drizzle ORM, TanStack Table, Recharts, Sonner, Zustand.

### Origin
Derived from the battle-tested admin panel of **Perfume Vault** (a premium e-commerce store), refactored and generalized to be domain-agnostic.

---

## 2. Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Framework** | Next.js 16 (App Router) | Latest stable, RSC, Server Actions, Turbopack |
| **Language** | TypeScript (strict, no `any`) | Type safety across the entire stack |
| **Styling** | Tailwind CSS v4 + shadcn/ui | Modern utility-first CSS with composable components |
| **Database** | Drizzle ORM + Any Postgres | Portable across Supabase, Neon, Railway, self-hosted |
| **Auth** | Auth.js v5 (NextAuth) | Framework-native, credentials + OAuth, session management |
| **State** | Zustand | Minimal client state (sidebar, preferences) |
| **Forms** | React Hook Form + Zod | Performant forms with schema validation |
| **Tables** | TanStack Table v8 | Headless, typed, sorting/filtering/pagination |
| **Charts** | Recharts | React-native, composable charting |
| **Toasts** | Sonner | Beautiful toast notifications by shadcn creator |
| **File Upload** | UploadThing | Modern file upload service built for Next.js |
| **PDF** | @react-pdf/renderer | React components that render to PDF |
| **Email** | Resend + React Email | Modern transactional email with React templates |
| **Icons** | Lucide React | Consistent, tree-shakeable icon set |
| **Animations** | Framer Motion | Declarative animations for React |
| **Package Manager** | pnpm | Fast, disk-efficient |

---

## 3. Project Structure

```
admin-kit/
├── app/
│   ├── (admin)/                    # Admin route group (configurable base path)
│   │   ├── layout.tsx              # Admin shell: sidebar + topbar + providers
│   │   ├── dashboard/
│   │   │   └── page.tsx            # KPI tiles, charts, recent orders
│   │   ├── products/
│   │   │   ├── page.tsx            # Product list (TanStack Table)
│   │   │   ├── new/page.tsx        # Create product form
│   │   │   └── [slug]/page.tsx     # Edit product form
│   │   ├── orders/
│   │   │   ├── page.tsx            # Order list with filters
│   │   │   └── [id]/page.tsx       # Order detail + invoice
│   │   ├── users/
│   │   │   ├── page.tsx            # User list
│   │   │   ├── new/page.tsx        # Create user
│   │   │   └── [id]/page.tsx       # User detail + order history
│   │   ├── brands/
│   │   │   ├── page.tsx            # Brand list
│   │   │   ├── new/page.tsx        # Create brand
│   │   │   └── [id]/page.tsx       # Edit brand
│   │   ├── collections/
│   │   │   └── page.tsx            # Collection manager
│   │   ├── coupons/
│   │   │   └── page.tsx            # Coupon manager
│   │   ├── flash-sales/
│   │   │   ├── page.tsx            # Flash sale list
│   │   │   └── [slug]/page.tsx     # Flash sale detail
│   │   ├── combos/
│   │   │   ├── page.tsx            # Combo list
│   │   │   ├── new/page.tsx        # Create combo
│   │   │   └── [id]/page.tsx       # Edit combo
│   │   ├── slides/
│   │   │   └── page.tsx            # Slide manager
│   │   ├── announcements/
│   │   │   └── page.tsx            # Announcement manager
│   │   ├── reviews/
│   │   │   └── page.tsx            # Review moderation
│   │   ├── payment-gateways/
│   │   │   └── page.tsx            # Payment config
│   │   ├── couriers/
│   │   │   └── page.tsx            # Courier config
│   │   └── settings/
│   │       └── page.tsx            # Site settings
│   ├── (auth)/
│   │   └── login/
│   │       └── [secret]/page.tsx   # Secret-URL admin login
│   ├── api/
│   │   ├── auth/[...nextauth]/     # Auth.js route handler
│   │   └── uploadthing/            # UploadThing route handler
│   ├── layout.tsx                  # Root layout (providers, fonts)
│   └── globals.css                 # CSS variables, Tailwind base
│
├── components/
│   ├── admin/                      # Admin-specific components
│   │   ├── layout/
│   │   │   ├── sidebar.tsx         # Collapsible sidebar with nav groups
│   │   │   ├── topbar.tsx          # Breadcrumbs, search, user menu, theme toggle
│   │   │   ├── mobile-nav.tsx      # Mobile hamburger + drawer
│   │   │   └── page-header.tsx     # Page title + description + actions
│   │   ├── dashboard/
│   │   │   ├── stat-card.tsx       # KPI stat card with trend indicator
│   │   │   ├── revenue-chart.tsx   # 30-day revenue line chart
│   │   │   ├── orders-status-chart.tsx   # Donut chart
│   │   │   ├── monthly-revenue-chart.tsx # 12-month bar chart
│   │   │   ├── payment-method-chart.tsx  # Pie chart
│   │   │   ├── orders-by-day-chart.tsx   # Day-of-week bar chart
│   │   │   ├── top-products-chart.tsx    # Horizontal bar chart
│   │   │   └── recent-orders-table.tsx   # Mini order table
│   │   ├── products/
│   │   │   ├── product-table.tsx         # TanStack Table for products
│   │   │   ├── product-form.tsx          # Create/edit product form
│   │   │   ├── variant-manager.tsx       # Add/edit/delete variants
│   │   │   ├── image-upload.tsx          # Product image gallery
│   │   │   └── flag-toggles.tsx          # Featured/trending/hot toggles
│   │   ├── orders/
│   │   │   ├── order-table.tsx           # TanStack Table for orders
│   │   │   ├── order-detail.tsx          # Full order view
│   │   │   ├── status-flow.tsx           # Status update buttons
│   │   │   ├── invoice-renderer.tsx      # @react-pdf invoice
│   │   │   ├── tracking-form.tsx         # Tracking update form
│   │   │   └── payment-status.tsx        # Payment status dropdown
│   │   ├── users/
│   │   │   ├── user-table.tsx
│   │   │   └── user-detail.tsx
│   │   ├── brands/
│   │   │   ├── brand-table.tsx
│   │   │   └── brand-form.tsx
│   │   ├── collections/
│   │   │   └── collection-manager.tsx    # Drag-reorder collection grid
│   │   ├── coupons/
│   │   │   ├── coupon-table.tsx
│   │   │   └── coupon-form.tsx
│   │   ├── flash-sales/
│   │   │   ├── flash-sale-table.tsx
│   │   │   ├── flash-sale-form.tsx
│   │   │   └── flash-sale-item-adder.tsx
│   │   ├── combos/
│   │   │   ├── combo-table.tsx
│   │   │   ├── combo-form.tsx
│   │   │   └── variant-selector.tsx
│   │   ├── slides/
│   │   │   └── slide-manager.tsx
│   │   ├── announcements/
│   │   │   └── announcement-manager.tsx
│   │   ├── reviews/
│   │   │   └── review-table.tsx
│   │   ├── settings/
│   │   │   └── settings-form.tsx
│   │   └── shared/
│   │       ├── status-pill.tsx
│   │       ├── toggle-switch.tsx
│   │       ├── confirmation-modal.tsx
│   │       ├── empty-state.tsx
│   │       ├── data-table.tsx            # Generic TanStack Table wrapper
│   │       ├── search-input.tsx
│   │       └── image-upload-field.tsx
│   ├── providers/
│   │   ├── theme-provider.tsx
│   │   ├── query-provider.tsx
│   │   └── toast-provider.tsx
│   └── ui/                         # shadcn/ui primitives
│       ├── button.tsx
│       ├── input.tsx
│       ├── dialog.tsx
│       ├── dropdown-menu.tsx
│       ├── select.tsx
│       ├── tabs.tsx
│       ├── badge.tsx
│       ├── separator.tsx
│       ├── sheet.tsx
│       ├── label.tsx
│       ├── skeleton.tsx
│       ├── tooltip.tsx
│       ├── card.tsx
│       ├── table.tsx
│       ├── avatar.tsx
│       ├── calendar.tsx
│       ├── popover.tsx
│       ├── command.tsx              # For command palette (⌘K)
│       └── breadcrumb.tsx
│
├── lib/
│   ├── db/
│   │   ├── index.ts                # Drizzle client initialization
│   │   ├── schema/
│   │   │   ├── products.ts         # Products + variants schema
│   │   │   ├── orders.ts           # Orders + order items schema
│   │   │   ├── users.ts            # Users + admin_users schema
│   │   │   ├── brands.ts           # Brands schema
│   │   │   ├── collections.ts      # Collections schema
│   │   │   ├── coupons.ts          # Coupons schema
│   │   │   ├── flash-sales.ts      # Flash sales + items schema
│   │   │   ├── combos.ts           # Combos + items schema
│   │   │   ├── slides.ts           # Slides schema
│   │   │   ├── announcements.ts    # Announcement items schema
│   │   │   ├── reviews.ts          # Reviews schema
│   │   │   ├── settings.ts         # Site settings schema
│   │   │   ├── payment-gateways.ts # Payment gateways schema
│   │   │   ├── couriers.ts         # Couriers schema
│   │   │   └── index.ts            # Re-exports all schemas
│   │   ├── migrations/             # Drizzle migration files
│   │   └── seed.ts                 # Seed script with sample data
│   ├── auth/
│   │   ├── config.ts               # Auth.js configuration
│   │   ├── guards.ts               # requireAdmin(), requireAdminAPI()
│   │   └── credentials.ts          # Credentials provider setup
│   ├── actions/                    # Server Actions (organized by module)
│   │   ├── products.ts
│   │   ├── orders.ts
│   │   ├── users.ts
│   │   ├── brands.ts
│   │   ├── collections.ts
│   │   ├── coupons.ts
│   │   ├── flash-sales.ts
│   │   ├── combos.ts
│   │   ├── slides.ts
│   │   ├── announcements.ts
│   │   ├── reviews.ts
│   │   ├── settings.ts
│   │   ├── toggles.ts
│   │   └── auth.ts
│   ├── queries/                    # Data fetching functions
│   │   ├── dashboard.ts
│   │   ├── products.ts
│   │   ├── orders.ts
│   │   ├── users.ts
│   │   ├── brands.ts
│   │   ├── collections.ts
│   │   ├── coupons.ts
│   │   ├── flash-sales.ts
│   │   ├── combos.ts
│   │   ├── slides.ts
│   │   ├── announcements.ts
│   │   ├── reviews.ts
│   │   └── settings.ts
│   ├── schemas/                    # Zod validation schemas
│   │   ├── product.ts
│   │   ├── order.ts
│   │   ├── coupon.ts
│   │   ├── flash-sale.ts
│   │   ├── combo.ts
│   │   ├── collection.ts
│   │   ├── slide.ts
│   │   ├── announcement.ts
│   │   ├── brand.ts
│   │   ├── settings.ts
│   │   └── auth.ts
│   ├── stores/                     # Zustand stores
│   │   ├── sidebar.ts              # Sidebar collapse state
│   │   └── preferences.ts          # User preferences
│   ├── email/
│   │   ├── client.ts               # Resend client
│   │   └── templates/
│   │       ├── order-confirmation.tsx
│   │       ├── order-shipped.tsx
│   │       └── order-status-update.tsx
│   ├── upload/
│   │   └── config.ts               # UploadThing configuration
│   ├── pdf/
│   │   └── invoice-template.tsx    # @react-pdf invoice component
│   └── utils/
│       ├── format-currency.ts      # Currency formatting (uses admin.config.ts)
│       ├── format-date.ts          # Date formatting
│       ├── cn.ts                   # clsx + tailwind-merge
│       └── slug.ts                 # Slug generation
│
├── drizzle/                        # Drizzle config directory
│   └── drizzle.config.ts
│
├── emails/                         # React Email templates (for preview)
│
├── admin.config.ts                 # ★ Central configuration file
├── auth.config.ts                  # Auth.js edge config
├── middleware.ts                   # Auth + admin route protection
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── package.json
├── pnpm-lock.yaml
├── .env.example
├── README.md
└── DESIGN.md
```

---

## 4. Configuration System (`admin.config.ts`)

The central configuration file that controls the entire admin panel:

```typescript
import { defineAdminConfig } from "@/lib/config";

export default defineAdminConfig({
  // Project info
  name: "AdminKit",
  description: "Admin Dashboard",

  // Route configuration
  basePath: "/admin",          // Configurable: /admin, /dashboard, /cms
  loginSlug: "secret-login",   // Secret URL segment for login page

  // Branding
  branding: {
    logo: "/logo.svg",
    favicon: "/favicon.ico",
    accentColor: "blue",       // Theme accent color
  },

  // Locale & Currency
  locale: {
    currency: "BDT",
    currencySymbol: "৳",
    locale: "en-BD",
    timezone: "Asia/Dhaka",
  },

  // Feature modules (toggle on/off)
  modules: {
    dashboard: true,
    products: true,
    orders: true,
    users: true,
    brands: true,
    collections: true,
    coupons: true,
    flashSales: true,
    combos: true,
    slides: true,
    announcements: true,
    reviews: true,
    paymentGateways: true,
    couriers: true,
    settings: true,
  },

  // Product configuration
  product: {
    variants: {
      label: "Size",             // "Size", "Color", "Weight", etc.
      presets: ["3ml", "5ml", "10ml", "15ml", "30ml"],
    },
    tags: ["new-arrival", "trending", "hot", "featured", "combo", "flash-sale"],
    genders: ["Men", "Women", "Unisex"],
  },

  // Order statuses
  order: {
    statuses: ["pending", "confirmed", "packed", "shipped", "out-for-delivery", "delivered", "cancelled", "returned"],
    paymentStatuses: ["pending", "paid", "failed", "refunded"],
    paymentMethods: ["cod", "bkash", "nagad", "card"],
  },

  // RBAC
  roles: {
    superadmin: { label: "Super Admin", permissions: "*" },
    admin: { label: "Admin", permissions: ["read", "write", "delete"] },
    staff: { label: "Staff", permissions: ["read"] },
  },
});
```

---

## 5. Database Schema (Drizzle ORM)

### Core Tables

| Table | Description | Key Fields |
|-------|-------------|------------|
| `admin_users` | Admin accounts | id, email, password_hash, role, name, avatar_url |
| `products` | Product catalog | id, name, slug, description, brand_id, gender, primary_image, gallery_images, tags[], notes, is_featured, is_new_arrival, is_trending, is_hot, deleted_at |
| `product_variants` | Product size/price | id, product_id, label, price, discount_price, stock_quantity, sku |
| `brands` | Brand directory | id, name, slug, logo_url, is_featured |
| `orders` | Customer orders | id, order_number, user_id, customer_info (jsonb), shipping_address (jsonb), items_subtotal, delivery_charge, discount_total, total, payment_method, payment_status, order_status, tracking_number, tracking_provider |
| `order_items` | Order line items | id, order_id, variant_id, combo_id, product_name, variant_label, unit_price, quantity, line_total |
| `users` | Customers | id, name, email, phone, avatar_url |
| `reviews` | Product reviews | id, product_id, user_id, rating, title, body, is_published |
| `collections` | Curated groups | id, title, subtitle, type, filter_value, image_url, display_order, is_active |
| `coupons` | Discount codes | id, code, type (percent/fixed/free_delivery), value, min_order, max_uses, uses_count, is_active, expires_at |
| `flash_sales` | Timed sales | id, slug, title, starts_at, ends_at, is_active |
| `flash_sale_items` | Sale items | flash_sale_id, variant_id, sale_price, stock_limit |
| `combos` | Product bundles | id, name, slug, combo_price, image_url, is_active, tag |
| `combo_items` | Bundle contents | combo_id, variant_id, quantity |
| `slides` | Hero carousel | id, image_url, headline, subheadline, link_url, display_order, is_active |
| `announcement_items` | Banner items | id, type, content, link_url, emoji, is_active, sort_order |
| `site_settings` | Global config | Singleton row with 20+ configurable fields |
| `payment_gateways` | Payment methods | id, name, slug, is_active |
| `couriers` | Delivery services | id, name, slug, is_active |

---

## 6. Feature Modules — Detailed Specifications

### 6.1 Dashboard
- **6 KPI stat cards**: Total Orders, Revenue, Products, Customers, Pending Orders, Avg Order Value
- **6 analytics charts** (Recharts):
  - Revenue by Day (30-day line/area chart)
  - Orders by Status (donut chart)
  - Monthly Revenue (12-month bar chart with order count overlay)
  - Payment Methods (pie chart)
  - Orders by Day of Week (bar chart)
  - Top Products by Revenue (horizontal bar chart)
- **Recent orders mini-table** with quick status actions
- **Low-stock alerts** (products below configurable threshold)
- All data fetched via a single `getDashboardAnalytics()` Drizzle query

### 6.2 Products CRUD
- **Product list**: TanStack Table with sorting, filtering (brand, gender, status, tags), column visibility, pagination
- **Active/Deleted tabs**: Soft-delete pattern with restore capability
- **Product form**: Name, slug (auto-generated), brand (select/create), gender, description, notes (top/middle/base), tags (multi-select), primary image, gallery images
- **Variant manager**: Add/edit/delete variants with label, price, discount_price, stock, SKU
- **Flag toggles**: Featured, New Arrival, Trending, Hot — instant toggle via server action
- **Bulk actions**: Select multiple → bulk delete, bulk update status

### 6.3 Orders Management
- **Order list**: TanStack Table with status filter pills, date range, search (order number, customer name, phone)
- **Order detail page**: Customer info, shipping address, order items with images, subtotals
- **Status flow**: Visual status pipeline with clickable status buttons (pending → confirmed → packed → shipped → out-for-delivery → delivered)
- **Payment status**: Dropdown to update (pending/paid/failed/refunded)
- **Tracking**: Form to add/update tracking number + provider
- **Invoice**: Generate + download PDF via @react-pdf/renderer
- **Email notifications**: Auto-send status update emails via Resend

### 6.4 Users Management
- **User list**: TanStack Table with avatar, name, email, phone, order count, total spent
- **User detail**: Profile info + full order history table
- **Create user**: Form with name, email, phone, password

### 6.5 Brands
- **Brand list**: Grid/table with logo, name, product count, featured toggle
- **Brand form**: Name, slug, logo upload, is_featured toggle
- **Delete protection**: Cannot delete brand if products are associated

### 6.6 Collections
- **Collection grid**: Cards with drag-to-reorder, type badges (gender/tag/brand/custom)
- **Create/edit modal**: Title, subtitle, type selector, filter value, image, display order
- **Active toggle**: Instantly show/hide collections

### 6.7 Coupons
- **Coupon table**: Code, type badge, value, min order, max uses, usage count, expiry, status
- **Coupon form**: Code, type (percentage/fixed/free_delivery), value, min order value, max uses, expiry date, active toggle
- **Timezone handling**: Auto-convert local dates to UTC for storage

### 6.8 Flash Sales
- **Flash sale list**: Cards with title, date range, item count, active toggle
- **Flash sale form**: Title, slug, start/end datetime, is_active
- **Item adder**: Search product variants → set sale price + stock limit → add to flash sale

### 6.9 Combos/Bundles
- **Combo list**: Cards with name, image, item count, price, active toggle
- **Combo form**: Name, slug, combo price, image, tag
- **Variant selector**: Search and select product variants + quantities

### 6.10 Slides (Hero Carousel)
- **Slide manager**: Cards with image preview, drag-to-reorder
- **Slide form**: Image upload, headline, subheadline, link URL, display order, is_active
- **Active toggle**: Show/hide slides

### 6.11 Announcements
- **Announcement list**: Items with type color coding (info/offer/promo/event/warning/notice)
- **Item form**: Type, content, link URL, emoji, sort order, is_active
- **Speed slider**: Control marquee scroll speed
- **Live preview**: See announcement bar as it would appear on the storefront

### 6.12 Reviews Moderation
- **Review table**: Product, customer, rating (stars), title, body, published status, date
- **Actions**: Publish/unpublish toggle, delete with confirmation

### 6.13 Payment Gateways
- **Toggle list**: Enable/disable payment methods (COD, bKash, Nagad, card, etc.)
- **Configuration**: Gateway-specific credentials in settings

### 6.14 Couriers
- **Toggle list**: Enable/disable courier services (Pathao, Steadfast, RedX, etc.)
- **Configuration**: Courier API credentials in settings

### 6.15 Site Settings
- **Typography**: Font picker for body + heading fonts
- **General**: Site title, logo, WhatsApp number
- **Display**: Variant display style, search display style
- **Social links**: Facebook, Instagram, YouTube, TikTok
- **Content**: Terms & Conditions, Privacy Policy (rich text)
- **Payments**: bKash number, QR code, API credentials
- **Couriers**: API credentials per courier
- **Maintenance mode**: Toggle the entire site on/off

---

## 7. Auth & RBAC

### Auth Flow
1. Admin navigates to `/{basePath}/login/{ADMIN_LOGIN_SLUG}` (secret URL)
2. Submits email + password via `AdminLoginForm`
3. Auth.js Credentials provider validates against `admin_users` table
4. Session created with role embedded in JWT
5. Redirect to `/{basePath}/dashboard`

### Route Protection
- **Middleware** (`middleware.ts`): Checks Auth.js session on all `/{basePath}/*` routes; redirects to login if unauthenticated
- **RSC Guard** (`requireAdmin()`): Server-side check in page components; validates role permissions
- **API Guard** (`requireAdminAPI()`): Same check for API routes, returns 401/403

### Roles
| Role | Permissions |
|------|------------|
| `superadmin` | Full access to everything including settings, user management, and delete operations |
| `admin` | CRUD on all content modules, but cannot manage admin users or system settings |
| `staff` | Read-only access to dashboard, orders, and products; can update order status |

---

## 8. Data Patterns

### Server Component Page → Server Action Flow
```
RSC Page (data fetching)
  ↓ calls query function
  ↓ Drizzle query → Postgres
  ↓ passes data as props to Client Component
Client Component (interactive)
  ↓ user interaction triggers Server Action
  ↓ Server Action validates with Zod
  ↓ Drizzle mutation → Postgres
  ↓ revalidatePath() → page re-renders with fresh data
```

### Image Upload Flow
```
Client → UploadThing widget → UploadThing service
  ↓ returns URL
  ↓ stored in product.primary_image / gallery_images
```

### Email Notification Flow
```
Order status change (Server Action)
  ↓ Drizzle update
  ↓ Resend.send() with React Email template
  ↓ revalidatePath()
```

---

## 9. Build Phases

### Phase 1: Foundation (Week 1)
- [ ] Initialize Next.js 16 project with pnpm
- [ ] Configure Tailwind CSS v4 + shadcn/ui
- [ ] Set up Drizzle ORM + connection
- [ ] Create `admin.config.ts` type-safe configuration
- [ ] Set up Auth.js v5 with Credentials provider
- [ ] Create admin layout (collapsible sidebar + topbar)
- [ ] Implement middleware route protection
- [ ] Build shared UI components (data-table, status-pill, toggle, etc.)
- [ ] Set up Sonner toast provider
- [ ] Create design tokens (CSS variables for light/dark)

### Phase 2: Core Modules (Week 2-3)
- [ ] Dashboard (stat cards + 6 charts + recent orders)
- [ ] Products (CRUD + variants + images + flags)
- [ ] Orders (list + detail + status flow + tracking)
- [ ] Users (list + detail + order history)
- [ ] Brands (CRUD + logo upload)

### Phase 3: Marketing Modules (Week 4)
- [ ] Collections (CRUD + drag reorder)
- [ ] Coupons (CRUD + rules)
- [ ] Flash Sales (CRUD + item management)
- [ ] Combos (CRUD + variant selector)
- [ ] Slides (manager + drag reorder)
- [ ] Announcements (manager + live preview)

### Phase 4: Config & Polish (Week 5)
- [ ] Reviews moderation
- [ ] Payment gateways toggle
- [ ] Couriers toggle
- [ ] Site settings (full form)
- [ ] Invoice PDF generation (@react-pdf)
- [ ] Email templates + Resend integration
- [ ] Seed script with sample data
- [ ] README + documentation

### Phase 5: Verification (Week 6)
- [ ] End-to-end testing of all modules
- [ ] Responsive testing (mobile sidebar, tables)
- [ ] Dark/light theme testing
- [ ] TypeScript strict mode compliance
- [ ] Performance optimization
- [ ] Accessibility audit (ARIA, keyboard nav)

---

## 10. Integration Guide (for future projects)

When cloning AdminKit into a new project:

```bash
# 1. Clone the template
git clone https://github.com/your-org/admin-kit.git my-project
cd my-project

# 2. Install dependencies
pnpm install

# 3. Copy env template and fill in values
cp .env.example .env.local

# 4. Customize admin.config.ts
# - Set project name, currency, enabled modules, etc.

# 5. Run database migrations + seed
pnpm db:setup

# 6. Start development
pnpm dev
```

### Env Variables Required
```env
# Database
DATABASE_URL="postgresql://..."

# Auth
AUTH_SECRET="..."
ADMIN_LOGIN_SLUG="your-secret-login-slug"

# UploadThing
UPLOADTHING_TOKEN="..."

# Email (Resend)
RESEND_API_KEY="..."
EMAIL_FROM="admin@yourdomain.com"
```

---

## 11. File Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Components | kebab-case | `product-table.tsx`, `stat-card.tsx` |
| Pages | `page.tsx` inside route folders | `app/(admin)/products/page.tsx` |
| Server Actions | kebab-case by module | `lib/actions/products.ts` |
| Queries | kebab-case by module | `lib/queries/products.ts` |
| Schemas | kebab-case by module | `lib/schemas/product.ts` |
| Drizzle schemas | kebab-case by table | `lib/db/schema/products.ts` |
| Utilities | kebab-case | `lib/utils/format-currency.ts` |

---

## 12. Quality Standards

- **TypeScript**: Strict mode, no `any`, no `@ts-ignore`
- **Functional**: No classes, pure functions + closures + composition
- **Validation**: Zod schemas for all form inputs and server actions
- **Error Handling**: Error boundaries on every page, try-catch in all server actions
- **Loading States**: Skeleton loaders for all async data
- **Accessibility**: ARIA roles, keyboard navigation, focus management
- **Performance**: RSC by default, client components only when needed, dynamic imports for heavy components (charts, PDF)
- **Motion**: `prefers-reduced-motion` respected everywhere
