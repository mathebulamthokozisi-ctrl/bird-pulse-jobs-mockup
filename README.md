# Bird Pulse — Jobs Module Mockup

A fully interactive, single-file HTML mockup of the Bird Pulse Jobs Module. **Design prototype only** — no backend, no build step, no dependencies beyond two Google Fonts CDN links.

---

## Running locally

```bash
cd job-mockup
python3 -m http.server 8080
# open http://localhost:8080
```

---

## Pages

### Jobs List
- 24 mock jobs, live search, filter sidebar, quick filter chips
- Column toggle modal, column drag-to-resize (persisted in localStorage)
- CSV export, New Job wizard, bulk actions (delete, assign, set status)
- Board / Kanban view
- **Pagination: 13 rows/page, 2 pages** (page buttons hide dynamically)

### Job Detail (click any row)
- 10-stage pipeline bar (border-radius: 6px), guard key validation
- **JOB EXECUTION zone** — amber shimmer animation (`#fef3c7` bg, `#fcd34d` border)
- Guidance banner, team sidebar, financials widget, documents section
- Tabs: Overview, Communication, Timeline, HSE
- Modals: Jobcard, Estimate, Quote, Actual Costs, Invoice, New Job, Assign Team, Columns, Unlock, Edit Job, Delete

### Dashboard
- 6 draggable widgets in a 3×2 grid (custom pointer-event DnD)
- Widgets: Profit Bridge, Revenue & Profit Trend, Job Pipeline, Job Profitability, Client Leaderboard, HSE Compliance
- KPI strip, period selector, widget library modal
- > **DnD handoff:** See `DND_HANDOFF.md` — replace with dnd-kit `rectSortingStrategy` for live proportional card-swap

---

## Design System

| Token | Value |
|---|---|
| Primary | `#4f46e5` (indigo-600) |
| Teal accent | `#00CED1` |
| Background | `#f3f5f8` |
| Surface | `#ffffff` |
| Border | `#e5e8ed` |
| Border strong | `#d0d5de` |
| Text primary | `#111827` |
| Text secondary | `#374151` |
| Text muted | `#6b7280` |
| Font | Inter (Google Fonts) |
| Border radius | `6px` (base), `6px` (widget cards), `12px` (xl) |
| Nav width | `220px` |
| Nav footer height | `41px` (aligned with pagination bar) |
| Pagination height | `41px` |
| Table page size | `13 rows` |
| Pipeline bar radius | `2px` (dashboard bars) |
| Workflow progress bar radius | `6px` (job detail) |

### Key colours

| Usage | Value |
|---|---|
| Teal gradient | `#007A7A` → `#00B4B4` → `#00CED1` |
| Exec zone bg | `#fef3c7` → `#fef9e7` (shimmer) |
| Exec zone border | `#fcd34d` |
| Amber accent | `#f59e0b` |

---

## Known issues / handoffs

- **DnD proportional swap** — `DND_HANDOFF.md`. Current DnD uses placeholder approach; live proportional card-swap requires dnd-kit `rectSortingStrategy`.

---

*Last updated: 6 Aug 2026*
