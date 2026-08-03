# Conversation Notes — Bird Pulse Jobs Module UI Mockup

> **Purpose:** This file is the single source of truth for all agreed rules, design decisions, and key discussion points from our design sessions. It must be read at the start of every session to prevent drift. Update it after every meaningful decision.

---

## 1. The Golden Rules (Non-Negotiable)

### 1.1 Backend-First Design
**Every feature in the mockup must be grounded in what actually exists in the production backend (`client-centric-stats` repo).** This is the most important rule.

- If a field, status, relation, or feature does not exist in the backend, it must NOT appear in the mockup.
- If something exists in the backend but is not yet surfaced in the UI, you MAY choose to surface it — but you must note it clearly (see Rule 1.2).
- If you want to introduce a genuinely new feature (not in the backend), you MUST annotate it with a comment block so Claude and Lovable know it is a newly suggested addition requiring backend work.

**Annotation format for new suggestions:**
```html
<!-- [NEW SUGGESTION] Feature: <name>
     Reason: <why this adds value based on code analysis>
     Backend work needed: <what table/RPC/hook would be required>
     Not yet in production. -->
```

### 1.2 Surfacing Hidden Backend Data
Some backend data exists but is not yet surfaced in the UI. This is encouraged — it gives users more context. Examples found in the audit:
- `job_nature` (`done_already` / `to_be_done`) — exists on the job record, shown in production detail page breadcrumb as a toggle badge
- `locked_at` — job lock state, shown as a "Locked" badge in production
- `site_contact_name` / `site_contact_phone` — on the site/property record
- `commission_paid` status — final workflow stage, exists in backend
- `quote_declined` / `cancelled` — exist as real statuses, must appear in board and filter

### 1.3 No Functionality Regression
**Never remove or break a feature that was previously working.** If a change causes a regression, it must be fixed before moving on. This rule was established after the "build all features" session caused dead interactions.

### 1.4 Design Consistency with Production CRM
The mockup must use the same design language as the production CRM (`client-centric-stats`). Key tokens:
- **Primary accent:** Indigo (`#6366f1` / `indigo-600`) — used for action buttons, active states, filter chips
- **Background:** Canvas is grey (`#f0f2f5`); all surfaces (sidebar, toolbar, panels, modals) are **white**
- **Table rows:** White / near-white (`#fafafa`) alternating — no visible grey zebra
- **Badges/Pills:** 1px coloured border, uppercase text, 10px, weight 600, letter-spaced — matching `DynamicStatusBadge` in production
- **Sidebar:** White background, indigo active state
- **Font:** Inter (same as production)
- **Icons:** Material Symbols (same as production)

### 1.5 Left-Align Everything in Tables
All table headers and cell content must be left-aligned — matching the production CRM invoice/jobs table style.

### 1.6 Filters Work by Inclusion, Not Exclusion
Filter checkboxes start **unchecked** (show all). Ticking an item means "show only this". Multiple ticked items in the same group are OR'd. Multiple groups are AND'd.

---

## 2. Agreed Design Decisions

### 2.1 Colour — White Surfaces
- Everything is white: sidebar, toolbar, filter panel, detail header, modals, cards, guard key bar, tabs bar.
- Grey (`#f0f2f5`) is reserved for the canvas/page background only.
- Table: white rows with `#fafafa` on alternating rows (barely perceptible, just enough for row tracking).

### 2.2 Table
- Columns: JOB #, CLIENT / SITE, DESCRIPTION, TYPE, PRIORITY, STATUS, FINANCE, TEAM, VALUE, DATE
- **Type column** is present in the table but the user has said they don't care much about it — it can stay but should not be prominent.
- **Type label is NOT shown on board cards** (user explicitly removed it — see session Aug 2026).
- All headers and cells: left-aligned.
- No centre-aligned or right-aligned columns.

### 2.3 Board View
- Exact 7 columns from production `JobsKanbanBoard.tsx`:
  1. **Booked** — `job_booked`, `estimate_done`
  2. **Quoting** — `quote_drafted`, `quote_sent`, `quote_finalized`
  3. **Accepted** — `quote_accepted`
  4. **In Progress** — `on_my_way`, `work_in_progress`, `job_in_progress`
  5. **Completed** — `job_completed`, `work_client_approved`
  6. **Invoiced** — `invoiced`, `paid`, `commission_paid`
  7. **Declined / Cancelled** — `quote_declined`, `cancelled`
