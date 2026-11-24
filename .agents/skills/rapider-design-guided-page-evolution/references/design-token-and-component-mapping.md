# Design Token Remapping & Non-Table 6-Pillar E2E Patterns

## 1. Remapping `_design/` Prototype CSS to Semantic `rapider-design-system.css` Tokens

When adapting custom layout CSS from `_design/` (`overview-top`, `overview-grid`, `form-grid`, `profilehero`, `quickgrid`, `help-hero`, `funnel-row`, `paper`, `toggle-row`, `savebar`, `kanban`, `lane`), **keep the layout geometry** and **replace every hardcoded color or color-named variable** so both Light & Dark modes and theme switching work cleanly and Pass 07 / Pass 19 checks pass with zero warnings:

| Legacy `_design/` Token / Raw Color | Semantic Replacement (`rapider-design-system.css`) |
| :--- | :--- |
| `--ink`, `#172137`, `#20283d` | `var(--primary-text-color)` |
| `--muted`, `#758097`, `#6e7890` | `var(--secondary-text-color)` |
| `#8b93a6`, `#9ba3b2`, `#939aac` | `var(--muted-text-color)` |
| `--line`, `#e9ecf3`, `#e8eaf1` | `var(--primary-border-color)` |
| `#dfe3ed`, `#e0e3ed` | `var(--secondary-border-color)` |
| `--bg`, `--page`, `#f5f6fa`, `#f6f7fb` | `var(--body-background-color)` |
| `--paper`, `#fff`, `#ffffff` (cards/surfaces) | `var(--component-inner-background-color)` |
| `#fafbfe`, `#fbfcfe`, `#fcfcfe` | `var(--alternative-background-color)` |
| `#f8f9fc`, `#f7f8fc`, `#f1f3f7` | `var(--subtle-background-color)` |
| `--purple`, `#6854df`, `#6b57d7` | `var(--primary-color)` (or `var(--link-color)` for text/links) |
| `--soft`, `#eeeaff`, `#f0ecfb`, `#ede9fb` | `var(--primary-soft-bg)` |
| `#e8e0f3`, `#ebe6f8`, `#dfd8fb` | `var(--primary-soft-border)` |
| `#473b68`, `#53389e`, `#6650ce` | `var(--primary-on-soft)` |
| `--dark`, `--night`, `#252840`, `#282a44` | `var(--hero-surface-bg)` (with `var(--hero-surface-text)` and `var(--hero-surface-muted)`) |
| `--green`, `#26836b` / `#e7f3ec` | `var(--success-color)` / `var(--success-soft-bg)` + `.tag.tag-success` |
| `--amber`, `--orange`, `#aa7621` / `#fff1e2` | `var(--warning-color)` / `var(--warning-soft-bg)` + `.tag.tag-warning` |
| `--red`, `#b25164` / `#faeaf0` | `var(--danger-text-color)` / `var(--danger-soft-bg)` + `.tag.tag-danger` |
| `--blue`, `#3b6ea8` / `#e9f1fb` | `var(--info-color)` / `var(--info-soft-bg)` + `.tag.tag-info` |
| `--gray-tag-text`, `#7c879a` / `#f1f3f7` | `var(--neutral-color)` / `var(--neutral-soft-bg)` + `.tag.tag-neutral` |

### Dark-Mode Guardrails for Custom `_design/` Blocks
- **KPI Metric Cards & Hero Cards:**
  - `.metric-card.hero` always uses `background: var(--hero-surface-bg); color: var(--hero-surface-text);` (stays rich slate in both Light and Dark mode, with `.kicker` / `.hero-note` using `var(--hero-surface-muted)`).
  - Never put `.primary` (button background class) on a metric number! Use `.metric-value.primary` or `color: var(--link-color)`.
- **Avatars & Mini-Icons (`.avatar`, `.mini-icon`, `.metric-icon`):**
  - Use semantic soft tints (`background: var(--primary-soft-bg); color: var(--link-color);`, `var(--info-soft-bg)` / `var(--info-color)`, `var(--success-soft-bg)` / `var(--success-color)`, `var(--warning-soft-bg)` / `var(--warning-color)`, `var(--danger-soft-bg)` / `var(--danger-text-color)`) so they glow subtly in Dark Mode instead of looking washed out or bright white.
- **Native Inputs, Selects & Textareas:**
  - Always inherit `background: var(--input-background-color); color: var(--primary-text-color); border-color: var(--secondary-border-color);`.
- **Search Input Icon Alignment (Zero Overlap Rule):**
  - Use a flex wrapper:
    ```html
    <div class="ds-search-box" style="display:flex;align-items:center;gap:8px;padding:0 12px;min-height:41px;border:1px solid var(--secondary-border-color);border-radius:var(--radius-sm);background:var(--input-background-color);">
      <i class="fas fa-search" style="color:var(--muted-text-color);flex-shrink:0;" aria-hidden="true"></i>
      <input id="searchInput" type="search" placeholder="Search..." style="border:0;background:transparent;outline:none;width:100%;color:var(--primary-text-color);font-size:13px;">
    </div>
    ```
    Never place an icon with `position: absolute; left: 10px` over an `<input>` without `padding-left: 36px`!
- **Modals & Native `<dialog>` Centering:**
  - Every `<dialog>` CSS rule MUST include `margin: auto;` (Pass 04 / Tailwind v4 preflight resets `* { margin: 0 }`).
  - Always guard `.field input:not([type="checkbox"]):not([type="radio"])` so checkboxes and toggle switches inside `.toggle-row` / `.field` stay `16px x 16px` or `36px x 20px` instead of stretching to `100%`.

