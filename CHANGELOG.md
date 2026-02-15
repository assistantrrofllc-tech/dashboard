# Changelog

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
