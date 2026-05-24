# AdminKit — Design System Document

> Visual identity, component patterns, layout system, and UI/UX specifications for the AdminKit admin panel.
> Inspired by **Shopify Admin (Polaris)** with a modern **light-first + dark toggle** approach.

---

## 1. Design Philosophy

### Core Principles
1. **Clarity over decoration** — Every pixel serves a purpose. Information density without visual clutter.
2. **Consistency** — Unified spacing, typography, and color usage across all modules.
3. **Feedback** — Every user action receives immediate visual feedback (hover, focus, loading, success, error).
4. **Accessibility** — WCAG 2.1 AA compliant. Keyboard navigable. Screen reader friendly.
5. **Responsive** — Mobile-first. Admin works on tablets and phones (though optimized for desktop).

### Design Inspirations
- **Shopify Admin (Polaris)** — Information hierarchy, practical component patterns, feature-rich e-commerce admin
- Influence from **Linear** (clean transitions), **Vercel** (data density), and **Stripe** (chart aesthetics)

---

## 2. Color System

### Design Tokens (CSS Variables)

We use a **semantic color system** with CSS custom properties for seamless light/dark theming.

#### Light Theme (Default)
```css
:root {
  /* Backgrounds */
  --color-bg:           #FAFAFA;         /* Page background — subtle warm gray */
  --color-surface:      #FFFFFF;         /* Card/panel surfaces */
  --color-surface-hover: #F7F7F7;        /* Surface hover state */
  --color-surface-active: #F0F0F0;       /* Surface active/pressed state */
  --color-surface-raised: #FFFFFF;       /* Elevated surfaces (modals, dropdowns) */
  --color-surface-sunken: #F3F3F3;       /* Sunken areas (code blocks, table headers) */

  /* Borders */
  --color-border:       #E4E4E7;         /* Default borders */
  --color-border-hover: #D4D4D8;         /* Border hover state */
  --color-border-focus: #3B82F6;         /* Focus ring color */

  /* Text */
  --color-ink:          #18181B;         /* Primary text */
  --color-ink-secondary: #71717A;        /* Secondary text */
  --color-ink-tertiary:  #A1A1AA;        /* Tertiary/muted text */
  --color-ink-disabled:  #D4D4D8;        /* Disabled text */
  --color-ink-inverse:   #FAFAFA;        /* Text on dark backgrounds */

  /* Brand / Accent (Blue — configurable) */
  --color-accent:       #3B82F6;         /* Primary accent */
  --color-accent-hover: #2563EB;         /* Accent hover */
  --color-accent-light: #EFF6FF;         /* Accent background tint */
  --color-accent-text:  #1D4ED8;         /* Accent text */

  /* Semantic */
  --color-success:      #22C55E;         /* Green — positive actions, published, active */
  --color-success-light: #F0FDF4;
  --color-success-text: #15803D;
  --color-warning:      #F59E0B;         /* Amber — caution, pending */
  --color-warning-light: #FFFBEB;
  --color-warning-text: #B45309;
  --color-danger:       #EF4444;         /* Red — destructive, errors, stock out */
  --color-danger-light: #FEF2F2;
  --color-danger-text:  #DC2626;
  --color-info:         #3B82F6;         /* Blue — informational */
  --color-info-light:   #EFF6FF;
  --color-info-text:    #1D4ED8;

  /* Shadows */
  --shadow-xs:   0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-sm:   0 1px 3px rgba(0, 0, 0, 0.08), 0 1px 2px rgba(0, 0, 0, 0.04);
  --shadow-md:   0 4px 6px rgba(0, 0, 0, 0.07), 0 2px 4px rgba(0, 0, 0, 0.04);
  --shadow-lg:   0 10px 15px rgba(0, 0, 0, 0.08), 0 4px 6px rgba(0, 0, 0, 0.04);
  --shadow-xl:   0 20px 25px rgba(0, 0, 0, 0.08), 0 8px 10px rgba(0, 0, 0, 0.04);
  --shadow-focus: 0 0 0 3px rgba(59, 130, 246, 0.3);

  /* Sidebar */
  --sidebar-bg:     #FFFFFF;
  --sidebar-border: #E4E4E7;
  --sidebar-item-hover: #F4F4F5;
  --sidebar-item-active-bg: #EFF6FF;
  --sidebar-item-active-text: #2563EB;
  --sidebar-icon:   #71717A;
  --sidebar-icon-active: #2563EB;
  --sidebar-width: 260px;
  --sidebar-width-collapsed: 68px;
}
```

