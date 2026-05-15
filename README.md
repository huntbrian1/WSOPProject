# WSOP Main Event Analytics

Analysis of WSOP Main Event field growth, payout structure, fee drag, simulated variance, and event-time player segmentation.

## What This Project Shows

This project combines event-level economics with a separate player-status label layer. The goal is to show how the Main Event changed as a market: larger fields, broader prize distribution, measurable rake, and rare but important amateur breakthrough outcomes.

## Headline Results

- Field size grew from **6 entries in 1971** to **9,735 in 2025**.
- The largest field in the dataset was **10,112 entries in 2024**.
- Winner share of prize pool declined from **100.0%** to **11.0%** across the dataset window.
- Champion labels classify **47 Pro**, **7 Amateur**, and **1 Uncertain-Crossover** results.
- In heads-up finals from 2001-2025, Pro vs Pro was the most common matchup type, followed by Pro vs Amateur and Amateur vs Pro.

## Repository Structure

```text
wsop-main-event-analytics/
  README.md
  REPORT.md
  notebooks/
    WSOP_Main_Event_Analytics.ipynb
  data/
    labels/
      wsop_champion_eventtime_labels.csv
      wsop_headsup_eventtime_labels.csv
    processed/
      wsop_yearly_economics_with_status.csv
      wsop_simulated_player_cohort.csv
      wsop_dataset_reconciliation_summary.csv
      wsop_dataset_reconciliation_detail.csv
      wsop_headsup_mix_by_year.csv
      wsop_era_champion_status_summary.csv
      wsop_uncertainty_stress_test.csv
      wsop_key_metrics.csv
  outputs/
    charts/
      field_size_trend.png
      winner_share_trend.png
      rake_percentage.png
      champion_status_timeline.png
      heads_up_mix_counts.png
      simulated_roi_by_volume.png
      skill_vs_cash_rate.png
```

## How to Run

Open `notebooks/WSOP_Main_Event_Analytics.ipynb` and run all cells from the repository root. The notebook includes the reconciliation checks, label tables, segmentation summaries, charts, simulation outputs, and limitations. It reads the included CSVs and regenerates the derived tables and charts.

Required Python packages:

```bash
pip install pandas numpy matplotlib scipy
```

## Notes

The source result files contain tournament outcomes, not player occupations. Player status is therefore handled through separate label files with confidence notes. The project uses event-time labels rather than permanent lifetime identities.
