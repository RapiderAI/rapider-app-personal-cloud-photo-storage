# Pass 22: Domain Fidelity, Spec Alignment & Anti-Boilerplate Standard

**Goal:** Guarantee that every single HTML page (`pages/[route]/[route].html`) is custom-authored one-by-one from the application's Statement of Work (`rapider-sow.md` / `_design/<appKey>/sow.md`) and its specific `webPages` + `dataModels` schema in `app.manifest.json`, with **ZERO bulk-generated boilerplate templates, ZERO generic KPI placeholders, and ZERO cross-page or cross-app layout cloning**.

## 1. Strict Ban on Bulk / Scripted HTML Page Generation
- **NEVER** write Node/Python/Bash generator scripts or template loops that stamp out `pages/[route]/[route].html` across multiple pages or multiple applications.
- Every page MUST be authored **one page at a time** in an isolated build pass after reading:
  1. The app's SOW (`rapider-sow.md` / `_design/<appKey>/sow.md` / `apps/combined-features/*.md`),
  2. The target `webPage` object (`name`, `route`, `group`, `developmentDetails`) in `app.manifest.json`,
  3. The exact `dataModels` (`fields`, `types`, `relations`) referenced by that page.

## 2. Forbidden Boilerplate & Generic Placeholder Signatures
- **NEVER** render generic devops/template filler KPIs on business pages unless explicitly mandated by the domain SOW:
  - Forbidden strings: `Sub-200ms target SLA`, `AVG LATENCY`, `42 ms`, `COMPLIANCE RATE: 99.4%`, `Audit trail verified`, `Passing policy checks`, `Live SWR synchronized`, `Enterprise Primary Record`, `Enterprise Operations Record`, `Enterprise Compliance Record`, `METADATA / CODE`, `configuration (1)`.
- **NEVER** dump the raw `webPage.developmentDetails` specification prompt string verbatim into visible `<p>` subtitles or page descriptions. Translate technical requirements into authentic domain UI labels, KPIs, columns, filters, and workflows.

## 3. Canonical Data Model & Real Schema Field Alignment
- If `webPage.developmentDetails` names specific canonical entities (e.g., `EduStudentProfile` on `student-directory`, `MedLabOrder` on `orders-accessioning`, `HospRatePlan` / `HospDailyInventory` on hotel yield pages), the page **MUST** query and mutate those exact `dataModels` via `window.rapiderApi` — never bind a randomly chosen model from `manifest.dataModels` (such as binding `EduGuardianLink` on `Student 360 Directory`).
- Every data-bound page MUST hydrate and render **at least 3 real domain-specific fields** defined on its target `dataModels` in `app.manifest.json` (for example `studentNumber`, `firstName`, `lastName`, `gradeLevel`, `cumulativeGpa`, `ferpaRestricted`, `enrollmentStatus` — never generic `title`, `code`, `status` columns that don't match the entity).
- If `webPage.developmentDetails` specifies `<rapider-kanban>` or `<rapider-phone-input>`, those exact Rapider custom elements MUST be present and wired.

## 4. Authentic Domain Seed Data & Non-Colliding ID Standard (`sample-data.json`)
- `sample-data.json` MUST contain realistic, human-authored domain records matching every field type and enum of `manifest.dataModels` (real person/organization names, domain codes, realistic dates, amounts, and foreign-key UUIDs).
- **NEVER** populate `sample-data.json` with synthetic string-concatenated placeholders like `"Guardian Link relationshipType configuration (1)"` or `"Enterprise Primary Record"`.
- **Zeroed-Out Prefix with Truly Random Remaining Digits (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx`):**
  - All sample record IDs MUST start with `00000000-`.
  - The remaining 24 hex digits **MUST BE TRULY RANDOM** (e.g. `00000000-74c6-417b-b141-468b9dbf3d5e`).
  - **STRICTLY BANNED:** Sequential or all-zero chunk patterns (e.g. `00000000-0000-0000-0000-000000000001`, `00000000-0001-4000-a000-000000000001`, `00000000-1111-4111-8111-111111111111`). Such IDs create duplicate key collisions when multiple models or apps seed into the database. Generate with `'00000000-' + crypto.randomUUID().slice(9)`.

## 5. Distinct Purpose-Built Page Layouts Within an App
- Sibling pages within the same application MUST NOT share identical `<th>` table column headers when bound to different domain workflows or entities. Dashboards need executive domain KPIs, charts/distributions, and action queues; directories need domain multi-facet filters and rich columns; workbenches/calendars/kanbans need purpose-built interactive views; detail pages need 360° relational tabs.

## 6. Design-Guide Preservation, Anti-Blind-Rebuild & Requirement-Driven Sibling Differentiation
- **Non-Table `_design/` Archetype Preservation (STRICT):** When `_design/<appKey>/` provides a matching page design whose layout archetype is NOT a standard CRUD table (e.g., executive overview split `.overview-top` + `.overview-grid` + `.funnel-row`, structured evaluation/scorecard `.form-grid` + `.sticky` summary, document/offer builder `.paper` + `.paper-meta` + `.net-total`, career portal, calendar/interview workbench, kanban board, settings console, analytics breakdown), NEVER flatten or convert that route into a generic `4 KPI Cards + Tabs + Search + Table` clone. Evolve the page using the pre-designed `<main>` layout archetype while harmonizing typography, spacing, components, and semantic light/dark design tokens.
- **Anti-Blind-Rebuild Gate:** Before editing an existing `pages/{route}/{route}.html`, diagnose its current state against the SOW and `_design/` guide:
  1. If the existing page is ALREADY bespoke, rich, and aligned with `_design/` + SOW, refine and fix defects surgical-first (tokens, contrast, SWR caching, table actions, validation) instead of wiping and rebuilding from scratch.
  2. If the existing page was previously flattened into a generic clone that deviates from `_design/` or misses key SOW requirements, restore the `_design/` layout archetype and upgrade it with full live CRUD/SWR workflows.
- **Requirement-Driven Differentiation of Similar `_design/` Wireframes:** Even when two pre-designed `_design/` files share similar table/list bones (for example `candidates` vs. `requisitions` vs. `interviews`), do NOT ship identical clones. Read the page's SOW responsibilities and craft genuinely purpose-built domain UX for each page (e.g., `interviews`: upcoming interview schedule strip / conflict awareness + panelist/duration/link details + quick feedback/scorecard launch + schedule panel; `candidates`: rich talent directory with skills tags, match/rating, consent badges, source, talent pool filter; `requisitions`: open headcount fill-progress bars, hiring manager/recruiter ownership, salary band & days-open urgency).
- **Zero Raw or Sliced UUID Leaks in Visible UI (STRICT):** Never display raw UUIDs or sliced UUID substrings (e.g. `Application ref: bcd29637`, `#00000000-...`, `candidateId.slice(0, 8)`) as human-facing labels or subtitle captions. Always resolve and render human-readable business numbers (`REQ-104`, `CAN-042`), entity titles, candidate names, or contextual timestamps instead.

