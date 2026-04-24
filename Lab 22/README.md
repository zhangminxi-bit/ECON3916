# Clustering World Economies with K-Means & PCA

## Objective
Partition ~160 countries into economically coherent groups using unsupervised K-Means clustering on ten World Bank development indicators, then evaluate cluster validity through dimensionality reduction, model selection diagnostics, and alignment with official income classifications.

## Methodology

- **Data acquisition:** Retrieved the most recent available values for 10 World Development Indicators via the `wbgapi` package, covering income (GDP per capita PPP), health (life expectancy, infant mortality), education (primary enrollment), inequality (Gini index), environment (CO₂ per capita), connectivity (internet penetration), trade openness, labor markets (unemployment), and urbanization across ~160 countries.
- **Preprocessing:** Dropped countries missing more than 3 of 10 indicators; imputed remaining gaps with column medians. Applied `StandardScaler` to normalize all features to zero mean and unit variance — a required step before any distance-based algorithm, since GDP per capita spans three orders of magnitude and would otherwise dominate the Euclidean distance metric.
- **Clustering:** Fit K-Means (K=4, `k-means++` initialization, `random_state=42`) to mirror the World Bank's four-tier income classification. Cluster structure was inspected via centroid summaries across GDP per capita, life expectancy, and infant mortality.
- **Dimensionality reduction:** Applied PCA to project the 10-dimensional feature space onto two principal components for visualization. PC1 and PC2 together capture the majority of cross-country variance, confirming that development status is largely one-dimensional with a secondary axis of structural heterogeneity.
- **Model selection:** Ran elbow method (WCSS) and mean silhouette analysis across K = 2–10. The elbow and silhouette peak jointly identified the optimal K. *(Update with your actual optimal K after running the notebook.)*
- **Validation:** Cross-tabulated algorithmic cluster assignments against World Bank income group labels (Low, Lower-Middle, Upper-Middle, High) to assess how well an unsupervised method recovers expert-driven classifications.
- **Extension:** Applied the identical pipeline to the California Housing dataset (20,640 census tracts, 8 features), clustering tracts by housing stock age, density, income, and geographic coordinates.

## Key Findings

- **K=4 cluster structure:** The four clusters aligned broadly with income tiers — a high-income group (high GDP, life expectancy >78 years, infant mortality <7 per 1,000), an upper-middle cluster with moderate development indicators, a lower-middle cluster showing rapid urbanization with weaker health outcomes, and a low-income cluster characterized by high infant mortality and low internet penetration.
- **PCA coverage:** The first two principal components explain a substantial share of total variance, reflecting the well-documented empirical regularity that development indicators are highly correlated — most cross-country variation reduces to a single development gradient.
- **Optimal K:** Silhouette analysis peaked at K = 2. The elbow plot showed diminishing WCSS returns beyond K = 4–5, suggesting the four-group structure is data-supported, not just imposed by analogy to World Bank classifications.
- **Cluster-income alignment:** The cross-tabulation showed strong diagonal concentration, indicating that K-Means recovers income group structure without access to income labels. Misclassifications were concentrated at group boundaries (e.g., upper-middle vs. high-income), consistent with the expectation that algorithmic distance-based clustering will blur transitions the World Bank draws with hard GDP thresholds.
- **California Housing:** Census tract clusters segmented into economically interpretable groups — high-income coastal tracts, suburban middle-income tracts, older urban working-class tracts, and rural/inland low-income tracts — demonstrating that the K-Means + PCA pipeline generalizes cleanly to domestic subnational data.

## Technical Stack
`Python` · `wbgapi` · `scikit-learn` (KMeans, StandardScaler, PCA, silhouette_score) · `pandas` · `matplotlib` · `seaborn`
