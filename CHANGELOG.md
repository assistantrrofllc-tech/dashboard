# Changelog

## [2.6.0] - 2026-02-20

### Data Freshness Improvements

**New Features:**
- **Auto-Sync Stale Cron Data (Change #1)**
  - Dashboard now automatically checks cron data age on load
  - If data is >6 hours old, triggers background sync to refresh timestamps
  - Prevents Rob from seeing outdated "next run" times
  - Console logs when auto-sync triggers for transparency

- **Manual "Sync Now" Button (Change #4)**
  - Added manual refresh button in cron tab header
  - Shows sync status: "Syncing...", "✓ Synced", or error message
  - Prevents multiple simultaneous syncs
  - Status message auto-clears after 3 seconds

**Technical:**
- Added `cronDataTimestamp` tracking to monitor data age
- New `checkAndSyncCronData()` function runs on page load
- New `syncCronData(isAuto)` function handles both manual and automatic refreshes
- Cron data fetches now include cache-busting timestamp parameter
- Sync button styled to match dashboard theme (cyan, glassmorphic)

## [2.5.0] - 2026-02-18

### Nightly Self-Improvement Update

**New Features:**
- **Approvals Queue — Full Backend** (`agents/approvals/submit-approval.py`)
  - Kai can now formally submit approval requests that appear in the dashboard
  - Supports types: email, social, purchase, action, research, external
  - Priorities: high (pulsing red), medium, low
  - Optional `--notify` flag sends Rob a Telegram message immediately
  - `submit`, `list`, `check`, `decide` subcommands for full lifecycle management
  - Decisions log written to `agents/approvals/decisions-log.json` for Kai to read back
- **Approval Decisions Now Persist** (localStorage)
  - Rob's Approve/Reject clicks survive page reloads and tab closures
  - On reload: localStorage decisions merged with latest `approvals.json` from disk
  - Items decided by Rob are removed from pending list even if file hasn't synced
  - New `formatDate()` handles both `requestedISO`, `createdAt`, and `requested` field formats
- **Approval Decision Toast Notifications**
  - Green toast for approvals, red toast for rejections with item title
  - Auto-dismisses after 2.8 seconds with fade-out animation
- **Approvals Empty State** — Shows "✅ Nothing pending — you're all caught up" instead of blank space
- **Full type icon set** — Added `research` 🔍 and `external` 🌐 to type icons
- **Decided items show decision timestamp** — Approved/rejected cards show when the decision was made

**Data:**
- Synced `data/cron-jobs.json` with live OpenClaw data (15 active jobs, accurate statuses)
  - 2 errors flagged: `youtube-learning` (timeout, fixed) + `Nightly GitHub Backup` (rate limit)
  - Live statuses: running, ok, warn, error, scheduled all reflecting real state

**Bug Fixes:**
- `youtube-learning` cron timeout fixed: 300s → 600s (was timing out daily)
- `renderCard()` date display fixed: now handles all 3 date field formats in approvals items

**Technical Notes:**
- `loadApprovals()` now handles both legacy array format and `{pending,approved,rejected}` object format
- Cache-busting query string on `approvals.json` fetch to avoid stale browser cache
- `saveDecisionsToLocalStorage()` — new function for decision persistence
- `showApprovalToast()` — new reusable toast notification system

---

## [2.4.0] - 2026-02-17

### Nightly Self-Improvement Update

**New Features:**
- **Error Status Indicators (Live)** — Cron job status dots now reflect real error states:
  - 🟢 Green: active, no errors
  - 🔵 Blue + pulsing: currently running
  - 🟡 Yellow: 1 consecutive error (warning)
  - 🔴 Red + pulsing: 2+ consecutive errors (critical)
  - ⚫ Gray: one-time scheduled job
  - Jobs with errors show a tooltip with error count + message
  - Running jobs show inline "● RUNNING" badge next to name
- **Data Last Updated Timestamp** — Cron section now shows "data as of [date/time]" based on most recent lastRun across all jobs. Helps Rob see if data is stale.
- **Morning Dashboard Brief** — New cron job added: runs every day at 7:30 AM, generates and sends a Telegram morning briefing covering overnight builds, scout highlights, blockers, approvals, and today's schedule.

**Data:**
- Updated `data/cron-jobs.json` with accurate Feb 17 timestamps for all 13 jobs (added Morning Dashboard Brief)
- All nextRun times now reflect 2026-02-17 schedule

**Technical Notes:**
- New agent: `agents/morning-brief/generate-brief.py` — standalone Python script, no dependencies beyond stdlib + json
- CSS: added `.cron-status.ok`, `.running`, `.warn`, `.error`, `.scheduled` + `pulse-blue` animation
- JS: `cronHTML()` now reads `consecutiveErrors`, `status`, and `errorMessage` from job data

---

## [2.3.1] - 2026-02-16

### Nightly Self-Improvement Update

**Fixed:**
- **Cron Jobs Data Accuracy** — Updated `data/cron-jobs.json` with real data from OpenClaw (was showing 7 fake placeholder jobs, now shows all 10 actual jobs)
- **Error Status Indicators** — Added `consecutiveErrors` and `errorMessage` fields to cron job cards for better visibility
- **Fake Approvals Removed** — Cleared placeholder approval items, now shows accurate empty state
- **Blocked Cron Jobs** — Fixed two legacy jobs (youtube-learning, workspace-backup) that were using blocked Codex model by updating to Sonnet

**Technical Notes:**
- Dashboard data last updated: 2026-02-16 02:00 AM
- All cron job times, schedules, and error states now reflect live OpenClaw data

---

## [2.3.0] - 2026-02-15

### Approvals Queue for Agents

**New View: Approvals Tab**
- Added "Approvals" tab with real-time counter badge in the navigation
- Shows pending requests from agents (Coder, Writer, Research, etc.)
- Card-based layout with type-specific icons (email, social, purchase, action)
- Color-coded priority badges (High/Medium/Low)
- **High Priority Pulse**: Pulsing red border animation for critical items
- Collapsible detail view for full context before deciding
- **Action Buttons**: Big ✅ Approve and ❌ Reject buttons
- Items move to "Approved" or "Rejected" sections upon action
- Collapsible history sections for past decisions

**Work Tab Integration:**
- "Pending Approvals" preview widget added to the top of the Work tab
- Only visible when there are active items waiting for Kai's review
- Quick link to jump to the full Approvals tab

**Data:**
- Created `data/approvals.json` to store pending and historical decisions
- Support for type, priority, agent attribution, and timestamps

---

## [2.2.0] - 2026-02-15

### URGENT: One-Click Unblockers

**New View: One-Click Unblockers (Default)**
- Added "🚀 Unblockers" as the default tab when opening the dashboard
- Large, prominent cards for items waiting on Rob's action
- High priority items (e.g., API keys) have a pulsing glow animation
- Each card includes:
    - Project icon + Name
    - Priority badge (High/Red, Medium/Yellow)
    - Days blocked counter (relative to today)
    - Clear description of what is blocking
    - **BIG Action Button**: Direct link to the unblock page (opens in new tab)
    - Step-by-step instructions below the button

**Interaction:**
- "Done" button on each card moves the item to a "Recently Resolved" section
- Real-time counter of waiting items in the tab header
- Mobile-optimized layout for Galaxy S22

**Data Integration:**
- Consumes `/data/blockers.json`
- Supports High and Medium priority categorization

---

## [2.1.0] - 2026-02-15

### Finance Tab — Banking-Style Transaction View

**Overview Section:**
- Net balance, money in, money out summary cards
- Deposit and transaction counts

**Transaction List:**
- Bank statement-style list sorted by date (newest first)
- Category icons and color-coded badges per transaction
- Green for income, red for expenses
- Receipt indicator dot on transactions with receipts
- Filterable by: All/Income/Expense + category dropdown

**Transaction Detail (click any row):**
- Full details: vendor, address, card used
- Item breakdown with subtotal + tax
- Receipt image viewer with click-to-zoom fullscreen
- Price tracking indicator if item is tracked

**Price Tracking Section:**
- Shows tracked items from `data/price-tracking.json`
- Price history with vendor and date
- Badge indicators for price changes (up/down/baseline)
- Lowest price reference

**Design:**
- Matches existing dark glassmorphism theme
- Mobile-responsive (finance overview stacks on mobile)
- Smooth overlay transitions, ESC to close
- Receipt fullscreen viewer with backdrop

---

## [2.0.0] - 2026-02-15

### Complete Rebuild — Dashboard V2

**Layout:**
- Top navigation tabs: Work | Stocks | Website | Blog | Socials | Calendar
- Tab switching changes main content area
- Work tab has 3 sections: Cron Jobs, Lead Agents, Open Projects

**Cron Jobs (Area 1):**
- Split view: next 12 hours + beyond 12 hours
- Shows emoji, name, description, next run time, relative countdown
- Green status dots, sorted by next run time
- Data from `/data/cron-jobs.json`

**Lead Agents (Area 2):**
- Grid of agent cards: Kai, Volt, Scout, Quill
- Each card clickable → opens slide-in profile panel
- Profile shows: avatar, name, title, status, backstory, teams, past jobs
- Data from `/data/agents.json`

**Open Projects (Area 3):**
- List of active projects with progress bars
- Each clickable → opens slide-in detail panel
- Detail shows: status, progress, task checklist, notes
- Data from `/data/projects.json`

**Design:**
- Dark theme matching TechQuest landing page
- Glassmorphism cards, cyan/green gradient accents
- Animated background orbs and particles
- Slide-in overlay panels (not page reloads)
- Mobile-responsive (Galaxy S22 optimized)
- Space Grotesk font

**Technical:**
- Pure HTML/CSS/JS — no frameworks
- All data in `/data/` as JSON files
- Overlay system with backdrop blur + ESC close
- Live clock in header

**Data files created:**
- `data/agents.json` — Kai, Volt, Scout, Quill
- `data/projects.json` — TechQuest Landing, Dashboard V2, Merch Store, YouTube School
- `data/cron-jobs.json` — heartbeat, youtube school, QMD, daily intel, git backup, stock scanner, memory maintenance

---

## [1.0.0] - 2026-02-11

### Initial Dashboard (V1)
- Title: "Kai Ops Dashboard — Live operating view for Rob"
- Tabs: Cross-Job Ops, Stocks (BBAI), Free Time Build Queue
- Status cards, Sparrow control points, 6-day ROI sprint
- Dark blue theme, card-based layout
