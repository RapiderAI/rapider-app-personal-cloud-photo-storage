---
name: rapider-design-guided-page-evolution
description: >-
  Evolve, harmonize, and upgrade Rapider microapplication HTML pages (`pages/<route>/<route>.html`)
  using pre-designed `_design/` layouts and existing page structure as a visual guide rather than
  blindly rebuilding or flattening pages into generic CRUD templates. Use whenever building, fixing,
  or QA-verifying a Rapider microapp page.
---

# Rapider Design-Guided Page Evolution & Design Language Harmonization

Your mission is **NOT** to wipe out bespoke designs and stamp out identical `4 KPI Cards + Tabs + Search + Flat CRUD Table` pages across an entire application.
Your mission **IS** to **understand each page's requirement**, **preserve and respect the `_design/` layout or existing bespoke architecture as your guide**, **harmonize the design language** (semantic multi-theme tokens, typography, spacing, dark/light polarity, Pass 21 collapsible sidebar, and shared Rapider components), and **thoughtfully elevate each page's UX and live SDK wiring** according to the SOW.

Read [references/design-token-and-component-mapping.md](references/design-token-and-component-mapping.md) whenever remapping `_design/` CSS variables or authoring 6-pillar Playwright tests on non-table pages.

---

## Stage 1 — Understand the Page Requirement First (Zero Blind Action)

Before editing or replacing any line of HTML:

1. **Read the Source-of-Truth Inputs:**
   - Target `webPage` spec (`route`, `name`, `displayOnMenu`, `developmentDetails`) in `.page-task-context.md` / `app.manifest.json`.
   - The application SOW (`rapider-sow.md` / `apps/rapider/sows/<appKey>-sow.md` / `_design/<appKey>/*.md`).
   - Exact `dataModels` (`fields`, types, foreign keys, status enums) and `workflows` in `app.manifest.json`.
   - Built sibling routes (`builtSiblings` in `.page-task-context.md`) so your page never duplicates another route's table headers or purpose.
   - Section 3.5 (`_design/` HTML/CSS guide) in `.page-task-context.md` AND the full `_design/` file on disk if referenced.
2. **Write Down (Mentally or in Plan) the Page Contract:**
   - **Primary Persona:** Who opens `#/<route>`? (e.g., Interviewer on `#/scorecard`, Recruiter/Coordinator on `#/interviews`, Hiring Lead on `#/overview`, External Candidate on `#/career-portal`, Compensation Approver on `#/offer`).
   - **Core Job-to-Be-Done:** What decision or workflow is performed here? (e.g., submitting structured criteria feedback vs. scheduling panel rounds vs. reviewing/accepting an immutable offer version).
   - **Differentiation vs. Similar Siblings:** Even when two `_design/` prototypes share a similar `.data-panel` shell (for example, `requisitions` vs. `candidates` vs. `interviews`), identify the distinct domain affordances, summary signals, filters, columns, and inline workflows that make THIS route unmistakably its own screen.

---

## Stage 2 — 3-Way Decision Gate: Preserve Guide, Elevate, Never Blindly Rebuild

Inspect the existing `pages/<route>/<route>.html` (if present) alongside the `_design/` guide in Section 3.5:

### Case A — Existing Page Already Honors the `_design/` Guide & SOW Archetype
- **DO NOT wipe and rewrite from scratch.**
- Improve incrementally in place:
  - Remap any raw hexes or color-named CSS variables (`--purple`, `--green`, `.tag.green`) to `rapider-design-system.css` semantic variables.
  - Ensure Pass 21 `<aside class="sidebar" id="suiteSidebar">` and `<main class="ds-main">` wrapper.
  - Fix spacing, Dark Mode contrast, input icon padding, `#ffffff !important` button text contrast, `<rapider-phone-input>`, `<rapider-kanban>`, `#activeFilterTagsBar` + `Clear all`, foreign-key resolution (zero raw or sliced UUIDs), live `window.rapiderApi` SWR bindings, and SOW workflow gaps.

### Case B — Page Does Not Exist Yet OR Was Previously Flattened Into a Cookie-Cutter `4-KPI + Tabs + CRUD Table` Clone
- Treat the `_design/` HTML & CSS as your **authoritative layout and composition blueprint**:
  - If `_design/` uses `.overview-top` + `.overview-grid` (Hero + 2 metric cards + `.insight` strip + split `Roles` / `Next conversations` + `Hiring flow` funnel meters), **build that exact composition** — do NOT downgrade it into a generic KPI strip + table!
  - If `_design/` uses `.form-grid` + `.sticky` (split guided composer on left, rubric checklist / offer summary / pre-publish checklist on right — e.g. `scorecard`, `offer`, `new-requisition`), **keep the guided form + sticky right rail** — do NOT turn a rubric or offer composer into a CRUD table!
  - If `_design/` uses `.profilehero` + `.detail-grid` + in-page tabs (`candidate`) or `.request-status` + `.grid.four` `.statbox` + `.detail-grid` (`requisition-detail`), **keep that 360° profile topology** with record-scoped in-page `<button type="button" data-tab="...">` panels.
  - If `_design/` uses `.quickgrid` `.quickcard` (`talent-pools`, `reports`), `.help-hero` (`career-portal`), `.kanban` / stage flow (`pipeline`), or `.toggle-row` + `.savebar` (`settings`), **keep those exact domain compositions**!
