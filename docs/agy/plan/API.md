# AdminKit — Server Actions & Queries API Contract

> This document defines the exact API contract for all Next.js Server Actions and Data Fetching queries required to build the AdminKit.

## Core Patterns

- **Server Actions** are used exclusively for mutations (create, update, delete, toggle).
- **Queries** are used exclusively for data fetching in Server Components.
- **Cache Invalidation** is done via `revalidatePath()` inside Server Actions after successful DB mutations.
- **Error Handling**: All actions return `{ success: boolean, data?: any, error?: string }` (or similar ZodAction pattern).

---

## 1. Product Actions & Queries (`lib/actions/products.ts`, `lib/queries/products.ts`)

### Mutations
- `createProduct(data: z.infer<typeof ProductSchema>)`: Creates product, inserts variants, handles image uploads.
- `updateProduct(id: string, data: z.infer<typeof ProductSchema>)`: Updates product details.
- `softDeleteProduct(id: string)`: Sets `deleted_at = NOW()`.
- `restoreProduct(id: string)`: Sets `deleted_at = null`.
- `permanentDeleteProduct(id: string)`: Hard deletes from DB (cascades variants).
- `toggleProductFlag(id: string, flag: 'is_featured' | 'is_new_arrival' | 'is_trending' | 'is_hot', state: boolean)`: Instantly toggles a boolean flag.
- `addProductVariant(productId: string, data: VariantData)`
- `updateProductVariant(variantId: string, data: VariantData)`
- `deleteProductVariant(variantId: string)`

### Queries
- `fetchAllProducts(includeDeleted: boolean = false)`: Returns products array.
- `fetchProductById(id: string)`: Returns product with its associated variants.

---

## 2. Order Actions & Queries (`lib/actions/orders.ts`, `lib/queries/orders.ts`)

### Mutations
- `updateOrderStatus(orderId: string, status: OrderStatus)`: Updates order status. If changing to 'shipped', triggers shipping email notification.
- `updatePaymentStatus(orderId: string, status: PaymentStatus)`: Updates payment state.
- `updateTracking(orderId: string, provider: string, trackingNumber: string)`: Attaches tracking info.

### Queries
- `fetchAllOrders(filters: { status?: string, search?: string })`: Paginated list of orders.
- `fetchOrderById(id: string)`: Full order details including `order_items`.

---

## 3. Brand Actions (`lib/actions/brands.ts`)

### Mutations
- `createBrand(data: { name: string, logo_url?: string, is_featured?: boolean })`
- `updateBrand(id: string, data: BrandUpdateData)`
- `deleteBrand(id: string)`: Fails if products are still attached.
- `toggleBrandFeatured(id: string, state: boolean)`

### Queries
- `fetchAllBrands()`: Returns brand list with associated product counts.

---

## 4. Dashboard Queries (`lib/queries/dashboard.ts`)

These fetch analytics data for the dashboard UI.
- `fetchDashboardStats()`: Returns `{ totalOrders, totalRevenue, totalProducts, totalUsers, pendingOrders, avgOrderValue }`.
- `fetchRecentOrders(limit: number)`: Returns last N orders.
- `getRevenueByDay(days: number)`: Returns array of `{ date, revenue }` for line chart.
- `getOrdersByStatus()`: Returns array of `{ status, count }` for donut chart.
- `getTopProductsByRevenue(limit: number)`: Returns array of `{ productName, revenue }` for bar chart.
- `getRevenueByMonth()`: Returns 12-month array of `{ month, revenue, orders }`.
- `getOrdersByPaymentMethod()`: Returns array of `{ method, count }`.

---

## 5. Collections Actions (`lib/actions/collections.ts`)

### Mutations
- `createCollection(data: CollectionData)`
- `updateCollection(id: string, data: CollectionData)`
- `deleteCollection(id: string)`
- `toggleCollection(id: string, isActive: boolean)`
- `reorderCollections(items: { id: string, sort_order: number }[])`: Updates display order in bulk.

---

## 6. Coupons Actions (`lib/actions/coupons.ts`)

### Mutations
- `createCoupon(data: CouponData)`: Creates coupon, parsing dates to UTC.
- `updateCoupon(id: string, data: CouponData)`
- `deleteCoupon(id: string)`
- `toggleCoupon(id: string, isActive: boolean)`

---

## 7. Flash Sales Actions (`lib/actions/flash-sales.ts`)

### Mutations
- `createFlashSale(data: FlashSaleData)`
- `updateFlashSale(id: string, data: FlashSaleData)`
- `deleteFlashSale(id: string)`
- `toggleFlashSale(id: string, isActive: boolean)`
- `addFlashSaleItem(flashSaleId: string, variantId: string, salePrice: number, stockLimit?: number)`
- `removeFlashSaleItem(itemId: string)`

---

## 8. Combos Actions (`lib/actions/combos.ts`)

### Mutations
- `createCombo(data: ComboData)`
- `updateCombo(id: string, data: ComboData)`
- `deleteCombo(id: string)`
- `toggleCombo(id: string, isActive: boolean)`
- `addComboItem(comboId: string, variantId: string, quantity: number)`
- `removeComboItem(itemId: string)`

---

## 9. Slides & Announcements Actions

### Mutations
- `createSlide(data: SlideData)`
- `updateSlide(id: string, data: SlideData)`
- `deleteSlide(id: string)`
- `toggleSlideActive(id: string, isActive: boolean)`
- `reorderSlides(items: { id: string, display_order: number }[])`

- `createAnnouncementItem(data: AnnouncementData)`
- `updateAnnouncementItem(id: string, data: AnnouncementData)`
- `deleteAnnouncementItem(id: string)`
- `toggleAnnouncementItem(id: string, isActive: boolean)`

---

## 10. Site Settings & Config

### Mutations
- `updateSiteSettings(data: z.infer<typeof SiteSettingsSchema>)`: Updates the singleton config row.
- `toggleGateway(id: string, isActive: boolean)`: Enables/disables a payment gateway.
- `toggleCourier(id: string, isActive: boolean)`: Enables/disables a courier.

### Queries
- `fetchSiteSettings()`: Fetches the singleton settings row.