#### Dark Theme
```css
.dark {
  /* Backgrounds */
  --color-bg:           #09090B;
  --color-surface:      #18181B;
  --color-surface-hover: #27272A;
  --color-surface-active: #3F3F46;
  --color-surface-raised: #27272A;
  --color-surface-sunken: #0F0F12;

  /* Borders */
  --color-border:       #27272A;
  --color-border-hover: #3F3F46;
  --color-border-focus: #60A5FA;

  /* Text */
  --color-ink:          #FAFAFA;
  --color-ink-secondary: #A1A1AA;
  --color-ink-tertiary:  #71717A;
  --color-ink-disabled:  #52525B;
  --color-ink-inverse:   #18181B;

  /* Brand / Accent */
  --color-accent:       #60A5FA;
  --color-accent-hover: #93C5FD;
  --color-accent-light: #172554;
  --color-accent-text:  #93C5FD;

  /* Semantic */
  --color-success:      #4ADE80;
  --color-success-light: #052E16;
  --color-success-text: #86EFAC;
  --color-warning:      #FBBF24;
  --color-warning-light: #451A03;
  --color-warning-text: #FDE68A;
  --color-danger:       #F87171;
  --color-danger-light: #450A0A;
  --color-danger-text:  #FCA5A5;
  --color-info:         #60A5FA;
  --color-info-light:   #172554;
  --color-info-text:    #93C5FD;

  /* Shadows (subtle glows in dark mode) */
  --shadow-xs:   0 1px 2px rgba(0, 0, 0, 0.3);
  --shadow-sm:   0 1px 3px rgba(0, 0, 0, 0.4), 0 1px 2px rgba(0, 0, 0, 0.3);
  --shadow-md:   0 4px 6px rgba(0, 0, 0, 0.4), 0 2px 4px rgba(0, 0, 0, 0.3);
  --shadow-lg:   0 10px 15px rgba(0, 0, 0, 0.4), 0 4px 6px rgba(0, 0, 0, 0.3);
  --shadow-xl:   0 20px 25px rgba(0, 0, 0, 0.4), 0 8px 10px rgba(0, 0, 0, 0.3);
  --shadow-focus: 0 0 0 3px rgba(96, 165, 250, 0.3);

  /* Sidebar */
  --sidebar-bg:     #18181B;
  --sidebar-border: #27272A;
  --sidebar-item-hover: #27272A;
  --sidebar-item-active-bg: #172554;
  --sidebar-item-active-text: #93C5FD;
  --sidebar-icon:   #A1A1AA;
  --sidebar-icon-active: #60A5FA;
}
```

---

## 3. Typography

### Font Stack
- **Primary**: `Inter` (Google Fonts) — clean, modern, excellent readability at small sizes
- **Monospace**: `JetBrains Mono` — for code, IDs, tracking numbers
- **Fallback**: `system-ui, -apple-system, sans-serif`

### Type Scale

| Token | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| `display` | 30px | 700 | 1.2 | Dashboard KPI values |
| `heading-1` | 24px | 600 | 1.3 | Page titles |
| `heading-2` | 20px | 600 | 1.3 | Section headings |
| `heading-3` | 16px | 600 | 1.4 | Card headings, table group headers |
| `body` | 14px | 400 | 1.5 | Default body text |
| `body-medium` | 14px | 500 | 1.5 | Emphasized body text |
| `body-small` | 13px | 400 | 1.5 | Secondary info, timestamps |
| `caption` | 12px | 400 | 1.4 | Labels, helper text, badges |
| `overline` | 11px | 600 | 1.3 | Nav group labels, uppercase section markers |
| `mono` | 13px | 400 | 1.5 | Order numbers, SKUs, tracking IDs |

---

## 4. Spacing System

Based on a **4px grid** with standard increments:

| Token | Value | Usage |
|-------|-------|-------|
| `space-0` | 0px | No spacing |
| `space-1` | 4px | Tight inline spacing |
| `space-2` | 8px | Icon-to-label gap, badge padding |
| `space-3` | 12px | Input padding, compact card padding |
| `space-4` | 16px | Default card padding, section gaps |
| `space-5` | 20px | Medium section gaps |
| `space-6` | 24px | Page content padding |
| `space-8` | 32px | Large section dividers |
| `space-10` | 40px | Page top/bottom margins |
| `space-12` | 48px | Major layout separators |
| `space-16` | 64px | Hero section spacing |

---

