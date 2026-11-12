# 🏏 SupeRR Selector Season 2: Rajasthan Royals IPL 2027 Auction Strategy

## 📌 Project Overview
This data-driven analytics system was built for the **Unstop SupeRR Selector Season 2** challenge. The objective is to identify and rank T20 batters and bowlers who most reliably perform under pressure, utilizing historical IPL and T20 International data (2022–2026), to guide Rajasthan Royals' (RR) retention and auction strategy ahead of the IPL 2027 mini-auction.

---

## 📂 Repository Structure & Datasets
The analysis processes delivery-level and match metadata from the supplied competition files:
* `ball_by_ball.csv` / `ball_by_ball_ipl_only.csv`: Delivery-level records (200,999+ deliveries)
* `matches.csv`: Match metadata, venues, toss details, and DLS/revised targets
* `competitions.csv`: Competition IDs, formats, and seasons (IPL 2022–2026 & T20I series)
* `playing_eleven.csv`: Playing XIs, impact player tags, and captaincy records
* `competition_team_player_mapping.csv`: Player mapping across teams and seasons
* `teams.csv`: Team names and IDs

---

## 🔬 Methodology & Core Models

### 1. Zero Data Leakage Pressure Definition
To evaluate pressure without hindsight bias, every delivery was classified using **pre-delivery match state** (`shift(1)` within innings). Across 836 senior matches, **39.86%** of deliveries met strict match-state criteria:
* **1st Innings (Setting a Target):** Powerplay collapses ($\ge 3$ wickets in 36 balls), middle-overs rebuilds ($\ge 4$ wickets down or CRR $< 6.2$), and death-overs acceleration (final 24 balls).
* **2nd Innings (Chasing):** Steep required run rates ($\text{RRR} \ge 11.0$), tight death chases ($\le 30$ balls remaining with $\text{RRR} \ge 8.5$), and wicket-depleted collapses.

### 2. Contextual Par (xRuns) & Empirical Bayes Shrinkage
* **Contextual Benchmarking:** Raw strike rates and economies penalize players due to era scoring inflation (IPL RPO surged from 7.72 in 2022 to 8.95 in 2026). All metrics were measured against Expected Runs (`xRuns`) for their exact `(competition_tier, season, innings, phase, pressure_state)` cell.
* **Sample Size Control:** Enforced minimum thresholds (180+ pressure balls, 12+ pressure matches, 60+ IPL pressure balls) and applied **Empirical Bayes Shrinkage** ($C \approx 410$ prior weight) to prevent small-sample anomalies from distorting rankings.

### 3. Pressure Performance Index (PPI) Formula
Active players (excluding retirees like Dinesh Karthik, Andre Russell, and Mohit Sharma) were scored on a 0–100 Z-scored index:
* **Batters:** 35% Context-Adjusted SR vs. Par, 20% Shrunk Pressure Average, 15% Clutch SR Lift, 15% Boundary-Dot Control, 15% Volume & Match Consistency.
* **Bowlers:** 35% Context-Adjusted Economy Saved vs. Par, 25% Shrunk Wickets per 4 Overs, 15% Clutch Economy Lift, 10% Dot-Boundary Control, 15% Volume & Match Consistency.

---

## 🏆 Final Player Rankings (Unstop Submission Order)

### Top 10 Batters Under Pressure
1. **Suryakumar Yadav** (PPI: 88.05 | Shrunk Avg: 40.06 | SR vs Par: +38.76)
2. **Tim David** (PPI: 79.08 | Shrunk Avg: 35.65 | Pressure SR: 180.78)
3. **Shashank Singh** (PPI: 75.47 | Shrunk Avg: 47.02 | Clutch Lift: +46.89)
4. **Naman Dhir** (PPI: 74.43 | Pressure SR: 195.19 | SR vs Par: +42.35)
5. **Shreyas Iyer** (PPI: 70.04 | Shrunk Avg: 29.32 | Pressure SR: 172.07)
6. **Heinrich Klaasen** (PPI: 68.30 | Pressure Runs: 1,348 | SR vs Par: +23.01)
7. **Namboori Tilak Varma** (PPI: 67.86 | Pressure Runs: 1,791 | Innings: 79)
8. **David Miller** (PPI: 67.48 | Pressure Runs: 1,450 | Clutch Lift: +36.11)
9. **Quinton de Kock** (PPI: 67.20 | Pressure SR: 175.32 | Shrunk Avg: 35.70)
10. **Phil Salt** (PPI: 64.10 | Pressure SR: 187.62 | SR vs Par: +42.30)

### Top 10 Bowlers Under Pressure
1. **Jasprit Bumrah** (PPI: 87.32 | Econ: 7.14 | Runs Saved/Ov vs Par: +2.19)
2. **Matheesha Pathirana** (PPI: 86.70 | Shrunk Wkts/4Ov: 1.87 | Econ: 7.77)
3. **Kuldeep Yadav** (PPI: 84.16 | Pressure Wkts: 69 | Econ: 7.33)
4. **Jamie Overton** (PPI: 78.38 | Clutch Lift: +1.88 RPO | Econ: 7.37)
5. **Sunil Narine** (PPI: 75.77 | Pressure Econ: 6.52 | Runs Saved/Ov vs Par: +2.00)
6. **Arshdeep Singh** (PPI: 73.77 | Pressure Wkts: 107 | Spells: 128)
7. **Varun Chakravarthy** (PPI: 73.15 | Pressure Wkts: 74 | Econ: 7.62)
8. **Mitchell Santner** (PPI: 71.91 | Pressure Econ: 6.72 | Runs Saved/Ov vs Par: +1.14)
9. **Rashid Khan** (PPI: 67.95 | Pressure Wkts: 85 | Spells: 91)
10. **Mustafizur Rahman** (PPI: 67.69 | Runs Saved/Ov vs Par: +1.30 | Econ: 7.69)

---

## 🎯 Rajasthan Royals IPL 2027 Auction Strategy
An audit of RR’s 2026 squad against the PPI model reveals that the franchise lacks top-tier pressure depth, with high-salary retentions like Sam Curran and Shimron Hetmyer exhibiting negative pressure lift. Recommended moves for the December 2026 mini-auction:
1. **Target a Death-Overs Finisher:** Release underperforming overseas assets to pursue chase anchors like **David Miller** or trade for domestic powerhouse **Ashutosh Sharma**.
2. **Upgrade Death Bowling:** Secure confirmed auction entrant **Mustafizur Rahman** (+1.30 runs saved/over) to partner Jofra Archer and fix high death-over economy rates.
3. **Reinforce Spin Control:** Target defensive left-arm control spinners like **Mitchell Santner** to anchor middle-to-death overs.

---
*Generated via Google Colab & Python (Pandas, NumPy, SciPy, Matplotlib).*
