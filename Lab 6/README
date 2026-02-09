# Lab 5: The Architecture of Bias

## Overview
This project examines how bias enters machine learning systems **before modeling begins**, through the **Data Generating Process (DGP)**, sampling design, and experiment infrastructure. Using simulations and real-world data, the lab demonstrates why many model failures are fundamentally *data failures*, not algorithmic ones.

---

## Objectives
- Analyze the role of the Data Generating Process in statistical bias  
- Identify and mitigate sampling bias and covariate shift  
- Perform forensic diagnostics on A/B tests to detect Sample Ratio Mismatch (SRM)  
- Connect empirical bias to economic concepts such as selection bias and survivorship bias  

---

## Tech Stack
- **Python**
- **pandas**, **numpy**
- **scipy.stats** (Chi-Square tests)
- **scikit-learn** (Stratified Sampling)

---

## Methodology

### 1. Simple Random Sampling (SRS)
- Manually simulated Simple Random Sampling on the **Titanic dataset**
- Demonstrated how finite samples can produce:
  - High variance in outcome estimates
  - Unstable class distributions
- Showed that random sampling does not guarantee representativeness

**Insight:** Randomness alone does not ensure statistical reliability.

---

### 2. Stratified Sampling
- Implemented stratified sampling using `sklearn`
- Preserved class proportions across key covariates
- Eliminated covariate shift between training and evaluation data

**Insight:** Bias can often be reduced at the sampling stage rather than corrected downstream.

---

### 3. Sample Ratio Mismatch (SRM) Audit
- Simulated a 50/50 A/B experiment
- Applied Chi-Square goodness-of-fit tests to compare observed vs. expected group sizes
- Detected SRM as evidence of potential engineering or assignment failures

**Insight:** If treatment assignment is biased, all causal conclusions from the experiment are invalid.

---

## Theoretical Analysis: Survivorship Bias and Ghost Data

### Problem
Analyzing only successful Unicorn startups (e.g., those featured on TechCrunch) introduces **Survivorship Bias**. The dataset conditions on success, systematically excluding startups that failed, exited early, or never achieved visibility.

This creates **selection bias**, as the probability of being observed depends on unobserved factors that also affect outcomes.

---

### Required Ghost Data for Heckman Correction
To correct this bias using a **Heckman Selection Model**, the following ghost data is required:

- Data on **non-Unicorn startups** operating in similar:
  - Time periods
  - Industries
  - Funding environments
- A **selection indicator**:
  - Whether a startup appears in TechCrunch or reaches Unicorn status
- At least one **exclusion restriction**:
  - A variable that affects selection (visibility or funding access)
  - But does not directly affect startup performance outcomes  
  - Examples: media exposure timing, investor network proximity, geographic press density

This information enables estimation of the selection equation and correction for the non-random observation process.

**Insight:** Bias arises not from missing values, but from missing selection mechanisms.

---

## Key Takeaways
- Bias is often structural and introduced upstream of modeling
- Sampling design is a first-order concern in machine learning systems
- Statistical tests like SRM checks are critical for experimental validity
- Causal inference requires modeling *who gets observed and why*

---

## References
- Heckman, J. J. (1979). Sample Selection Bias as a Specification Error
- Kohavi et al. (2020). Trustworthy Online Controlled Experiments