## 5. Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| `radius-sm` | 6px | Badges, small buttons, inputs |
| `radius-md` | 8px | Cards, panels, medium buttons |
| `radius-lg` | 12px | Modals, large cards, image containers |
| `radius-xl` | 16px | Floating panels, splash cards |
| `radius-full` | 9999px | Avatars, pill badges, circular buttons |

---

## 6. Layout System

### Admin Shell Structure

```
┌─────────────────────────────────────────────────────────┐
│                        TOPBAR (h:56px)                  │
│  ☰ Breadcrumbs            🔍 Search  🌙  👤 Admin ▾    │
├──────────┬──────────────────────────────────────────────┤
│          │                                              │
│ SIDEBAR  │              PAGE CONTENT                    │
│ (260px)  │              (flex: 1)                       │
│          │                                              │
│ ┌──────┐ │  ┌──────────────────────────────────┐       │
│ │ Logo │ │  │  Page Header                     │       │
│ ├──────┤ │  │  Title + Description + Actions   │       │
│ │      │ │  ├──────────────────────────────────┤       │
│ │ Nav  │ │  │                                  │       │
│ │ Group│ │  │  Page Content Area               │       │
│ │ Store│ │  │  (Cards, Tables, Forms)           │       │
│ │      │ │  │                                  │       │
│ │ Nav  │ │  │                                  │       │
│ │ Group│ │  │                                  │       │
│ │Mktg  │ │  │                                  │       │
│ │      │ │  └──────────────────────────────────┘       │
│ │ Nav  │ │                                              │
│ │ Group│ │                                              │
│ │Config│ │                                              │
│ │      │ │                                              │
│ ├──────┤ │                                              │
│ │Footer│ │                                              │
│ │↩ Back│ │                                              │
│ │🚪 Out│ │                                              │
│ └──────┘ │                                              │
└──────────┴──────────────────────────────────────────────┘
```

### Sidebar Behavior
- **Desktop (≥1024px)**: Fixed sidebar, 260px expanded / 68px collapsed (icons only)
- **Tablet (768–1023px)**: Sidebar collapsed by default, expand on hover/click
- **Mobile (<768px)**: Off-canvas drawer, triggered by hamburger in topbar

### Sidebar Navigation Groups
Three groups with dividers and overline labels:

**STORE**
- 📊 Dashboard
- 📦 Orders
- 🛍️ Products
- 🏷️ Brands
- 👥 Customers
- ⭐ Reviews

**MARKETING**
- 📢 Announcements
- 🖼️ Hero Slides
- ⚡ Flash Sales
- 🎟️ Coupons
- 🧩 Combos
- 📂 Collections

**CONFIG**
- 💳 Payment
- 🚚 Couriers
- ⚙️ Settings

### Topbar Layout
```
[☰ Toggle] [🏠 > Orders > #ORD-1234] ........ [🔍] [🌙/☀️] [👤 Admin ▾]
```
- **Left**: Sidebar toggle (collapse/expand) + Breadcrumbs (auto-generated from route)
- **Right**: Global search trigger (opens ⌘K palette), Theme toggle, User avatar dropdown (profile, logout)

### Content Area
- **Max width**: 1280px (centered with `mx-auto` on wide screens)
- **Padding**: 24px on desktop, 16px on mobile
- **Page header**: Title (h1) + optional description + action buttons (top-right)
- **Content sections**: Separated by 24px vertical gaps

---

## 7. Component Specifications

### 7.1 Stat Card (Dashboard KPI)

```
┌──────────────────────────┐
│  📦 Total Orders         │
│                          │
│  1,284                   │ ← display font, bold
│  ▲ 12.5% vs last month  │ ← caption, green/red with arrow
└──────────────────────────┘
```

- **Container**: `surface` bg, `radius-lg`, `shadow-sm`, padding `space-6`
- **Icon**: 20px, `ink-secondary` color, in a subtle `accent-light` circle
- **Label**: `caption` size, `ink-secondary`
- **Value**: `display` size, `ink` color
- **Trend**: `caption` size, green (▲) or red (▼), percentage + "vs last month"
- **Hover**: Subtle `shadow-md` elevation, 150ms transition
- **Grid**: 3 columns on desktop (1280px+), 2 on tablet, 1 on mobile

### 7.2 Data Table (TanStack Table)

