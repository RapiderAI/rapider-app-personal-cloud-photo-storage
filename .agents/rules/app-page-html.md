# Guide: Generating UI HTML Pages

When you are generating the actual frontend code for a page, you **MUST** save the generated HTML file in a nested directory based on the route key at the root of the repository: `pages/[route]/[route].html` (e.g., `pages/dashboard/dashboard.html`). This exact folder structure is required for the backend synchronization to work. You are acting as a Full-Stack UI/UX Engineer. Your objective is to generate a **single, fully functional HTML file** containing structure, styling, and working Vanilla JavaScript.

You must deliver a complete, interactive application that users can run inside their browser sandbox. The UI must be built using standard Tailwind CSS alongside the custom semantic theme, and the JavaScript MUST execute REAL CRUD operations against the backend using the injected `window.rapiderApi` SDK.

## 1. HTML Structure & Asset Loading

Your output must be a standard, standalone HTML5 document. You must load Tailwind CSS via CDN and inject the custom theme file.

Generate a complete, valid HTML5 document containing `<html>`, `<head>`, and `<body>` tags. Include the following in the `<head>`:

1. **FontAwesome:** `<script src="https://kit.fontawesome.com/5b23df26dc.js" crossorigin="anonymous"></script>`
2. **Custom Components Bundle:** `<script src="/assets/js/rapider-components.js" defer></script>`
3. **Tailwind CDN:** `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>`
4. **Semantic Theme Base:** `<link rel="stylesheet" href="/assets/themes/rapider-tailwind-theme.css">`
5. **Canonical Rapider Design System — Default Theme & Multi-Theme Contract (MANDATORY):** `<link rel="stylesheet" href="/assets/themes/rapider-design-system.css">` (Must be loaded **after** `rapider-tailwind-theme.css`. It defines the **Rapider Default Theme** in `:root` / `.dark` using strictly **semantic role variables** — `--primary-color`, `--secondary-color`, `--tertiary-color`, `--hero-surface-bg`, `--body-background-color`, `--component-inner-background-color`, `--primary-text-color`, `--secondary-text-color`, `--primary-border-color`, `--success-color`, `--warning-color`, `--danger-color`, `--info-color`, `--neutral-color` — so any user, workspace, or app can switch to another theme CSS later without changing a single line of page HTML or JS).

## 2. Design-Guided Evolution, Multi-Theme Tokens & Archetype-Diverse Architecture

Every microapp page MUST use the **Semantic Multi-Theme Tokens** (`rapider-tailwind-theme.css` + `rapider-design-system.css`) and the **Collapsible Suite Left Sidebar (`<aside class="sidebar" id="suiteSidebar">`)**, while **preserving `_design/` visual architecture as your guide and tailoring `<main class="ds-main">` to the SOW requirement** — **NEVER** blindly wiping bespoke designs or stamping every route into a single `KPI Strip + Tabs + CRUD Table` template!

Before authoring or editing any page HTML, read BOTH local skills (when present):
- `.agents/skills/rapider-design-guided-page-evolution/SKILL.md`
- `.agents/skills/rapider-modern-saas-page-craft/SKILL.md`

