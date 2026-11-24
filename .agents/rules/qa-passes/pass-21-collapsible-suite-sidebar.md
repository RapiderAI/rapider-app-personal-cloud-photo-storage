# Pass 21: Suite Brand & Collapsible Left Sidebar Navigation Standards

**Goal:** Ensure every Rapider application page renders a consistent, well-designed **Collapsible Suite Left Sidebar (`<aside class="sidebar">`)** inside `.ds-shell` / `.layout` — displaying the authentic Suite Icon and Suite Short Title in the brand header, an App Identity box (`.appbox`), all enabled menu pages (`enabled !== false && displayOnMenu !== false && displayInMenu !== false`) from `app.manifest.json`, full desktop collapse/expand mechanics (`68px` collapsed rail, hover right-caret expand indicator on the top Suite icon, `localStorage` persistence under `rapider_sidebar_collapsed`), clean semantic navigation colors (never saturated primary blue), and mobile off-canvas drawer responsiveness (`.mobile-menu` + `.shade`).

## 0. Mandatory In-App Collapsible Left Sidebar Shell (`.ds-shell.layout` + `<aside class="sidebar">`)
- Every application page (`pages/[route]/[route].html`) **MUST** include the canonical left `<aside class="sidebar">` navigation rail alongside `<main class="ds-main">` inside `<div class="ds-shell layout @container">`.
- **Draft & Hidden Pages Exclusion:** Never include pages where `"enabled": false` (draft pages) or `"displayOnMenu": false` / `"displayInMenu": false` (detail/edit/new sub-views) in the `<aside class="sidebar">` menu links. Detail/edit pages themselves still render the `<aside class="sidebar">` so the user always has the left navigation rail visible while viewing a record!

## 1. Manifest Suite Definition (`app.manifest.json`)
Every app manifest MUST include an `appSuite` object with:
- `key`: The suite acronym (e.g. `HR`, `FIN`, `APM`, `OPS`, etc.).
- `title`: The full title (e.g. `Rapider HR & Talent Suite`).
- `shortTitle`: The concise suite display name (e.g. `HR Suite`, `Finance Suite`).
- `icon`: The canonical FontAwesome icon class (e.g. `fas fa-users`, `fas fa-wallet`, `fas fa-layer-group`).

```json
"appSuite": {
  "key": "HR",
  "title": "Rapider HR & Talent Suite",
  "shortTitle": "HR Suite",
  "icon": "fas fa-users"
}
```

## 2. Elimination of Fake Rapider Branding in Sidebars
- **FORBIDDEN:**
  - Fake purple square "r" brandmarks (`<span class="brandmark">r</span>`).
  - "rapider ai" superscript or subtitle text in the brand header.
  - Generic "rapider" text in the left navigation header.
- **MANDATORY:**
  - Sidebar header MUST display the Suite Icon (`manifest.appSuite.icon`) and the Suite Name (`manifest.appSuite.shortTitle`) inside a `<button type="button" class="brand" data-action="expand-sidebar">` (NEVER an `<a class="brand" href="#/...">` tag, because the Rapider iframe host registers a capture-phase `document` click listener on `<a>` tags that would hijack clicks on the collapsed sidebar header and trigger route navigation instead of expanding the menu!).
  - Example: `<button type="button" class="brand" data-action="expand-sidebar" title="HR Suite" aria-label="HR Suite"><span class="suite-icon-container"><i class="suite-icon fas fa-users"></i><i class="suite-expand-icon fas fa-chevron-right"></i></span><span class="suite-title">HR Suite</span></button>`

## 3. Navigation Menu Item Styling (Strictly Prohibit Saturated Blue)
- **FORBIDDEN:**
  - Never use saturated primary blue (`#0070f3`, `bg-primary text-white`, `text-blue-600`, `bg-blue-600`) on navigation items.
