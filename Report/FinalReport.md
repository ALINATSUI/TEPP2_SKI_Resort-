<img width="1640" height="924" alt="Image" src="https://github.com/user-attachments/assets/397e6a5d-4b69-4506-97e0-4202f898cec6" />
# Final Report

## 1. Detailed EDA & Preprocessing Decisions
- **Data Cleaning:** Filtered out edge cases, including dropping resorts where `TotalSlope == 0` since non-operational or zero-slope entries offer no signal for price modeling.
- **Distribution & Correlation Analysis:** Evaluated the distributions of elevation, slope counts (Beginner, Intermediate, Difficult), lift capacities, and day pass prices across 27 European countries. Identified strong correlations between resort scale andterrain difficulty and adult day pass pricing, with notable price baseline differences across country borders.

## 2. Unsupervised Learning Approach & Findings
### Feature Selection
Selected physical resort scale and terrain metrics (slope lengths, total slopes, lifts, elevation range) to identify natural groupings independent of pricing.

### PCA (Principal Component Analysis)
We applied PCA to reduce feature dimensionality, capturing the majority of structural variance across resorts while eliminating multi-collinearity among physical scale metrics. For example `HighestPoint` was dropped in favor of `VerticalDrop` due to a strong 0.838 correlation between the two. 

Across the 7 clustering features, PC1 alone captured ~75% of the total variance (0.751) and PC1 combined with PC2 captured ~85% (0.751 + .102). By comparison, PC3 only added another 7% (0.066) so PC3 and beyond were excluded from the final model. 

Based on this, `n_components=2` was selected for the final PCA transformation which enabled us to plot a 2D visualization of resorts and later used to plot and interpret K-means clusters. 

### Cluster Determination (Elbow Method/Silhouette Score)
To determine k number of clusters, both the Elbow Method and Silhouette scores were evaluated across k=1 through 10. 

We plotted the Elbow method using WCSS/inertia against k, inertia declined sharply from k=1 to k=2 (2172.76 -> 828.38), and again from k=2 to k=3 (828.38 -> 587.54) before it started to flatten out. The curve leveled out noticeably around  k=3-4, marking the elbow. 

Silhouette scores told a different story: the highest score occurred at k=2 (0.7157), decreasing after that and settling into 0.38-0.45 range for k>=5. 

We chose k=4 as it sits right between where the elbow starts to flatten and  
creates 4 meaningful clusters instead of collapsing resorts into just 2 groups. This 
results in 4 clusters grouped by resort sizes (Small Resort, Mid-Size Resort, Large Resort and Mega-Resort). 

### K-Means Clustering & Cluster Interpretation
- K-Means with k=4 to group resorts into four distinct operational tiers based on resort: **Small/Budget**, **Mid-Size**, **Large**, and **Mega Resort**.  Average day pass price increased consistently with resort tier:

- Mega Resorts average 57.67
- Large Resorts average 53.61
- Mid-Size Resorts average 45.60
- Small/Budget Resorts average 36.21

This indicates that resort size and scale are a strong driver of baseline pricing. 

- **Insight:** Clustering established a natural baseline for resort scale, making it possible to identify which specific resorts price above or below their physical peer group.

### Price Variance Within Clusters
For each of the 4 clusters, the average day pass price was calculated and compared against each resort's actual
price to determine which resorts were priced above or below their cluster's average. This provides an indication of which resorts are a relative "good buy" compared to similarly-tiered resorts offering premium pricing. 

All 5 of the resorts with the biggest premiums over their cluster average are in Switzerland which reinforces the country premium that we've already seen in the supervised model's feature importance ranking. 

Underpriced relative to cluster: the largest negative price gaps were geographically scattered to Germany, Eastern Europe, and Southern Russia. 

Takeaway: After accounting for resort size, Swiss resorts consistently command a premium price over its counterparts with similar physical amenities. This mirrors the country impact that we found in the supervised model and suggests that "Country_Switzerland" premium isn't just limited to slope-count but it holds also for our resort tiers. 

Price variance analysis helps reveal regional market differences that reflect brand prestige, reputation or other  
factors beyond slope count as the primary basis for comparison. 

