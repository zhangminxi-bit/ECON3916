# Audit 02: Deconstructing Statistical Lies

This audit investigates three common statistical distortions that mislead decision-making: latency skew from outliers, false confidence from high “accuracy” models, and survivorship bias in power-law markets. Each phase demonstrates how intuitive metrics can conceal structural risk.

---

## Phase 1 — Latency Skew: When Averages Lie

**Simulation Setup**
- 980 normal traffic observations (20–50 ms)
- 20 extreme spikes (1000–5000 ms)

The distribution is heavily right-skewed.

**Key Observation**
- Standard Deviation (SD) explodes because deviations are squared.
- Median Absolute Deviation (MAD) remains stable because deviations scale linearly from the median.

**Finding**

A small number of extreme spikes dramatically inflate SD while leaving MAD largely unchanged. This shows why variance-based metrics are fragile in heavy-tailed systems.

**Insight**

In operational systems, rare spikes dominate variance but do not represent typical performance. Robust statistics (median, MAD) provide a more reliable signal.

---

## Phase 2 — False Positives: The Base Rate Trap

**Model Assumptions**
- Sensitivity = 98%
- Specificity = 98%

Bayes’ Theorem was used to compute:

P(Cheater | Flagged)

**Key Result**

When the base rate is low (e.g., 0.1%), most flagged cases are false positives — despite the model being “98% accurate.”

**Finding**

Accuracy without base rate context is misleading. In low-prevalence environments, false positives dominate outcomes.

**Insight**

High accuracy does not imply high reliability. Statistical claims must be evaluated relative to prevalence.

---

## Phase 3 — Sample Ratio Mismatch (SRM)

**Observed Split**
- Control: 50,250
- Treatment: 49,750
- Expected: 50,000 / 50,000

Chi-Square Statistic:
χ² = 2.5

Critical value (df=1, α=0.05): 3.84

**Finding**

Since 2.5 < 3.84, the deviation is not statistically significant. The experiment passes the SRM check.

**Insight**

Apparent imbalance does not imply system failure. Formal hypothesis testing prevents premature conclusions.

---

## Phase 4 — Survivorship Bias in Crypto Markets

**Simulation**
- 10,000 token launches
- Pareto (power-law) distribution
- Top 1% classified as “survivors”

**Results**
- Mean Market Cap (All Tokens): ≈ $1.48M
- Mean Market Cap (Top 1%): ≈ $6.33M
- Survivorship Bias Multiplier: ≈ 4.27×

**Finding**

Analyzing only surviving tokens inflates perceived average success by more than 4×.

**Insight**

Power-law markets produce extreme winners and a large graveyard. Ignoring failures manufactures false optimism.

---

# Overall Conclusion

This audit exposes three structural statistical illusions:

1. Latency Skew — Outliers distort variance-based metrics.
2. False Positives — Accuracy collapses without base rate awareness.
3. Survivorship Bias — Ignoring failures exaggerates success.

Across all cases, the pattern is consistent:

Metrics without context create narratives that are technically correct but strategically misleading.

Statistical literacy is not about computation — it is about resisting seductive summaries.

