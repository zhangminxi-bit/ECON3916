# Macroeconomic Correlation Traps: A Time-Series Diagnostics Lab

## Overview

This project investigates the dangers of **spurious correlation and multicollinearity** in macroeconomic time-series data using publicly available data from the Federal Reserve Economic Data (FRED) API. Macroeconomic variables frequently exhibit strong trends over time, which can lead to misleading statistical relationships when analyzed in raw levels.  

The objective of this lab was to demonstrate how naïve correlation analysis can produce false signals, and to illustrate a workflow for diagnosing and correcting these issues using standard econometric techniques.

---

## Methodology

### 1. Data Acquisition

Macroeconomic time-series data were retrieved from the **FRED API** using Python. The dataset includes key indicators such as:

- Consumer Price Index (CPI)
- Unemployment Rate
- Federal Funds Rate
- Industrial Production
- Money Supply (M2)

These variables were selected because they commonly appear in macroeconomic modeling and exhibit strong long-term trends.

---

### 2. Exploratory Correlation Analysis

Initial analysis examined **pairwise correlations of the raw level data** using `pandas` and `seaborn`.

Correlation heatmaps revealed extremely high relationships between several variables, particularly those with persistent upward trends. These results demonstrate the classic **spurious correlation problem** in non-stationary time series: variables may appear highly correlated simply because they share a common time trend rather than a meaningful economic relationship.

---

### 3. Multicollinearity Diagnostics

To quantify redundancy among predictors, **Variance Inflation Factor (VIF)** diagnostics were computed using `statsmodels`.

VIF measures how much the variance of a regression coefficient is inflated due to correlation with other explanatory variables. High VIF values indicated substantial multicollinearity in the raw-level dataset, confirming that many macroeconomic variables contain overlapping trend information.

---

### 4. Stationarity Transformation

To mitigate trend-driven correlations, the variables were transformed into **Year-over-Year (YoY) growth rates**.

This transformation removes much of the long-run trend component and emphasizes **cyclical fluctuations**, producing a dataset that more closely resembles stationary macroeconomic dynamics. Recomputing correlations on the YoY series significantly reduced the magnitude of previously observed relationships, illustrating how proper transformations help reveal more meaningful economic comovement.

---

### 5. Structural Interpretation with Directed Acyclic Graphs

Finally, **Directed Acyclic Graphs (DAGs)** were used to represent plausible structural relationships between macroeconomic variables. DAGs provide a conceptual framework for distinguishing:

- causal relationships
- common drivers
- spurious associations created by shared trends

This step emphasizes that correlation matrices alone cannot identify economic structure without domain knowledge and causal reasoning.

---

## Tools & Libraries

- **Python**
- **pandas** – data manipulation
- **seaborn** – correlation heatmap visualization
- **statsmodels** – econometric diagnostics (VIF)
- **FRED API** – macroeconomic data retrieval
- **networkx / DAG frameworks** – causal structure visualization

---

## Key Takeaways

- Raw macroeconomic level data often produce **misleadingly high correlations** due to shared time trends.
- **Variance Inflation Factor diagnostics** help identify multicollinearity among explanatory variables.
- Transformations such as **Year-over-Year growth rates** reduce spurious relationships by removing trend components.
- **Causal diagrams (DAGs)** provide a useful framework for interpreting macroeconomic relationships beyond simple correlation analysis.

---

## Project Goal

This lab demonstrates a practical workflow for **diagnosing correlation traps in macroeconomic data**, combining exploratory visualization, statistical diagnostics, and causal reasoning. The project highlights why careful preprocessing and domain-informed modeling are essential when working with economic time series.