- Column headers: coloured top-border accent line + drop shadow (user explicitly requested the shadow lines).
- Cards show: Job #, Date, Client, Site, Description, Priority pill, Finance pill, Team avatars, Value. **No Type label on cards.**

### 2.4 Pipeline Bar (Detail Page)
- Uses the exact 5-stage grouping from production `JobProgressPipeline.tsx`:
  1. **Estimate** — `job_booked`, `estimate_done`
  2. **Quote** — `quote_drafted`, `quote_sent`, `quote_finalized`, `quote_accepted`, `quote_declined`
  3. **Work** — `on_my_way`, `work_in_progress`, `job_in_progress`, `job_completed`, `work_client_approved`
  4. **Invoice** — `invoiced`
  5. **Paid** — `paid`, `commission_paid`
- Each stage shows a sub-status label (e.g. "Sent", "Approved", "En Route", "In Progress", "Completed").

### 2.5 Guard Keys (Detail Page)
- Exact 7 keys from production `useWorkflowGuardKeys.ts` / `GUARD_KEY_LABELS`:
  1. `estimate_finalized` — Estimate Finalized
  2. `quote_ready` — Quote Ready
  3. `client_contact_present` — Client Contact
  4. `quote_approval_present` — Quote Approved
  5. `invoice_ready` — Invoice Ready
  6. `payment_recorded` — Payment Recorded
  7. `commission_enabled` — Commission Enabled
- NOT invented keys like "Jobcard Signed" (that is not a guard key in the backend — it is a jobcard field).

### 2.6 Job Types (from `JOB_TYPE_LABELS` in production)
Real values: `repair`, `maintenance`, `install` (→ "Installation"), `other`, `reinstatement`, `new_installation` (→ "New Installation"), `new_build`, `supply_and_deliver` (→ "Supply & Deliver").
- **NOT** "Electrical", "Plumbing", "HVAC" etc. — those are descriptions, not job types.

### 2.7 Assignment Roles (from `JobAssignmentPanel.tsx`)
Real roles: **Sales**, **Technical**, **Coordinator**, **Job Owner** (Job Owner must be one of Technical or Coordinator).
- **NOT** invented roles like "Lead Tech", "Site Manager", "PM".

### 2.8 Job Nature (`job_nature`)
Real values: `done_already` (→ "Done Already" — amber badge) and `to_be_done` (→ "To Be Done" — green badge).
- Shown as a toggleable badge in the detail page breadcrumb.
- This is surfaced backend data — it exists on the job record.

### 2.9 Status Guidance Banner (Detail Page)
Exact messages from production `STATUS_GUIDANCE` map in `JobNextActions.tsx` / `JobWorkflowDetail.tsx`. Each status has a specific icon, message, and CTA module.

### 2.10 Communication Tab
- In production, the Communication tab shows **Client Portal actions** (`client_portal_actions` table): quote approvals, revision requests, jobcard sign-offs/refusals.
- It is NOT a general helpdesk thread view — it is specifically portal actions tied to the job's quotes, invoices, and jobcards.
- The mock "3 helpdesk threads with reply flow" is an invented feature. It should be replaced with portal action cards.

### 2.11 Timeline Tab
- In production, the Timeline tab merges two data sources:
  1. `job_status_log` — status change events (who changed it, when, optional note)
  2. `comms_log` — email/WhatsApp communications sent from the system
- Each item has a `type` of either `'status'` or `'comms'`.
- The mock "Show full message" email body expansion is directionally correct for comms log items.

---

## 3. What Was Invented (Must Be Fixed or Annotated)

| Feature | Status | Action |
|---|---|---|
| "New" as a job status | **Wrong** — not a real backend status | Replaced with `job_booked` / `estimate_done` |
| "Booked", "Quoted", "In Progress" etc. as simplified statuses | **Wrong** — real statuses are snake_case keys | Replaced with real backend keys |
| "Jobcard Signed" guard key | **Invented** | Removed — not in `GUARD_KEY_LABELS` |
| "Invoice Ready" guard key (as manually toggled) | **Partially real** — `invoice_ready` IS a real guard key | Keep but make it auto-computed, not manually toggled |
| Communication tab as helpdesk threads | **Invented** | Should be portal action cards |
| "Lead Tech", "Site Manager", "PM" roles | **Invented** | Replaced with Sales, Technical, Coordinator, Job Owner |
| Job types: "Electrical", "Plumbing", "HVAC" | **Invented** | Replaced with real `JOB_TYPE_LABELS` values |
| 10-stage pipeline bar | **Wrong** — production uses 5 grouped stages | Replaced with 5-stage pipeline |
| HSE 6-stage pipeline | **Needs verification** — HSE pack exists but stages differ | Annotated as needing review |
| Auto-transitions toggle | **Invented** | Annotated as `[NEW SUGGESTION]` |
| "Presence indicator" (who else is viewing) | **Real** — `useRealtimePresence` hook exists in production | Keep — it's surfaced backend data |