- When a route is a new SOW route with `matchType: "sibling_guide_only"` (no 1:1 file in `_design/`), use the sibling `_design/` pages' CSS/spacing grammar as your visual guide and apply `rapider-modern-saas-page-craft` to select the right SaaS archetype for its SOW purpose.

### Case C — Similar `_design/` Pages (`requisitions`, `candidates`, `interviews`, etc.)
- Preserve the calm `_design/` visual cadence (header, panel hairline, clean toolbar, scannable rows), **and** upgrade each page around its SOW requirement so they are NOT clones of each other:
  - Example (`interviews` vs `candidates` vs `requisitions`):
    - **Requisitions:** Add hiring velocity / headcount fulfillment progress (`filledCount / headcount`), compensation range formatting, department & approval status filters, `Submit for Approval` / `Publish` workflow actions, and clickable title -> `#/requisition-detail/<id>`.
    - **Candidates:** Emphasize candidate identity avatar, skills/tags pills, GDPR `consentGiven` indicator, multi-application summary without person duplication, source/stage filters, `<rapider-phone-input>` in Add/Edit modal, and clickable name -> `#/candidate/<id>`.
    - **Interviews:** Preserve `_design/interviews.html`'s dual structure (upcoming interviews table + `Schedule a conversation` form or modal/panel), add `Today / Upcoming / Needs Scorecard` coordination summary or tab strip, formatted date/time + duration + interview type + interviewer, and a direct `Scorecard` link (`#/scorecard?interviewId=...&candidateId=...`).

---

## Stage 3 — Design Language Harmonization (Unify System Without Destroying Layout)

Apply these harmonizations across every page so the whole app shares one cohesive, theme-swappable design language:

1. **Head Assets & Semantic Multi-Theme Contract:**
   - Load in `<head>` in this exact order:
     1. FontAwesome kit script
     2. `<script src="/assets/js/rapider-components.js" defer></script>`
     3. `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>` (if Tailwind utility classes are used)
     4. `<link rel="stylesheet" href="/assets/themes/rapider-tailwind-theme.css">`
     5. `<link rel="stylesheet" href="/assets/themes/rapider-design-system.css">`
   - **Remap Every Prototype Color Token:** Copy/adapt the `_design/` layout CSS rules inside `<style>`, replacing ALL raw hex colors and color-named CSS variables (`--purple`, `--green`, `--amber`, `--red`, `--blue`, `--ink`, `--muted`, `--line`, `--bg`, `--paper`, `--dark`, `#fff`, `#f5f6fa`) with `rapider-design-system.css` semantic variables (`var(--primary-color)`, `var(--primary-soft-bg)`, `var(--hero-surface-bg)`, `var(--component-inner-background-color)`, `var(--body-background-color)`, `var(--primary-text-color)`, `var(--secondary-text-color)`, `var(--primary-border-color)`, `var(--success-color)`, `var(--success-soft-bg)`, `var(--warning-color)`, `var(--warning-soft-bg)`, `var(--danger-text-color)`, `var(--danger-soft-bg)`, `var(--info-color)`, `var(--info-soft-bg)`, `var(--neutral-color)`, `var(--neutral-soft-bg)`).
   - **Dark Mode Parity:** Ensure `.dark` / `html.dark` overrides or semantic surface tokens cover every custom card (`.quickcard`, `.statbox`, `.paper`, `.flow > div`, `.lane`, `.ai-inline`, `.message`, `dialog`, `.metric-icon`, `.avatar` soft tints). Never leave hardcoded `#fff` or `#ffffff` backgrounds on cards or inputs!

