# Dashboard DnD — Handoff Note for Claude / Lovable

## What was requested

Cards should swap in **real time** as you drag — the target card should start sliding out of the way proportionally to how far the dragged card has entered it, moving in the opposite direction. Like a live push/swap, not a snap-at-threshold.

## What is currently built

A custom pointer-event based DnD using a **placeholder approach**:

1. On `pointerdown` on the drag handle, the dragged widget is lifted out of the grid (`position: fixed`) and a dashed placeholder takes its original slot.
2. On `pointermove`, `document.elementsFromPoint()` (with the dragged widget's `pointer-events` temporarily set to `none`) finds the widget under the cursor.
3. A proportional `translateX`/`translateY` push is applied to the hovered widget.
4. When the cursor crosses 50% into the target, the placeholder moves to that slot (DOM `insertBefore`).
5. On `pointerup`, the dragged widget animates back to the placeholder position, then is re-inserted into the grid.

**Implementation:** `index.html` — function `dbInitDragAndDrop()` (search for that string, ~line 8952).

## What is NOT working as intended

- The neighbouring cards do **not** slide in real time as the dragged card enters them. The grid only reorders at the 50% threshold snap — there is no proportional live movement of the surrounding cards.
- The proportional push `translateX`/`translateY` is applied only to the single directly-hovered widget. Because the placeholder is a separate DOM element (not the dragged card itself), the visual feedback doesn't feel like a true card-swap — it feels like a placeholder jumping around.
- The user's live app uses **dnd-kit**, which achieves continuous proportional live-swap behaviour out of the box.

## What the user wants (reference: dnd-kit `rectSortingStrategy`)

- As you drag card A over card B, card B should **immediately** start sliding out of the way in the opposite direction, proportional to how far card A has entered card B's bounds.
- The swap should feel continuous and fluid — by the time card A has moved 50% into card B's space, card B should have moved 50% out of the way.
- This is best achieved by **not using a placeholder at all**, and instead using `transform: translate()` on all grid items to simulate the reorder live, only committing the actual DOM reorder on drop.

## Recommended fix

Replace the current custom DnD with **[dnd-kit](https://dndkit.com/)**:

```bash
npm install @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities
```

Use `SortableContext` with `rectSortingStrategy` on the widget grid. This gives the exact proportional live-swap behaviour out of the box with zero custom collision math.

```jsx
import { DndContext, closestCenter } from '@dnd-kit/core';
import { SortableContext, rectSortingStrategy, useSortable } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';
```

Each widget becomes a `useSortable` item. The `transform` from `useSortable` is applied as a CSS transform — dnd-kit handles all the proportional live-movement of surrounding items automatically via `rectSortingStrategy`.

## Current widget order (default)

1. Profit Bridge
2. Revenue & Profit Trend
3. Job Pipeline
4. Job Profitability
5. Client Leaderboard
6. HSE Compliance

Grid layout: 3 columns × 2 rows (`grid-template-columns: repeat(3, 1fr)`), defined in `dashboard.css` on `#db-widget-grid`.