* **🎨 UNDERSTAND REQUIREMENT → PRESERVE `_design/` GUIDE → HARMONIZE → IMPROVE (3-WAY RULE):**
  - **Design Language Harmony ≠ Identical DOM/Page Clones:** All Rapider applications share one design language (semantic theme tokens, calm hairline surfaces, typography, spacing, and Pass 21 Collapsible Suite Left Sidebar), **NOT** the exact same inner DOM!
  - **Rule A — When `_design/` HTML Exists (Section 3.5 of `.page-task-context.md`):**
    - Take the `_design/` HTML/CSS as your **primary layout, composition, and visual guide**!
    - Preserve its page archetype and bespoke visual hierarchy (e.g., `.overview-top` + `.overview-grid` split cockpits, `.form-grid` + `.sticky` guided evaluation/offer/intake studios, `.profilehero` + `.detail-grid` 360° profiles, `.quickgrid` card grids, `.help-hero` portals, `.inbox` split triage, `.paper` offer/statement sheets, `.funnel-row` / `.meter` / `.chart` visualizers, `.toggle-row` + `.savebar` settings).
    - Harmonize the design language by remapping raw prototype colors (`--purple`, `--green`, `--amber`, `--dark`, `#fff`) to `rapider-design-system.css` semantic tokens (`var(--primary-color)`, `var(--hero-surface-bg)`, `var(--component-inner-background-color)`, `.tag-success`, etc.), enforcing `#ffffff !important` on `.btn.primary`/`.btn.danger`, keeping the Pass 21 Collapsible Suite Left Sidebar, and wiring live `window.rapiderApi` SWR calls.
    - **NEVER blindly rebuild** a page whose existing HTML already honors its `_design/` layout and SOW purpose — improve it incrementally in place.
    - **Differentiate similar wireframes:** When two `_design/` list pages share a clean `.data-panel` skeleton (e.g., `requisitions`, `candidates`, `interviews`), keep the calm visual cadence AND upgrade each page around its SOW requirement (distinct summary strip or inline scheduling/workflow panel, distinct domain columns, progress/consent/schedule badges).
  - **Rule B — When a Page Was Previously Flattened or Has No Exact `_design/` Match (`sibling_guide_only` / `none`):**
    - If an existing page flattened a bespoke `_design/` route (e.g. turning `overview` or `scorecard` into a generic `4 KPI + Tabs + Table` clone), **restore and elevate** it around the `_design/` guide and SOW.
    - For greenfield or extension routes without a 1:1 `_design/` file, use sibling `_design/` pages' token/spacing grammar as your visual guide and choose the best SaaS archetype (`Executive Cockpit`, `Stage Flow & Kanban Workbench`, `Differentiated Operational Ledger`, `360° Entity Profile`, `Split-Pane Guided Composer`, `Curated Card Grid / Portal`, `Analytics Studio`, or `Policy & Governance Control Center`).
  - **NEVER Render Raw or Sliced Foreign-Key UUIDs (`00000000-...` or `id.slice(0, 8)`) in Visible UI:** Always fetch related entities via `window.rapiderApi.findSWR`, resolve lookup maps (`candidateById`, `reqById`, `interviewById`), and display human-readable names and domain codes (`REQ-2026-101`, `Jordan Ellis`).
  - **Align KPI & Filter Calculations with Actual `sample-data.json` Values:** Inspect the exact status/enum values in `sample-data.json` and `app.manifest.json` so KPI cards and tabs reflect live counts accurately.

* **❌ BANNED VISUAL & THEMING ANTI-PATTERNS:**
  - **NO Color-Named Variables, Tailwind Hue Utilities, or Raw Hexes in Page HTML/CSS/JS:** Never hardcode color-named CSS variables (`--purple`, `--green`, `--amber`, `--red`), legacy color modifiers (`.tag.green`, `.tag.amber`, `.metric-card.dark`), Tailwind hue utilities (`bg-purple-600`, `bg-blue-600`, `text-indigo-600`), or raw `#hex` colors (`#6854df`, `#1890ff`).
  - **NO `.primary` Button Background on Standalone KPI Numerals:** Use `.metric-value.primary` (`color: var(--link-color)`) for text-colored KPI figures, never `.primary` button styling on KPI numbers.
  - **NO Overlapping Search Icons:** Keep search icons in a flex `.ds-search-box` beside the `<input>` or use `padding-left: 36px` so icons never collide with placeholder text.
  - **NO 5-Column Cramped KPI Strips or Truncated Labels:** When using summary KPI cards, use 3–4 cards max (`.metrics-grid-3`, `.metrics-grid-4`, or `.overview-top`) with clean non-truncated headings.
  - **NO Loud `rounded-full` Status Pills or Heavy Drop-Shadows:** Use flat `1px solid var(--primary-border-color)` hairline borders (`box-shadow: none`) and `5px`-radius semantic `.tag` chips (`.tag-primary`, `.tag-secondary`, `.tag-tertiary`, `.tag-success`, `.tag-warning`, `.tag-danger`, `.tag-info`, `.tag-neutral`).

