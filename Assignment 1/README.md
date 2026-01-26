# The Cost of Living Crisis: A Student-Focused Inflation Index

## Overview
The official Consumer Price Index (CPI) measures inflation for the average urban consumer, but it may not accurately reflect the cost-of-living changes faced by students. Students allocate a disproportionate share of their budget to tuition, rent, and essential services, which often experience different inflation dynamics than the national consumption basket.

This project constructs a **Student Spending Price Index (Student SPI)** to evaluate whether official inflation understates the real cost pressures experienced by students.

---

## Data & Tools
- **Language:** Python
- **Libraries:** pandas, matplotlib, fredapi
- **Data Source:** Federal Reserve Economic Data (FRED)

**Student-relevant categories analyzed:**
- Tuition
- Rent
- Food
- Subscription-based services (e.g., streaming)

---

## Methodology

### 1. Data Collection
Historical price indices are retrieved from FRED for categories most relevant to student spending patterns.

### 2. Re-Indexing (Index Theory)
All series are normalized to a common base year (**2016 = 100**) to eliminate distortions caused by differing original base years.

Mathematically:

```
Index_t = (Price_t / Price_2016) × 100
```

This ensures that relative growth rates across categories are directly comparable.

### 3. Student SPI Construction
A **Student Spending Price Index (Student SPI)** is constructed using a weighted average of student-centric categories:

```
Student_SPI_t = Σ (w_i × Index_{i,t})
```

where `w_i` represents the student expenditure share for category *i*.

This approach mirrors a **Laspeyres-style price index**, measuring the change in the cost of a fixed student consumption basket over time.

### 4. Benchmark Comparison
The Student SPI is compared against the official national CPI to quantify differences in inflation experiences.

---

## Key Findings
- Student-specific inflation consistently exceeds national CPI inflation.
- Over the analysis period, **Student SPI diverges from official CPI by approximately 4.40%**.
- The divergence is driven primarily by rapid increases in tuition and housing costs, which receive lower weights in the official CPI but dominate student budgets.

---

## Why It Matters
Aggregate inflation measures can mask substantial heterogeneity across demographic groups. This analysis shows that relying solely on the official CPI may significantly understate real cost-of-living pressures for students.

These findings have implications for:
- Financial aid and tuition policy
- Wage adjustments for student workers
- Cost-of-living assessments in higher education planning

---

## Repository Structure

```
├── data/        # Raw and processed CPI data
├── notebooks/   # Exploratory analysis and visualizations
├── src/         # Index construction and data pipelines
└── README.md
```

---

## Future Extensions
- Regional student inflation indices (e.g., Boston vs national CPI)
- Sensitivity analysis on student expenditure weights
- Expansion to other demographic-specific inflation baskets

---

## Author Notes
This project demonstrates applied macroeconomic reasoning, index theory, and data-driven policy analysis using real-world economic data.

