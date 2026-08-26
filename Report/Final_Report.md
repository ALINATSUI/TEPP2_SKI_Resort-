# Final Report 


## Section 1 
## Detailed EDA & Preprocessing decisions






## Section 2: Unsupervised Learning Approach & Findings

### Feature Selection

### PCA

### K-Means Clustering



#### Cluster Interpretation







# Section 3: Supervised Models tested & compared


## Feature Engineering
Before building either mode, a handful of resorts with `TotalSlope ==0` were dropped from the dataset, since a resort with no recorded slopes did not provide any benefit for a model to investigate how slope counts could relate to day pass price.  



### Model 1: Slopes-Only Random Forest
As we were interested in determining whether number of slopes has any impact on the day pass price, I selected 'BeginnerSlope', 'IntermediateSlope' and 'DifficultSlope' values to be included as the features and referenced as 'X' for our independent variable. 

### Model 2: Slopes + Country Random Forest

Building on Model 1, the 'Country' column was one-hot encoded into dummy variable to test whether resort location also impacted day pass price, with countries having fewer than 5 resorts grouped into an 'Other' category to avoid overly sparse features. These dummy variables were added to the slope features to build the second model. 

### Model Comparison

The R<sup>2</sup> score tells us how much variance the model explains on unseen data. Adding the country specific feature drastically improved the test R<sup>2</sup> score as this represented the unseen data. Comparing Model 1 (Slopes-Only Random Forest) to Model 2 (Slopes + Country specific Random Forest), we observe that Model 1's R<sup>2</sup> test score increased from .3814 to Model 2's R<sup>2</sup> test score of .7352, when we accounted for max_depth. 
We also observed with country specific feature were added to the model, there was a reduction in overfitting. 

While R<sup>2</sup> was able to tell us about variance in our model's evaluation, we needed to see how much of an improvement our prediction were through RMSE (Root Mean Squared Error). In Model 1, the RMSE was 8.27, whereas Model 2, RMSE improved to 6.9178. 

Furthermore, Mean Absolute Error (MAE) provides another lens to see how accurate our model's predictions. On average, Model's 2 MAE was off by about 5.41 EUR, whereas Model 1's MAE was off by 6.274 EUR, which represents a 12.93% MAE error for Model 2 and 15% for Model 1. 



# Section 4: Model selection & rationale 

While several supervised models were available to choose from, Random Forest was selected for its ensemble learning approach: it builds decision trees,  each trained on a bootstrapped sample (random sampling with replacement) of the dataset and restricted to a random subset of features at each split.

Each captures a slightly different patterns in the data. The forest's final prediction is the average of all individual trees prediction which helps to minimize errors and converges towards the true underlying signal. 

## Findings
While intermediate slope count is the strongest predictor of price (supervised finding), it does not dominate the resorts' overall structural variation (unsupervised finding) — beginner and intermediate slope counts vary together across resorts at similar magnitudes. This suggests intermediate terrain specifically carries pricing power beyond what its overall variability alone would suggest.

Switzerland stood out in feature importance as it had the highest mean day pass price among countries with a decent sample size (€52.84 vs. the €41.83 overall average). 

# Section 5: Detailed results


![Feature Importance](../supervised_learning/Feature_Importance.png)

![Actual vs Predicted: Tuned Hyperparameters](../supervised_learning/actual_vs_predicted_tuned_hyperparams.png)

![Elbow Method](../unsupervised_learning/Elbow_Method.png)
![K-means](../unsupervised_learning/K-Means_Cluster_Comparison.png)
![Resort Segment Profile](../unsupervised_learning/Resort_Segment_Profile.png)

!['R2 Train Score by Comparison](../supervised_learning/table.svg)

~~# European Ski Resorts~~

~~*Do More Slopes Drive Up Day Pass Prices?*~~

~~## Technical Documentation~~

### European Ski Resorts Analysis – Capstone Project

~~### Supervised & Unsupervised Machine Learning Tableau Dashboards~~

~~The Knowledge House Data Analytics Fellowship~~

---

~~## 1. Summary~~

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
- ~~Switzerland-based resorts are consistently priced near their cluster average.~~

---

~~## 2. Project Overview

### Objective~~

Test the hypothesis that resort scale drives adult day pass pricing by leveraging both supervised and unsupervised data to triangulate the answer.

~~### Deliverables

~The team produced the following deliverables:

- Three Jupyter notebooks: exploratory data analysis, preprocessing, supervised modeling, and unsupervised modeling~~
- Seven CSV data outputs
- Three Tableau dashboards:
  - Exploratory Data Analysis
  - Supervised Model Results
  - Unsupervised Cluster Analysis
- Technical documentation covering methodology and findings~

~~### Data Source

~Dataset: European Ski Resorts (Kaggle), originally sourced from skiresort.info.

The dataset includes 377 resorts across 27 countries, with 18 raw features covering elevation, slope difficulty, lifts, snowmaking, and adult day pass pricing.~

---

~## 3. Methodology

### Data Preparation~

~The raw dataset was cleaned and standardized to ensure data quality and consistency before modeling. This included handling missing values, validating numerical ranges, and preparing features for both supervised and unsupervised analysis.~

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

~## 5. Conclusion~

The analysis suggests that adult ski day pass pricing is driven by a combination of terrain mix, resort scale, and country context. While the number of slopes contributes to pricing, the composition of the terrain—especially intermediate slopes—is the strongest signal. Country effects remain important, but the physical resort profile is the most informative foundational driver.

This project demonstrates how supervised and unsupervised learning can be combined to answer business questions with both predictive accuracy and interpretability.

---

~## 6. Appendix: Project Scope Summary

- Problem: Does resort terrain explain adult day pass pricing?
- Dataset: European Ski Resorts (Kaggle)~
~- Methodology: EDA, preprocessing, RandomForest regression, PCA + K-Means
- Visualization: Tableau dashboards~
~- Final conclusion: Price is partially explained by terrain mix; intermediate terrain is the strongest driver, with country also contributing materially.
~