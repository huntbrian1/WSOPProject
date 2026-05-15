# WSOP Main Event Analytics Report

## Summary

This project analyzes WSOP Main Event economics, payout structure, simulated variance, and event-time player status. The analysis uses a yearly economics panel, separate champion and heads-up label files, a cross-source reconciliation check, and a simulated player cohort. The csvs used are syntheisized from two Kaggle sources, one of which has all of the entrants from 1971-2025, and another of which has the final table contestants from 2001-2025. Here are the links to the original datasets:
https://www.kaggle.com/datasets/cviaxmiwnptr/wsop-main-event-results-1971-2024
https://www.kaggle.com/datasets/cviaxmiwnptr/wsop-main-event-final-table-20012024


## Key Findings

- Main Event field size increased from **6 entries in 1971** to **9,735 entries in 2025**. The largest observed field was **10,112 entries in 2024**.
- Winner prize-pool share fell from **100.0%** in 1971 to **11.0%** in 2025, reflecting the shift from small-field winner-take-most economics to a broader payout structure.
- Since the modern large-field era, rake/fee drag is visible in the event-level economics. The latest year in the file shows an estimated rake of **7.00%**.
- Champion labels show **47 Pro**, **7 Amateur**, and **1 Uncertain-Crossover** outcomes across the labeled champion table.
- In the 2001-2025 heads-up sample, **11** years were Pro vs Pro, **7** were Pro vs Amateur, **4** were Amateur vs Pro, **2** were Pro vs Uncertain-Crossover, and **1** was Uncertain-Crossover vs Pro.
- In the simulated cohort, the correlation between assigned skill percentile and realized cash rate is **0.175**, illustrating how noisy realized outcomes remain even when underlying skill differs.

## Data Assets

| File | Role |
|---|---|
| `data/processed/wsop_yearly_economics_with_status.csv` | Yearly Main Event economics with champion event-time status |
| `data/labels/wsop_champion_eventtime_labels.csv` | Champion label file with status, basis, confidence, and notes |
| `data/labels/wsop_headsup_eventtime_labels.csv` | Heads-up finalist label file for 2001-2025 |
| `data/processed/wsop_dataset_reconciliation_summary.csv` | Cross-source reconciliation summary |
| `data/processed/wsop_dataset_reconciliation_detail.csv` | Row-level reconciliation detail |
| `data/processed/wsop_simulated_player_cohort.csv` | Simulated player cohort for variance and ROI analysis |
| `data/processed/wsop_headsup_mix_by_year.csv` | Derived heads-up matchup table |
| `data/processed/wsop_era_champion_status_summary.csv` | Champion status by era |
| `data/processed/wsop_uncertainty_stress_test.csv` | Reclassification sensitivity table |
| `data/processed/wsop_key_metrics.csv` | One-row project metric summary |

## Method

### Event-level economics

The yearly panel tracks entries, buy-in, prize pool, winner prize, winner share of prize pool, estimated rake percentage, winner ROI, and era. This supports trend analysis across field growth, payout concentration, and fee drag.

### Cross-source check

The project includes a lightweight reconciliation between the full-results and final-table sources. The dataset is small, but checking overlap, name variants, prize differences, and entrant-count consistency is still useful before combining source-specific fields.

### Player-status labels

Player status is treated as an event-time label rather than a permanent identity. The labels use three categories:

| Status | Meaning |
|---|---|
| Pro | Player was meaningfully a professional poker player or professional gambler at the time of the result |
| Amateur | Player's event-time identity was primarily non-poker, recreational, satellite-qualified, or explicitly amateur |
| Uncertain-Crossover | Mixed or borderline case where the event-time evidence does not cleanly support Pro or Amateur |

The source result files do not contain occupation, biography, prior live earnings, or satellite-qualifier fields. Those fields would be useful for a more automated label process, so the current version keeps labels separate and includes confidence notes.

## Charts

- `outputs/charts/field_size_trend.png`
- `outputs/charts/winner_share_trend.png`
- `outputs/charts/rake_percentage.png`
- `outputs/charts/champion_status_timeline.png`
- `outputs/charts/heads_up_mix_counts.png`
- `outputs/charts/simulated_roi_by_volume.png`
- `outputs/charts/skill_vs_cash_rate.png`

## Special-Year Notes

- **1970** is excluded from the main yearly economics panel because it was not a standard freezeout Main Event in the same sense as the later dataset.
- **2020** should be treated as a structural exception because it used a split-format pandemic structure. The label table can include 2020, but payout trend interpretation should treat it carefully.

## Limitations

- The label layer is partly manual because the source result files do not include player occupation or biography fields.
- Some borderline players are intentionally left as Uncertain-Crossover rather than forced into a cleaner binary category.
- The simulation is illustrative, not a direct reconstruction of player-level Main Event probabilities.
- Name normalization reduces matching errors but cannot fully eliminate homonym or spelling-variant risk.
