# WSOP Main Event Analytics

A poker analytics project analyzing the World Series of Poker Main Event as a case study in market structure, payout design, fee drag, variance, and performance measurement.

Using WSOP Main Event results from 1971–2025, the project examines how the tournament changed as it scaled from a small-field competition into a mass-market event. It also adds event-time professional/amateur player labels and a simulated player cohort to study how skill, variance, sample size, and realized outcomes interact.

## Why This Project Matters

This is not just a poker project. The broader analytical question is how to interpret performance in a system where scale, incentives, fees, and variance all affect observed outcomes.

The project connects poker analytics to broader business problems:

- market scaling and payout concentration
- fee drag and expected value
- performance attribution under uncertainty
- noisy outcome measurement
- player segmentation and event-time classification

## Headline Results

- Field size grew from 6 entries in 1971 to 9,735 in 2025.
- The largest field in the dataset was 10,112 entries in 2024.
- Winner share of the prize pool declined from 100.0% in the early years to roughly 11.0% by 2025.
- Champion labels classify most winners as professional, with a smaller number of amateur or uncertain-crossover cases.
- In heads-up finals from 2001–2025, Pro vs Pro was the most common matchup type.
- The simulation shows that skill and realized outcomes are positively related, but short-run results remain highly noisy.

## Project Components

### 1. Historical WSOP Economics

Analyzes field size, buy-in, prize pool, winner share, estimated rake, and event-era changes from 1971–2025.

### 2. Professional vs Amateur Segmentation

Adds a separate event-time label layer for champion and heads-up player status. Labels are treated as contextual classifications rather than permanent player identities.

### 3. Simulation of Skill, Variance, and ROI

Creates a simulated player cohort to test how underlying skill translates into realized cash rates and lifetime ROI under noisy tournament conditions.

## Repository Structure

```text
WSOPProject/
  README.md
  REPORT.md
  requirements.txt
  notebooks/
    WSOP_Main_Event_Analytics.ipynb
  data/
    labels/
    processed/
  outputs/
    charts/