# European Ski Resorts

*Do More Slopes Drive Up Day Pass Prices?*

## Technical Documentation

### European Ski Resorts Analysis – Capstone Project

### Supervised & Unsupervised Machine Learning Tableau Dashboards

The Knowledge House Data Analytics Fellowship

---

## 1. Summary

This project asks a practical business question: does the number and difficulty mix of ski slopes at a resort explain what it charges for an adult day pass?

Using a cleaned dataset of 369 European ski resorts from Kaggle’s European Ski Resorts dataset, the team built a full analytics pipeline including exploratory data analysis, data cleaning, a supervised RandomForest regression model, an unsupervised PCA + K-Means segmentation, and four Tableau dashboards to answer that question.

The answer is: partially.

Slope counts explain some variation in price, but the best single predictor is the amount of intermediate-difficulty slopes. Terrain and country do not matter nearly as much as the terrain does.

### Key takeaways

- Intermediate slope length is the single strongest terrain predictor of day pass price.
- Country matters almost as much as terrain. Being located in Switzerland accounted for about 30% of the model’s decision-making weight, second only to intermediate slope length.
- Adding country as a feature improved the model and reduced mean absolute percentage error.
- Unsupervised clustering (K-Means, k=4) independently confirmed a natural segmentation into small-budget, midsize, large, and mega resorts based on physical scale.
- This revealed which resorts are priced above or below what their peers typically charge.
- Switzerland-based resorts are consistently priced near their cluster average.

---

## 2. Project Overview

### Objective

Test the hypothesis that resort scale drives adult day pass pricing by leveraging both supervised and unsupervised data to triangulate the answer.

### Deliverables

The team produced the following deliverables:

- Three Jupyter notebooks: exploratory data analysis, preprocessing, supervised modeling, and unsupervised modeling
- Seven CSV data outputs
- Three Tableau dashboards:
  - Exploratory Data Analysis
  - Supervised Model Results
  - Unsupervised Cluster Analysis
- Technical documentation covering methodology and findings

### Data Source

Dataset: European Ski Resorts (Kaggle), originally sourced from skiresort.info.

The dataset includes 377 resorts across 27 countries, with 18 raw features covering elevation, slope difficulty, lifts, snowmaking, and adult day pass pricing.

---

## 3. Methodology

### Data Preparation

The raw dataset was cleaned and standardized to ensure data quality and consistency before modeling. This included handling missing values, validating numerical ranges, and preparing features for both supervised and unsupervised analysis.

### Exploratory Data Analysis

EDA focused on identifying relationships between resort characteristics and adult day pass pricing. Variables such as slope count, difficulty mix, resort size, and country were examined for distributions, correlations, and pricing patterns.

### Supervised Modeling

A RandomForest regressor was used to estimate adult day pass price using terrain and location variables. The model was evaluated using performance metrics such as error and feature importance to determine which variables most strongly drive price.

### Unsupervised Learning

PCA was used to reduce dimensionality and capture the most informative variation in resort characteristics. K-Means clustering was then applied to segment resorts into distinct groups based on size and terrain profile.

### Tableau Dashboards

Four dashboards were created to communicate findings visually:

1. EDA dashboard for distribution and price relationships
2. Supervised model dashboard for feature importance and price prediction insights
3. Unsupervised clustering dashboard for segment profiles and resort comparisons
4. Final summary dashboard linking model results and cluster insight

---

## 4. Findings

### Terrain Matters More Than Raw Slope Count Alone

The project showed that simply having more slopes does not fully explain pricing. The most important predictor was the amount of intermediate terrain, suggesting that resort variety and skier experience are more influential than scale alone.

### Country Exerts a Meaningful Pricing Premium

The model showed that Switzerland had an outsized impact on pricing. This indicates that country-specific market positioning, demand, and prestige contribute meaningfully to the cost of a day pass beyond physical terrain.

### Cluster Structure Confirms Market Segments

K-Means clustering grouped resorts into a coherent set of segments that align with size and scale: small-budget resorts, midsize resorts, large resorts, and mega resorts. These clusters provided additional evidence that resort pricing is tied to market tier and physical scale.

### Pricing Signals Compared to Peer Averages

Cluster analysis helped identify resorts that are priced above or below that of similar peers. This offers a valuable business lens for comparing market position, value perception, and competitive strategy.

---

## 5. Conclusion

The analysis suggests that adult ski day pass pricing is driven by a combination of terrain mix, resort scale, and country context. While the number of slopes contributes to pricing, the composition of the terrain—especially intermediate slopes—is the strongest signal. Country effects remain important, but the physical resort profile is the most informative foundational driver.

This project demonstrates how supervised and unsupervised learning can be combined to answer business questions with both predictive accuracy and interpretability.

---

## 6. Appendix: Project Scope Summary

- Problem: Does resort terrain explain adult day pass pricing?
- Dataset: European Ski Resorts (Kaggle)
- Methodology: EDA, preprocessing, RandomForest regression, PCA + K-Means
- Visualization: Tableau dashboards
- Final conclusion: Price is partially explained by terrain mix; intermediate terrain is the strongest driver, with country also contributing materially.