2. **Outer Shell & Header Cleanup:**
   - Wrap page in `<div class="ds-shell layout @container">` with canonical Pass 21 `<aside class="sidebar" id="suiteSidebar">` (`<button type="button" class="brand" data-action="expand-sidebar">` + `.sidebar-collapse-btn` + `<script data-collapsible-sidebar>`) + `<div class="shade" data-action="close-menu"></div>` + `<main class="ds-main" id="main-content">`.
   - Strip prototype-only chrome from `_design/`: remove duplicate legacy `<aside>` / `.topbar`, fake `<span class="brandmark">r</span>`, `Sample company`, `Simple/Advanced` mode switchers, `Sample data for design review · Reset preview`, and `window.PEOPLE_PAGE` / `localStorage` preview stubs.
   - Keep `.page-head` clean: `.mobile-menu` hamburger button, uppercase `.eyebrow`, crisp `h1.page-title`, calm `.subtitle`, and domain-appropriate `.head-actions` (`Ask Rapider` + primary CTA when applicable).

3. **Standardized Shared Components & Anti-Defect Guards:**
   - **Search Inputs:** Never overlap a magnifying glass icon over placeholder text! Either put `<i class="fas fa-search">` inside a flex `.search-box` wrapper (`display: flex; align-items: center; gap: 8px;`) beside `<input type="search" style="border:0;background:transparent;width:100%;">` OR omit the inline icon / add `padding-left: 36px` with `position: relative` container.
   - **KPI Values & Contrast:** Never apply `.primary` (purple button background class!) directly to KPI count elements like `<strong class="primary">5</strong>` — use `.metric-value.primary` (which styles text color `var(--link-color)` without a solid button background). Always enforce `#ffffff !important` on `.btn.primary` and `.btn.danger`.
   - **Data Tables (When a Page Has a Table):**
     - Wrap in `.table-wrap` (`overflow-x-auto w-full`).
     - Primary identifier cell must be a clickable link/button navigating to `#/<detail-route>/${encodeURIComponent(record.id)}` (NEVER `?id=`) (with `data-record-code="..."` for workspace-agnostic E2E targeting).
     - **NO standalone View (`fa-eye`) button** in the `Actions` column! Use icon+text `Edit` (`<i class="fas fa-edit mr-1"></i><span>Edit</span>`) and icon-only `Delete` (`title="Delete" aria-label="Delete"`) — or domain action button where `_design/` uses a workflow transition like `Scorecard` or `Apply`.
     - Include `#activeFilterTagsBar` with dismissible `.filter-chip` pills + `#btnClearAllFilters` (`Clear all`) whenever search or filters are active.
   - **Zero Raw or Sliced UUIDs (`00000000-...` or `id.slice(0, 8)`):**
     - Always fetch related entities (`HrCandidate`, `HrRequisition`, `HrInterview`, `HrApplication`) via `window.rapiderApi.findSWR`, build lookup maps by `id`, and display human names/codes (`Jordan Ellis`, `REQ-2026-101`, `APP-2026-001`). Never display `Application ref: bcd29637`!
   - **Live Data & Form Persistence:**
     - Hydrate 100% from `window.rapiderApi.findSWR` / `countSWR` / `findById` (no redundant `isDeleted` in query filters; filter `r && r.isDeleted !== true` defensively in memory).
     - Use `<rapider-phone-input>` for phone numbers and `<rapider-kanban>` for Kanban stage boards.
     - Persist all forms/modals/drawers via `create` / `updateById` / `deleteById` / `startWorkflow` inside `try / catch / finally` (always restoring button text/disabled state in `finally`, clearing `swr_find_` + `swr_count_` in `sessionStorage`, updating optimistic state immediately, and using `#rapiderConfirmOkBtn` custom modal for deletions — zero native `alert`/`confirm`/`prompt`).
     - Wire `[data-action="ai"]` and inline AI threads to `window.rapiderApi` (`startWorkflow` / `broadcast`) with live page/record context and meaningful data-grounded responses.

---

## Stage 4 — Visual & E2E Verification Against the Design Guide

1. Generate/update the 6-pillar `e2e/expectations.json` test suite strictly matching real physical selectors on the page (see `references/design-token-and-component-mapping.md` for form-page, profile-page, cockpit, and card-grid E2E patterns).
2. Run `node rapider-apps/rapider-apps-development/scripts/page-builder-cli.js verify --app=<appKey> --route=<route>`.
3. Open BOTH Light (`e2e/screenshots/<route>.png`) and Dark (`e2e/screenshots/<route>-dark.png`) screenshots with `read_file` and check:
   - **Design Fidelity Check:** Compare visually against `_design/`. Does `overview` still look like an executive split cockpit with funnel bars? Does `scorecard` still look like a guided rubric form + sticky panel? Does `interviews` clearly stand out as interview coordination? Does `talent-pools` render `.quickgrid` cards?
   - **Polish Check:** Zero clipped text, zero overlapping search icons, zero unwanted purple background boxes behind KPI numbers, zero raw/sliced UUIDs, crisp Light & Dark mode contrast, full width utilization without awkward empty bands.
4. Fix any visual flaw, re-run `verify`, write `e2e/analysis/<route>.md`, and run `complete`.
