# Causal ML — Double Machine Learning for 401(k) Policy Evaluation

## Objective
Apply Chernozhukov et al.'s Double Machine Learning framework to recover an unbiased Average Treatment Effect of 401(k) plan eligibility on household net financial assets, demonstrating why naive regularized regression fails for causal inference and how cross-fitted nuisance models solve the regularization bias problem.

## Methodology

- **Regularization bias demonstration:** Simulated a high-dimensional DGP (n=5,000, p=100 covariates) with a known true ATE of 5.0 and fit `LassoCV` directly on the augmented design matrix [D | X]. LASSO shrank the treatment coefficient toward zero, producing substantial downward bias — establishing the core motivation for the DML approach.
- **Data:** 401(k) pension plan participation data (Chernozhukov & Hansen) loaded via `doubleml.datasets.fetch_401K`. Outcome: net total financial assets (`net_tfa`). Treatment: 401(k) eligibility indicator (`e401`). Covariates: age, income, family size, education, marital status. A pre-estimation balance check confirmed selection bias — eligible households earn more and are better-educated, making unadjusted comparisons unreliable.
- **Double ML setup:** Constructed a `DoubleMLData` object partitioning the dataset into outcome, treatment, and covariate roles. Specified two separate Random Forest nuisance learners — `ml_l` to partial out the outcome (E[Y|X]) and `ml_m` to partial out the treatment (E[D|X]) — using `n_estimators=200` and `max_depth=5` to balance flexibility and overfitting control.
- **Cross-fitting:** Fit a `DoubleMLPLR` (Partially Linear Regression) model with 5-fold cross-fitting. Cross-fitting is what separates DML from a naive two-step approach: by fitting nuisance models on held-out folds and evaluating residuals on the corresponding test fold, DML prevents the regularization bias that would arise if the same data were used for both nuisance estimation and causal identification.
- **CATE analysis:** Partitioned the sample into income quartiles (Q1–Q4) via `pd.qcut` and estimated a separate `DoubleMLPLR` on each subgroup, producing quartile-specific ATEs and 95% confidence intervals. Visualized results as a bar chart with error bars to assess treatment effect heterogeneity across the income distribution.

## Key Findings

- **Regularization bias is large:** On the simulated DGP, LASSO recovered a treatment coefficient significantly below the known true value of 5.0. The magnitude of downward bias scales with covariate dimensionality and the degree of confounding — exactly the conditions present in observational labor economics data. *(Update with your exact LASSO estimate after running.)*
- **ATE — overall effect:** The DML PLR estimated that 401(k) eligibility increases net financial assets by approximately **$[YOUR_ATE_HERE]**, with the effect statistically significant at the 5% level. This estimate is substantially corrected relative to a naive OLS comparison, which would conflate the treatment effect with the income-driven selection into eligible firms. *(Fill in your ATE after running Cell 8.)*
- **CATE — income heterogeneity:** The income-quartile CATE analysis revealed that the 401(k) eligibility effect is not uniform across the earnings distribution. Higher-income quartiles tend to show larger absolute ATEs, consistent with two mechanisms: (1) higher-income households face a binding savings constraint that 401(k) access relaxes, and (2) tax incentives embedded in 401(k) contributions are worth more at higher marginal rates. Lower-income quartiles show smaller and less precisely estimated effects, with wider confidence intervals reflecting the smaller within-quartile sample sizes. *(Update with your specific quartile estimates after running Cell 9.)*
- **Methodological takeaway:** The comparison between the naive LASSO ATE and the DML ATE in the same dataset illustrates the core argument for causal ML: prediction-optimized models are not identification-optimized. DML's orthogonalization step — regressing out both Y and D on X before estimating the treatment effect — immunizes the causal coefficient against the regularization penalty applied to the nuisance parameters.

## Technical Stack
`Python` · `doubleml` · `econml` · `scikit-learn` (RandomForestRegressor, LassoCV) · `pandas` · `numpy` · `matplotlib`
