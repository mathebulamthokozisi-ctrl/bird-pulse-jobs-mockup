# Conversation Notes — Bird Pulse Jobs Module UI Mockup

> **Purpose:** This file is the single source of truth for all agreed rules, design decisions, and key discussion points from our design sessions. It must be read at the start of every session to prevent drift.

---

## 1. The Golden Rules (Non-Negotiable)

### 1.1 The Mockup IS the Standard
**The mockup (`index.html`) is the design authority. These notes and the UI Kit (`ui-kit.html`) exist to DOCUMENT what the mockup already does — not to dictate changes to it.**

When we do a production code audit, the findings go into these notes as observations. We NEVER change the mockup to match an audit report without the user explicitly approving each change.

### 1.2 Undo / Redo Discipline
- We rely strictly on Git for state management. We do NOT manually delete code.
- Before any large refactor, create a snapshot commit.
- To undo: `git revert HEAD --no-edit` (safe, creates new commit) or `git reset --hard HEAD~1` (destructive, use with caution).
- To restore a specific file: `git checkout <commit-hash> -- <file>`.

### 1.3 Design Consistency with Production CRM
The mockup uses the same design language as the production CRM (`client-centric-stats`), but the layout and features in the mockup take precedence. Key tokens:
- **Primary accent:** Indigo (`#4f46e5` / `indigo-600`) — used for action buttons, active states, filter chips
- **Background:** Canvas is grey (`#f0f2f5`); all surfaces (sidebar, toolbar, panels, modals) are **white**
- **Table rows:** White / near-white (`#fafafa`) alternating — no visible grey zebra
- **Font:** Inter
- **Icons:** Material Symbols

### 1.4 Left-Align Everything in Tables
All table headers and cell content must be left-aligned — matching the production CRM invoice/jobs table style.

### 1.5 Filters Work by Inclusion, Not Exclusion
Filter checkboxes start **unchecked** (show all). Ticking an item means "show only this". Multiple ticked items in the same group are OR'd. Multiple groups are AND'd.

---

## 2. Documented UI State (Current Mockup)

This section documents the exact state of the UI as currently built in `index.html`.

### 2.1 Detail Page Layout
- **Breadcrumb & Header:** Compact single-line breadcrumb, job title, and meta tags (Job Nature, Created, Priority, Job Type, Reference).
- **Pipeline Bar:** Green gradient progress bar (`linear-gradient(90deg, #22c55e, #10b981)`) spanning the top width. Stages: Estimate → Quote → Work → Invoice → Paid.
- **Guard Keys & Execution:** Sits below the pipeline. Guard keys are on the left, Execution zone (amber gradient) is on the right.
- **Guidance Bar:** Indigo left-border guidance message below guard keys.
- **Right Sidebar (`detail-sidebar`):** The sidebar is on the **RIGHT** side (`border-left`). It contains:
  1. **Team** (Tech Manager, Coordinator, Sales Rep, Job Owner)
  2. **Financials** (Estimated P/L, Quote, Estimate Cost, Actual Cost, Commission, Est. Profit)
  3. **VAT Summary** (Output VAT, Input VAT, Net VAT)
  4. **Documents** (Estimate, Quote, Invoice, Actual Costs, Jobcard, HSE Pack)
- **Main Content (`detail-main`):** On the left. Contains Job Info card and Tabs (Overview, Communication, Timeline, HSE).

### 2.2 Tabs
- **Overview:** Contains 4 KPI stat cards (Quote Value, Estimated Cost, Actual Cost, Est. Profit) with reactive gradients (`--mc-glow`). Below them is a "Cost by Category" bar chart and a Variance table.
- **Communication:** Threaded helpdesk UI with rich HTML, images, and attachments.
- **Timeline:** Vertical activity feed (Status changes, Emails, Docs, Notes).
- **HSE:** HSE Pipeline (Draft → Assembling → Internal Review → Awaiting Signatures → With Client → Approved), inheritance banner, and Risk Assessment table.

### 2.3 Board View (Kanban)
- 7 columns: Booked, Quoting, Accepted, In Progress, Completed, Invoiced, Declined / Cancelled.
- Coloured top-border accent on column headers.
- Cards show: Job #, Status, Client, Site, Description, Team avatars, Value.

### 2.4 Document Editors (Modals)
The mockup includes fully functional modal editors for:
- **Estimate:** Line item table (Desc, Qty, Unit Cost, Markup, Unit Price, Total).
- **Quote:** Line item table (same as estimate).
- **Invoice:** Line item table (same as estimate).
- **Actual Costs:** Entry table (Date, Supplier, Category, Ref, Amount).
- **Jobcard:** 4x4 photo grid slot and signature capture box.

### 2.5 Status & Field Data
The mockup currently uses a mix of production backend keys and human-readable strings.
- **Statuses:** `job_booked`, `estimate_done`, `quote_drafted`, `quote_sent`, `quote_finalized`, `quote_accepted`, `quote_declined`, `on_my_way`, `work_in_progress`, `job_in_progress`, `job_completed`, `work_client_approved`, `invoiced`, `paid`, `commission_paid`, `cancelled`.
- **Job Types:** Electrical, Plumbing, HVAC, General, Security, Cleaning, Landscaping.
- **Job Nature:** To Be Done, Done Already.
- **Priorities:** Low, Medium, High, Urgent.

---

## 3. Session Log

| Date | Key Decision |
|---|---|
| Session 1 | Original task: redesign Job Module UI to match CRM design language |
| Session 2-6 | Built full interactive prototype, established git workflow |
| Session 7-14 | Refined layout, made surfaces white, inverted filters, built board view |
| Session 15 | Added Document Editors (Estimate, Quote, Invoice, Actual Costs, Jobcard) |
| Session 16 | Added HSE tab and risk matrix, finalized green pipeline bar and right-side panel |
| Session 17 (Rollback) | Wrong branch was edited. Fully rolled back to the correct task state (`qnEiEnf2baqVStUL8Mdnyc`). Mockup IS the standard rule locked. |