## 3. Supervised Models Tested & Compared
### Before building either mode, a handful of resorts with TotalSlope ==0 were dropped from the dataset, since a resort with no recorded slopes did not provide any benefit for a model to investigate how slope counts could relate to day pass price.

Model 1: Slopes-Only Random Forest
As we were interested in determining whether number of slopes has any impact on the day pass price, I selected 'BeginnerSlope', 'IntermediateSlope' and 'DifficultSlope' values to be included as the features and referenced as 'X' for our independent variable.

### Model 1: Slopes-Only Random Forest
- **Features:** `BeginnerSlope`, `IntermediateSlope`, `DifficultSlope`
- **Performance:** $R^2 = 0.3814$, $\text{RMSE} = 8.27$, $\text{MAE} = 6.274\text{ EUR}$ (~15% error).
- 
Model 2: Slopes + Country Random Forest
Building on Model 1, the 'Country' column was one-hot encoded into dummy variable to test whether resort location also impacted day pass price, with countries having fewer than 5 resorts grouped into an 'Other' category to avoid overly sparse features. These dummy variables were added to the slope features to build the second model.

### Model 2: Slopes + Country Random Forest
- **Features:** Slope breakdown + One-Hot Encoded Country features.
- **Performance:** $R^2 = 0.7352$, $\text{RMSE} = 6.9178$, $\text{MAE} = 5.41\text{ EUR}$ (~12.93% error).

### Model Comparison

The R<sup>2</sup> score tells us how much variance the model explains on unseen data. Adding the country specific feature drastically improved the test R<sup>2</sup> score as this represented the unseen data. Comparing Model 1 (Slopes-Only Random Forest) to Model 2 (Slopes + Country specific Random Forest), we observe that Model 1's R2 test score increased from .3814 to Model 2's R2 test score of .7352, when we accounted for max_depth. We also observed with country specific feature were added to the model, there was a reduction in overfitting.

Adding country-specific features significantly expanded the variance explained ($R^2$ jumped from ~0.38 to ~0.74), drastically reduced overall error (RMSE dropped by 1.35 EUR), and minimized model overfitting.

While R<sup>2</sup> was able to tell us about variance in our model's evaluation, we needed to see how much of an improvement our prediction were through RMSE (Root Mean Squared Error). In Model 1, the RMSE was 8.27, whereas Model 2, RMSE improved to 6.9178.

Furthermore, Mean Absolute Error (MAE) provides another lens to see how accurate our model's predictions. On average, Model's 2 MAE was off by about 5.41 EUR, whereas Model 1's MAE was off by 6.274 EUR, which represents a 12.93% MAE error for Model 2 and 15% for Model 1.

Feature Engineering
- Target Variable: Adult Day Pass Price (EUR).
- One-Hot Encoding: Encoded `Country` into dummy variables, grouping countries with fewer than 5 resorts into an `'Other'` category to prevent feature sparsity.


## 4. Model Selection & Rationale
While several supervised models were available to choose from, Random Forest was selected for its ensemble learning approach: it builds decision trees, each trained on a bootstrapped sample (random sampling with replacement) of the dataset and restricted to a random subset of features at each split.

Each captures a slightly different patterns in the data. The forest's final prediction is the average of all individual trees prediction which helps to minimize errors and converges towards the true underlying signal.

### Key Findings
While intermediate slope count is the strongest predictor of price (supervised finding), it does not dominate the resorts' overall structural variation (unsupervised finding) — beginner and intermediate slope counts vary together across resorts at similar magnitudes. This suggests intermediate terrain specifically carries pricing power beyond what its overall variability alone would suggest.

Switzerland stood out in feature importance as it had the highest mean day pass price among countries with a decent sample size (€52.84 vs. the €41.83 overall average).

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
- Intermediate slope length is the primary predictor of day pass price.
- Country matters almost as much as terrain. Being located in Switzerland accounted for about 30% of the model’s decision-making weight, second only to intermediate slope length.
- Adding country as a feature improved the model and reduced mean absolute percentage error.
- Unsupervised clustering (K-Means, k=4) independently confirmed a natural segmentation into small-budget, midsize, large, and mega resorts based on physical scale.
This revealed which resorts are priced above or below what their peers typically charge.

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