---

## 4. Features That Exist in Backend But Are Not Yet Surfaced

These are opportunities to surface real backend data in the mockup. Each is annotated in the HTML.

| Feature | Backend source | Notes |
|---|---|---|
| `job_nature` toggle badge | `jobs.job_nature` field | Shown in production detail breadcrumb — add to mockup detail header |
| `locked_at` / Locked badge | `jobs.locked_at` field | Shown in production — add to mockup detail header |
| `commission_enabled` guard key | `GUARD_KEY_LABELS` | Add to guard keys bar |
| `payment_recorded` guard key | `GUARD_KEY_LABELS` | Add to guard keys bar |
| Site contact name/phone | `client_contacts` table | Could be shown in the site card on detail page |
| Photo count | `jobcardPhotos.length + photos.length + deliveryNotePhotoCount` | Could be shown as a count chip on the Jobcard CTA |
| VAT card | `JobVATCard` component exists in production | Output VAT = quote excl VAT × tax_percentage (from company settings) |
| Variance analysis | `VarianceAnalysis` component exists in production | Already partially shown in Overview tab |

---

## 5. Session Log

| Date | Key Decision |
|---|---|
| Session 1 | Original task: redesign Job Module UI to match CRM design language |
| Session 2 | "Build all features, fully" — all features wired in one pass |
| Session 3 | Mock data added so jobs are interactive |
| Session 4 | Regression discovered — features went dead. Restored original locked file |
| Session 5 | GitHub repo created: `bird-pulse-jobs-mockup` (private) |
| Session 6 | Git history process established — descriptive commits, CONTRIBUTING.md |
| Session 7 | "Make everything white, grey only for canvas" |
| Session 8 | Left-align all table headers and cells |
| Session 9 | Filter logic inverted — unchecked = show all, checked = include only |
| Session 10 | Board column headers redesigned with coloured top-border accent + drop shadow |
| Session 11 | Board cards enriched with all fields |
| Session 12 | "Based on actual code" rule established — all features must match real backend |
| Session 13 | Status values migrated to real backend snake_case keys |
| Session 14 | Board columns updated to exact 7 KANBAN_STAGES from production |
| Session 15 | CONVERSATION_NOTES.md created. Type label removed from board cards. Full backend audit completed. |

---

## 6. Reference Files

| File | Purpose |
|---|---|
| `client-centric-stats/src/lib/job-workflow-types.ts` | All status enums, type labels, guard key labels, metrics calculation |
| `client-centric-stats/src/pages/JobWorkflowDetail.tsx` | Full job detail page — tabs, sidebar, header, all data fields |
| `client-centric-stats/src/pages/JobsWorkflow.tsx` | Jobs list page — table columns, filter logic, board toggle |
| `client-centric-stats/src/components/jobs/JobsKanbanBoard.tsx` | Exact 7 KANBAN_STAGES and card structure |
| `client-centric-stats/src/components/jobs/JobProgressPipeline.tsx` | Exact 5-stage pipeline bar |
| `client-centric-stats/src/components/jobs/JobNextActions.tsx` | Status guidance messages and CTA modules |
| `client-centric-stats/src/hooks/useWorkflowGuardKeys.ts` | Real guard key names and descriptions |
| `client-centric-stats/src/components/jobs/JobAssignmentPanel.tsx` | Real assignment roles |
| `client-centric-stats/src/components/jobs/NewJobWizard.tsx` | All fields captured on job creation |
| `client-centric-stats/src/components/jobs/tabs/JobTimelineTab.tsx` | Timeline tab data structure |
| `client-centric-stats/src/components/jobs/tabs/JobCommunicationTab.tsx` | Communication tab — portal actions |
| `client-centric-stats/src/hooks/useJobPortalActions.ts` | Portal action data shape |
