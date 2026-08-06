# Mockup Status

## Live URL
https://8080-ii56y9231pksds6gvu8sv-3c2d8d14.us2.manus.computer

## Current State — 6 Aug 2026
All core features working. Latest session changes:

### UI fixes applied
- **Dashboard DnD** — complete rewrite with placeholder-based DnD (horizontal + vertical). Hysteresis dead zone (35%) eliminates oscillation/blinking. See `DND_HANDOFF.md` for remaining proportional-swap limitation.
- **Job Pipeline widget** — taller bars (26px), bigger fonts (12px labels), `justify-content:space-between` rows fill card height
- **Client Leaderboard** — value column fixed-width right-aligned (R 249K → R 63K), profit badge column fixed-width right-aligned
- **HSE Compliance widget** — gauge scaled up, stat tiles stretched to fill card height
- **Pagination bar** — `justify-content:space-between`, fixed 41px height, page size 13 rows (2 pages for 24 jobs)
- **Nav footer** — role text removed, single-line 41px height aligned with pagination bar
- **Nav width** — 220px (was narrower, clipping "Company Profitability" and "WhatsApp Verification")
- **Workflow progress bar** — border-radius 99px → 6px (less pill-shaped)
- **Dashboard widget cards** — border-radius reduced to 6px
- **Dashboard pipeline bars** — border-radius 2px
- **JOB EXECUTION zone** — saturation boosted (#fef3c7 bg, #fcd34d border), amber shimmer animation (3s linear infinite)

### Known issues / handoffs
- **DnD proportional swap** — see `DND_HANDOFF.md`

## File location
`/home/ubuntu/job-mockup/index.html`

## Server
`python3 -m http.server 8080` from `/home/ubuntu/job-mockup/`

---
## Session: 6 Aug 2026 (Customers Module)

### New: Customers Module
- **Customers list page** — KPI strip, sortable table (9 columns), search, status filter chips, date filter, export CSV, New Customer form drawer, quick drawer on row click
- **Customer detail page** — 9 tabs: Overview, Sites, Contacts, Portfolio, Billing, Rate Card, Income, Comms, HSE
- **Health score badge** — colour-coded by risk level (low/medium/high/critical) with breakdown bars
- **Nav wired** — Customers nav link routes to page-customers
- **customers.css** — new stylesheet for the Customers module
