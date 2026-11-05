<!-- ... existing code ... -->
# Version 1.1 Release Notes — NBA Analyzer

This document highlights all new features, decision logic updates, and bug fixes implemented in **NBA Analyzer**.

---

## 1. App Rebranding & Nomenclature

* **App Title**: Renamed to **NBA Analyzer** (formerly QuantEdge NBA).
* **Tier 2 Renamed**: "Tier 2 Leans" is now streamlined simply to **Leans**.

---

## 2. Updated Delta Gatekeeper Thresholds

The analytical threshold structure has been updated:

| Delta Range | Classification | Action | Description |
| :--- | :--- | :--- | :--- |
| $\ge 4.0\text{ pts}$ | **Strong Bet** | High Confidence Play | High-confidence premium edge where model consensus significantly deviates from market pricing. |
| $0.0 - 3.5\text{ pts}$ | **Lean** | Solid Model Lean | Solid analytical model lean detecting pricing discrepancy against the line. |

### Key Structural Changes:
* **Noise Pass Removed**: The "Noise Pass / No Play" zone has been eliminated. All games with model projection agreeably produce an actionable classification (**Strong Bet** or **Lean**).
* **Noise Filter & Suppression Count Removed**: The noise filter pill and the suppressed noise count card have been removed from the UI.
* **Balanced Record Grid**: The Record Tab now features 3 symmetrical KPI hero cards:
  1. **Strong Bets** ($\ge 4.0$ pts)
  2. **Leans** ($0.0 - 3.5$ pts)
  3. **Overall Record** (All Plays)

---

## 3. Bug Fixes: Performance Timeline Date Tracking

### Bug Fix 1: Yesterday's Records Missing Under "Yesterday"
* **Root Cause**: When date strings (e.g. `"2026-11-04"`) were parsed with `new Date("YYYY-MM-DD")`, JavaScript parsed them as UTC midnight. In all US timezones (EST, CST, PST), converting back to local time shifted the day back by one (`Nov 3`), preventing yesterday's games from matching the yesterday filter.
* **Resolution**: Implemented timezone-safe string-level ISO extraction (`getLocalIsoDate`, `getTodayIso`, `getYesterdayIso`). Date equality checks are now executed directly on local calendar strings without UTC conversion drift.

### Bug Fix 2: Games Live or Finished Same Day Missing in "Today"
* **Root Cause**: Same-day finished games suffered from the UTC date offset mentioned above, and live in-progress games were only maintained in transient memory without being merged into the historical ledger.
* **Resolution**: 
  * Same-day completed games now immediately bind to the current local calendar date (`getTodayIso()`).
  * Live in-progress games with actionable signals (**Strong Bet** or **Lean**) are now dynamically integrated into the ledger under the **Today** timeframe with a pulsing `LIVE` outcome badge and live score indicator, transitioning to `WIN`, `LOSS`, or `PUSH` as soon as the final buzzer sounds.

---

## 4. Automatic Chronological Tip-Off Scheduling

Matchup cards and summary tables are sorted chronologically by game tip-off time:
* Multi-source time resolution from ESPN and spreadsheet headers.
* Slate progression from early tip-offs to late-night games.