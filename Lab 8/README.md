# Hypothesis Testing & Causal Evidence Architecture

## Overview

This project operationalizes the scientific method within an econometric framework to adjudicate between competing narratives of causality. Using the Lalonde (1986) dataset, I reframed the analytical objective from simple *estimation* of treatment effects to rigorous *falsification* of causal claims.

Rather than asking “What is the effect?”, the project asks:
> “Can the Null Hypothesis survive disciplined statistical attack?”

The result is a structured evidence architecture that prioritizes statistical contradiction over narrative confirmation.

---

## Objective

Most applied data work focuses on point estimation — producing a number and interpreting it. This lab pivots toward a Popperian falsification framework:

- Define a Null Hypothesis (no earnings effect from job training).
- Attempt to disprove it using independent statistical tests.
- Accept causal lift only if the null fails under multiple methodological stress tests.

The goal was not to *fit* a story, but to break one.

---

## Dataset

- **Source:** Lalonde (1986)
- **Context:** Randomized evaluation of a job training program
- **Outcome Variable:** Post-intervention real earnings
- **Treatment Variable:** Participation in job training program

The dataset provides a canonical benchmark for causal inference under experimental conditions.

---

## Technical Approach

All statistical testing was implemented using `SciPy` to ensure reproducibility and statistical rigor.

### 1. Parametric Test — Welch’s T-Test

- Estimated the Average Treatment Effect (ATE)
- Used Welch’s correction to account for unequal variances
- Quantified the signal-to-noise ratio between treatment and control groups
- Controlled for Type I error (α = 0.05)

This provided an initial parametric test of mean differences under relaxed homoskedasticity assumptions.

---

### 2. Non-Parametric Validation — Permutation Test (10,000 Resamples)

Given the known non-normal distribution of earnings data:

- Implemented a 10,000-resample permutation test
- Randomly reassigned treatment labels to simulate the null distribution
- Compared observed treatment effect against the empirical null

This approach removes distributional assumptions and stress-tests the inference mechanism itself.

---

## Key Findings

- Observed statistically significant lift in real earnings: **~$1,795**
- Null Hypothesis rejected under both parametric and non-parametric frameworks
- Results robust to non-normal earnings distributions

Inference was established through *statistical contradiction*, not narrative alignment.

---

## Business Insight

In an algorithmic economy saturated with data, hypothesis testing functions as a **safety valve**.

Without disciplined falsification:

- Data grubbing produces fragile insights
- Spurious correlations masquerade as strategy
- Organizations overfit narratives to noise

Rigorous hypothesis testing prevents false positives from scaling into operational decisions. It enforces epistemic discipline in environments where machine learning models can detect patterns faster than humans can validate them.

The ability to distinguish signal from noise is not just statistical hygiene — it is a competitive advantage.

---

## Skills Demonstrated

- Experimental causal inference
- Parametric and non-parametric hypothesis testing
- Type I error control
- Statistical validation under distributional uncertainty
- Evidence architecture design

---

## Author Perspective

This project reflects a shift from *analysis as estimation* to *analysis as epistemology* — designing systems that challenge claims before deploying them.

In production environments, that distinction determines whether models generate durable value or expensive illusion.
