---
name: vue-saas-redesign
description: Redesign a Vue 3 application's UI into a modern SaaS-style interface with a left vertical navigation sidebar, consistent spacing, and a polished professional look. Use when asked to modernize, restyle, or redesign the app's layout or overall visual design.
---

# Vue 3 SaaS-Style UI Redesign

Transform a Vue 3 app from a top-nav layout into a modern SaaS interface: fixed left sidebar, consistent spacing scale, and polished component styling. Work incrementally — shell first, then views — and verify each stage in the browser before moving on.

## Process

1. **Audit before touching anything.** Screenshot every view in its current state. List the app shell file (usually `App.vue`), all routed views, shared components, and where global styles live. Note existing design tokens or hardcoded colors.
2. **Define design tokens first.** Add CSS variables (see palette below) to the global stylesheet before restyling any component. Every subsequent change references tokens, never raw hex values.
3. **Rebuild the shell.** Convert the top nav bar into a left sidebar (spec below). Get the shell pixel-right before touching views — it sets the grid every page lives in.
4. **Migrate views one at a time.** Apply the spacing scale and component patterns to each view, verifying in the browser after each one. Don't batch-edit all views blind.
5. **Consistency pass.** Sweep all views for stray margins, mismatched paddings, leftover top-nav styles, and non-token colors.
6. **Final verification.** Screenshot every view again at desktop and narrow widths; compare against the audit screenshots to confirm nothing regressed functionally (filters, modals, charts still work).

## Sidebar spec

- Fixed left column, `240px` wide, full viewport height, `position: fixed` or grid column; main content offset accordingly.
- Structure top-to-bottom: product name/logo block (~64px, border-bottom), nav links, then any user/profile block pinned to the bottom.
- Nav links: full-width rows, 10–12px vertical padding, 16–20px horizontal, 6–8px border-radius, subtle hover background, and a clearly distinct active state (accent-tinted background + accent text, or a 3px left accent bar). Use `router-link` with `active-class`/`exact-active-class` — don't hand-roll active detection.
- Optional icons: inline SVG, 18–20px, aligned with a fixed-width slot so labels line up.
- Below ~900px viewport width, collapse to icons-only (~64px) or an off-canvas drawer with a hamburger toggle — never let the sidebar crush content.
- Anything that lived in the old top bar (filters, profile menu, language switcher) moves to either a slim top bar inside the content area (56–64px, border-bottom) or the sidebar bottom block. Don't drop functionality.

## Layout and spacing

- App grid: `display: grid; grid-template-columns: 240px 1fr` on the shell (or fixed sidebar + `margin-left: 240px` on main).
- Content area: max-width 1200–1400px, centered or left-aligned, 24–32px padding.
- Use a fixed spacing scale — 4 / 8 / 12 / 16 / 24 / 32 / 48px — and nothing in between. Card padding 24px; gap between cards 16–24px; section gaps 32–48px.
- Page header pattern on every view: page title (20–24px, semibold) + optional description line, consistent margin below (24px) before content starts.

## Visual language

Design tokens (adjust hues to the app's existing brand if it has one):

```css
:root {
  --bg: #f8fafc;          /* app background */
  --surface: #ffffff;     /* cards, sidebar */
  --border: #e2e8f0;
  --text: #0f172a;
  --text-muted: #64748b;
  --accent: #2563eb;      /* primary actions, active nav */
  --success: #16a34a; --warning: #d97706; --danger: #dc2626;
  --radius: 8px;
  --shadow: 0 1px 3px rgb(15 23 42 / 0.06);
}
```

- Cards: `--surface` background, 1px `--border`, `--radius`, `--shadow`. Prefer borders over heavy shadows.
- Tables: no vertical rules; 1px row separators; muted uppercase 12px header row; 12–16px cell padding; hover row highlight.
- Status indicators: pill badges (tinted background + darker text of the same hue), not raw colored text.
- Buttons: primary = accent fill; secondary = surface + border; consistent 8–10px vertical padding, 6px radius.
- Typography: keep the system font stack; establish scale 12 / 13.5 / 15 / 20 / 24px; use weight and `--text-muted`, not more sizes, for hierarchy.
- Focus states: visible `outline` or ring on all interactive elements — don't remove outlines without a replacement.
- No emojis in the UI.

## Vue-specific rules

- Global tokens and shell layout live in the root component or global CSS; view-specific styles stay `scoped`.
- Restyle by editing templates/styles — do not restructure component logic, props, or data flow as part of a redesign.
- Preserve every existing behavior: filters, modals, charts, routing. A redesign that breaks a filter is a failed redesign.
- Respect repository rules that govern .vue edits (e.g. this repo requires delegating .vue file changes to the vue-expert subagent).

## Verification checklist

- [ ] Every route renders with the sidebar; active link matches the current route
- [ ] No horizontal scrollbar at 1280px or 1024px widths
- [ ] Narrow viewport (~800px) shows the collapsed/drawer sidebar, content still usable
- [ ] All modals open, position correctly over the new layout, and close
- [ ] Filters and interactive controls still round-trip to the API
- [ ] No hardcoded colors left outside the token definitions (`grep -rn "#[0-9a-fA-F]\{3,6\}" src/ --include=*.vue` minus the token file)
