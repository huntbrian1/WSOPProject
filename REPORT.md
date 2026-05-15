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


# Skill, Variance, and Fee Drag: A Quantitative Analysis of WSOP Main Event Economics

## Executive Summary
## Why This Project Matters
## Data Sources and Method
## Dataset Reconciliation
## Event-Time Player Labeling Framework
## Historical Economics Analysis
  - Field Size and Market Scaling
  - Winner Share and Payout Concentration
  - Rake and Fee Drag
## Professional vs Amateur Segmentation
  - Champion Status
  - Heads-Up Matchups
  - Ambiguous Cases and Sensitivity Checks
## Simulation: Skill, Volume, and Realized Outcomes
## Key Findings
## Limitations
## Conclusion

# Skill, Variance, and Fee Drag: A Quantitative Analysis of WSOP Main Event Economics

## Executive Summary

This project evaluates the WSOP Main Event not simply as a poker tournament, but as a market-design and performance-measurement problem.

Using WSOP Main Event data from 1971–2025, I analyze how field size, payout concentration, winner economics, and estimated rake evolved as the event scaled from a small-field competition into a mass-market tournament. I also add an event-time professional/amateur labeling layer to understand how player composition appears in champion and heads-up outcomes.

The final section uses a simulated player cohort to examine a harder question: even if skill exists, how quickly does that skill become visible in realized outcomes once variance, limited sample size, and fee drag are introduced?

The core conclusion is that poker behaves like many real business environments: structural economics matter, edge can exist while remaining hard to observe, and decision-makers can easily mistake noisy short-run outcomes for durable signal.

## Why This Project Matters

At surface level, this is a poker analysis. At a higher level, it is a study of market structure, incentives, and performance measurement under uncertainty.

The WSOP Main Event is useful because its economics are unusually visible. Buy-ins, prize pools, field sizes, payouts, and final placement outcomes can all be observed over time. That makes the tournament a clean case study for business questions that appear in pricing, marketplace design, investing, talent evaluation, and performance attribution.

The project focuses on three questions:

1. **How does scale change economics?**  
   As participation increases, the economics of the tournament change even if the rules of the game do not. Payout concentration falls, the participant base broadens, and the experience of playing the event becomes structurally different.

2. **How do fees distort expected value?**  
   In poker, that friction is rake. In business, similar frictions appear as platform fees, distribution costs, commissions, customer acquisition costs, payment processing, or marketplace take rates.

3. **How much evidence is needed before performance can be trusted?**  
   Small samples create false confidence. Real skill often reveals itself slowly, while random success can create misleading narratives in the short run.

This makes the project relevant beyond poker. The same analytical challenge appears whenever performance is noisy, incentives matter, and outcomes are shaped by both skill and structure.

## Data Sources and Method

The project uses two main WSOP result files:

| Dataset | Purpose |
|---|---|
| Full Main Event results, 1971–2025 | Used to build the year-level economics panel |
| Final-table results, 2001–2025 | Used to analyze heads-up player matchups and late-stage player composition |

The historical economics panel is built at the year level. For each Main Event year, I preserve event-level fields such as entries, buy-in, and prize pool, then merge in the winner payout from the first-place row.

From this panel, I derive:

- winner share of prize pool
- estimated rake percentage
- winner return relative to buy-in
- era classification
- champion status label
- heads-up matchup type

The simulation section creates a 5,000-player cohort with an underlying skill parameter, variable tournament-entry counts, binomial cash outcomes, and lifetime ROI. The goal is not to claim that the simulation recreates actual WSOP performance. The goal is to test how skill signal behaves once variance and sample size are layered into the system.

## Dataset Reconciliation

Before interpreting results, the notebook reconciles the available result files and checks whether they are being used for the right analytical purpose.

The full results file is treated as the backbone for historical economics because it contains yearly event-level fields such as entries, buy-in, prize pool, and player placements. The final-table file is used primarily for heads-up and late-stage segmentation because it cleanly identifies finalists from 2001 onward.