### Outer Shell Contract (`<aside class="sidebar">` + `<main class="ds-main">`)
Every page MUST use the canonical Pass 21 outer shell below while composing `<main class="ds-main">` according to its `_design/` guide and SOW archetype:
```html
<body class="bg-body-background text-primary-text">
  <div class="ds-shell layout @container">
    <aside class="sidebar" id="suiteSidebar" aria-label="Application navigation">
      <!-- Automatically synchronized by sync-collapsible-sidebar.js -->
    </aside>
    <div class="shade" data-action="close-menu"></div>
    <main class="ds-main" id="main-content">
      <header class="page-head">
        <div>
          <button type="button" class="btn icon-only quiet mobile-menu" data-action="menu" aria-label="Open navigation"><i class="fas fa-bars"></i></button>
          <span class="eyebrow">SUITE / DOMAIN</span>
          <h1 class="page-title">Editorial page title</h1>
          <p class="subtitle">Calm, requirement-accurate subtitle.</p>
        </div>
        <div class="head-actions">
          <button type="button" id="btnAskRapider" class="btn"><i class="fas fa-wand-magic-sparkles"></i><span>Ask Rapider</span></button>
          <!-- Primary CTA only when the page creates/starts an action -->
        </div>
      </header>
      <!-- Compose inner sections from _design/ guide & SOW archetype:
           * Executive Cockpit: .overview-top (.hero + 2 .metric) + .insight-strip + .overview-grid
           * Guided Form / Rubric / Offer: .form-grid (grouped .panel form sections + .sticky guidance rail)
           * 360° Record Profile: .profilehero + in-page button[data-tab] + .detail-grid
           * Card Grid / Portal: .help-hero / .quickgrid .quickcard
           * List / Register (only when page is a directory/register): .panel > .ds-toolbar + #activeFilterTagsBar + .table-wrap > table.ds-table -->
    </main>
  </div>
</body>
```
* **Non-wrapping Semantic Status Tags:** Always use `.tag` (`border-radius: 5px; white-space: nowrap; flex-shrink: 0;`) with semantic role modifiers (`.tag-primary`, `.tag-secondary`, `.tag-tertiary`, `.tag-success`, `.tag-warning`, `.tag-danger`, `.tag-info`, `.tag-neutral`).

### High-Contrast Button Invariant
* Primary buttons (`bg-primary`, `.btn.primary`) and danger buttons (`bg-danger`, `.btn.danger`, `bg-red-600`) MUST ALWAYS enforce white text:
  - Add `!text-white text-white` classes (or `.btn.primary` / `.btn.danger` semantic classes).
  - Add explicit inline style: `style="color: #ffffff !important;"`.
* **BANNED:** Never use `text-white` on light or neutral backgrounds like `bg-button-bg` or `bg-alt-background`. Secondary buttons must use `text-secondary-button-text`.
* **BANNED:** "Install Sample Data" or "Seed Demo Records" buttons are **STRICTLY FORBIDDEN** in the header or toolbar. They may ONLY be rendered inside an empty-state container when `records.length === 0`.


## 3. Functional JavaScript & Real API Integration

Your generated HTML must include a `<script>` tag containing vanilla JavaScript to make the page 100% interactive.

### The `window.rapiderApi` SDK
Do NOT mock API calls. A proxy SDK is already injected into your environment. You MUST use it to communicate with the backend data models defined in `app.manifest.json`.

* `await window.rapiderApi.create(entityName, body)`: Create a new entity instance.
* `await window.rapiderApi.findSWR(entityName, filter)`: Find an array of entities with a LoopBack filter using Stale-While-Revalidate caching for immediate rendering. You **MUST** use this instead of `find`.
  * *Example:* `const data = await window.rapiderApi.findSWR('CrmAccount', { where: { isDeleted: false }, order: ['createdDate DESC'] });`
* `await window.rapiderApi.findById(entityName, id, filter)`: Get a specific entity object by its ID.
* `await window.rapiderApi.updateById(entityName, id, body)`: Patch specific fields of an entity by ID.
* `await window.rapiderApi.deleteById(entityName, id)`: Delete an entity by ID.
* `await window.rapiderApi.countSWR(entityName, where)`: Return the count of entities matching a where filter using SWR caching. You **MUST** use this instead of `count`.
* `await window.rapiderApi.startWorkflow(workflowName, payload)`: Triggers a backend workflow asynchronously.
  * **Strict Workflow Decision Rule (IF AND ONLY IF):** Define and trigger a workflow **IF AND ONLY IF** the action cannot be achieved by one or multiple standard CRUD calls (e.g. converting markdown to PDF, multi-step AI pipelines like analyzing data and generating a PDF report, batch OCR, multi-language document translation). For standard entity CRUD operations, always use CRUD methods instead.
  * **Draft Manifest Requirement (`type: "pageWorkflow"`):** When using `startWorkflow('{uniqueWorkflowName}', payload)`, the workflow MUST be defined in detail and saved into `app.manifest.json` under `workflows` with `type: "pageWorkflow"`, `status: "draft"`, and detailed `input` and `output` schemas.

