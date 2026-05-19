# WSOP Main Event Analytics

> **Business Question:** As the WSOP Main Event scaled from a 6-player invitational to a 10,000-entry mass-market tournament, did structural economics — payout concentration, fee drag, and participant composition — shift in ways that mirror how marketplace operators, platform businesses, and performance evaluators should think about scale, friction, and signal extraction?

---

## What This Project Does

This project uses WSOP Main Event data (1971–2025) to answer three practical questions that generalize well beyond poker:

1. **How does scale change economics?** Field growth changed payout concentration, winner share, and competitive composition even when the rules of the game stayed the same.
2. **How does fee drag distort expected value?** Estimated rake shows the structural friction baked in before any player decision is made — a direct analogue to platform take rates, AUM fees, and distribution costs.
3. **How much evidence do you need before trusting performance?** A simulated 5,000-player cohort shows that skill signal is real but slow to emerge under tournament variance.

---

## Metric Definitions

| Metric | Definition | Source |
|---|---|---|
| **Entries** | Total paid entrants in the Main Event that year | Full results file |
| **Buy-in** | Entry cost in USD (typically $10,000) | Full results file |
| **Prize Pool** | Total prize money awarded that year | Full results file |
| **Winner Prize** | First-place payout in USD | Full results file |
| **Winner Share** | Winner Prize ÷ Prize Pool, expressed as % | Derived |
| **Estimated Rake %** | (Entries × Buy-in − Prize Pool) ÷ (Entries × Buy-in) | Derived |
| **Winner ROI** | (Winner Prize − Buy-in) ÷ Buy-in | Derived |
| **Era** | Categorical period label (Early, Growth, Modern) | Derived |
| **Champion Status** | Event-time label: Pro / Amateur / Uncertain-Crossover | Manual label layer |
| **Cash Rate** | Simulated: share of entries that finish in the money | Simulation |
| **Skill Percentile** | Simulated: latent skill rank assigned at cohort creation | Simulation |

---

## Headline Results

- Field grew from **6 entries (1971)** to **9,735 (2025)**; peak was **10,112 in 2024**
- Winner's prize-pool share fell from **100%** to **~11%** as the field scaled
- Estimated rake in the latest year: **7.0%** — structural friction before competition begins
- **47 Pro / 7 Amateur / 1 Uncertain-Crossover** champion outcomes across the labeled history
- Skill-vs-cash-rate correlation in simulation: **0.175** — skill is real, but short-run noise dominates

---

## Data Validation

| Check | Method | Result |
|---|---|---|
| Source overlap | Reconciled full-results and final-table files on year + name | Minor name variants flagged; no structural doubles |
| Prize pool integrity | Compared Entries × Buy-in against reported Prize Pool to derive rake | Consistent across modern era; 1970 and 2020 flagged as structural exceptions |
| Label coverage | Champion label file covers all winners with status + confidence note | 55 labeled; 0 missing |
| Heads-up coverage | Heads-up label file covers 2001–2025 | 25 years; all finalists labeled |
| Simulation calibration | Cohort of 5,000 players; binomial cash outcomes; spot-checked against expected cash rates | Distributions within expected range |

> **Special-year flags:** 1970 excluded (non-standard format). 2020 treated as structural exception (pandemic split format).

---

## Key Charts

| Chart | What It Shows |
|---|---|
| `field_size_trend.png` | Main Event scale growth 1971–2025 |
| `winner_share_trend.png` | Payout concentration decline over time |
| `rake_percentage.png` | Fee drag by year |
| `champion_status_timeline.png` | Pro vs Amateur vs Crossover outcomes over eras |
| `skill_vs_cash_rate.png` | Simulation: skill signal vs realized cash rate |

---

## Repository Structure

```
WSOPProject/
  README.md                        ← Start here
  REPORT.md                        ← Full analytical write-up
  WSOP_pipeline_notes.md           ← Pipeline and data decisions
  pipeline.py                      ← Data processing and chart generation
  requirements.txt
  notebooks/
    WSOP_Main_Event_Analysis.ipynb
  data/
    labels/                        ← Manual event-time player labels
    processed/                     ← Derived CSVs from pipeline
    raw                            ← Kaggle data (big files)
  outputs/
    charts/                        ← All chart PNGs
```

---

## Setup

```bash
pip install -r requirements.txt
python pipeline.py
```

**Data sources:** [Full Main Event results (Kaggle)](https://www.kaggle.com/datasets/cviaxmiwnptr/wsop-main-event-results-1971-2024) · [Final table results (Kaggle)](https://www.kaggle.com/datasets/cviaxmiwnptr/wsop-main-event-final-table-20012024)
