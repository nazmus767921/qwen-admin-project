# AdminKit — Architecture Decisions Record

> All decisions made during the /grill-me brainstorming session on 2026-05-24.

---

## Decision Summary

| # | Decision Area | Choice | Rationale |
|---|--------------|--------|-----------|
| 1 | **Distribution** | Template Repository | Clone/copy, full customization freedom per project |
| 2 | **Database** | Drizzle ORM + Any Postgres | Portable across Supabase, Neon, Railway, self-hosted |
| 3 | **Auth** | Auth.js v5 (NextAuth) | Framework-native, credentials + OAuth, session management |
| 4 | **Styling** | Tailwind CSS v4 + shadcn/ui | Modern, customizable, industry standard |
| 5 | **Feature Scope** | All 15 modules | Full feature parity with Perfume Vault admin |
| 6 | **File Storage** | UploadThing | Modern file upload service built for Next.js |
| 7 | **Charts** | Recharts | React-native, composable, well-maintained |
| 8 | **State** | Zustand | Minimal, fast, works great with Next.js |
| 9 | **Data Fetching** | RSC + Server Actions | Native Next.js pattern, revalidatePath for cache |
| 10 | **Mounting** | `/admin` with configurable base path | Flexible: `/admin`, `/dashboard`, `/cms` via config |
| 11 | **RBAC** | Simple Role-Based (3 roles) | superadmin, admin, staff with hardcoded permissions |
| 12 | **Theme** | Light-first with dark toggle | Clean, bright default (Shopify/Linear style) |
| 13 | **Design Inspiration** | Shopify Admin (Polaris) | E-commerce focused, practical, feature-rich |
| 14 | **Navigation** | Collapsible sidebar + topbar | Icons-only when collapsed, breadcrumbs in topbar |
| 15 | **Tables** | TanStack Table v8 | Headless, typed, sorting/filtering/pagination |
| 16 | **Forms** | React Hook Form + Zod | Performant forms with schema validation |
| 17 | **Toasts** | Sonner | Beautiful, minimal API, by shadcn creator |
| 18 | **PDF** | @react-pdf/renderer | React components → PDF, no browser dependency |
| 19 | **Email** | Resend + React Email | Modern transactional email with React templates |
| 20 | **Config** | `admin.config.ts` + env vars | Type-safe config file for modules, branding, locale |
| 21 | **Currency** | Configurable via config | `formatCurrency()` utility reads from config |
| 22 | **Deployment** | Vercel | Zero-config, first-class Next.js hosting |
| 23 | **Next.js** | v16 (latest) | Bleeding edge, same as Perfume Vault |
| 24 | **Project Name** | AdminKit | Simple, clean, toolkit approach |
| 25 | **DB Setup** | Full migrations + seed script | `pnpm db:setup` for instant working state |
| 26 | **Build Location** | New separate repository | `c:\Users\Admin\Desktop\Projects\admin-kit` |

---

## Tech Stack Summary

```
Next.js 16 + React 19 + TypeScript (strict)
Tailwind CSS v4 + shadcn/ui
Drizzle ORM + PostgreSQL
Auth.js v5 (Credentials provider)
TanStack Table v8
React Hook Form + Zod
Recharts
Zustand
Sonner
UploadThing
Resend + React Email
@react-pdf/renderer
Framer Motion
Lucide React
pnpm
```

---

## Modules (All 15 Enabled)

1. ✅ Dashboard (KPIs + 6 charts + recent orders)
2. ✅ Products (CRUD + variants + images + flags)
3. ✅ Orders (list + detail + status flow + tracking + invoice)
4. ✅ Users (list + detail + order history)
5. ✅ Brands (CRUD + logo upload)
6. ✅ Collections (CRUD + drag reorder)
7. ✅ Coupons (CRUD + rules)
8. ✅ Flash Sales (CRUD + item management)
9. ✅ Combos (CRUD + variant selector)
10. ✅ Slides (manager + drag reorder)
11. ✅ Announcements (manager + live preview)
12. ✅ Reviews (moderation)
13. ✅ Payment Gateways (toggle)
14. ✅ Couriers (toggle)
15. ✅ Site Settings (full form)