### State and DOM Manipulation
Create local Javascript state to hold fetched data, write functions to render the UI dynamically based on the state, and bind event listeners to buttons and forms. Re-fetch or update the local state and DOM after successful API mutations.

### 🚫 STRICT BAN: No Static Mock Records in HTML Markup
- **Never hardcode static mock entity records, fake cards, or static `<tbody>` rows in HTML markup.**
- All entity tables, card lists, and metric figures must be rendered dynamically via JavaScript from `window.rapiderApi.findSWR(...)` and `window.rapiderApi.countSWR(...)`.
- When `findSWR` returns `[]` (empty database), the page MUST render a clean Empty State with an action to create the first record or "Seed Demo Records". Never leave fake mock data on screen when the backend is empty!

## 4. UI Actions, Overlays & Navigation

The SDK exposes methods to control the parent application's layout, navigation, and overlays. Use these to build rich, multi-page interactions directly from your Javascript event listeners.

* **`window.rapiderApi.showNotification({ type, message })`**
  * **type**: `'success' | 'error' | 'info' | 'warning'`
* **`window.rapiderApi.showConfirmModal({ title, message, okText, cancelText, isDanger, onConfirm, onCancel })` / `window.showConfirmModal(options)`**
  * Opens a confirmation modal dialog.
* **`window.rapiderApi.navigate(route)`**
  * Navigates the parent router to a new URL.
* **`window.rapiderApi.showPageModal({ route, payload, title, width })`**
  * Opens a modal rendering another dynamic page definition.
* **`window.rapiderApi.showPageDrawer({ route, payload, title, placement, size, width })`**
  * Opens a sliding drawer from the edge of the screen. `placement` can be `right`, `left`, `top`, `bottom`.
* **`window.rapiderApi.showPageSplitter({ route, payload, title, icon, defaultSize, min, max, resizable, layout })`**
  * Transforms the current view into a resizable split-pane, projecting the target route side-by-side with the current page.

### 🚫 STRICT BAN: No Pure JS `alert()`, `confirm()`, or `prompt()`
- **NEVER use pure JavaScript `alert()`, `confirm()`, or `prompt()` anywhere in page scripts.**
- Pure JS alerts halt UI execution, break iframe sandboxes, and create a poor user experience.
- **For confirmations (deleting records, status transitions, irreversible actions):**
  Always use a nice modal confirmation dialog (e.g. `showConfirmModal` or a styled Tailwind confirmation modal / popconfirm). The modal must display:
  - An informative title and clear description message (including the entity or item name being deleted).
  - Explicit action buttons: "Cancel" and "Delete" (or "Confirm").
  - Danger styling (red badge/button) for destructive operations.
  - Double-click / re-entrancy protection.
  ```javascript
  showConfirmModal({
    title: 'Delete Item',
    message: `Are you sure you want to delete "${itemName}"? This action cannot be undone.`,
    okText: 'Delete Item',
    isDanger: true,
    onConfirm: async () => {
      await window.rapiderApi.deleteById('EntityName', id);
      window.rapiderApi.showNotification({ type: 'success', message: 'Item deleted successfully' });
      await loadItems();
    }
  });
  ```
- **For notifications, errors, and status feedback:**
  Always use `window.rapiderApi.showNotification({ type: 'success' | 'error' | 'warning' | 'info', message: '...' })`. Never use `alert()`.

**CRITICAL**: If you need to pass data to a detail view (e.g., an ID), you MUST send it as a `payload` object (e.g., `payload: { id: recordId }`) when calling `showPageModal`, `showPageDrawer`, or `showPageSplitter`. The child page being opened MUST define its own input shape and strictly read this data dynamically. Because the child page may be loaded in a standard iframe (`app-harness`) OR a `blob:` iframe sandbox (`rapider-ui`), `window.location.search` might be empty. You MUST attempt to read from both: `const urlParams = new URLSearchParams(window.location.search); const id = urlParams.get('id') || (window.rapiderApi ? window.rapiderApi.getRouteParam('id') : null);`. DO NOT attempt to read `window.parent.location.hash` or use any other hacky cross-origin DOM lookups, as components are rendered in isolated app-agnostic sandboxes.

## 5. Cross-Iframe Communication (Event Bus)

Because components might be rendered in separate, isolated `iframe` sandboxes, they cannot access each other's `window` objects directly. Use the built-in Event Bus to communicate.

* **`window.rapiderApi.broadcast(eventName, payload)`**: Sends data out to all other iframes.
  * *Example:* `window.rapiderApi.broadcast('UPDATE_CHART_DATA', { value: 5000 })`
