# Pass 7: Semantic Multi-Theme Tokens & Design System Compliance
**Goal:** Verify every page imports `/assets/themes/rapider-design-system.css` (after `rapider-tailwind-theme.css`) and uses **100% semantic design tokens and semantic classes** (`primary`, `secondary`, `tertiary`, `hero`, `success`, `warning`, `danger`, `info`, `neutral`) so the entire application can switch themes per user, workspace, or tenant simply by swapping the theme CSS file — across both Light and Dark modes.

- **Mandatory Design System Stylesheet:** Every page `<head>` MUST load `<link rel="stylesheet" href="/assets/themes/rapider-tailwind-theme.css">` followed by `<link rel="stylesheet" href="/assets/themes/rapider-design-system.css">`.
- **Semantic Role Architecture (NEVER Color-Named Tokens in Page HTML/CSS/JS):**
  - **Brand & Hierarchy Roles:** `var(--primary-color)` / `bg-primary` / `text-primary` / `.btn.primary`; `var(--secondary-color)` / `bg-secondary` / `.btn.secondary`; `var(--tertiary-color)` / `bg-tertiary` / `.btn.tertiary`; `var(--hero-surface-bg)` / `.metric-card.hero`.
  - **Surfaces, Text & Borders:** `var(--body-background-color)` (`bg-body-background`), `var(--component-inner-background-color)` (`bg-component-inner`), `var(--alternative-background-color)` (`bg-alt-background`), `var(--primary-text-color)` (`text-primary-text`), `var(--secondary-text-color)` (`text-secondary-text`), `var(--eyebrow-text-color)`, `var(--primary-border-color)` (`border-primary-border`), `var(--secondary-border-color)`.
  - **Status Roles:** `.tag-primary`, `.tag-secondary`, `.tag-tertiary`, `.tag-success` (`var(--success-color)`), `.tag-warning` (`var(--warning-color)`), `.tag-danger` (`var(--danger-color)`), `.tag-info` (`var(--info-color)`), `.tag-neutral` (`var(--neutral-color)`).
  - **Strict Ban on Hue/Color Names in Pages:** Never reference color-named CSS variables (`--purple`, `--green`, `--amber`, `--red`, `--blue`), color-named classes (`.tag.green`, `.tag.amber`, `.metric-card.dark`), Tailwind hue classes (`bg-purple-600`, `bg-blue-600`, `text-indigo-600`), or raw `#hex` / `rgb()` colors on text or container backgrounds inside `<style>`, `class=""`, or JS!
- **CRITICAL TEXT CONTRAST & BUTTON INVARIANT:**
  1. Primary buttons (`.btn.primary`, `.btn-primary`, `bg-primary`) and danger buttons (`.btn.danger`, `.btn-danger`, `bg-danger`) MUST ALWAYS enforce pure white text:
     - Apply `!text-white text-white` classes.
     - Add inline style: `style="color: #ffffff !important;"`.
  2. **STRICT BAN ON WHITE TEXT ON LIGHT BACKGROUNDS:**
     - NEVER use `text-white` or `style="color: #fff;"` on neutral or light backgrounds like `bg-button-bg`, `bg-alt-background`, or `.btn`.
     - Secondary/neutral buttons MUST use `.btn` / `.btn.secondary` (`var(--secondary-button-text-color)`).
- **CSS BALANCED BRACES INVARIANT:**
  - All `<style>` tags must be structurally balanced. Every opening brace `{` must have a corresponding closing brace `}`.

**✅ BEST PRACTICE (Semantic & Theme-Swappable):**
```css
.custom-panel {
  background-color: var(--component-inner-background-color);
  color: var(--primary-text-color);
  border: 1px solid var(--primary-border-color);
}
.active-accent {
  color: var(--primary-color);
  background-color: var(--primary-soft-bg);
}
```

**❌ BAD PRACTICE (Color-Locked — Breaks Theme Switching & Dark Mode):**
```css
.custom-panel {
  background-color: #ffffff;
  color: var(--purple);
  border: 1px solid #dfd8fb;
}
```
