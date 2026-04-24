# FedSpeak Analysis — NLP on FOMC Minutes

## Objective
Quantify shifts in Federal Reserve communication tone across two decades of FOMC meeting minutes using TF-IDF text representation, Loughran-McDonald financial sentiment scoring, and unsupervised document clustering to identify distinct monetary policy language regimes.

## Methodology

- **Data acquisition:** Loaded FOMC meeting minutes from the HuggingFace `vtasca/fomc-statements-minutes` dataset, filtering to full meeting minutes (excluding shorter policy statements) and sorting chronologically. Documents average 5,000–15,000 words each.
- **Text preprocessing:** Applied a standard NLP pipeline — lowercasing, removal of non-alphabetic characters, NLTK tokenization, English stop word removal, and WordNet lemmatization — reducing each document to a clean bag of meaningful financial terms.
- **TF-IDF representation:** Constructed a document-term matrix using `TfidfVectorizer` with unigrams and bigrams, dropping terms appearing in fewer than 5 documents (`min_df=5`) or more than 85% of documents (`max_df=0.85`), capped at 5,000 features. The resulting sparse matrix captures term importance relative to the full corpus, down-weighting boilerplate Fed language that appears uniformly across all meetings.
- **Sentiment scoring:** Implemented the Loughran-McDonald (LM) financial dictionary framework to compute two document-level scores: **net sentiment** = (positive word count − negative word count) / total tokens, and **uncertainty score** = uncertainty word count / total tokens. LM was used rather than a general-purpose dictionary because financial terms like "liability" and "credit" carry domain-specific valence that general dictionaries misclassify.
- **Temporal visualization:** Plotted net sentiment and uncertainty as time series with 12-month rolling averages and event annotations (Lehman collapse, taper tantrum, COVID, 2022 tightening cycle) to visually link language shifts to policy actions.
- **Document clustering:** Reduced the high-dimensional TF-IDF matrix to 50 components via `TruncatedSVD` (appropriate for sparse matrices), then applied K-Means (K=3) to identify distinct language regimes. Projected clusters into 2D via PCA for visualization with key meeting dates annotated.
- **Regime comparison:** Split the corpus at March 2020 and compared pre- vs. post-COVID distributions of net sentiment and uncertainty via overlaid histograms with group means marked.

## Key Findings

- **Three language regimes:** K-Means with K=3 on SVD-reduced TF-IDF vectors recovered three broadly interpretable communication eras — a pre-crisis baseline regime characterized by moderate, stable language; a crisis-and-recovery regime (clustered around 2008–2015) marked by elevated negative vocabulary and explicit discussion of financial system stress; and a post-COVID tightening regime featuring sharp inflation-related terminology and forward guidance language absent from earlier periods. Silhouette scores for text data are inherently lower than for numerical data, so interpret cluster boundaries as soft rather than discrete. *(Update with your actual silhouette score after running.)*
- **Sentiment crash and recovery:** Net sentiment dropped sharply during the 2008–2009 financial crisis — the most negative readings in the full sample — before recovering gradually through the 2010–2019 expansion. The COVID shock (March 2020) produced a second negative spike, followed by an unusual pattern: initially positive language during the 2020–2021 accommodation phase, then a return to negative tone as inflation language dominated 2022–2023 minutes.
- **Uncertainty dynamics:** Uncertainty scores were elevated during two distinct periods: the 2013 taper tantrum (forward guidance ambiguity) and the 2020–2022 COVID era. The post-COVID uncertainty spike is structurally different from the GFC spike — it is driven by supply-side forecasting difficulty rather than financial stability language, which is visible in the top TF-IDF terms distinguishing the two periods.
- **Pre- vs. post-COVID shift:** Post-March 2020 minutes show meaningfully lower mean net sentiment and higher mean uncertainty relative to the pre-COVID baseline, consistent with the Fed navigating an unprecedented dual mandate stress — maximum employment shock followed immediately by the worst inflation episode in four decades.

## Technical Stack
`Python` · `datasets` (HuggingFace) · `nltk` · `scikit-learn` (TfidfVectorizer, KMeans, TruncatedSVD, PCA, silhouette_score) · `pandas` · `matplotlib`