* **`window.rapiderApi.on(eventName, callbackFunction)`**: Listens for incoming data.
  * *Example:* `window.rapiderApi.on('UPDATE_CHART_DATA', (payload) => { /* Mutate DOM */ })`

## 6. Custom Component Dictionary (Dynamic Fetching)

Our system exposes powerful proprietary Web Components (like `<rapider-chart>`, `<rapider-select>`, `<rapider-tooltip>`, `<rapider-phone-input>`, `<rapider-kanban>`, `<rapider-list-grid>`) that are already bundled in `rapider-components.js`. Instead of building raw HTML equivalents or hacking custom masks, you **must** use these components when appropriate.

### 📞 Phone Number Fields: `<rapider-phone-input>`
Whenever building a page with phone inputs (leads, contacts, accounts, users, settings):
- **NEVER** use plain text inputs with hardcoded single-country masks (like US-only `+{1} (000) 000-0000`).
- **ALWAYS** use `<rapider-phone-input>` (or `RapiderPhoneInput.attach(inputEl)`):
  ```html
  <rapider-phone-input id="leadPhone" default-country="US"></rapider-phone-input>
  ```
- Reading `.value` (e.g. `document.getElementById('leadPhone').value`) transparently returns the full international number with country dial code (`+1 (534) 454-5454`) or custom text.
- Includes 45+ international countries with flags and dial codes, dynamic mask formatting per country, and a "Custom" option (`🌐 Custom`) for freeform unmasked numbers.

1. **Discover Available Components:**
   Fetch `https://dev.app.rapider.ai/assets/components/components.json?t=[TIMESTAMP]` (replace `[TIMESTAMP]` with the current unix timestamp to bypass caching).
   This JSON array will list all available components (`tag`), a brief `description`, and a `docUrl`.

2. **Read Component Documentation:**
   If you find a component that fits your needs (e.g., you need to render a chart or phone input), you **MUST** fetch its detailed markdown documentation from the URL provided in `docUrl`.
   This markdown file contains the exact Inputs, Outputs, and Usage Examples for the component. You must read it *before* you write the HTML to ensure you are passing the correct data structures.

## 7. Manifest Synchronization (Mandatory)

The `app.manifest.json` file is the ultimate source of truth. Every time you generate or update a UI page, you **MUST** update the `app.manifest.json` file to explicitly link the implementation to the plan.

1. Ensure the `webPages` array exists at the root of `app.manifest.json`.
2. Add a new object to the `webPages` array with the following mapping:
```json
{
  "name": "Dashboard",
  "route": "dashboard",
  "htmlFilePath": "pages/dashboard/dashboard.html",
  "implementationPlanRoute": "dashboard"
}
```
**CRITICAL**: You MUST include the `htmlFilePath` containing the exact relative path from the repository root (e.g., `pages/dashboard/dashboard.html`). Do not rely on convention alone; the backend requires this path explicitly in the manifest.

## 8. Search, Active Filter Tags & Reset Standards

When generating list/table pages with search and filters:
1. **Search Mode**:
   - **Bulk Data (findSWR without pagination)**: Perform client-side JavaScript filtering across primary text fields synchronously without an API call.
   - **Paginated Data (with limit/skip)**: Query the backend API using debounced input with `skip: 0`.
2. **Active Filter Tags Bar (`#activeFilterTagsBar`)**:
   - Always display dismissible tag chips for active search queries (`Search: "..." ✕`) and active filters (`Status: "..." ✕`).
   - Include a one-click "Clear all" button (`#btnClearAllFilters` / `window.clearAllFilters()`) that clears all search inputs, resets filters to default ("ALL"), and restores the full original dataset.
   - Safely escape all search terms and filter labels with `escapeHtml()` to prevent XSS.
3. **Synchronous Multi-View Updates**:
   - Re-render both data tables/lists and Kanban boards immediately upon applying or clearing filters.

## 9. Pre-Designed HTML Wireframe Conversion, Record ID Routing & Detail Page Integrity (Mandatory)