This distinction matters. Using both files without validation would risk double-counting or mixing incompatible levels of detail. The project therefore separates:

- event-level economics
- champion outcomes
- heads-up finalist outcomes
- manual event-time status labels

The key methodological choice is to use the raw CSVs as the authority for who finished where, in what year, and for what payout, while using a separate label layer for professional/amateur status.

## Event-Time Player Labeling Framework

The source result files contain tournament outcomes, not player occupations. They identify who finished where and how much they were paid, but they do not contain clean fields such as occupation, biography, prior live earnings, or satellite qualifier status.

For that reason, professional/amateur segmentation is handled through a separate label layer. This keeps the classification assumptions auditable instead of burying them inside code.

The labels use an event-time framework:

- **Pro:** player was clearly understood as a professional poker player, online pro, or full-time poker player at the time of the event.
- **Amateur:** player was clearly described as amateur, recreational, or primarily tied to a non-poker occupation at the time of the event.
- **Uncertain-crossover:** player had mixed evidence, usually because contemporary coverage foregrounded a non-poker identity while later career evidence points clearly toward professional poker.

This distinction matters because a player’s lifetime poker identity can differ from their event-time status. A breakout Main Event result can itself be the event that changes a player’s career trajectory.

The label layer also includes confidence notes. Ambiguous cases should not be hidden; they should be surfaced and, where possible, tested through sensitivity analysis.

## Historical Economics Analysis

### Field Size and Market Scaling

The field-size trend shows the Main Event changing from a small, invitation-like competition into a mass-market tournament. The important point is not simply that participation increased. The scale of the event changed the economics around the game.

In business terms, this resembles a niche product reaching mainstream distribution. User volume rises, the participant base broadens, and legacy economics no longer describe the system well.

The strategic takeaway is that scale is not just a growth metric. Once scale changes enough, payout design, competitive intensity, and participant outcomes change with it.

### Winner Share and Payout Concentration

As the field scales, the winner captures a smaller share of the total prize pool even when the absolute first-place prize remains large. This separates top-line growth from value concentration.

A larger prize pool does not automatically mean that participants face the same economics. As a market matures, total value can expand while becoming more widely distributed.

The sharper analytical question is not simply whether the prize pool grew. It is who captures the incremental value as the system scales.

### Rake and Fee Drag

Estimated rake isolates the structural fee drag embedded in the tournament. Before any player decision is made, part of the value pool has already been removed.

That makes this less about poker specifically and more about frictional economics. Similar dynamics appear in marketplace take rates, platform fees, advisory fees, payment processing, distribution costs, and customer acquisition costs.

The strategic takeaway is that strong operators think in net terms, not gross terms. If friction is ignored, expected value is overstated.

## Professional vs Amateur Segmentation

The professional/amateur segmentation is designed to add context to historical outcomes without pretending that every player fits neatly into a permanent identity bucket.

The strongest takeaway is not that amateurs never win. The better interpretation is that amateur or crossover champions are rare, historically important, and often clustered around moments where the event’s participant base was changing.

The champion label distribution shows that most Main Event winners are professionals. However, several amateur or non-professional champions are central to the event’s history, including cases such as Hal Fowler, Jim Bechtel, Robert Varkonyi, Chris Moneymaker, Jamie Gold, and Jerry Yang.

The heads-up finalist analysis adds a second layer. Looking only at champions can hide the composition of the final duel. Heads-up matchups help show whether late-stage outcomes were primarily Pro vs Pro, Pro vs Amateur, or Amateur vs Pro.

This is useful because poker’s public narrative often over-focuses on the winner. From an analytics perspective, the composition of the final two players gives a richer view of how often non-professionals actually reach the most important decision point of the tournament.

### Ambiguous Cases and Sensitivity Checks

Some players require special handling. The purpose of the label layer is not to force false certainty, but to make judgment calls transparent.

Examples include:

