---
title: "Shot Probability in Possession"
description: "Built a match-grouped logistic regression on 10,999 custom possessions from the 2022 World Cup, achieving 0.824 ROC-AUC, taking 1st of 6 at the BU Sports Analytics Hackathon."
date: 2026-04-01
lastmod: 2026-04-01
draft: false
role: "Modelling and visualization"
period: "Apr 2026"
status: "Completed"
featured: true
category: "Data Analytics"
summary: "A four-person team took StatsBomb's 2022 World Cup event data, defined 10,999 custom possessions, and built a logistic regression that predicted whether each possession ended in a shot. We placed 1st of 6."
stack:
  - Python
  - pandas
  - NumPy
  - scikit-learn
  - Matplotlib
  - mplsoccer
  - Jupyter
links: []
---

## Context

The Boston University Sports Analytics Group runs a yearly hackathon that pairs student teams with real sports data. The 2026 edition asked teams to identify the factors that affect the probability of a possession ending in a shot, judged by analysts from the New England Revolution and U.S. Soccer.

The data was [StatsBomb Open Data](https://github.com/statsbomb/open-data) for all 64 matches of the 2022 FIFA World Cup, including both the regular event stream and the newer 360 freeze-frame data that records player positions at the moment of each event. StatsBomb's built-in possession field wasn't fine-grained enough for the question, so the team had to define its own possession boundaries and engineer features at that level.

We worked as a four-person team. The goal was a defensible model and a presentation that turned the model's outputs into tactical recommendations a professional coach could actually act on.

## Contribution

I built the first end-to-end logistic regression on the possession-level data, and converted the standardized coefficients into odds ratios, then used them to identify the factors most strongly associated with shot-ending possessions. This set the direction for everyone else's later work and informed the rest of the team which features were worth refining. I also contributed to the slide deck and verbal presentation to the judges.

As a team, we defined our custom possession boundaries, converted event-level match data to possession-level records, engineered the spatial, temporal, passing, pressure, restart, and player-involvement features, and produced the tactical visualizations and recommendations.

## Approach

### The data

From the source csv, here are some quick facts about the dataset. As we can see the dataset is quite large.

| Metric | Value |
|---|---:|
| Matches | 64 |
| Event rows | 234,652 |
| Event types | 33 |
| Passes | 68,515 |
| Carries | 53,764 |
| Pressure events | 16,553 |
| Shots | 1,494 |
| Goals | 195 |
| Event rows with 360 visible-area and freeze-frame data | 203,887 |
| Custom possessions | 10,999 |
| Possessions with valid start coordinates | 10,927 |
| Shot-ending possessions with valid start coordinates | 1,331 |
| Possession-level output fields | 20 |

The 20 output fields include identifiers, labels, and engineered features.

### Custom possession definition

A possession is a continuous in-play sequence controlled by one team. The pipeline starts a new possession when a match or period begins, when the possession team changes, when the previous event sends the ball out of play, or when play restarts through a throw-in, free kick, corner, goal kick, kickoff, or goalkeeper restart. Stoppage rows aren't assigned to a possession. The foul-followed-by-free-kick split is one of the trickier rules and is the kind of thing that has to be right before the model can be trusted.

### Features

The team's engineered features cluster into a few groups:

- Time on the pitch: time in the defensive, middle, and attacking thirds
- Passing: number of passes and average pass distance
- Movement: vertical distance advanced and directness
- Restart context: set-piece restart flag and possession start third
- Player involvement: number of players involved
- Pressure: proportion of events under pressure
- Goal-kick specifics: goal-kick start, first-pass length, wide-to-central action ratio

### Model

A logistic regression on the possession-level binary outcome (ended in a shot or not). Numeric features were median-imputed and standardized; start-third categories were one-hot encoded. The headline numbers come from five-fold grouped cross-validation, grouped by match. Random row splits would have leaked match-specific patterns and inflated the apparent performance.

| Metric | Model | Constant-rate baseline |
|---|---:|---:|
| PR-AUC | 0.3881 ± 0.0292 | n/a |
| ROC-AUC | 0.8238 ± 0.0143 | n/a |
| Log loss | 0.3051 ± 0.0093 | 0.3689 |
| Brier score | 0.0910 ± 0.0038 | 0.1064 |
| Accuracy | 0.8784 ± 0.0047 | n/a |

The shot-ending possession rate was 12.10%, which is why accuracy is weak evidence on its own. PR-AUC, log loss, and Brier score all matter more for a class this imbalanced. Because the numeric predictors were standardized, the odds ratios describe a one-standard-deviation increase, not a one-unit increase in the original measurement.

## Outcome and learning

Placed 1st of 6 teams, presented to judges from the New England Revolution and U.S. Soccer.

### What the model said

The strongest positive associations in the full fitted model:

- Time in the attacking third: odds ratio 2.41
- Number of players involved: odds ratio 2.14

Strong negative associations included a defensive-third start, number of passes, directness, and percentage of events under pressure. These are associations within the fitted model, not causal effects or universal tactical laws.

### What the tactical analysis said

Beyond the model, the team quantified shot-ending rates for specific action types:

- Pass into the box: 183 of 363 possessions led to a shot (around 50%)
- Cutback: 55 of 124 led to a shot (around 44%)
- Cross: 499 of 1,486 led to a shot (around 33%)
- Completed attacking-third wing dribble: 164 of 426 led to a shot (38.5%)
- Medium goal kicks: 29 of 271 led to a shot (10.7%), compared with 7.0% for short goal kicks and 6.0% for long goal kicks

Shot-ending possessions had a median duration of 19.8 seconds. The current goal-kick pipeline produces 1,023 goal-kick possessions (1,006 with valid directness, 76 shot-ending cases) and a match-grouped model with 0.908 ROC-AUC and 0.247 log loss versus a 0.268 baseline.

## Reflections

Coming soon.