When converting pre-designed `_design/*.html` wireframes into working pages (`pages/[route]/[route].html`):
1. **Strict Record ID Path Routing (`#/<route>/<uuid>`, NEVER `?id=` and NEVER `?person=<name>` or `?name=<title>`)**:
   - `rapider-ui` maps detail/edit/profile routes via path parameters (`/app/:appId/:pageKey/:entityId` and `/app-builder/:appId/preview/:pageKey/:entityId`). Every link from any list, table, card, org chart, or activity item to a record detail/edit/action page (`#/profile`, `#/edit-employee`, `#/change-employee`, etc.) **MUST** pass the record's unique database `id` as a path segment: `#/profile/${encodeURIComponent(record.id)}` or `window.rapiderApi.navigate(['profile', record.id])` (trailing secondary params like `?tab=history` go after the ID: `#/profile/${encodeURIComponent(record.id)}?tab=history`). NEVER construct `?id=` entity URLs.
   - Never route by display name (`?person=Marcus+1+Vance`).
2. **100% Dynamic Field Hydration on Detail Pages (Zero Leftover Wireframe Persona Data)**:
   - On detail/profile pages, updating only the hero title while leaving hardcoded wireframe fields (e.g., `amelia.brooks@example.com`, `EMP-0108`, `Maya Chen`, `Product & Design`, `People around Amelia`, `Help me prepare Amelia's upcoming review...`, or hardcoded `Recent history` items) is a critical bug.
   - Every single field, badge, relational card, timeline, and form input value must be dynamically hydrated from the resolved record (`record.id`) and its related entities (`departmentId`, `managerId`, `locationId`, `legalEntityId`).
3. **Detail Page Tabs Must Switch In-Page Panels (Never Navigate Away to Root App Pages)**:
   - Tabs inside a record detail page (e.g. `Overview`, `Job & employment`, `Documents`, `History & changes` inside `<nav class="tabs">`) **MUST** switch in-page tab panels (`data-tab="..."` / `[data-tab-panel="..."]`) scoped strictly to `employeeId === currentRecord.id`.
   - Never wire detail-page tabs as `<a href="#/documents">` or `<a href="#/lifecycle">` that navigate the user out of the record profile to global app pages.
4. **Real Rapider AI Assistant & Thread SDK Wiring (Zero Dead Preview Mocks)**:
   - Strip all static design-preview AI mocks (`"Design preview · AI not connected"`, `"No AI request or business action was sent"`, `"Sample data for design review"`, `data-action="reset-preview"`, and `window.PEOPLE_PAGE` wireframe blocks).
   - Wire all `Ask Rapider` buttons (`[data-action="ai"]`), inline thread forms (`[data-ai-inline]`), and AI drawer forms (`[data-ai-form]`) to `window.rapiderApi.triggerWorkflow(...)` / `window.rapiderApi.broadcast('RAPIDER_AI_ASSISTANT_QUERY', ...)` passing live page and record context (`appSlug`, `route`, `recordId`, `recordSummary`, `prompt`).
5. **Mandatory SDK Form Mutation Persistence (`<form data-save-form>`)**:
   - Every creation, edit, or change-request form (including `<form data-save-form>`) **MUST** bind its `submit` handler to `window.rapiderApi` mutation methods (`create`, `updateById`, `deleteById`, `startWorkflow`, or `triggerWorkflow`) and invalidate SWR cache upon completion — never save only to `localStorage` preview state.
6. **Multi-Script Helper Scope Integrity (`window.esc` / `window.toast`)**:
   - When a page has multiple `<script>` blocks, helper functions like `esc()` and `toast()` called in subsequent script tags **MUST** be exposed on `window` (`window.esc = esc; window.toast = toast;`) or defined locally inside each script scope to prevent runtime `ReferenceError` crashes during SWR data rendering.

## 10. 🚫 STRICT BAN on Bulk/Scripted HTML Page Generation (One-Page-at-a-Time Domain Fidelity)

1. **Zero Template Loops or Generator Scripts**:
   - You **MUST NEVER** write a script (Node.js, Python, Bash, etc.) that loops over `webPages` or multiple apps to stamp out `pages/[route]/[route].html` from a shared HTML skeleton.
   - Every single page MUST be crafted **one page at a time** in its own dedicated context after reading the app's SOW (`rapider-sow.md`), feature specs, and exact `app.manifest.json` `dataModels`.
2. **Exact Entity & Field Alignment**:
   - Always bind `window.rapiderApi` to the exact `dataModels` specified in `webPage.developmentDetails` and render real domain fields from `manifest.dataModels[i].fields` (never generic `IDENTIFIER | METADATA / CODE | STATUS` columns, never verbatim `developmentDetails` text in `<p>` subtitles, and never fake filler metrics like `COMPLIANCE RATE: 99.4%` or `AVG LATENCY: 42 ms / Sub-200ms target SLA`).
