# Recovering Experimental Truths via Propensity Score Matching

## Overview

Observational data often fail to recover true causal effects due to systematic selection bias. In this lab, I implemented a Propensity Score Matching (PSM) framework to reconstruct the experimental treatment effect from biased observational data using the Lalonde dataset.

---

## Objective

To correct for selection bias in observational data by modeling the treatment assignment mechanism and recovering the experimental benchmark treatment effect through structured matching.

---

## Data

- **Dataset:** Lalonde (1986)
- **Subset Used:** Observational sample
- **Outcome Variable:** 1978 real earnings (`re78`)
- **Treatment Indicator:** Participation in job training program (`treat`)

The observational subset is known to produce severely biased naive estimates, making it a canonical stress test for causal inference methods.

---

## Methodology

The analysis was implemented in Python using Pandas and Scikit-Learn, structured around three core steps:

- **Modeled Selection Bias**
  - Specified and estimated a logistic regression model predicting treatment assignment.
  - Included pre-treatment covariates such as age, education, race, marital status, prior earnings, and degree status.
  - Explicitly estimated the probability of treatment conditional on observed characteristics.

- **Estimated Propensity Scores**
  - Computed individual-level treatment probabilities from the logistic model.
  - Interpreted propensity scores as a balancing score to approximate randomized assignment.

- **Applied Nearest Neighbor Matching**
  - Matched treated observations to control units with similar propensity scores.
  - Constructed a balanced matched sample to reduce observable confounding.
  - Estimated the treatment effect within the matched sample.

This pipeline reframed causal inference as an engineering problem: model the assignment mechanism first, then compare like with like.

---

## Key Findings

- **Naive Observational Estimate:** −$15,204  
  Severe bias driven by non-random treatment selection.

- **Matched Estimate:** Approximately +$1,800  
  Successfully recovered the experimental benchmark treatment effect.

The matching procedure eliminated substantial selection bias and restored the correct directional and quantitative treatment effect.

---

## Interpretation

This project demonstrates that causal failures in observational data are not necessarily informational failures — they are modeling failures. By explicitly modeling the treatment assignment process and enforcing covariate balance through matching, experimental truths can be recovered even in non-randomized settings.

Propensity Score Matching, when carefully implemented, serves as a practical bridge between observational data and experimental credibility.

---

## Skills Demonstrated

- Causal inference design in observational data
- Logistic regression modeling of treatment assignment
- Propensity score estimation and diagnostics
- Nearest Neighbor matching implementation
- Bias correction and treatment effect recovery
- Econometric reasoning applied in a production-grade Python workflow
