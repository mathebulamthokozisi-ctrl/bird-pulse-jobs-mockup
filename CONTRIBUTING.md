# Bird Pulse Jobs Mockup — Git Workflow & History Guide

This document defines the **mandatory process** for making changes to this mockup, writing commit messages, and undoing or redoing any change. Every session that touches `index.html` must follow this process without exception.

---

## 1. Branching Strategy

| Branch | Purpose |
|---|---|
| `main` | Stable, reviewed, working state. Never commit directly unless it is a hotfix. |
| `feature/<short-name>` | New feature work (e.g. `feature/hse-risk-table`) |
| `fix/<short-name>` | Bug or visual regression fixes (e.g. `fix/pill-border-removal`) |
| `design/<short-name>` | Pure visual/CSS changes (e.g. `design/table-zebra-stripe`) |
| `snapshot/<label>` | Point-in-time snapshots before a risky change (e.g. `snapshot/before-full-rebuild`) |

**Before starting any work:**
```bash
git checkout main
git pull origin main
git checkout -b feature/my-feature-name
```

**After finishing work:**
```bash
git add .
git commit -m "..."   # see commit format below
git push origin feature/my-feature-name
# then merge into main:
git checkout main
git merge feature/my-feature-name
git push origin main
```

---

## 2. Commit Message Format

Every commit **must** follow this structure exactly:

```
<type>(<scope>): <short summary — max 72 chars>

<body — mandatory, minimum 3 sentences>
- What changed (specific elements, lines, functions)
- Why it changed (design decision, bug fix, user feedback)
- What the visual/functional effect is (what the user sees differently)

<footer — optional>
Refs: #issue-number
Breaking: <describe any breaking change>
Snapshot: <branch name if a snapshot was taken before this change>
```

### Type prefixes

| Type | When to use |
|---|---|
| `feat` | Adding a new feature or UI section |
| `fix` | Correcting a bug, broken interaction, or JS error |
| `design` | Visual-only change — CSS, spacing, colours, typography |
| `data` | Changes to mock data only (JOBS array, TIMELINE_ITEMS, etc.) |
| `refactor` | Code restructuring with no visual change |
| `docs` | Changes to README, CONTRIBUTING, or inline comments only |
| `snapshot` | A commit whose sole purpose is to save state before a risky change |

### Scope values (use the most specific one)

`table` · `pills` · `nav` · `toolbar` · `filter-sidebar` · `detail-header` · `pipeline-bar` · `guard-keys` · `guidance-bar` · `detail-sidebar` · `financials` · `documents` · `tabs` · `overview-tab` · `comms-tab` · `timeline-tab` · `hse-tab` · `jobcard-modal` · `estimate-modal` · `quote-modal` · `actual-costs-modal` · `invoice-modal` · `new-job-modal` · `columns-modal` · `assign-modal` · `board-view` · `pagination` · `bulk-bar` · `data-jobs` · `data-estimate` · `data-comms` · `data-timeline` · `data-hse` · `js-logic` · `css-global`

---

## 3. Example Commit Messages

### Good example — feature
```
feat(jobcard-modal): add photo slot click-to-add interaction and counter badge

Previously the four photo slots in the Jobcard modal were static placeholder
boxes with no interaction. Clicking them now marks them as "added" (fills the
slot with a green checkmark and a mock filename), increments a live counter
badge on the Documents sidebar card ("0 photos" → "1 photo" etc.), and updates
the Jobcard guard key status from Draft to Photo Added once at least one photo
is present. The Save Jobcard button was also wired to persist the signed state
and photo count back to the JOBS array so re-opening the detail reflects the
correct state without a page reload.

Refs: #12
Snapshot: snapshot/before-jobcard-photo-interaction
```

### Good example — design fix
```
design(table): remove pill borders, tighten row padding, add zebra striping

All status, finance, and priority pills previously had a 1px coloured border
(e.g. indigo-200 for blue pills) which made them look heavy and cluttered
compared to the locked reference screenshot. Borders have been removed — pills
now rely solely on background colour and text colour for visual distinction.
Row top/bottom padding was reduced from 10px to 8px to match the more compact
row height visible in the reference. Even-numbered rows now receive a #fafbfc
background (barely perceptible off-white) to create a subtle zebra stripe that
helps the eye track across wide rows without adding visual noise.

Snapshot: snapshot/before-pill-redesign
```

### Good example — data
```
data(data-jobs): add `type` field to all 24 JOBS entries and wire Type column

The JOBS data array previously had no `type` field, so the Type column that
was visible in the locked reference screenshot (showing values like Electrical,
Maintenance, Installation, Repair, Other) could not be rendered. A `type`
property has been added to all 24 job objects with values derived from each
job's description. The thead now includes a `<th data-col="type">Type</th>`
column between Description and Priority, `visibleCols` includes `type: true`,
the renderJobs() function renders the value as plain muted-grey text (no pill),
and the Columns modal includes a toggleable Type entry. The column is togglable
and resizable like all other columns.
```

---

## 4. Undo Process (rolling back a change)

### Option A — Undo the last commit but keep the file changes (safest)
```bash
git revert HEAD --no-edit
git push origin main
```
This creates a **new commit** that reverses the last one. The history is preserved — nothing is deleted. This is the preferred method.

### Option B — Undo the last N commits entirely (destructive — use with caution)
```bash
# Undo last 1 commit and discard all changes
git reset --hard HEAD~1
git push origin main --force
```
> **Warning:** `--force` rewrites history. Only use this if the commit was never shared or reviewed.

### Option C — Restore a specific file to a previous commit's version
```bash
# Find the commit hash you want to restore from
git log --oneline index.html

# Restore that file to that commit's version
git checkout <commit-hash> -- index.html

# Commit the restoration
git add index.html
git commit -m "fix(table): revert index.html to <commit-hash> — <reason>"
git push origin main
```

### Option D — Jump the entire repo back to a previous state (snapshot restore)
```bash
# Find the snapshot branch or commit
git log --oneline --all

# Create a new branch from that point
git checkout -b restore/from-<commit-hash> <commit-hash>

# Or reset main to that point (destructive)
git checkout main
git reset --hard <commit-hash>
git push origin main --force
```

---

## 5. Redo Process (re-applying a reverted change)

```bash
# Find the commit that was reverted
git log --oneline --all

# Cherry-pick it back onto main
git checkout main
git cherry-pick <commit-hash>
git push origin main
```

---

## 6. Snapshot Before Risky Changes

Before any large or experimental change, always create a snapshot commit:

```bash
git add .
git commit -m "snapshot(before-<change-name>): save stable state before <what you are about to do>

This is a safety snapshot taken before <describe the upcoming change in detail>.
The mockup is in a known-good state at this point: <describe what is working>.
If the upcoming change causes a regression, restore with: git checkout <this-hash> -- index.html"

git push origin main
```

---

## 7. Viewing History

```bash
# One-line log
git log --oneline

# Full log with body
git log

# Log for a specific file
git log --follow -p index.html

# See what changed in a specific commit
git show <commit-hash>

# Compare two commits
git diff <hash-A> <hash-B> -- index.html
```

---

## 8. Session Checklist

Before every work session:
- [ ] `git pull origin main` — get the latest
- [ ] `git checkout -b <type>/<scope>` — never work on main directly
- [ ] Take a snapshot commit if the change is large or risky

After every work session:
- [ ] `git add .`
- [ ] Write a commit message following Section 2 above — body is **mandatory**
- [ ] `git push origin <branch>`
- [ ] Merge to main if the change is complete and verified

---

*Last updated: 3 Aug 2026 by Manus*
