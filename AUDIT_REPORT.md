# Bird Pulse Jobs Module — UI Audit Report

This report compares the production `client-centric-stats` repository (the source of truth) against the `bird-pulse-jobs-mockup` repository to identify UI drift, missing features, and new feature proposals.

## 1. The Design System (Locked Theme)
**Status:** The mockup needs structural updates to comply with the locked CRM theme.
- **Background:** Production uses `#F7F8FA` (`--workspace-bg`), while the mockup uses `#f1f5f9`.
- **Topbar:** Production detail pages use a sticky 52px white topbar (`<PageTopBar>`) with back button, avatar, title, and ghost CTAs. The mockup currently uses a custom breadcrumb and header block.
- **KPI Cards:** Production KPI cards (`<KpiCard>`) MUST have a 3px coloured top accent bar, white background, no box-shadow, and 8px border-radius. The mockup currently has standard cards without the accent line.
- **Tables:** Production tables must be edge-to-edge inside their white card container (no side padding gaps), with a filter bar living INSIDE the card above the table. The mockup currently has a separate filter sidebar and floating table.
- **Tabs:** Production uses an underline style (`border-b-2`, active indigo `#4F46E5`), 13px/500 font.

## 2. Jobs List Page (`JobsWorkflow.tsx`)
**Status:** Several columns and filters in production are missing or misrepresented in the mockup.

**Missing / Drifted Columns:**
- `assigned_to` uses `job_owner_name` and an avatar circle with initials.
- `job_stage` is the workflow status (using `DynamicStatusBadge`).
- `progress` is a derived column showing "Job Completed", "Work In Progress", or "—".
- `finance_stage` is a derived column (Estimated, Quoted, Part Invoiced, Invoiced, Paid).
- `job_nature` shows "Done Already" (warning) or "To Be Done" (success).
- `profit_loss` shows actual vs expected profit based on invoice/cost presence.
- `created_by` uses `created_by_name` and an avatar circle.
- `created_date` shows date and time stacked.

**Missing / Drifted Features:**
- The filter sidebar in production has specific URL-synced filters: `status`, `client`, `site`, `employee`, `finance`, `dateFrom`, `dateTo`.
- Quick chips are: `mine`, `unassigned`, `active`, `thisWeek`.
- Production has a "Show deleted jobs" toggle and a separate deleted jobs table.
- Timer indicator: Row shows a pulsing warning timer icon if `has_active_timer` is true.
- "Done Already" indicator: Row shows a "DA" badge next to the job number if `job_nature === 'done_already'`.

## 3. Job Detail Page (`JobWorkflowDetail.tsx`)
**Status:** The mockup layout diverges significantly from the production layout pattern.

**Layout Drift:**
- Production uses a sticky 52px topbar for navigation and title.
- Below the topbar is the `JobProgressPipeline`.
- Below the pipeline is the Header (title, job type, stat chips, and action dropdown).
- Below the Header is the `JobProgressControls` (Mark WIP / Mark Completed buttons).
- Below the controls is the Next Action Banner (`STATUS_GUIDANCE`).
- Below the banner is an optional Helpdesk Ticket Banner.
- The main body is a 300px left rail + flexible right content area.

**Left Rail Drift:**
- **Team:** Shows exactly three slots: Tech Mgr, Coordinator, Sales. It uses a popover to assign employees. `job_owner` is enforced to be either the Tech Mgr or Coordinator.
- **P/L Widget:** Shows Expected/Actual Profit, Margin/Markup, and Output VAT calculation.
- **Documents:** Uses a specific `DocumentRow` pattern with `iconBg`, `iconColor`, `iconName`, `name`, `meta`, `statusPill`, and `emailStatusSlot`. It does NOT show HSE Pack here.

**Main Content Drift:**
- **Job Info Card:** Sits at the top of the main content area. It shows Client, Site, Job Type, Created Date, and Description. It has inline editing for Client, Site, and Description.
- **Tabs Card:** Sits below the Job Info Card. Contains Overview, Communication, Timeline, and HSE tabs.

**Tabs Drift:**
- **Overview:** Production only shows a `VarianceAnalysis` component (profit summary + category cost bar chart). The mockup has 4 KPI cards and a different layout.
- **Communication:** Production shows a `JobCommunicationTab` which is a read-only feed of *Client Portal Actions* (quote approved, jobcard signed, PO uploaded, etc.), NOT a threaded helpdesk/reply UI.
- **HSE:** Production shows a `JobHSEPackCard` widget embedded in the tab, focusing on client-file inheritance and a risk assessment list, not a full 6-stage pipeline.

## 4. Next Steps
To bring the mockup into perfect alignment with production (fitting "like a glove"), we need to:
1. Update `ui-kit.html` with the locked theme primitives (Topbar, KPI Strip, Edge-to-edge Tables).
2. Rebuild the Jobs List page to match the exact columns and filter layout.
3. Rebuild the Job Detail page layout (Topbar -> Pipeline -> Header -> Controls -> Banner -> Left Rail + Main Content).
4. Correct the Communication tab to be a portal activity feed.
5. Correct the Overview tab to be the Variance Analysis view.