```
┌─────────────────────────────────────────────────────────┐
│ ☑ Select All  │  Name ↕  │  Status  │  Price ↕  │  ⋯  │ ← Header row (sunken bg)
├───────────────┼──────────┼──────────┼───────────┼──────┤
│ ☐  Product A  │  Draft   │  ৳1,200 │  ⋮ Menu  │      │ ← Row (surface bg)
│ ☐  Product B  │  Active  │  ৳950   │  ⋮ Menu  │      │ ← Alternating subtle bg
│ ☑  Product C  │  Draft   │  ৳2,100 │  ⋮ Menu  │      │ ← Selected row (accent-light bg)
├───────────────┴──────────┴──────────┴───────────┴──────┤
│ Showing 1-10 of 156  │  ◀ 1 2 3 ... 16 ▶             │ ← Footer (pagination)
└─────────────────────────────────────────────────────────┘
```

- **Header**: `surface-sunken` bg, `overline` text, sortable columns with ↕ indicator
- **Rows**: `surface` bg, `border-bottom`, hover: `surface-hover`
- **Selected rows**: `accent-light` bg with accent left border
- **Cell padding**: `space-3` vertical, `space-4` horizontal
- **Row actions**: Overflow menu (⋮) with edit, delete, duplicate options
- **Pagination**: Bottom bar with page info, prev/next, page number buttons
- **Filters toolbar**: Above table — search input + filter dropdowns + clear all
- **Empty state**: Centered illustration + message + CTA button
- **Loading**: Skeleton rows (3-5 shimmer rows)

### 7.3 Status Pill / Badge

Rounded pill badges with semantic colors:

| Status | Background | Text | Border |
|--------|-----------|------|--------|
| `active` / `published` | `success-light` | `success-text` | `success` (20% opacity) |
| `pending` / `draft` | `warning-light` | `warning-text` | `warning` (20% opacity) |
| `inactive` / `cancelled` | `danger-light` | `danger-text` | `danger` (20% opacity) |
| `shipped` / `info` | `info-light` | `info-text` | `info` (20% opacity) |
| `neutral` | `surface-sunken` | `ink-secondary` | `border` |

- **Size**: `caption` text, padding `2px 10px`, `radius-full`
- **Dot variant**: Optional leading dot (8px circle) for compact indicators

### 7.4 Form Components

#### Input Field
```
┌──────────────────────────────┐
│  Label *                     │ ← body-medium, ink color
│  ┌──────────────────────────┐│
│  │  Placeholder text...     ││ ← body text, ink-tertiary
│  └──────────────────────────┘│
│  Helper text or error msg    │ ← caption, ink-secondary or danger
└──────────────────────────────┘
```

- **Border**: `border` color, 1px solid
- **Focus**: `border-focus` color + `shadow-focus` ring
- **Error**: `danger` border + `danger-text` helper text
- **Disabled**: `surface-sunken` bg + `ink-disabled` text
- **Height**: 40px (default), 36px (compact)
- **Border radius**: `radius-sm`

#### Toggle Switch
- **Track**: 44px × 24px, `border` bg (off), `accent` bg (on)
- **Thumb**: 20px circle, white, centered vertically
- **Transition**: 200ms ease, with subtle bounce
- **Label**: Placed to the left of the toggle

### 7.5 Card Component

```
┌──────────────────────────────┐
│  Card Title                  │ ← heading-3
│  Description text            │ ← body, ink-secondary
│  ┌──────────────────────────┐│
│  │  Card Content            ││
│  │  (Form, table, etc.)     ││
│  └──────────────────────────┘│
│  ──────────────────────────  │ ← separator
│  [Cancel]          [Save]    │ ← card footer with actions
└──────────────────────────────┘
```

- **Background**: `surface`
- **Border**: 1px `border`, `radius-lg`
- **Shadow**: `shadow-sm`
- **Padding**: `space-6`
- **Hover (interactive cards)**: `shadow-md`, 150ms transition
- **Sections**: Separated by 1px `border` lines

### 7.6 Modal / Dialog

```
┌──────────────────────────────────────┐
│  ╳                                   │ ← Close button (top-right)
│                                      │
│  Modal Title                         │ ← heading-2
│  Description or context              │ ← body, ink-secondary
│                                      │
│  ┌──────────────────────────────────┐│
│  │  Modal Content                   ││
│  └──────────────────────────────────┘│
│                                      │
│  ──────────────────────────────────  │
│  [Cancel]              [Confirm]     │ ← action buttons
└──────────────────────────────────────┘
```

