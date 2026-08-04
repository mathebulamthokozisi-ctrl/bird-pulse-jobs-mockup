# Editor Audit Notes — Missing Fields to Surface

## QUOTE EDITOR (lines 2507–2692)
### Already present:
- Header card: Bill To, Ship To, Quote Details (Quote#, Date, Valid Until, Job#, Site)
- Client PO Number field
- PDF Options bar (Show line items, Display subtotals, PDF Preferences)
- Line items table: Description, Qty, Unit, Unit Price, Total
- Footer: Subtotal excl VAT, VAT 15%, Total incl VAT
- T&C textarea
- Sidebar: Quote Summary, Margin, Document Info, Actions

### MISSING (from production QuoteSummaryFooter.tsx + BOQGridRow.tsx):
1. **Line item columns** — Commission %, Commission (R) read-only, Discount %, Tax Type (VAT 15%/Zero Rated/Exempt)
2. **Footer left** — Notes textarea (for client), Client Discount block (% or R toggle, value input, "Reduce commission proportionally" switch + explanation text)
3. **Footer right** — Deposit % input + Deposit Amount row, Subtotal before discount row (when discount active), Profit row, Commission row (with "scaled" label when discount affects commission)
4. **Bank Details** block in footer (Bank name, Account Name, Account Number, Branch Code)
5. **Sidebar** — Sales Rep field in Document Info, Customer Code field

## INVOICE EDITOR (lines 2695–2881)
### Already present:
- Header: Bill To, Service Location, Invoice Details (Invoice#, Job#, Invoice Date, Due Date, Terms)
- Progress Claim row (Claim#, Claim%, Client PO#)
- Line items table: Description, Qty, Unit, Unit Price, Total
- Footer: Subtotal, VAT, Total Due
- Payment Notes textarea (bank details)
- Sidebar: Invoice Summary, Document Info, Actions, QBO Push suggestion

### MISSING (from invoiceStore.ts + InvoiceSummary.tsx):
1. **Line item Tax Type column** — same as quote (VAT 15%/Zero Rated/Exempt per line)
2. **Footer Notes** — dedicated Notes textarea (separate from bank details)
3. **Footer Summary** — proper 2-column layout: Notes left, Summary right (Subtotal Excl VAT, VAT, Total Incl VAT, Paid/Part-Paid status banner)
4. **Bank Details card** — separate card with Bank, Account Name, Account No, Branch Code (currently just in notes textarea)
5. **Sidebar** — QBO sync status badge (already has suggestion box but should be real status)

## ESTIMATE EDITOR (need to find in index.html)
### MISSING (from estimate-types.ts + EstimateEditorPage.tsx + EstimateSummaryPanel.tsx):
1. **Scope/Method Statement card** — Job Description HTML + Method Statement HTML (rich text areas)
2. **Estimate metadata header** — Estimate#, Estimate Date, Prepared By, Title
3. **Grid columns** — Row#, Section-specific 2nd col (Owned/Hired for equip, Type for labour, Category for others), Description, Unit (ea/m/m²/m³/hr/day/lot), Qty, Unit Cost, Total, Notes icon
4. **Section structure** — 4 collapsible sections: Materials, Equipment, Labour, Preliminaries (each with subtotal)
5. **Summary sidebar** — Total Projected Cost, By Section bars (Materials/Equipment/Labour/Preliminaries), Warnings
6. **PO Panel** — Link PO button, imported PO items badge
7. **Finalize/Unlock** — Finalize button in topbar, status badge

## ACTUAL COSTS EDITOR (lines 2883–2995)
### Already present:
- Category filter tabs (All/Materials/Labour/Sub-Con/Plant-Equip/Travel/Waste/Site Est./HSE/Asset Rec./Other)
- Cost entries table: Date, Category, Description, Supplier/Resource, Reference No., Proof Type, Amount
- Variance table vs Estimate
- Sidebar: Cost Summary, vs Estimate, By Category bar

### MISSING (from ActualCostForm.tsx + cost-types.ts):
1. **Proof upload column** — Proof URL/receipt upload (mandatory), with OCR auto-fill
2. **Confirmed vs Provisional** status per entry (bank-confirmed toggle)
3. **Notes** field per entry
4. **Asset Recoupment** special picker row
5. **Extra proof pages** (multi-page supporting docs)
6. **Summary cards** — Confirmed total, Provisional total, Invoiced total, Projected profit
7. **Add Cost dialog** — Full form: date, category, description, amount, supplier, reference, proof type, proof upload, notes

## JOBCARD EDITOR (lines 2997–3219)
### Already present:
- Before Section: Scope of Work, Findings & Root Cause, Before Photos, Recommendations
- After Section: Work Performed (with Materials Used textarea), After Photos
- Client Acceptance: Technician Signature (name + canvas), Client Sign-Off (client name, signatory name, canvas, confirm button, refusal button)
- Sidebar: Jobcard Status, Team on Site, Schedule

### MISSING (from jobcard-types.ts + JobcardEditorPage.tsx):
1. **Team Members** — team_members_json: list of team members on site (currently only shows 2 in sidebar)
2. **Delivery Note fields** — delivery_address, recipient_name, company_rep_name, company_rep_signature (for delivery note variant)
3. **Client Comments** — client_notes field after signature
4. **Refusal fields** — refusal_reason textarea, refused_by_name (shown when client refuses)
5. **Jobcard status actions** — "Request Signature" button (sends signing link to client), "Send to Client" button
6. **Template selector** — template_id dropdown in header
7. **Materials Used** — currently a textarea; should be a proper table (materials_used structured)
8. **Sidebar** — Signing link status, template name, last saved time
