# Final Report

## 1. Detailed EDA & Preprocessing Decisions
- **Data Cleaning:** Filtered out edge cases, including dropping resorts where `TotalSlope == 0` since non-operational or zero-slope entries offer no signal for price modeling.
- **Distribution & Correlation Analysis:** Evaluated the distributions of elevation, slope counts (Beginner, Intermediate, Difficult), lift capacities, and day pass prices across 27 European countries. Identified strong correlations between resort scale andterrain difficulty and adult day pass pricing, with notable price baseline differences across country borders.

## 2. Unsupervised Learning Approach & Findings
### Feature Selection
Selected physical resort scale and terrain metrics (slope lengths, total slopes, lifts, elevation range) to identify natural groupings independent of pricing.

### PCA (Principal Component Analysis)
Applied PCA to reduce feature dimensionality, capturing the majority of structural variance across resorts while eliminating multi-collinearity among physical scale metrics.

### K-Means Clustering & Cluster Interpretation
- Applied K-Means ($k=4$) to segment resorts into four distinct operational tiers: **Small-Budget**, **Midsize**, **Large**, and **Mega Resorts**.
- **Insight:** Clustering established a natural baseline for resort scale, making it clear which specific resorts price above or below their physical peer group.

## 3. Supervised Models Tested & Compared
### Feature Engineering
- Target Variable: Adult Day Pass Price (EUR).
- One-Hot Encoding: Encoded `Country` into dummy variables, grouping countries with fewer than 5 resorts into an `'Other'` category to prevent feature sparsity.

### Model 1: Slopes-Only Random Forest
- **Features:** `BeginnerSlope`, `IntermediateSlope`, `DifficultSlope`
- **Performance:** $R^2 = 0.3814$, $\text{RMSE} = 8.27$, $\text{MAE} = 6.274\text{ EUR}$ (~15% error).

### Model 2: Slopes + Country Random Forest
- **Features:** Slope breakdown + One-Hot Encoded Country features.
- **Performance:** $R^2 = 0.7352$, $\text{RMSE} = 6.9178$, $\text{MAE} = 5.41\text{ EUR}$ (~12.93% error).

### Model Comparison
Adding country-specific features significantly expanded the variance explained ($R^2$ jumped from ~0.38 to ~0.74), drastically reduced overall error (RMSE dropped by 1.35 EUR), and minimized model overfitting.

## 4. Model Selection & Rationale
Selected the **Random Forest Regressor** due to its ensemble structure. By averaging predictions across multiple decision trees trained on bootstrapped samples and random feature subsets, Random Forest effectively handles non-linear relationships, mitigates individual tree variance, and reduces overfitting compared to single decision trees or linear models.

### Key Findings
- **Intermediate Slope Power:** Intermediate slope count is the single strongest terrain predictor of pricing power, despite beginner and intermediate slope counts showing similar physical variance in unsupervised PCA.
- **Country Weight:** Geographic location acts as a major price driver. For example, location in Switzerland accounted for ~30% of feature importance, reflecting higher base operating costs and local market pricing (€52.84 mean pass price vs. €41.83 overall average).

## 5. Detailed Results
![Feature Importance](../supervised_learning/Feature_Importance.png)
![Actual vs Predicted: Tuned Hyperparameters](../supervised_learning/actual_vs_predicted_tuned_hyperparams.png)
![Elbow Method](../unsupervised_learning/Elbow_Method.png)
![K-means](../unsupervised_learning/K-Means_Cluster_Comparison.png)
![Resort Segment Profile](../unsupervised_learning/Resort_Segment_Profile.png)
!['R2 Train Score Comparison](../supervised_learning/table.svg)

### Key Takeaways
- Intermediate slope length is the primary driver of price.
- Country location is almost as impactful as physical terrain scale.
- Unsupervised clustering ($k=4$) successfully validates pricing tiers against physical scale baselines.

## 6. Dashboard Interpretation
- **EDA Dashboard:** Demonstrates positive skewness in day pass prices and illustrates how pricing scales with total slope kilometer distribution across regions.
- **Supervised Model Dashboard:** Highlights feature importance weights, demonstrating the dominating influence of intermediate terrain and Switzerland dummy variables on price predictions.
- **Unsupervised Cluster Dashboard:** Visualizes the 4 resort segments along PCA axes, allowing side-by-side comparisons of physical scale vs. actual pass price.
- **Summary Dashboard:** Triangulates model predictions against cluster peer groups to flag underpriced or overpriced individual resorts.

## 7. Limitations
- **Omitted Variables:** Lacks operational costs, seasonal snow conditions, luxury amenities, lodging density, and real-time demand data, which strongly influence pass pricing.
- **Geographic Representation:** Sparser data for smaller European countries required grouping them into an `'Other'` category, losing localized pricing nuances for minor markets.
- **Tree Interpretation:** While feature importances are clear, tree-based ensembles lack direct coefficient interpretations compared to linear models.

## 8. Recommendations
- **Tiered Pricing Strategy:** Align day pass pricing with physical peer groups identified in K-Means clustering before setting regional pricing.
- **Capital Investment Prioritization:** Expand intermediate terrain over beginner/difficult terrain when looking to increase pricing power, as market data demonstrates the highest elasticity for intermediate slopes.
- **Regional Dynamic Pricing:** Factor local economic benchmarks into model baselines when expanding into high-cost regions like Switzerland.
