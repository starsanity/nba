# 🏀 NBA Analyzer (v1.3)

An automated NBA spread analytics, live market line comparison, Closing Line Value (CLV), and multi-tier prediction model tracking application powered by Google Sheets, live ESPN scoreboard/odds feeds, and Google Cloud Firestore.

---

## Core Features

### 1. Matchup Selector & Full Slate View
- **Focused Single-Matchup View**: Select any matchup from the main dropdown (or cycle with `◀` / `▶`) to inspect that game's Delta edge, CLV tracker, and efficiency breakdown without clutter.
- **Display All Games (`📋 Display All Games`)**: Expand the full daily slate into a responsive multi-card grid with quick-filter pills (`All Matchups`, `🔴 Live Now`, `Strong Bets`, `Leans`) and multi-token franchise search (city, mascot, or abbreviation).

### 2. Delta Gatekeeper Classification
$$ \Delta = |\text{Model Consensus Spread} - \text{Live ESPN Line}| $$

| Delta Range | Classification | Description |
|---|---|---|
| $\ge 4.0\text{ pts}$ | **STRONG BET** | High-confidence edge where top-tier model consensus significantly deviates from live ESPN pricing. Downgraded to **LEAN** if Tier 3 contrarians oppose. |
| $0.0 - 3.5\text{ pts}$ | **LEAN** | Actionable model lean detecting edge against the market spread. |

- **Zero Passes**: Every matchup on the board produces an actionable pick (`STRONG BET` or `LEAN`).

### 3. Individual Prediction Model Leaderboard & Dynamic ATS Tiers
- **Automatic Google Sheet Discovery**: Automatically detects and tracks all prediction model columns present in your connected Google Sheet—including any new models added later in the season.
- **Daily Auto-Grading & Tier Reassignment**: As games go final, each model's ATS pick is graded automatically in Cloud Firestore and assigned to its active tier based on Season-To-Date ATS win percentage:
  - **Tier 1 ($\ge 54.00\%$ ATS)**: Core consensus models ($\ge 25$ games predicted).
  - **Tier 2 ($50.00\% - 53.99\%$ ATS)**: Secondary consensus models ($\ge 25$ games predicted).
  - **Tier 3 ($\le 49.99\%$ ATS)**: Contrarian check models ($\ge 25$ games predicted).
  - **New Model ($< 25$ Games Predicted)**: Newly added sheet models tracked in `NEW MODEL` status until reaching 25 predicted games.
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
- Streamlined main matchup dropdown defaulting to a clean selector prompt with **`📋 Display All Games`** option.
- Single-matchup **Closing Line Value (CLV) & Line Movement Tracker**.
- Dynamic **Prediction Model Leaderboard** with auto-discovery of new Google Sheet models, 25-game graduation rules, **Yesterday ATS**, and **Last 2 Weeks** columns.
- Rematch-safe historical cloud synchronization with Firebase authentication and instant cache mirroring.