# Version 1.3 Release Notes — NBA Analyzer

This document covers the new features and interface updates introduced in **Version 1.3** of **NBA Analyzer**.

## 1. Individual Prediction Model Leaderboard & Daily Dynamic ATS Tiers (Approved Sheet Models Only)

* **Strict Intersection Filtering (`buildSheetModelCatalog`)**: Automatically cross-references your Google Sheet's active columns against your approved Season-To-Date standings table (`agatha.gg`, `Versus Sports Simulator`, `Sagarin Predictor`, `DirectorOfInformation.com`, `Dokter Entropy`, `Game Time Decision`, `Sagarin Rating`, `Sagarin Golden Mean`, `Sagarin Recent`, `Sonny Moore`, `Massey Ratings`, `System Average`, `D Ratings.com`, `Dunkel Index`, `ESPN BPI`, `Talisman Red`, `Computer Adjusted`, `Model Edge Sports`). Any model not in that table or not present in your Google Sheet is excluded.

* **New `MODELS` Leaderboard Tab**: Ranks your active prediction models by live ATS Win %, Season ATS Record, Today ATS, and Active Tier.

* **Automated Daily ATS Updates & Tier Re-Ranking**: As daily NBA games go final, each active model's ATS pick is graded automatically in Cloud Firestore (`modelGameLogs`). Tiers 1, 2, and 3 dynamically re-sort every day based on live ATS win percentage:

  * **Tier 1 (Core Consensus)**: Top ATS performing models

  * **Tier 2 (Secondary Consensus)**: Middle ATS performing models

  * **Tier 3 (Contrarian Check)**: Lowest ATS performing models

## 2. Main Page Matchup Dropdown Selector & "Display All Games" Option

* **Clean Dropdown Prompt on Open**: Opens directly to a compact matchup selector prompt (`— Click to Select a Matchup or Display All —`) so you can immediately pick the exact game you want to inspect.

* **Single-Matchup Focus View**: Select any individual matchup from the dropdown menu (or step through games using `◀` / `▶`) to focus exclusively on that single matchup card.

* **Display All Games Option (`📋 Display All Games`)**: Choose **Display All Games** inside the dropdown at any time to expand all matchups into a responsive multi-card grid with quick-filter pills and the franchise search bar enabled.

## 3. Closing Line Value (CLV) & Line Movement Tracker (Single-Matchup View)

* **Contextual Visibility**: Renders strictly when an individual matchup is selected from the dropdown (hidden in the multi-game grid view to keep cards compact).

* **Opening vs. Current / Closing Spread Comparison**: Captures the opening spread baseline and tracks real-time ESPN line updates up until tip-off, automatically locking the closing spread once the game tips off.

* **Net CLV Edge Calculation**: Computes exact Closing Line Value in points (`+CLV` when the market moves in favor of the model's target pick, `-CLV` when the market drifts against it) alongside a chronological **Line Movement Timeline** synced in Cloud Firestore.