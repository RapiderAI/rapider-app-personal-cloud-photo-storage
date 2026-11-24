---
name: rapider-modern-saas-page-craft
description: >-
  Craft modern, high-grade SaaS UI architecture (Linear, Greenhouse, Lever, Stripe, Ramp caliber)
  across Rapider microapplications. Use when choosing the right layout archetype for a page's SOW
  requirement, elevating thin wireframes, or differentiating sibling pages so every route feels
  intentional, cohesive, and unmistakable in purpose.
---

# Rapider Modern SaaS Page Craft & Archetype Selection

Great SaaS products share **one design language** (typography, semantic colors, radii, calm whitespace, navigation shell, and shared controls) while giving **each screen a distinct layout tailored to its workflow**.
Never force a Scorecard Rubric into a CRUD table, never turn a Hiring Cockpit into a bare data grid, and never leave two sibling list pages as look-alike clones.

---

## 1. The 8 Canonical SaaS Page Archetypes

Select the archetype that matches `_design/` (if present) and the route's SOW job-to-be-done:

### Archetype 1 — Executive Pulse & Action Cockpit (`overview`, `home`, `dashboard`)
- **Best for:** Morning check-in, cross-entity pipeline health, bottlenecks, and next actions.
- **Structure:**
  - `.overview-top` (`1.65fr 1fr 1fr`): Dark executive `.hero` card with primary figure, SVG sparkline/trend, and deep-link footer + 2 crisp supporting `.metric` cards.
  - `.insight-strip`: Actionable AI recommendation banner surfacing real pending bottlenecks calculated from live data.
  - `.overview-grid` (`1.75fr 1fr`): Left primary operational feed (`.listrow` items with avatars, metadata, and right-aligned counts/statuses linking directly to detail routes) + Right `.rightstack` with secondary schedule/queue list and visual `.funnel-row` stage conversion bars.

### Archetype 2 — Stage Flow & Dual-View Workbench (`pipeline`, `operations`, `board`)
- **Best for:** Moving work across lifecycle stages (`Applied -> Screening -> Interview -> Offer -> Hired`).
- **Structure:**
  - Compact stage-flow header strip (`.flow`) showing live counts per stage + conversion/aging callouts.
  - View mode switcher (`List View` + `<rapider-kanban>` `Board View`, respecting SOW accessibility "list first; Kanban optional").
  - Multi-facet filter toolbar (requisition, recruiter, stage, source, duplicate/consent signal badges) + `#activeFilterTagsBar`.
  - One-click audited stage transition modal/drawer and direct drill-down to `#/candidate/<id>`.

### Archetype 3 — Differentiated Operational Ledger (`requisitions`, `candidates`, `interviews`, `records`)
- **Best for:** High-clarity directories, schedules, and operational registers.
- **How to Prevent Look-Alike Clones Between Sibling Lists:**
  - Give each directory a **domain-authentic top summary strip or coordination focus** (only 3–4 compact cards or a specialized coordination bar — never generic boilerplate!).
  - Give each table **distinct, data-rich columns** using visual micro-components:
    - **Requisitions:** Role code & title (`primary-id-link`), Department/Location, Hiring Team (HM + Recruiter), Headcount progress bar (`filledCount / headcount`), Compensation Band, Approval/Posting Status tag, workflow trigger (`Submit for Approval` / `Publish Posting`).
    - **Candidates:** Avatar initials + Full Name (`primary-id-link`) + Current Title/Company, Skills/Tags chips (max 2–3), Multi-Application / Stage indicator, Source, GDPR Consent status tag (`Granted` / `Missing` / `Withdrawn`), Phone/Location.
    - **Interviews:** Candidate + Role, Interview Round Type (`Portfolio Review`, `Technical`, `Team`), Formatted Date & Time + Duration (`60 min`), Lead Interviewer + Panel chip, Confirmation Status tag, direct `Scorecard` CTA (`#/scorecard?interviewId=...&candidateId=...`) + inline/modal `Schedule Conversation` panel (`_design/interviews.html` style!).

### Archetype 4 — 360° Entity Profile with Record-Scoped Tabs (`candidate`, `requisition-detail`, `detail`)
- **Best for:** Deep inspection and multi-step action on a single record resolved via path route `#/<route>/<uuid>` (`getRouteParam('id')`).
- **Structure:**
  - Breadcrumb / `← Back to ...` link + `.profilehero` (`.avatar.big`, entity title, role/code subtitle, semantic `.tag-list`, primary workflow CTAs like `Move Stage`, `Schedule Interview`, `Prepare Offer`).
  - Record-scoped in-page `<nav class="ds-tabs">` using `<button type="button" data-tab="...">` (Overview, Applications/Applicants, Interviews & Scorecards, Consent & History) — **never** navigating away to root pages.
  - `.detail-grid` (`1.75fr 1fr`): Left primary `.detailfields`, `.timeline`, and `.ai-inline` contextual assistant thread; Right `.rightstack` with next scheduled milestone, screening/publishing checklist, and relational summary cards.