- **Overlay**: `rgba(0, 0, 0, 0.5)` with blur backdrop
- **Container**: `surface-raised`, `radius-xl`, `shadow-xl`
- **Max width**: 560px (default), 720px (wide), 480px (compact)
- **Animation**: Scale from 95% → 100% + fade in, 200ms ease-out
- **Close**: ESC key + click outside + ╳ button

### 7.7 Charts (Recharts)

#### Shared Chart Styling
- **Container**: Card with `heading-3` title + `caption` subtitle
- **Grid lines**: Dashed, `border` color, 0.5 opacity
- **Axis text**: `caption` size, `ink-tertiary` color
- **Tooltip**: `surface-raised` bg, `shadow-lg`, `radius-md`, custom content
- **Legend**: Below chart, `caption` text, color dots

#### Color Palette for Charts
```
--chart-1: #3B82F6  /* Blue */
--chart-2: #8B5CF6  /* Violet */
--chart-3: #22C55E  /* Green */
--chart-4: #F59E0B  /* Amber */
--chart-5: #EF4444  /* Red */
--chart-6: #06B6D4  /* Cyan */
--chart-7: #EC4899  /* Pink */
--chart-8: #14B8A6  /* Teal */
```

---

## 8. Animation & Transitions

### Transition Tokens
| Token | Duration | Easing | Usage |
|-------|----------|--------|-------|
| `transition-fast` | 100ms | ease-out | Button press, toggle switch |
| `transition-base` | 150ms | ease-out | Hover effects, color changes |
| `transition-slow` | 200ms | ease-out | Sidebar collapse, modal open |
| `transition-enter` | 250ms | cubic-bezier(0.16, 1, 0.3, 1) | Element entrance (spring-like) |
| `transition-exit` | 150ms | ease-in | Element exit |

### Micro-Animations
1. **Sidebar collapse**: Width transitions from 260px to 68px, labels fade out, icons remain
2. **Table row hover**: Background color change + subtle left border accent
3. **Button press**: Scale to 98%, 100ms
4. **Toggle switch**: Thumb slides + track color fills, 200ms with slight bounce
5. **Page transitions**: Content fades in + translates up 8px
6. **Stat card value**: Counter animation on dashboard load (optional)
7. **Chart entrance**: Bars/lines animate in on viewport entry, staggered 50ms
8. **Toast entrance**: Slides in from top-right, subtle spring
9. **Modal**: Scale 95% → 100% + overlay fade in
10. **Dropdown**: Scale from 95% + fade, origin top-left

### Motion Preferences
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 9. Responsive Breakpoints

| Breakpoint | Width | Layout Changes |
|-----------|-------|----------------|
| `xs` | 0–639px | Single column, mobile nav drawer, stacked cards, mobile-optimized tables |
| `sm` | 640–767px | Slightly wider cards, 2-column stat grid |
| `md` | 768–1023px | Collapsed sidebar (icons), 2-column layouts |
| `lg` | 1024–1279px | Full sidebar, 3-column stat grid, standard tables |
| `xl` | 1280–1535px | Max-width content, comfortable spacing |
| `2xl` | 1536px+ | Centered content, generous margins |

### Mobile Table Pattern
On mobile (<768px), data tables transform into **card lists**:
```
┌──────────────────────────┐
│ #ORD-1234                │ ← order number (heading-3)
│ John Doe • ৳2,400       │ ← customer + total
│ [🟡 Pending]   2h ago   │ ← status pill + relative time
│ ─────────────────────── │
│ [View] [Update Status]   │ ← action buttons
└──────────────────────────┘
```

---

## 10. Icon System

Using **Lucide React** (tree-shakeable, consistent 24px grid).

### Icon Usage
- **Navigation**: 20px, `sidebar-icon` / `sidebar-icon-active` color
- **Actions (buttons)**: 16px, matches button text color
- **Inline (form labels)**: 16px, `ink-secondary`
- **Status indicators**: 14px, semantic color
- **Empty states**: 48px, `ink-tertiary`

### Key Icon Mappings
| Context | Icon |
|---------|------|
| Dashboard | `LayoutDashboard` |
| Orders | `ShoppingBag` |
| Products | `Package` |
| Brands | `Tag` |
| Users | `Users` |
| Reviews | `Star` |
| Announcements | `Megaphone` |
| Slides | `Image` |
| Flash Sales | `Zap` |
| Coupons | `Ticket` |
| Combos | `Layers` |
| Collections | `FolderOpen` |
| Payment | `CreditCard` |
| Couriers | `Truck` |
| Settings | `Settings` |
| Search | `Search` |
| Theme toggle | `Sun` / `Moon` |
| Collapse sidebar | `PanelLeftClose` / `PanelLeftOpen` |

