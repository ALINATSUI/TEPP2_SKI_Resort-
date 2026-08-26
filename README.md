Sarah: 
Created three Tableau Dashboards (EDA, Supervised, Unsupervised) 
Tableau Public Link: https://public.tableau.com/app/profile/sarah.nalepa/vizzes


## Section 6: **<u>High Level Methodology/Approach</u>**



### **<u>Unsupervised Learning (PCA & K-Means Clustering)**</u>

We selected 7 features (core characteristics) of  the resorts while leaving price out of the initial grouping. This included vertical drop, snowmaking abilities, varying slope levels and lift types. This allowed us to see where the natural groupings of the resorts were based on the physical amenities that they offered. 

All features were scaled and data was simplified to 2 components as this allowed us to keep 85% of the original data detail and made it easier to map the resorts to find the natural groupings that had similarities to each other.  This method made it easier to also visually compare where each group had noticeable differences.  

There were 4 clusters based on the physical size of the resort (Small/Budget, Mid-Size, Large and Mega Resort). We then compared each cluster’s average day pass price against its physical amenities to see how closely aligned the pricing structure was across categories. 

### **<u>Supervised Learning</u>** 

We selected key resort features to predict adult day pass prices, focusing on physical amenities that drive consumer value — slope counts across three difficulty tiers (Beginner, Intermediate, Difficult). We trained two Random Forest Regressor models to isolate the effect of slope count from regional pricing effects: a slopes-only baseline, and a second model adding country/region. Both were tuned via GridSearchCV (n_estimators, max_depth) with cross-validation to control for overfitting.

Slope count alone had a real but limited relationship with day pass price (R² = 0.38). Adding country/region drastically improved performance (R² = 0.74), indicating that regional market factors — likely cost of living, currency, and resort brand — play a larger role in pricing than terrain size alone. Within the slope features, intermediate-difficulty terrain was the strongest individual predictor of price.



## **<u>Part 4: Individual Contributions**</u>

### Team Members: 

Alina Tsui: Built the supervised (Random Forest regression) and unsupervised (K-Means/PCA clustering) modeling - including feature selection, hyperparameter tuning via GridSearch CV, plus cluster-count selection via Elbow method and silhouette score analysis. Built on Allan Solomon's EDA and initial country-grouping work as starting point for regression features. 