- **MANDATORY:**
  - **Inactive Items:** Muted slate color `#67728a` (hover `#1e293b` with `#f8fafc` background).
  - **Active Item:** Subtle soft lavender/purple background `#f3f0ff`, dark purple text `#53389e`, font-weight 600.
  - **Icons:** Inactive items have `#67728a` icon; active item has `#53389e` icon.
  - Consistent across light and dark modes (in dark mode: inactive `#94a3b8`, active `#312e81` bg with `#c7d2fe` text).

## 4. Mobile Responsive Off-Canvas Drawer Standard
- On mobile viewports (`max-width: 768px` / `680px`):
  1. The left sidebar MUST be off-canvas by default:
     ```css
     @media (max-width: 768px) {
       aside.sidebar, aside#suiteSidebar, aside {
         position: fixed !important;
         left: 0 !important;
         top: 0 !important;
         bottom: 0 !important;
         height: 100vh !important;
         height: 100dvh !important;
         width: 260px !important;
         transform: translateX(-100%) !important;
         transition: transform 0.25s cubic-bezier(0.4, 0, 0.2, 1) !important;
         z-index: 50 !important;
       }
       .navopen aside.sidebar, body.navopen aside.sidebar {
         transform: translateX(0) !important;
         box-shadow: 0 10px 25px rgba(0,0,0,0.2) !important;
       }
       .navopen .shade, body.navopen .shade {
         display: block !important;
         position: fixed !important;
         inset: 0 !important;
         background: rgba(15, 23, 42, 0.5) !important;
         z-index: 45 !important;
       }
       .mobile-menu {
         display: inline-flex !important;
       }
       .sidebar-collapse-btn {
         display: none !important;
       }
     }
     ```
  2. The top header (`<header class="topbar ...">`) MUST contain a standard mobile hamburger menu button at the top left:
     ```html
     <button class="mobile-menu" data-action="menu" aria-label="Open navigation" aria-expanded="false">
       <i class="fas fa-bars"></i>
     </button>
     ```
  3. Clicking `.mobile-menu` toggles `body.navopen`, displaying the sidebar and backdrop shade (`<div class="shade"></div>`). Tapping the shade or pressing `Escape` dismisses the drawer.

## 5. Collapsible Sidebar Mechanics (Desktop)
1. **Collapse Toggle Button:**
   - In expanded state, a collapse button (`<button class="sidebar-collapse-btn"><i class="fas fa-chevron-left"></i></button>`) must be visible in the brand header row.
   - Clicking this button toggles the sidebar into collapsed mode (`sidebar-collapsed` class on `<aside>` and `.layout`).
2. **Compact Width & Label Hiding:**
   - In collapsed state, the sidebar collapses to ~64-68px width.
   - All text labels (`.sidebar-label`, `.suite-title`, `.navlabel`, `.appbox span:not(.appicon)`, unwrapped nav link text, footer profile names) MUST be hidden with `display: none !important;` (and `font-size: 0 !important;` for text nodes).
   - Navigation icons remain visible and centered.
3. **Hover Expand Indicator on Top Suite Icon & Capture-Phase Expand Listener (CRITICAL):**
   - When collapsed, hovering anywhere on the collapsed sidebar (`.sidebar.sidebar-collapsed:hover` or `aside.collapsed:hover`) transforms the top Suite icon into a right caret / expand icon (`<i class="suite-expand-icon fas fa-chevron-right"></i>`) with `pulse-caret` animation.
   - **MANDATORY `window` Capture-Phase Listener:** `<script data-collapsible-sidebar>` MUST attach its expand/collapse click listener on `window` in the **capture phase** (`window.addEventListener("click", ..., true)`) so it executes BEFORE any `document` capture-phase anchor interceptor injected by the Rapider preview iframe SDK (`html-preview.component.ts`).
   - Clicking `.brand`, `.suite-icon-container`, `.brand-row`, or `[data-action="expand-sidebar"]` while collapsed immediately calls `e.preventDefault(); e.stopPropagation(); e.stopImmediatePropagation(); setCollapsed(false);`.
4. **State Persistence:**
   - The collapsed state MUST be saved in `localStorage.setItem('rapider_sidebar_collapsed', 'true' | 'false')` and restored immediately on page load so user preference persists seamlessly across navigation.