---

## 2. Shared Component Upgrades (Apply Only Where Appropriate)

1. **Tables (Only on List/Register Pages or Ledger Tabs):**
   - Wrap in `.table-wrap` (`overflow-x-auto w-full`).
   - Primary identifier cell uses `<a class="primary-id-link" href="#/<detail-route>/${encodeURIComponent(rec.id)}" data-record-code="${esc(rec.code)}">...</a>`.
   - Actions column: **NO standalone View (`fa-eye`) button.** Use icon+text Edit (`<button type="button" class="btn sm" data-action="edit" data-id="${esc(rec.id)}"><i class="fas fa-edit mr-1"></i><span>Edit</span></button>`) and icon-only Delete (`<button type="button" class="btn sm icon-only danger-ghost" data-action="delete" data-id="${esc(rec.id)}" title="Delete" aria-label="Delete"><i class="fas fa-trash-alt"></i></button>`). On specialized workflow tables (e.g., `_design` `interviews` or `career-portal`), preserve the primary workflow button (`Scorecard` or `Apply`) alongside Edit/Delete if CRUD is needed.
   - Include `#activeFilterTagsBar` with dismissible chips and `#btnClearAllFilters` (`Clear all`) whenever search/filters exist.
2. **Foreign-Key Hydration (Zero Raw or Sliced UUIDs):**
   - Fetch related collections in `Promise.all([...])` using `window.rapiderApi.findSWR(...)`.
   - Map `candidateById[r.candidateId]`, `reqById[r.requisitionId]`, `appById[r.applicationId]`, `interviewById[r.interviewId]`.
   - Resolve human names (`Jordan Ellis`, `Senior Product Designer`, `REQ-2026-101`, `APP-2026-004`). Never render `.slice(0, 8)` of an ID as an application/reference label!
3. **Kanban Boards:** Use `<rapider-kanban>` (with table/list view toggle when SOW states "accessible list first; Kanban optional").
4. **Phone Fields:** Always use `<rapider-phone-input id="..." default-country="US"></rapider-phone-input>`.
5. **Confirmations & Toasts:** Always use custom `#rapiderConfirmOkBtn` modal / `window.showConfirmModal` and `window.rapiderApi.showNotification` (zero native `alert`, `confirm`, `prompt`).

---

## 3. Satisfying 6-Pillar Playwright E2E Naturally on Non-Table Pages

Do **NOT** add fake CRUD tables just to write `e2e/expectations.json`. Match the 6 pillars to each archetype's real DOM:

| Page Archetype | Pillar 1 (`click >= 2`) | Pillar 2 (Read `assert_api_payload`) | Pillar 3 (Navigation) | Pillar 5 (Form & Mutation `expectedPayload`) |
| :--- | :--- | :--- | :--- | :--- |
| **Executive Cockpit (`overview`)** | Click `Ask Rapider` (`#btnAskRapider`), close drawer, click `New Requisition` or quick-action tab/link | `findSWR` on primary KPI models (`HrRequisition`, `HrApplication`, `HrInterview`, `HrOffer`) | Click a role/candidate item in `.listrow` -> assert `requisition-detail/` or `candidate/` | Submit `Ask Rapider` prompt (`startWorkflow`/`create`) or quick intake modal with `expectedPayload` |
| **Guided Form / Studio (`scorecard`, `offer`, `new-requisition`)** | Click rating/criteria selects or `Preview Offer` / `Ask Rapider`, then click Submit button | `findSWR` / `findById` loading candidate/interview/requisition context | Click `Cancel` / back link or submit redirect / `category: "navigation"` nav check | Fill the real form fields (`type` + `select`), click submit, assert `create` or `updateById` with exact `expectedPayload` |
| **360° Entity Profile (`candidate`, `requisition-detail`)** | Click each in-page tab (`[data-tab="..."]`), click `Ask Rapider` / `Schedule` / `Move Stage` modal button | `findById` / `findSWR` filtered or resolved for the active `/:id` (`getRouteParam('id')`) record | Click back link (`Back to candidates`) or tab switches + `assert_route` stays on `#/candidate/` | Submit inline AI thread (`[data-ai-inline]`) or `Move Stage` / `Schedule` modal mutation with `expectedPayload` |
| **Card Grid / Portal (`talent-pools`, `career-portal`, `reports`)** | Click filter/search/report card (`[data-report="..."]`), click `Apply` / `New Talent Pool` / `Ask Rapider` | `findSWR` on `HrTalentPool` / `HrJobPosting` / `HrApplication` | Click `View people` / `Apply` / `View posting` navigation or `category: "navigation"` | Fill `New Talent Pool` or `Apply for Role` modal form, submit `create` with `expectedPayload` |
| **Settings & Governance (`settings`)** | Toggle `.toggle-row input[type="checkbox"]`, click rule configuration card / `Ask Rapider` | `findSWR` on config/policy/stage models | Click nav / section card or back link with `category: "navigation"` | Update preference / display form inputs, click `Save Preferences`, assert `create`/`updateById`/`startWorkflow` with `expectedPayload` |

Always target rows/cards by `[data-record-code="..."]` or text (`tr:has-text("...")`) rather than seed UUIDs (`00000000-...`) because `populate-sample-data` assigns workspace-scoped UUIDs at runtime.
