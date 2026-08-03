# Bird Pulse — Jobs Module Mockup

A fully interactive, single-file HTML mockup of the Bird Pulse Jobs Module. This is a **design prototype only** — no backend, no build step, no dependencies beyond two Google Fonts CDN links. Open `index.html` in any browser or serve it with any static file server.

---

## What is in the mockup

### Jobs List page
- 24 mock job records with full data (client, site, description, type, priority, status, finance stage, team, value, date)
- Live search across job number, client, site, and description
- Filter sidebar with workflow status, priority, client, and assignee checkboxes
- Quick filter chips: My jobs, Unassigned, Active only, This week, Overdue
- Column toggle modal — show/hide any column, locked columns cannot be hidden
- Column drag-to-resize — widths persist in `localStorage`
- CSV export of all filtered jobs
- New Job wizard modal — full form with client, site, type, nature, description, priority, dates, reference, and team
- Row checkboxes with bulk actions: delete, assign, set status, export selected
- Board / Kanban view grouped by workflow status
- Pagination (12 per page, 3 pages)

### Job Detail page (click any row)
- Breadcrumb navigation back to list
- Auto-transitions toggle (ON/OFF)
- 10-stage pipeline bar with guard key validation
- Guard keys bar: Estimate Finalized, Quote Ready, Client Contact, Quote Approved, Jobcard Signed, Invoice Ready
- Guidance banner with context-aware next-action prompt
- Team sidebar with edit-in-place role assignment
- Financials widget: estimated P/L, VAT summary, live margin calculation
- Documents section: Estimate, Quote, Invoice, Actual Costs, Jobcard, HSE Pack

#### Tabs
- **Overview** — 4 KPI metric cards, bar chart by cost category (estimated vs actual), variance table
- **Communication** — 3 client portal threads, expand/collapse, reply flow, mark resolved, new message modal
- **Timeline** — 6 activity items with expandable full email bodies
- **HSE** — 6-stage HSE pipeline, client master file inheritance banner, inline-editable risk assessment table (L × C auto-calculates rating with colour coding)

#### Modals
- **Jobcard** — 4 photo slots (click to add), work notes textarea, signature capture (click to sign → updates Jobcard Signed guard key), Export PDF, Save
- **Estimate** — 12 line items, inline editing, live totals, Save & Finalise (updates P/L widget, sets Estimate Finalized guard key)
- **Quote** — view, re-send, accept (advances pipeline), reject
- **Actual Costs** — add/remove entries, live total, Save (updates P/L widget, unlocks Invoice Ready guard key)
- **Invoice** — manual creation with date, due date, and amount fields
- **New Job** — full creation wizard
- **Assign Team** — pick team member per role
- **Columns** — toggle column visibility
- **Unlock** — override guard keys with a reason (logged to timeline)
- **Edit Job** — edit client, site, type, nature, priority, dates, description
- **Delete Confirm** — confirmation dialog

---

## Running locally

```bash
cd job-mockup
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## Git workflow

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full commit convention, branching strategy, and undo/redo process.

---

## Design system

| Token | Value |
|---|---|
| Primary | `#4f46e5` (indigo-600) |
| Background | `#f1f5f9` (slate-100) |
| Surface | `#ffffff` |
| Border | `#e2e8f0` (slate-200) |
| Text primary | `#0f172a` (slate-900) |
| Text secondary | `#334155` (slate-700) |
| Text muted | `#64748b` (slate-500) |
| Font | Inter (Google Fonts) |
| Icons | Material Symbols Outlined (Google Fonts) |
| Border radius | `6px` |

---

*Last updated: 3 Aug 2026*
