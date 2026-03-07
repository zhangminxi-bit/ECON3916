# SwiftCart Loyalty Program Causal Analysis

## Overview

This project investigates the marketing team's claim that users who subscribe to the **SwiftPass premium loyalty program** spend **300% more per month** than non-subscribers.

While this claim appears convincing at first glance, it likely suffers from **selection bias**. High-volume "power users" are naturally more likely to subscribe to the loyalty program because they benefit more from reduced delivery fees.

Therefore, simply comparing spending between subscribers and non-subscribers may lead to misleading conclusions.

The objective of this analysis is to determine whether:

- SwiftPass **causes higher spending**, or
- High spenders were **already different before subscribing**

To answer this question, we compare:

1. **Naive Simple Difference in Means (SDO)**
2. **Propensity Score Matching (PSM)** to estimate the **Average Treatment Effect on the Treated (ATT)**.

---

# Dataset

**File:** `swiftcart_loyalty.csv`

The dataset contains both **pre-treatment user characteristics** and **post-treatment spending outcomes**.

## Variables

| Variable | Description |
|--------|-------------|
| `D` | Treatment indicator (1 = SwiftPass subscriber, 0 = Non-subscriber) |
| `pre_order_volume` | Number of orders before subscription |
| `account_age` | Length of time the account has existed |
| `support_tickets` | Historical number of support requests |
| `post_spending` | Monthly spending after treatment |

The **pre-treatment variables** are used to estimate the likelihood that a user subscribes to SwiftPass.

---

# Step 1: Naive Difference in Means (SDO)

The **Simple Difference in Means** compares the average spending between subscribers and non-subscribers.

### Formula

```
SDO = E[Y | D = 1] − E[Y | D = 0]
```

Where:

- `Y` = Post-treatment spending
- `D` = Treatment indicator (SwiftPass subscription)

### Interpretation

This estimate simply measures:

```
Average spending of subscribers
−
Average spending of non-subscribers
```

However, this estimate is **biased** because it ignores the fact that **users self-select into the program**.

Heavy users are more likely to subscribe, so the difference in spending may not be caused by SwiftPass itself.

---

# Step 2: Propensity Score Estimation

To correct for selection bias, we estimate a **Propensity Score**.

The propensity score represents the **probability that a user subscribes to SwiftPass given their observable characteristics**.

### Mathematical Representation

```
P(D = 1 | X)
```

Where:

- `D` = Treatment status
- `X` = Pre-treatment covariates

### Logistic Regression Implementation

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(X, y)

propensity_scores = model.predict_proba(X)[:,1]
```

Where:

- `X` = pre-treatment variables
- `y` = treatment indicator

The output is the estimated **probability of each user subscribing to SwiftPass**.

---

# Step 3: Nearest Neighbor Matching

After estimating propensity scores, we match each subscriber to the **non-subscriber with the closest propensity score**.

This ensures we compare **users with very similar characteristics**.

### Matching Implementation

```python
from sklearn.neighbors import NearestNeighbors

nn = NearestNeighbors(n_neighbors=1)

nn.fit(control_scores)

distances, indices = nn.kneighbors(treated_scores)
```

This creates a **matched control group** that is statistically comparable to the treated group.

---

# Step 4: Average Treatment Effect on the Treated (ATT)

After matching, we estimate the **Average Treatment Effect on the Treated (ATT)**.

### Formula

```
ATT = E[Y₁ − Y₀ | D = 1]
```

Where:

- `Y₁` = observed spending of treated users
- `Y₀` = counterfactual spending estimated from matched controls

### Interpretation

ATT measures:

```
How much more subscribers spend
compared to similar users who did not subscribe
```

This estimate is **closer to the true causal effect**.

---

# Step 5: Covariate Balance Check (Love Plot)

To verify that matching worked properly, we generate a **Love Plot**.

A Love Plot visualizes **Standardized Mean Differences (SMD)** across all covariates:

- **Before matching**
- **After matching**

### Standardized Mean Difference

```
SMD = (Mean_treated − Mean_control) / StandardDeviation
```

### Balance Threshold

A commonly accepted rule:

```
|SMD| < 0.1
```

If all covariates fall below this threshold **after matching**, then the groups are considered **well balanced**.

This indicates that the **selection bias has been substantially reduced**.

---

# Key Insight

The **Naive Difference in Means** is misleading because it confuses **correlation with causation**.

Subscribers appear to spend more because:

- High-frequency users are **more likely to join SwiftPass**

After applying **Propensity Score Matching**, we compare **similar users**.

In most cases, the estimated **ATT is significantly smaller than the naive estimate**, suggesting that SwiftPass itself may not be responsible for the large spending difference claimed by marketing.

---

# Conclusion

This analysis demonstrates the importance of **causal inference techniques** in business decision-making.

Without correcting for **selection bias**, organizations risk making large strategic investments based on misleading correlations.

By implementing **Propensity Score Matching**, we obtain a more credible estimate of the **true causal effect of the SwiftPass loyalty program**.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-Learn
- Matplotlib
- Seaborn

---

# Project Structure

```
project/
│
├── swiftcart_loyalty.csv
├── analysis.ipynb
├── love_plot.py
└── README.md
```

---

# Author

Econometric Causal Inference Analysis  
Propensity Score Matching Implementation
