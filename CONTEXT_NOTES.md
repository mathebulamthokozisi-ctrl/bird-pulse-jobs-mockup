# Bird Pulse Jobs Mockup — Context Notes
_Last updated: 2026-08-04_

## Key Files
- `/home/ubuntu/job-mockup/index.html` — single-file mockup (all HTML/CSS/JS)
- Live URL: https://8080-ii56y9231pksds6gvu8sv-3c2d8d14.us2.manus.computer/
- GitHub: https://github.com/mathebulamthokozisi-ctrl/bird-pulse-jobs-mockup.git (main)
- Latest commit: `6f90447` — filter sidebar background → pure white

## Design System Tokens
- `--t-fast: 100ms ease` | `--t-base: 150ms ease`
- `--turquoise-600: #00CED1` | `--turquoise-700: #00B5B8` | `--turquoise-800: #008B8B`
- `--indigo-600: #4f46e5` | `--red-600: #dc2626`
- `--radius: 6px` | `--radius-lg: 8px` | `--radius-xl: 12px`
- `--shadow-md: 0 2px 6px rgba(0,0,0,.06)` | `--shadow-lg: 0 4px 16px rgba(0,0,0,.08)`

## Current Interaction State (before micro-interaction pass)
- Table rows: `transition:background var(--t-fast)` only, hover → `#f0f4ff`
- Buttons: `transition:background/border-color/color var(--t-fast)`, `:active` → `translateY(1px)`
- Modal overlay: `.modal-overlay.open{display:flex}` — hard cut, no animation
- Editor open: `.doc-editor.open` — hard cut, no animation
- openDetail: `style.display='none'/'flex'` — hard cut, no animation
- showList: `style.display='flex'/'none'` — hard cut, no animation
- Pipeline bar fill: `transition:width .5s cubic-bezier(.4,0,.2,1)` ✓ already smooth
- Pipe shimmer: `@keyframes pipe-shimmer 2.2s linear infinite` ✓ already animated
- Pipe pulse dot: `@keyframes pipe-pulse-anim 1.6s ease-in-out infinite` ✓ already animated
- Toast: `@keyframes toast-in` — scale+fade ✓ already animated
- Metric cards: `transition:box-shadow/transform var(--t-base)`, hover → `translateY(-1px)` ✓

## Key JS Functions
- `openDetail(num)` — line 4734: flips page-list/page-detail with style.display
- `showList()` — line 4434: flips back to list
- `openModal(id)` — line 4406: adds `.open` class to modal overlay
- `closeModal(id)` — line 4407: removes `.open` class
- `openEditor(id)` — line 5344: adds `.open` to `.doc-editor`
- `closeEditor(id)` — line 5348: removes `.open`
- `openDetail` calls pipeline/guard rendering at lines 4776-4892

## Micro-Interaction Plan (current task)
Add these interactions WITHOUT breaking existing design:
1. Table rows: left-border accent on hover (turquoise 2px), smoother transition
2. Buttons: `scale(0.97)` on `:active` (replace translateY)
3. Nav items: icon translate-x 1px on hover
4. Doc-rows: bg tint on hover + action button opacity reveal
5. Filter items: smoother colour transitions
6. Modal overlay: fade-in backdrop + panel scale 0.97→1 entrance
7. Editor open: slide-up + fade entrance
8. Detail page: fade+slide-up on open
9. Guard keys: check pop animation
10. Search bar: smooth focus expand (already has box-shadow transition)
11. Sidebar sections: staggered fade-in on detail open

## Doc-Row Grid Layout (last fix)
```css
.doc-row {
  display: grid;
  grid-template-columns: 28px 1fr 52px auto;
  column-gap: 8px;
}
```
HTML: `.doc-icon` | `.doc-info` (.doc-name + .doc-meta) | `.doc-tag-slot` | `.doc-action`

## Filter Sidebar
- Background: `var(--white)` (was `var(--surface-2)`) — fixed in commit 6f90447