- **Greg Raymer:** often described through a patent-attorney identity around the 2004 win, but later clearly a professional poker figure.
- **Jim Bechtel:** described as a cotton farmer, but with meaningful prior Main Event experience.
- **Steven Jones:** tied to a real-estate broker identity, but not a pure one-off recreational case.
- **2020:** structurally unusual because of the pandemic split-format Main Event.

These cases should be treated as classification-sensitive observations. The report should therefore avoid overclaiming based on exact counts alone. The stronger conclusion is directional: professional players dominate champion and heads-up outcomes, but amateur/crossover cases are real and analytically important.

## Simulation: Skill, Volume, and Realized Outcomes

The historical data explains tournament economics, but it cannot directly observe latent player skill. The simulation fills that gap by creating a controlled environment where skill, variance, entry volume, cash outcomes, and ROI can be examined together.

The purpose is not to pretend the simulation is reality. The purpose is to test how performance metrics behave under plausible tournament conditions.

This is useful beyond poker. In many business settings, real data tells us what happened, but not always what would happen under alternative assumptions. Simulation is valuable when it is framed as a decision-support tool rather than as false certainty.

The simulation shows that skill and realized cash outcomes are positively related, but the relationship is noisy. Players with very few entries can have extreme outcomes that say more about variance than true ability. As tournament volume increases, realized performance becomes more interpretable.

The broader lesson is about decision hygiene. Small samples generate false stories. A manager, investor, or operator who judges performance from too few observations can easily confuse randomness for skill.

## Key Findings

1. **Scale changed the economics of the Main Event.**  
   The tournament expanded from a small-field event into a mass-market tournament, changing the interpretation of field size, payout structure, and final-table outcomes.

2. **Winner concentration declined as the tournament matured.**  
   The first-place prize remained large in absolute terms, but the winner’s share of the total prize pool declined as participation expanded.

3. **Fee drag is a first-order economic force.**  
   Rake is built into the system and affects expected value before competition begins.

4. **Professional players dominate, but amateur cases matter.**  
   Most champions and heads-up finalists are professionals, but amateur and crossover cases are historically important and analytically meaningful.

5. **Event-time labels are more defensible than lifetime labels.**  
   A player’s status at the time of the event can differ from their later career identity. This is why the project separates raw results from the player-label layer.

6. **Skill exists, but realized performance is noisy.**  
   The simulation produces a positive but imperfect relationship between underlying skill and realized outcomes.

7. **Small samples are dangerous.**  
   Short-run performance can produce false confidence. This lesson generalizes to business settings where decision-makers over-interpret limited data.

## Limitations

This analysis has several important limitations.

First, the source files contain tournament results, not complete biographical or occupational histories. Professional/amateur status therefore requires a separate label layer and cannot be inferred directly from the raw result files alone.

Second, the event-time labels involve judgment. The project reduces this risk by using confidence notes and separating labels from the raw data, but classification uncertainty remains.

Third, the simulation is illustrative rather than predictive. It is designed to show how skill, variance, sample size, and ROI can interact under tournament-like conditions. It should not be interpreted as a calibrated model of actual WSOP player economics.

Fourth, estimated rake is derived from available prize pool, entries, and buy-in fields. It is useful for structural comparison, but it should be interpreted as an estimate rather than as a complete accounting reconstruction.

Finally, special years such as 1970 and 2020 require caution. 1970 was not a standard modern freezeout Main Event, and 2020 used a pandemic-era split format.

## Conclusion

The most interesting question is not whether poker is “skill” or “luck” in a simplistic binary sense. The more useful framing is whether skill can be economically meaningful in a noisy system with structural friction.

This analysis suggests that the answer is yes, but only with patience, scale, and disciplined interpretation.

The WSOP Main Event is a useful case study because the economics are visible. Field size, buy-in, payout structure, rake, player composition, and outcomes can all be examined together.

The broader analytical lesson is that many business problems have the same structure. The hard part is not merely measuring outcomes. The hard part is distinguishing durable edge from randomness while accounting for incentives, sample size, and fee drag.

