<img width="1640" height="924" alt="Image" src="https://github.com/user-attachments/assets/397e6a5d-4b69-4506-97e0-4202f898cec6" />


# Repository Structure
```
│      README.md
│   
├───code
│       .DS_Store
│       ski_resorts_eda_preprocessing.ipynb
│       
├───dashboard
│       EDA Dashboard.png
│       K-mean Dashboard.png
│       ML Liner Regression Dashboard.png
│       
├───data
│       .DS_Store
│       European_Ski_Resorts.csv
│       ski_resorts_clean.csv
│       ski_resorts_ML.csv
│       ski_resorts_processed.csv
│       ski_resorts_supervised_predictions.csv
│       ski_resorts_unsupervised.csv
│       ski_resorts_unsupervised_predictions.csv
│       
├───Report
│       Final_Report.md
│       
├───supervised_learning
│       supervised.ipynb
│       
└───unsupervised_learning
        unsupervised.ipynb
```
# European Ski Resort Analysis
Dataset: 376 resorts, 16 features covering elevation, slopes, lifts, pricing, and amenities.
## Problem
This project explores a practical business question: does the number and difficulty mix of a resort’s ski slopes help explain the price it charges for an adult day pass?
## Intended Audience
- Data practitioners
- Ski Resort Researchers
- Business Strategists
- Outdoor Recreation Enthusiasts
## Data Source
European Ski Resorts (Kaggle) — 376 resorts, 16 features covering elevation, slopes, lifts, pricing, and amenities for 2022. 
## Technologies Used
- Visual Studio Code: Python, Jutyper Notebooks
- Tableau Public 
- CSV Data Outputs
- GitHub
## Methodology

### **Exploratory Data Analysis**
- Examined distributions, feature relationships, and data quality issues in the raw dataset.

### **Preprocessing**
- Cleaned the data, handled missing values, encoded categorical features, and scaled numerical variables.


### **Modeling Outputs**

**`ski_resorts_clean.csv`**  
- Cleaned dataset for EDA and visualization.

**`ski_resorts_processed.csv`**  
- Encoded and scaled dataset for supervised and unsupervised modeling.


### **Unsupervised Learning (PCA & K-Means Clustering)**

- Selected 7 core resort features (vertical drop, snowmaking capacity, slope difficulty mix, lift types, etc.) and excluded price to let natural groupings form based on physical amenities.

- Scaled all features and reduced the dataset to 2 principal components, preserving 85% of the original variance and making clusters easier to visualize.

- Identified 4 resort clusters based on physical size: Small/Budget, Mid‑Size, Large, and Mega.

- Compared each cluster’s average day‑pass price to its physical attributes to evaluate how well pricing aligns with amenity levels.

### **Supervised Learning**

- Selected key slope‑related features (Beginner, Intermediate, Difficult counts) to predict adult day‑pass prices, focusing on physical amenities that influence consumer value.

- Trained two **Random Forest Regressor**  models:

    - **Baseline**: slope counts only

    - **Enhanced**: slope counts + country/region

- Tuned both models using GridSearchCV (n_estimators, max_depth) with cross‑validation to reduce overfitting.

- Found that slope count alone had a moderate relationship with price (R² = 0.38).

- Adding country/region significantly improved performance (R² = 0.74), showing that regional market factors influence pricing more than terrain size alone.

- Among slope features, intermediate‑difficulty terrain was the strongest individual predictor of price.

## Key Findings
  ### 1. Terrain size influences pricing, but with limits. 
  Resorts with more slopes (especially intermediate terrain) consistently charge higher day‑pass prices.  
  ### 2. Resort size categories create clear pricing tiers. 
  Small/Budget, Mid‑Size, Large, and Mega resorts form distinct clusters, each with its own typical price range.
  ### 3. Terrain alone can’t fully explain pricing.
  Supervised modeling shows slope count predicts price moderately well, but regional factors dramatically improve accuracy, meaning location and market conditions matter as much as terrain.
  ### 4. Certain resorts price above or below terrain‑based expectations.
  Prediction errors highlight outliers, likely driven by brand reputation, destination status, or local cost structures.

## Tableau Dashboards
Tableau Public Link: https://public.tableau.com/app/profile/sarah.nalepa/vizzes

![alt text](<dashboard/EDA Dashboard.png>)
![alt text](<dashboard/K-mean Dashboard (2).png>)
![alt text](<dashboard/ML Liner Regression Dashboard (1).png>)
## Recommendations
1. **Use terrain mix and resort size as baseline pricing inputs.** More intermediate terrain and larger resort size consistently align with higher day‑pass prices.
2. **Factor regional context into pricing decisions.** Location and market conditions influence price beyond physical amenities.
3. **Review outlier resorts for strategic opportunities.** Pricing deviations may signal brand, positioning, or operational gaps.
4. **Apply cluster categories for segmentation.** Small/Budget, Mid‑Size, Large, and Mega clusters support differentiated marketing and investment planning. 

## Limitations
1. Dataset is Europe‑focused, limiting generalization to global markets.
2. Terrain features alone are incomplete, lacking variables like snow reliability, accessibility, or brand reputation.
3. Price data reflects a single snapshot, not seasonal or dynamic pricing.
## Next Steps
Integrate regional economic indicators, add weather/snow data, and test additional models (e.g., gradient boosting) to improve predictive accuracy.
## Team Member Contributions

### Alina Tsui
Built the supervised (Random Forest regression) and unsupervised (K-Means/PCA clustering) modeling - including feature selection, hyperparameter tuning via GridSearch CV, plus cluster-count selection via Elbow method and silhouette score analysis. Built on Allan Solomon's EDA and initial country-grouping work as starting point for regression features.

### Allan Solomon
Data preprocessing and exploratory analysis were the tasks assigned to me. Apart from the highly right-skewed, inter-correlated features of size and the feature of prices, centered on €40, which reflect the size of resorts and the height above sea level, analyzing 376 resorts led to the discovery of missing data hidden behind zeros and “no reports”. Then, I created the data cleaning pipeline by removing eight indoor, dry-slope and planned resorts (so 368 resorts in 26 countries), imputing hidden missing data, adding the VerticalDrop feature, encoding categoricals, and standardizing the numbers.

### Cherry Hill
Collaborated with the team to scope the project, evaluate candidate datasets, and select the European Ski Resorts dataset based on data completeness, features, and alignment with the analytical objectives. Defined the core problem statement by formalizing business questions, specifying analytical hypotheses, and outlining required preprocessing and modeling workflows. Completed the final project report (Final_Report.md), implementing a structured technical narrative with consistent formatting,  methodology documentation, and submission ready organization. Integrated detailed explanations of our data pipeline, feature engineering decisions, model development and evaluation results, identified limitations, and data recommendations.
-
### Sarah Nalepa
Product Manager and Tableau Dashboards (EDA, Supervised, Unsupervised)

Tableau Public Link: https://public.tableau.com/app/profile/sarah.nalepa/vizzes
### Zaria Taylor
Collaborated with my team to select our project topic, identify the dataset, and define the analytical problem we aimed to solve. Completed the README.md and ensured the final project documentation was comprehensive, visually clear, and fully prepared for submission. 
## Link to Final Report
