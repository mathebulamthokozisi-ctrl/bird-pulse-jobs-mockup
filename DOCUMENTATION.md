# Bird Pulse — Jobs Module Documentation

## 1. Overview
The **Bird Pulse Jobs Module** is a comprehensive, interactive frontend mockup built to demonstrate the core workflow, document generation, and job management capabilities of the Bird Pulse platform. It serves as a high-fidelity prototype for field service management, specifically tailored for electrical, mechanical, plumbing, and civil contractors.

The mockup is built as a single-page application (SPA) using vanilla HTML, CSS, and JavaScript. It is hosted in a GitHub repository and served via a local Python HTTP server during development, ensuring a lightweight and easily deployable environment.

## 2. Core Architecture & Layout

The application architecture relies on a persistent left-hand sidebar that divides the system into two primary zones. The **Workspace** section houses the Dashboard, active Jobs, Clients, Invoices, and HSE modules. The **CRM** section contains the Cockpit, People, Companies, and Deals modules. This clear separation ensures users can quickly switch between operational execution and client management.

The main content area implements a master-detail pattern. The master view is a comprehensive data table listing all jobs, equipped with extensive filtering capabilities for status, priority, and assignees. When a job is clicked, the detail view slides over as a side panel, providing deep context and document management for that specific job without losing the context of the broader list.

The Job Detail panel (`#job-detail-panel`) serves as the central hub for managing a single job, such as `JOB-0042 — Electrical Rewiring`. The left sidebar of this panel contains document generation cards for Estimates, Quotes, Invoices, Actual Costs, Jobcards, and HSE Packs, alongside a summary of workflow automation rules. The main content area is divided into tabs: Overview, Communication, Timeline, and HSE. The Overview tab presents critical financial metrics including Quote Value, Estimated Cost, Actual Cost, and Estimated Profit, supported by cost breakdown charts and variance tables. The Communication tab handles client portal queries and internal messaging, while the Timeline provides a full audit trail.

## 3. Document Editors (The "Doc Editors")

A major focus of the development has been refining the full-screen document editors. These editors slide over the entire user interface to provide a focused, distraction-free environment for financial and operational document creation.

All editors share a consistent structural language to reduce cognitive load. The toolbar (`.de-topbar`) consistently houses the "Back to Job" button, a document-specific icon, the title, document ID, and primary actions such as Save, Export PDF, and Send to Client. Below this, the status bar (`.de-statusbar`) displays breadcrumbs and a visual pipeline known as the Stage Strip. The main layout utilizes a two-column design, with the primary document content on the left and a summary sidebar on the right. Content is logically grouped into cards with consistent headers.

| Editor | Primary Purpose | Key Features |
|---|---|---|
| **Estimate** | Internal costing and scoping before quoting | Separated line item tables for Materials, Equipment, Labour, and Preliminaries. Sidebar tracks Total Projected Cost and margin vs quote. |
| **Quote** | Client-facing proposal generation | Auto-populates from Estimate with configurable markup. Features inline discount inputs and a dedicated, visually separated Margin Card showing Profit and Commission. |
| **Invoice** | Billing and payment tracking | Clean header with inline status pills for overdue dates. Line items inherit directly from the Quote. Action buttons for marking as paid or sending reminders. |
| **Actual Costs** | Tracking real-world spend against estimates | Replaces the stage strip with category filter tabs. Grid layout for quick entry of supplier invoices. Real-time variance tracking against estimated costs. |
| **Jobcard** | Field data capture and client sign-off | Captures scope, findings, before/after photos, and client signatures directly in the field. |
| **HSE Pack** | Safety and compliance management | Manages safety checklists and risk assessments, with document inheritance from client master files. |

## 4. Key Design Decisions & Refinements

During the development and polish phases, several specific design decisions were enforced to maintain a clean, professional aesthetic. The Stage Strip styling was refined so that active stages use only text color and an underline, explicitly removing background shading to reduce visual noise. 

Redundant informational banners were systematically removed. For example, the QuickBooks sync banner and the full-width due date banner on invoices were eliminated. This information is now handled via toolbar buttons or compact inline status pills, preserving valuable vertical screen real estate.

In the Quote editor, Profit and Commission data were moved out of the standard totals table and into a dedicated, visually distinct Margin Card below the line items. This creates clear separation between client-facing numbers (Subtotal, VAT, Total) and internal economics, ensuring sales representatives understand the deal margins before sending.

A comprehensive consistency audit was performed across all editors. This ensured that the Estimate materials table headers aligned correctly with the JavaScript rendering order, preventing text truncation in the Description column. It also standardized toolbar button class orders, ensured breadcrumbs consistently show just the Job ID, and unified terminology such as standardizing on "Finalise" and "Mark Paid" across the entire application.

## 5. Proposed Future Features

Based on the current architecture, several features are proposed for future iterations to enhance functionality. 

A PO Linking Modal should be implemented for the "Link PO" button, allowing users to upload a client Purchase Order PDF, parse it, and automatically populate quote or invoice line items. The line item tables in the Estimate and Quote editors would benefit from drag-and-drop handles to allow easy reordering of rows. Furthermore, allowing users to group quote line items under sub-headings with subtotals per section would greatly improve the readability of complex proposals.

For field operations, implementing actual file upload and thumbnail preview functionality for the "Before" and "After" photo sections in the Jobcard editor is a priority. A dynamic split-screen toggle in the editors to show a live, real-time preview of the generated PDF document alongside the data entry fields would provide immediate visual feedback. Finally, expanding financial integrations beyond QuickBooks to include Xero, with corresponding toolbar actions and sync status indicators, would broaden the platform's market appeal.
