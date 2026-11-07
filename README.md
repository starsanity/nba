# Version 1.2 Release Notes — NBA Analyzer

This document details the new features, UI enhancements, and analytical metric engines implemented in **Version 1.2** of **NBA Analyzer**.

## 1. Multi-Token Team & Mascot Search Engine (Fixed)

* **The Problem**: Spreadsheets typically list teams by city name only (e.g. `Boston`, `Detroit`, `Cleveland`), while users frequently search by mascot (`Celtics`, `Pistons`, `Cavaliers`), abbreviation (`BOS`, `DET`, `CLE`), or common nicknames (`Cavs`, `Sixers`, `Dubs`). The previous search performed only a strict literal check (`g.home.includes(q)`), causing all mascot and abbreviation searches to yield zero results.
* **The Fix**: 
  - Integrated `NBA_TEAMS_DATA` covering all 30 NBA franchises with cities, mascots, full names, abbreviations, and common nicknames.
  - Added `getTeamSearchTokens()` and `gameMatchesSearch()` to tokenize queries and match any combination of city, mascot, abbreviation, or target pick team.
  - Multi-word search support now understands matchup queries such as `BOS @ DET`, `Lakers Warriors`, or `Boston Celtics`.
  - Searches automatically filter both the **Games** board and the **Summary** table.

## 2. Matchup Deep-Dive Drawer

Every matchup card can now be expanded into a comprehensive analytical drawer by clicking the matchup header or the **`📊 Open Matchup Deep-Dive`** button.

### What is Included in the Deep-Dive Drawer:

1. **Head-to-Head & Splits Matrix**:
   - **Season Series Status**: Dynamically extracts season series records directly from the ESPN Scoreboard API (e.g., `BOS leads 2-0` or `Series tied 1-1`).
   - **Overall Record**: Displays season records for both teams.
   - **Home vs. Road Splits**: Highlights the away team's record specifically on the road, contrasted with the home team's record at home.

2. **Advanced Efficiency Ratings (per 100 possessions)**:
   - **Offensive Rating (ORtg)**: Points scored per 100 possessions.
   - **Defensive Rating (DRtg)**: Points allowed per 100 possessions (color-coded, with lower values highlighted as superior defense).
   - **Net Rating (NetRtg)**: Cumulative point differential per 100 possessions ($$ \text{ORtg} - \text{DRtg} $$).
   - **Pace (Possessions/48 min)**: Team tempo ratings.
   - **Projected Game Pace**: Calculated expected pace for the matchup:
     $$ \text{Projected Pace} = \frac{\text{Away Pace} + \text{Home Pace}}{2} $$
   - **Scoring Baseline**: PPG vs. Opponent PPG comparisons.

3. **Prediction Models Breakdown**:
   - Sub-tab inside the drawer allows switching directly to the Tier 1, Tier 2, and Tier 3 model projections matrix.

## 3. Dynamic Team Record Enrichment from ESPN Scoreboard

* The ESPN Scoreboard API parsing engine (`fetchLiveScores`) now extracts:
  - `competitor.records` (total, home split, away split).
  - `competitions[0].series.summary` (head-to-head season series).
* Automatically falls back to the embedded 30-team baseline ratings database if ESPN records have not yet posted.

## 4. Game Date Calibration & Auto-Reconciliation Fix

* **The Problem**: Late-night NBA games (finishing past midnight) or games viewed the morning after completion were receiving the user's *current clock date* (e.g. `2026-11-06`) instead of the game's true calendar date (`2026-11-05`), preventing games from appearing under the **Yesterday** filter.
* **The Fix**: 
  - All game grading now references `getGameDateIso(matchedEvent.date)` configured to `America/New_York` timezone, which guarantees that all evening and late-night games lock to their official scheduled date.
  - Added an automated date reconciliation loop that automatically checks past records in the cloud against ESPN scoreboard events and updates any `2026-11-06` tags on 11/5 games back to `2026-11-05`.

## 5. Permanent Cloud Ledger (Deletion Controls Removed)

* **Zero Deletion / Permanent Integrity**: The "Action" column and "Delete" (`✕`) buttons have been removed from the Record tab.
* All games graded into the cloud ledger remain permanent and immutable across all devices.