# Fantasy Draft Tool — Session Notes

**Purpose:** Paste this file into new Claude conversations so the AI has full context of the project and all past work.

---

## Project Overview
- **Name:** The Draft Decider — Fantasy Football 2026
- **Live URL:** https://bradrw17.github.io/fantasy-draft/fantasy_draft_tool.html
- **Local repo:** C:/Users/bradr/OneDrive/Documents/fantasy/fantasy-draft/
- **Main file:** `fantasy_draft_tool.html` (~3,115 lines, single-file React-free vanilla JS app)
- **Owner:** Brad (GitHub: bradrw17)

## Tech Stack
- Vanilla HTML/CSS/JavaScript (no frameworks)
- Single self-contained HTML file
- Hosted on GitHub Pages
- Purple/blue gradient theme (`#667eea` → `#764ba2`)
- Mobile-responsive with sticky tab nav

## App Structure (5 Tabs)

### 1. Roster View (Draft Tab)
- **Scenario A vs B comparison mode** — side-by-side roster builders
- Each scenario has: QB, 2x RB, 2x WR, TE, FLEX (7 slots)
- Autocomplete player search from 558-player database
- Shows projected fantasy points per player
- Round input for each pick
- Copy Scenario A → B button
- Comparison summary bar showing point differential and winner
- Toggle between comparison mode and single-scenario mode
- Hide/show individual rows

### 2. Round View
- Round-based draft layout (shows picks by round)
- "Show Rounds" dropdown: 1-15 rounds, default shows rounds 1-7
- Same player search and point projection system

### 3. Rankings Tab
- Player rankings list
- Title dynamically updates based on PPR setting (PPR/Half PPR/Standard Rankings)
- Sorted by FantasyPros 2026 consensus ADP

### 4. League Assumptions Tab
- Configurable scoring settings:
  - Pass yards per point, passing TD points, interception penalty
  - Rush yards per point, receiving yards per point
  - Reception points (PPR setting)
  - Receiving TD points, rush TD points
  - Fumble penalty
- League Players setting (affects ADP calculations)
- Changes recalculate all projections in real-time

### 5. Raw Data Tab
- Full table of all player projected statistics
- Shows: passYds, passTD, int, rushYds, catches, recYds, td, fumbles

## Player Data
- **558 players** with Rotowire 2026 projections
- Stats stored as JS object keyed by lowercase player name
- Each player has: passYds, passTD, int, rushYds, catches, recYds, td, fumbles
- Player info includes: team name, bye week
- **ADP data** from FantasyPros 2026 consensus (328+ players matched)

## Key Features Built (Chronological)
1. Initial draft tool with player search and point projections
2. Home page with blue glassmorphism design
3. Renamed "My Draft" → "Roster View"
4. Added Round View tab
5. Added ADP column and League Players setting
6. Fixed ADP to show overall draft position (not filtered rank)
7. Removed help text under assumption inputs
8. Added hide/show toggle for roster/round view rows
9. Added Raw Data tab with full stat table
10. Added "Show Rounds" dropdown (1-15, default 1-7)
11. Improved mobile responsiveness for tabs, raw data, round controls
12. Dynamic rankings tab title based on PPR setting
13. Replaced all player data with Rotowire 2026 projections (558 players)
14. Changed BENCH → FLEX on roster view
15. Sorted rankings by FantasyPros 2026 consensus ADP

## Git Info
- 77+ commits
- Last commit (March 5, 2026): "Use FantasyPros 2026 consensus ADP for rankings order"

---

## Session Log

### Session — March 6, 2026
- Reviewed full project state and created this session_notes.md file
- **Default tab changed to Round View** — swapped `active` class from draftTab to roundTab and corresponding tab buttons
- **Position-filtered autocomplete on Roster View** — RB slots only show RBs, WR slots only WRs, QB slots only QBs, TE slots only TEs, FLEX slots show RB/WR/TE. Modified `handleInput()` function (~line 2421) to read `data-position` from the parent `.roster-slot` and filter `PLAYER_NAMES` accordingly. Round View is unaffected (those slots don't have `data-position` set, so they show all players).

---

*Update this file at the end of each work session with what was changed.*
