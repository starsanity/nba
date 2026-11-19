# Version 1.3 Release Notes — NBA Analyzer

This document highlights all new features, UI enhancements, and architectural updates implemented in **Version 1.3** of **NBA Analyzer**.

## 1. Tier Cover Recommendation Engine (Delta Gatekeeper Removed)

* **Removed Delta Gatekeeper Classification**: Replaced point-spread delta thresholds with unanimous tier cover agreement rules:

  * **Strong Bet**: Triggered when **Tier 1 = 100% cover**, OR when **Tier 1 (< 100% & $\ge 50\%$ cover) & Tier 2 = 100% cover** on the target side (requiring at least 50% of Tier 1 models to cover alongside 100% of Tier 2 models).

  * **Lean**: Triggered when **Tier 1, Tier 2, or Tier 3 < 100% cover** (including games where Tier 2 = 100% cover but fewer than 50% of Tier 1 models cover on that target side).

* **Card & Dropdown Tier Cover Readouts**: Displays `T1 %`, `T2 %`, and `T3 %` cover agreement directly on every game card and inside the matchup dropdown selector.

## 2. Individual Prediction Model Leaderboard & ATS Win % Tier Rules

* **Updated ATS Win % Tier Thresholds & Sample Size Graduation**:

  * **Tier 1 (**$\ge 54.00\%$ **ATS)**: Core consensus models with at least 25 total games predicted (`agatha.gg`, `Versus Sports Simulator`, `Sagarin Predictor`).

  * **Tier 2 (**$50.00\% - 53.99\%$ **ATS)**: Secondary consensus models with at least 25 total games predicted (`DirectorOfInformation.com`, `Game Time Decision`, `Dokter Entropy`, `Sagarin Rating`, `Sagarin Golden Mean`, `System Average`, `Sonny Moore`, `Massey Ratings`, `D Ratings.com`).

  * **Tier 3 (**$\le 49.99\%$ **ATS)**: Contrarian check models with at least 25 total games predicted (`Sagarin Recent`, `Dunkel Index`, `ESPN BPI`, `Computer Adjusted`, `Talisman Red`, `Model Edge Sports`).

  * **New Model (**$< 25$ **Total Games Predicted)**: Any newly added Google Sheet model starts in **`NEW MODEL`** status (`X/25 GP`), is tracked and graded daily in the background, and automatically graduates into Tier 1, Tier 2, or Tier 3 once it reaches **25 total games predicted**.

* **Recent Form Columns (Yesterday ATS & Last 2 Weeks)**:

  * Synchronized **Season To Date**, **Yesterday ATS** (`2026-11-17`), and rolling 14-day **Last 2 Weeks** ATS records directly on the **MODELS** leaderboard, with dedicated sort pills and daily automatic updates from `2026-11-18` onward.

* **Automatic New Sheet Model Discovery (`buildSheetModelCatalog`)**: Automatically scans your Google Sheet every 5 minutes (or on manual **`↻ Sync Sheet`** click) so any newly added prediction model columns are discovered, graded daily in Cloud Firestore (`modelGameLogs`), and ranked on the **MODELS** leaderboard.

* **Baseline Epoch Calibration (`2026-11-18`)**: Anchors the updated Season-To-Date, Yesterday (`2026-11-17`), and Last 2 Weeks totals to `2026-11-18` so only new games graded from today onward increment those records—preventing any double-counting of prior historical logs.

## 3. Streamlined Matchup Dropdown & Focused Single-Game View

* **Default Selector Prompt**: Opens directly to the clean matchup selector prompt (`— Click to Select a Matchup or Display All —`) with **`📋 Display All Games`** as an option inside the dropdown and `◀` / `▶` cycling controls.

* **Contextual Filter Bar**: Quick-filter pills (`All Matchups`, `🔴 Live Now`, `Strong Bets`, `Leans`) and multi-token team search appear only when **Display All Games** is active.

## 4. Single-Matchup Closing Line Value (CLV) & Line Movement Tracker

* **Contextual CLV Module**: Appears inside the **Matchup Deep-Dive Drawer** when an individual matchup is selected, displaying Opening Spread, Live/Locked Closing Spread, Model Line, net `+CLV` / `-CLV` points relative to the target pick, and a chronological line movement timeline.

## 5. Historical Ledger Cloud Protection & Read-Before-Write Merge

* **Authenticated Cloud Sync & In-Memory Persistence**: Enables `firebase.auth.Auth.Persistence.NONE` fallback authentication so Cloud Firestore listeners and REST API hydration authenticate seamlessly across both `file:///` and hosted environments without `"Cloud Read Restricted"` blocks.

* **Read-Before-Write Cloud Merge & Snapshot Guard (`saveGradesToCloud`)**: Prevents startup score checks from writing to `nba_tracker_shared/main` before existing cloud records are loaded, and fetches the latest server document before every write so historical matches are never overwritten.

* **Pure Automated Ledger**: Preserves all historical game records permanently across season rematches under date-prefixed keys (`YYYY-MM-DD__Road__Home`) with zero manual entry or deletion controls.