### Archetype 5 — Split-Pane Guided Composer / Evaluation Studio (`scorecard`, `offer`, `new-requisition`)
- **Best for:** Focused creation, structured evaluation, and high-stakes approvals where guidance matters.
- **Structure:**
  - `.form-grid` (`minmax(0, 1fr) 310px`):
    - **Left Primary Workspace:** Grouped `.panel` sections with clean 2-column `.form-fields`, structured rating selectors / competency score pills / compensation calculator inputs, and explicit form actions (`Cancel`, `Save Draft`, `Submit`).
    - **Right `.sticky` Guidance Rail:** Context-aware summary card (`Candidate`, `Version`, `Approval Path`, `Status`), calibration checklist (`.progress-step`), SOW privacy/immutability callout (`.note`), and quick preview or switch-record selector.
  - On `offer`: include a crisp `.paper` formal offer letter preview (`.paper-meta` grid + `.net-total` total compensation summary + immutable version badge + `Request Approval` / `Send Offer` / `Accept & Handoff to Employees`).

### Archetype 6 — Curated Card Grid & Branded Portal (`talent-pools`, `career-portal`, `catalog`)
- **Best for:** Visual browsing, communities, and external/self-service portals.
- **Structure:**
  - On **Portals** (`career-portal`): `.help-hero` high-contrast welcome banner (`var(--hero-surface-bg)`) + live filterable open roles showcase + candidate `Apply` modal with consent notice and duplicate detection.
  - On **Communities / Collections** (`talent-pools`): `.quickgrid` of `.quickcard` entities showing category icon, description, live member count, tag pills, lead recruiter, `View People` drill-down, `Draft Outreach` AI action, and dedicated bottom action panel (e.g., `Employee Referral Program`).

### Archetype 7 — Interactive Funnel & Cohort Analytics Studio (`reports`, `analytics`)
- **Best for:** Diagnostic metrics, conversion funnels, aging bottlenecks, and exportable insights.
- **Structure:**
  - `.quickgrid` report selector cards (`Time to Fill`, `Pipeline Conversion`, `Source Quality`, `Interview Experience`, `Offers & Acceptance`, `Recruiter Workload`) that dynamically switch the active visualization below!
  - Visualization stage: Pure-CSS / SVG bar charts (`.chart` / `.barcol`), conversion funnel meters (`.funnel-row`), source breakdown donut/legend, period filter (`Last 6 months`, `Last 3 months`, `This month`), CSV export, and SOW compliance banner (explicitly excluding private interviewer scorecard notes).

### Archetype 8 — Policy & Governance Control Center (`settings`, `policies`)
- **Best for:** Workflow toggles, approval rules, retention windows, stage configuration, and integrations.
- **Structure:**
  - `.grid.two` pairing **Workflow & Compliance Toggles** (`.toggle-row` switches for Requisition Approval, Private Scorecard Policy, GDPR Consent & Retention, Stage Reminders) with **Connected Workspace & RBAC** cards (`.connection-card`).
  - `.savebar` persisting rules cleanly + `.quickgrid` `.rule-card` tiles (`Pipeline Stages`, `Message Templates`, `Consent Notice`, `Posting Channels`, `Retention Rules`) + Locale/Display preferences panel.

---

## 2. Modern High-Grade Visual Polish Checklist

1. **Typographic Restraint & Tabular Numerals:** Editorial `30px` `h1.page-title` (`-0.9px` tracking), `11px` uppercase `.eyebrow`, `font-variant-numeric: tabular-nums` on every currency, count, and time metric.
2. **Calm Hairline Depth:** `1px solid var(--primary-border-color)` borders with `border-radius: 14px` and zero heavy drop shadows; reserve shadows for floating `<dialog>` modals and drawers.
3. **Zero Visual Clutter:**
   - Max 2–3 badges/tags per table row or card (`5px` radius `.tag`, never loud `rounded-full` pills).
   - Never color KPI numbers with `.primary` button backgrounds — text tone only (`.metric-value.primary`).
   - Never overlap search icons over input placeholders.
   - Never display raw or sliced UUIDs (`00000000-...`, `bcd29637`); always resolve human names and domain codes (`REQ-2026-101`, `Jordan Ellis`).
4. **Thoughtful Micro-Interactions:** Smooth tab switching, immediate optimistic updates on create/edit/delete/toggle, active filter chips with `Clear all`, clear empty states when `0` records match, and contextual `Ask Rapider` prompts tailored to the exact page route.
