# WSOP Project Data Pipeline Notes

## What the two Kaggle datasets can generate directly

Inputs:
- `wsop_main_event_results_1971-2025.csv`
- `wsop_main_event_final_table_2001-2025.csv`

Directly generated from those two raw datasets:
- `data/raw/wsop_main_event_results_1971-2025.csv`
- `data/raw/wsop_main_event_final_table_2001-2025.csv`
- `data/processed/wsop_dataset_reconciliation_detail.csv`
- `data/processed/wsop_dataset_reconciliation_summary.csv`
- `data/processed/wsop_full_results_top2_coverage_check.csv`
- `data/processed/wsop_full_results_expanded_heads_up_base.csv`

Generated from the full-results Kaggle dataset plus champion event-time labels:
- `data/labels/wsop_champion_eventtime_labels.csv`
- `data/processed/wsop_yearly_economics_with_status.csv`
- `data/processed/wsop_era_champion_status_summary.csv`
- `data/processed/wsop_uncertainty_stress_test.csv`
- `data/processed/wsop_key_metrics.csv`

Generated from the final-table Kaggle dataset plus heads-up event-time labels:
- `data/labels/wsop_headsup_eventtime_labels.csv`
- `data/processed/wsop_headsup_mix_by_year.csv`
- `data/processed/wsop_headsup_mix_counts.csv`

Generated synthetically:
- `data/processed/wsop_simulated_player_cohort.csv`

Generated from processed outputs:
- `outputs/charts/field_size_trend.html`
- `outputs/charts/winner_share_trend.html`
- `outputs/charts/rake_percentage.html`
- `outputs/charts/champion_status_timeline.html`
- `outputs/charts/heads_up_mix_counts.html`
- `outputs/charts/simulated_roi_by_volume.html`
- `outputs/charts/skill_vs_cash_rate.html`

## Why pro/amateur labels cannot come only from Kaggle

The Kaggle tournament datasets contain tournament result facts: year, place, name, prize, entries/entrants, buy-in, and prize pool. They do not contain player occupation, bio, prior live earnings, satellite flag, or event-time player identity. Therefore, `status_at_event`, `status_basis`, `confidence`, and `evidence_note` require a separate researched label layer.

## Reconciliation logic

The full-results dataset and final-table dataset overlap because both contain final-table placement rows. The reconciliation audit merges rows on `year + place`, then checks:
- row presence in both files
- exact name match
- normalized name match
- exact prize match
- entrant count match (`final_table.entrants` vs `results.entries`)

This is a discrepancy check, not a vague source-selection exercise.

## Expanded heads-up logic

The current heads-up labeled output uses the final-table source, so it covers 2001-2025. The full-results dataset can create a broader champion/runner-up base for years where both place 1 and place 2 exist. The script exports:
- `wsop_full_results_top2_coverage_check.csv`
- `wsop_full_results_expanded_heads_up_base.csv`

That expanded base is not automatically labeled pro/amateur unless an expanded runner-up label layer is created.