---

## 11. Empty States

Every data view needs an empty state:

```
┌─────────────────────────────────────┐
│                                     │
│           📦                        │ ← 48px icon, ink-tertiary
│                                     │
│     No products yet                 │ ← heading-3, ink
│     Get started by adding your      │ ← body, ink-secondary
│     first product to the catalog.   │
│                                     │
│     [+ Add Product]                 │ ← accent button
│                                     │
└─────────────────────────────────────┘
```

---

## 12. Loading States

### Skeleton Patterns

#### Stat Card Skeleton
```
┌──────────────────────────┐
│  ██████████              │ ← label skeleton
│                          │
│  ████████                │ ← value skeleton (wider)
│  ██████                  │ ← trend skeleton
└──────────────────────────┘
```

#### Table Skeleton
```
┌─────────────────────────────────────────┐
│  ████████  │  ██████  │  ████  │  ██   │ ← header
├────────────┼──────────┼────────┼────────┤
│  █████████ │  ████    │  ███   │  █    │ ← row (shimmer animation)
│  ███████   │  █████   │  ██    │  █    │
│  ██████    │  ██████  │  ████  │  █    │
└─────────────────────────────────────────┘
```

- **Animation**: Left-to-right shimmer gradient, 1.5s infinite
- **Colors**: `surface-sunken` base with `surface-hover` shimmer highlight
- **Count**: Show 3-5 skeleton rows for tables, match expected card count for grids

---

## 13. Dark Mode Specifics

### Key Differences from Light
1. **Surfaces**: Slightly lighter than `bg` to create depth (not purely black)
2. **Borders**: More visible, slightly lighter to separate content areas
3. **Shadows**: Replaced with subtle border highlights or reduced opacity
4. **Charts**: Slightly brighter colors to maintain readability against dark backgrounds
5. **Images**: No filter changes; rely on content contrast
6. **Accent colors**: Shifted lighter for better contrast on dark backgrounds

### Toggle Behavior
- **Default**: Light theme
- **Toggle**: Sun/Moon icon in topbar
- **Persistence**: Stored in `localStorage` + cookie (for SSR hydration)
- **Transition**: 150ms color transition on all elements
- **SSR**: Use `next-themes` to prevent flash of wrong theme

---

## 14. Print Styles

For invoice printing:
```css
@media print {
  /* Hide admin chrome */
  .sidebar, .topbar, .page-header-actions { display: none; }

  /* Reset backgrounds */
  body, .card { background: white; color: black; }

  /* Remove shadows and borders */
  .card { box-shadow: none; border: 1px solid #ddd; }

  /* Full width */
  .content-area { max-width: 100%; padding: 0; }
}
```

---

## 15. Accessibility Checklist

- [ ] **Color contrast**: All text meets 4.5:1 ratio (AA)
- [ ] **Focus indicators**: Visible focus ring on all interactive elements (`shadow-focus`)
- [ ] **Keyboard navigation**: All actions accessible via keyboard (Tab, Enter, Escape, Arrow keys)
- [ ] **ARIA labels**: All icon-only buttons have `aria-label`
- [ ] **Skip to content**: Hidden link to skip sidebar navigation
- [ ] **Form errors**: Associated with inputs via `aria-describedby`
- [ ] **Loading states**: `aria-busy` on loading containers
- [ ] **Modals**: Focus trap, `role="dialog"`, `aria-modal="true"`
- [ ] **Tables**: `scope="col"` on headers, `aria-sort` on sortable columns
- [ ] **Status changes**: `aria-live` regions for dynamic updates
- [ ] **Reduced motion**: Respect `prefers-reduced-motion`
- [ ] **Screen reader text**: `.sr-only` class for visually hidden labels

---

## 16. Design Token Implementation

All tokens are implemented as Tailwind CSS v4 theme extensions:

```css
/* globals.css */
@import "tailwindcss";

@theme {
  --color-bg: var(--color-bg);
  --color-surface: var(--color-surface);
  --color-ink: var(--color-ink);
  /* ... all tokens mapped to Tailwind utilities */

  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
}
```

This enables usage like:
```html
<div class="bg-surface text-ink border-border rounded-lg shadow-sm">
  <h1 class="text-xl font-semibold">Page Title</h1>
  <p class="text-ink-secondary text-sm">Description</p>
</div>
```
