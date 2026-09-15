# ⚽ Premier League Performance & Results Sustainability Analysis

## Overview

A team's recent results show how many points it has earned, but they may not
fully represent the quality of its underlying performances.

This project investigates whether underlying performance metrics such as
expected goals (xG), expected goals against (xGA), shots, shots on target,
and possession provide additional information about a Premier League team's
future results.

Using five seasons of Premier League match data, I compare a results-only
baseline model against a model incorporating underlying performance metrics
to predict points earned over a team's following five matches.

---

## Research Questions

1. How predictive are recent results of points earned over the following five matches?
2. Do underlying performance metrics improve predictions beyond recent results alone?
3. Can disagreement between recent results and underlying performance help identify potentially unsustainable runs of form?

---

## Dataset

The original dataset contained **4,788 team-match observations** across
multiple Premier League seasons.

During preprocessing, inconsistencies in season labels revealed hidden
duplicate match records. Seasons were reconstructed using match dates and
logical duplicates were removed.

The cleaned dataset contains:

- **3,800 team-match observations**
- **5 Premier League seasons**
- Match statistics including xG, xGA, possession, shots, shots on target,
  goals, results, opponents, and venue

After constructing rolling five-match features and requiring both five
previous and five future matches, **2,800 observations** were available for
modeling.

---

## Methodology

### 1. Data Cleaning

- Checked missing and invalid values
- Identified hidden duplicate match observations
- Reconstructed unreliable season labels from match dates
- Removed logical duplicates
- Validated numerical and categorical variables

### 2. Feature Engineering

For each team and season, rolling statistics were calculated using the
**previous five matches**.

Features included:

- Points
- Expected goals (xG)
- Expected goals against (xGA)
- Possession
- Shots
- Shots on target

The prediction target was the number of points earned over the
**following five matches**.

Rolling features were shifted so that information from the current or future
matches could not enter the historical feature window.

### 3. Time-Aware Model Evaluation

Instead of randomly splitting matches, earlier seasons were used for training
and the **2023–24 Premier League season was held out for testing**.

Two linear regression models were compared:

**Results-Only Baseline**

Previous 5-match points → Next 5-match points

**Results + Performance Model**

Previous 5-match points + xG + xGA + possession + shots + shots on target
→ Next 5-match points

---

## Results

| Metric | Results Only | Results + Performance |
|---|---:|---:|
| MAE | 2.762 | **2.540** |
| RMSE | 3.323 | **3.106** |
| R² | 0.117 | **0.228** |

Adding underlying performance metrics produced:

- **8.03% lower MAE**
- **6.51% lower RMSE**
- An increase in R² from **0.117 to 0.228**

These results suggest that underlying performance contains predictive
information about future results beyond recent points alone.

---

## Sustainability Analysis

A performance-implied points estimate was created using underlying metrics,
and compared with the points teams actually earned over their previous five
matches.

A positive gap indicates that recent results exceeded the level historically
associated with the team's underlying performance, while a negative gap
indicates the opposite.

The results gap had a **-0.539 correlation** with subsequent changes in
five-match points.

Because recent points are mathematically involved in both measures, this
relationship is treated as descriptive evidence rather than an independent
causal result.

The stronger evidence comes from the out-of-sample model comparison, where
adding underlying performance metrics improved predictions on the held-out
season.

---

## Key Takeaway

**Recent results alone do not tell the entire story.**

Across the held-out Premier League season, incorporating underlying
performance indicators reduced prediction error compared with using recent
points alone.

This suggests that metrics such as xG, xGA, shooting, and possession can
provide useful context when evaluating whether a team's recent run of results
is likely to continue.

---

## Tools

- Python
- Pandas
- NumPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

---

## Limitations

The model does not explicitly account for opponent strength, injuries,
player availability, tactical changes, transfers, or managerial changes.

Five-match windows can also contain substantial short-term variation, and
the model was evaluated on one held-out Premier League season.

Future versions could incorporate opponent-adjusted metrics, additional
seasons, and nonlinear machine-learning models.

---

## Project Structure

```text
premier-league-performance-analysis/
│
├── README.md
├── premier_league_analysis.ipynb
└── data/
    └── matches.csv

## Author

**Nathaniel Ebare**

Computer Science student interested in Data Science, Machine Learning,
and Software Engineering.
