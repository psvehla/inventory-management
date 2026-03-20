---
name: saas-redesign
description: Redesign a Vue 3 application's UI into a modern SaaS-style interface with a vertical navigation sidebar, consistent spacing, and a polished professional look. Use when the user wants to modernize the app layout or switch from top nav to sidebar nav.
user-invocable: true
argument-hint: [component-or-scope]
---

# SaaS UI Redesign

Redesign the Vue 3 application into a modern SaaS-style interface. If `$ARGUMENTS` is provided, limit the redesign scope to that component or area. Otherwise, redesign the full application layout.

## Design Principles

- **Vertical sidebar navigation** replaces any horizontal top nav bar
- **Clean, spacious layout** with consistent padding and margins
- **Professional color palette** using neutral tones with a single accent color
- **Clear visual hierarchy** through typography weight, size, and spacing
- **Subtle depth** via light borders and minimal shadows (no heavy drop shadows)
- **Responsive** sidebar collapses on small screens

## Step-by-Step Process

### 1. Explore the Current Layout

Before making any changes, understand the existing structure:

- Read the root component (typically `App.vue`) to identify the current navigation pattern, layout structure, and global styles
- Read the router configuration to identify all routes and their labels
- Read 2-3 view components to understand content area patterns
- Identify any existing composables for shared state (auth, i18n, filters, theme)
- Note all navigation items, their labels, routes, and any active-state logic

### 2. Design the Sidebar Layout

Replace the current layout with this structure:

```
+------------------+----------------------------------------------+
|                  |  Top Bar (breadcrumb / page title / actions) |
|    Sidebar       +----------------------------------------------+
|    (fixed)       |                                              |
|                  |           Main Content Area                  |
|  - Logo/Brand    |           (scrollable)                       |
|  - Nav Items     |                                              |
|  - Bottom Items  |                                              |
+------------------+----------------------------------------------+
```

**Sidebar specifications:**
- Width: 240px (expanded), 64px (collapsed on mobile)
- Background: dark (`#0f172a` slate-900) or white with left border
- Fixed position, full viewport height
- Sections: brand/logo at top, main nav items, utility items at bottom (profile, settings, logout)
- Active nav item highlighted with accent color background + left border indicator
- Nav items include an icon placeholder (use simple CSS shapes or inline SVG) and label text
- Smooth hover transitions on all interactive elements

**Top bar specifications:**
- Height: 56-64px
- Contains: page title (from current route), right-aligned actions (filters, user profile, language switcher)
- Light background with subtle bottom border
- Sticky at top of content area

**Content area specifications:**
- Left margin equal to sidebar width
- Max-width container (1280px) centered with auto margins, or full-width with consistent padding (24-32px)
- Background: `#f8fafc` (slate-50)

### 3. Implement the CSS Architecture

Use CSS custom properties for the design system:

```css
:root {
  /* Layout */
  --sidebar-width: 240px;
  --sidebar-collapsed-width: 64px;
  --topbar-height: 56px;
  --content-padding: 24px;

  /* Colors - Neutral */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f8fafc;
  --color-bg-sidebar: #0f172a;
  --color-text-primary: #0f172a;
  --color-text-secondary: #64748b;
  --color-text-sidebar: #cbd5e1;
  --color-text-sidebar-active: #ffffff;
  --color-border: #e2e8f0;

  /* Colors - Accent */
  --color-accent: #3b82f6;
  --color-accent-light: #dbeafe;
  --color-accent-hover: #2563eb;

  /* Colors - Status (preserve existing) */
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-danger: #ef4444;
  --color-info: #3b82f6;

  /* Typography */
  --font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 0.9375rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;

  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;

  /* Borders & Shadows */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 1px 3px rgba(0, 0, 0, 0.08);
}
```

### 4. Build the Sidebar Component

Create or modify the root layout component. The sidebar should:

- Render all navigation items from the router config
- Highlight the active route using `$route.path`
- Include the app brand/logo at the top
- Place utility items (profile, logout, language switcher) at the bottom
- Support a collapsed state toggled by a button (store in a ref or composable)
- Use `<router-link>` for navigation items
- Animate transitions (width change, icon-only mode)

**Nav item HTML pattern:**
```html
<router-link to="/path" class="nav-item" :class="{ active: $route.path === '/path' }">
  <span class="nav-icon"><!-- icon --></span>
  <span class="nav-label">Label</span>
</router-link>
```

**Nav item CSS pattern:**
```css
.nav-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-4);
  margin: 0 var(--space-2);
  border-radius: var(--radius-sm);
  color: var(--color-text-sidebar);
  text-decoration: none;
  font-size: var(--font-size-sm);
  transition: all 0.15s ease;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.08);
  color: var(--color-text-sidebar-active);
}

.nav-item.active {
  background: var(--color-accent);
  color: var(--color-text-sidebar-active);
  font-weight: 500;
}
```

### 5. Modernize Content Components

For each view component, ensure:

- **Page headers**: Use a consistent pattern with title (font-size-2xl, font-weight-700) and optional subtitle (font-size-sm, color-text-secondary)
- **Cards**: White background, 1px border (color-border), border-radius-lg, padding of space-6. No heavy shadows.
- **Tables**: Clean with subtle row borders, header row in bg-secondary, font-size-sm for data
- **Stat cards / KPI cards**: Consistent height, label on top (font-size-xs, uppercase, text-secondary), value below (font-size-2xl, font-weight-700)
- **Badges**: Pill-shaped (border-radius: 9999px), small (font-size-xs, padding: 2px 8px), status-colored backgrounds at 10% opacity with matching text color
- **Buttons**: Primary uses accent color, consistent padding (space-2 space-5), border-radius-sm, font-weight-500
- **Spacing**: Use the spacing scale consistently. Cards in a grid should have gap of space-6. Sections separated by space-8.
- **Grid layouts**: Use CSS Grid with consistent gaps. Stat cards in 2-4 column grids. Main content in single column or 2-column where appropriate.

### 6. Delegate Vue File Changes

**IMPORTANT**: Per project rules, delegate creation or significant modification of `.vue` files to the `vue-expert` subagent. Provide the vue-expert with:

- The specific file to modify
- The exact design tokens (CSS variables) to use
- The layout structure to implement
- Reference to existing patterns in the codebase

### 7. Polish and Consistency Pass

After the structural changes:

- Verify all pages look consistent by navigating through each route
- Check that active states work correctly in the sidebar
- Ensure filters, modals, and dropdowns still function
- Verify responsive behavior (sidebar collapse)
- Test that existing functionality (data loading, filtering, form submission) is unbroken
- Use Playwright MCP tools to visually verify the result in the browser

## What NOT to Change

- Do not alter business logic, API calls, or data flow
- Do not modify backend code
- Do not remove existing features or functionality
- Do not change i18n keys unless adding new ones for sidebar labels
- Do not change the router paths, only the visual presentation
- Preserve all existing status colors and their semantic meaning
