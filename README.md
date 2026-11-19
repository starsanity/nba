# 🏀 NBA Analyzer (v1.3)

An automated NBA spread analytics, live market line comparison, Closing Line Value (CLV), and multi-tier prediction model tracking application powered by Google Sheets, live ESPN scoreboard/odds feeds, and Google Cloud Firestore.

---

## Core Features

### 1. Matchup Selector & Full Slate View
- **Focused Single-Matchup View**: Select any matchup from the main dropdown (or cycle with `◀` / `▶`) to inspect that game's Tier Cover consensus, CLV tracker, and efficiency breakdown without clutter.
- **Display All Games (`📋 Display All Games`)**: Expand the full daily slate into a responsive multi-card grid with quick-filter pills (`All Matchups`, `🔴 Live Now`, `Strong Bets`, `Leans`) and multi-token franchise search (city, mascot, or abbreviation).

### 2. Tier Cover & Edge Recommendation Classification
- Recommendations (**STRONG BET** or **LEAN**) are determined by unanimous model cover agreement combined with Tier average spread edge against the live ESPN Sportsbook line (with PredictionTracker.com sheet lines automatically converted from `+` Home Favored / `-` Visitor Favored into standard Sportsbook spread notation):

| Classification | Rule Condition | Description |
|---|---|---|
| **STRONG BET** | $\text{Tier 1 Cover} = 100\%\text{ same side} \land |\text{Tier 1 Avg} - \text{Current Line}| \ge 2.0\text{ pts}$ **OR** $\text{Tier 1 Cover} < 100\% \land \text{Tier 2 Cover} = 100\%\text{ same side} \land |\text{Tier 2 Avg} - \text{Current Line}| \ge 3.0\text{ pts}$ | High-confidence play where $100\%$ of Tier 1 models cover the same side with at least a $2.0$-point Tier 1 average edge, OR Tier 1 is under $100\%$ while $100\%$ of Tier 2 models cover the same side with at least a $3.0$-point Tier 2 average edge. |
| **LEAN** | Anything that does not meet Strong Bet criteria | Actionable model lean on any matchup that does not satisfy the Strong Bet cover and point-edge thresholds above. |

- **Zero Passes**: Every matchup on the board produces an actionable pick (`STRONG BET` or `LEAN`).

### 3. Individual Prediction Model Leaderboard & Dynamic ATS Tiers
- **Automatic Google Sheet Discovery**: Automatically detects and tracks all prediction model columns present in your connected Google Sheet—including any new models added later in the season.
- **Daily Auto-Grading & Tier Reassignment**: As games go final, each model's ATS pick is graded automatically in Cloud Firestore and assigned to its active tier based on Season-To-Date ATS win percentage:
  - **Tier 1 (Top 3 ATS% Excl. New Models)**: Top 3 highest ATS win percentage models among qualified models ($\ge 25$ games predicted).
  - **Tier 2 ($> 50.00\%$ ATS Excl. Tier 1 & New Models)**: Any remaining qualified model ($\ge 25$ games predicted) with an ATS win rate above $50.00\%$.
  - **Tier 3 ($< 50.00\%$ ATS Excl. New Models)**: Any qualified model ($\ge 25$ games predicted) with an ATS win rate below $50.00\%$.
  - **New Model ($< 25$ Games Predicted)**: Newly added sheet models tracked in `NEW MODEL` status (and excluded from Tiers 1–3) until reaching $25$ predicted games.
- **Recent Form Tracking**: Displays **Today ATS**, **Yesterday ATS**, and rolling 14-day **Last 2 Weeks** ATS records for every model.

### 4. Closing Line Value (CLV) & Line Movement Tracker
- Available inside the **Matchup Deep-Dive Drawer** whenever an individual game is selected.
- Captures the opening spread baseline, tracks live ESPN spread shifts with timestamps, locks the closing spread at tip-off, and calculates net CLV points (`+CLV` / `-CLV`) relative to the target pick.

### 5. Matchup Deep-Dive Drawer & Live Cover Probability
- **Ratings & H2H**: Season series summary, overall and home/road split records, Offensive/Defensive/Net Ratings per 100 possessions, and projected game pace.
- **Live In-Game Win/Cover Tracker**: Real-time cover margin and time-decay normal CDF cover probability bar during live games.

### 6. Automated Cloud Firestore Historical Ledger
- Automatically grades completed games using official `America/New_York` game dates and syncs all outcomes, scores, and model logs across all devices via Google Cloud Firestore (`nba_tracker_shared/main`).
- Preserves all historical games permanently—including season rematches—with zero manual saving or importing required.

---

## Version 1.3 Release Highlights
- Interactive **`v1.3`** header badge to view `README.md` directly inside the app.
- Tier Cover & Edge Recommendation Engine (**Strong Bet**: Tier 1 Cover = $100\%$ on the same side and $|\text{Tier 1 Average} - \text{Current Line}| \ge 2.0$ pts, or Tier 1 Cover $< 100\%$ and Tier 2 Cover = $100\%$ on the same side and $|\text{Tier 2 Average} - \text{Current Line}| \ge 3.0$ pts; **Lean**: anything that does not meet Strong Bet criteria).
- Streamlined main matchup dropdown defaulting to a clean selector prompt with **`📋 Display All Games`** option.
- Single-matchup **Closing Line Value (CLV) & Line Movement Tracker**.
- Dynamic **Prediction Model Leaderboard** with Top 3 Tier 1, $>50\%$ Tier 2, $<50\%$ Tier 3, 25-game New Model graduation rules, **Yesterday ATS**, and **Last 2 Weeks** columns.
- Rematch-safe historical cloud synchronization with read-before-write protection and instant cache mirroring.