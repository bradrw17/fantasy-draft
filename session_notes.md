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
- **Default players in Roster View** — Hardcoded full 7-slot lineups in the HTML for both scenarios:
  - Scenario A: QB Trevor Lawrence (Rd 7), RB James Cook (Rd 2), RB Kyren Williams (Rd 4), WR Amon-Ra St. Brown (Rd 1), WR Garrett Wilson (Rd 3), TE Tyler Warren (Rd 6), FLEX Travis Etienne (Rd 5) — Total: 1605
  - Scenario B: QB Lamar Jackson (Rd 3), RB Kyren Williams (Rd 4), RB Quinshon Judkins (Rd 6), WR Amon-Ra St. Brown (Rd 1), WR Emeka Egbuka (Rd 5), TE Trey McBride (Rd 2), FLEX Michael Wilson (Rd 7) — Total: 1522
- **Default players in Round View** — `prefillRoundSlot()` helper in `initRoundView()`:
  - Scenario A: Trey McBride (Rd 2), D'Andre Swift (Rd 6)
  - Scenario B: James Cook (Rd 2), Tyler Warren (Rd 6)
  - All rows except rounds 2 and 6 hidden by default (uses `hidden-row` class + `updateShowAllBtn`). "Show All Rows" button appears to reveal them.
- **Show Lineup dropdown on Roster View** — Mimics the Round View "Show Rounds" pattern. 10 configurable slots: QB, RB 1, RB 2, WR 1, WR 2, WR 3, TE, FLEX 1, FLEX 2, SF. Default shows 7 (QB, RB×2, WR×2, TE, FLEX). WR 3, FLEX 2, and SF are hidden until toggled on via dropdown checkboxes.
  - New HTML: 3 extra roster slots (WR, FLEX, SF) added to both rosterA and rosterB with `style="display:none"`
  - New CSS: `.show-lineup-btn`, `.show-lineup-wrapper`, `.position-label.SF`
  - New JS: `LINEUP_SLOTS` config array, `initLineupDropdown()`, `toggleLineupDropdown()`, `toggleLineupSlot(index, visible)`. Called from DOMContentLoaded.
  - SF (Superflex) autocomplete allows QB + RB + WR + TE (added in `handleInput()`)
  - Close-on-outside-click handler updated for `#lineupDropdown`
- **Clear Lineup button** added to both Roster View and Round View. Layout order: Show Lineup/Rounds → Comparison Toggle → Clear Lineup. Uses `.show-rounds-btn` styling. Calls `clearAllRosterSlots()` (which calls `clearScenario('a')` + `clearScenario('b')`) and `clearAllRoundSlots()` (which calls `clearRoundScenario('a')` + `clearRoundScenario('b')`).

### Session — March 6, 2026 (continued)
- **Source selector sub-tabs on Rankings tab** — Added a `.source-selector` bar with "FantasyPros" button between tab nav and search bar. Extracted `adpOrder` array from `populateRankings()` into a `RANKINGS_SOURCES` registry object. `populateRankings()` now reads from `RANKINGS_SOURCES[currentRankingsSource].adpOrder`. Ready for future sources (ESPN, Yahoo, etc.) to be added to the registry.
- **Source selector sub-tabs on Raw Data tab** — Added a `.source-selector` bar with "Rotowire" and "Manual" buttons. Renamed `PLAYER_STATS_RAW` data to `ROTOWIRE_STATS` and created `STATS_SOURCES` registry. `PLAYER_STATS_RAW` is now a `let` pointer that gets reassigned when switching sources. `switchStatsSource()` handles toggling between Rotowire (original data) and Manual (deep-cloned editable copy).
- **Manual editing mode on Raw Data tab** — When "Manual" is selected: notice bar appears with "Reset to Rotowire" button, stat cells become clickable (class `editable` with `data-player` and `data-stat` attributes). Click-to-edit flow: click cell → inline `<input>` appears → type new number → Enter/blur saves (updates `PLAYER_STATS_RAW`, calls `recalculateAllPoints()`, updates points cell) → Escape cancels. `resetManualStats()` deep-clones `ROTOWIRE_STATS` to wipe all edits. Manual edits persist when switching away and back (until reset). All views (Roster, Round, Rankings) reflect the active stat source.
- **New CSS:** `.source-selector`, `.source-tab`, `.manual-edit-notice`, `.reset-manual-btn`, `td.editable`
- **New JS functions:** `switchRankingsSource()`, `switchStatsSource()`, `resetManualStats()`, `attachEditHandlers()`
- **New JS variables:** `RANKINGS_SOURCES`, `ROTOWIRE_STATS`, `STATS_SOURCES`, `currentRankingsSource`, `currentStatsSource`, `manualStatsData`

---

*Update this file at the end of each work session with what was changed.*
