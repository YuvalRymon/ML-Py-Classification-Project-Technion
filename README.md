# Students Performance Classification
<img width="831" alt="image" src="https://github.com/user-attachments/assets/26df3d7c-7b71-4d10-a88b-93ac22ac7b4d" />

## 1. Data and Mission
This project analyzes a very small dataset (1000 rows, 8 features), trying to maximize its value by formulating and researching a relevant question. The dataset features include gender, race, parental education, lunch, test preparation, and student grades in math, reading, and writing.
### Sample Data (Head of DataFrame):

![image](https://github.com/user-attachments/assets/91ab2fa4-2289-49f4-b64a-203af0411378)

## 2. Exploratory Data Analysis (EDA)
### Steps Performed:
1. Examined distribution of values in each feature.
2. Converted gender, lunch, and test preparation to binary values.
3. Checked for missing values (NANs).
4. Added two new features: average grade and pass/fail status (by median grade).
5. Analyzed the correlation matrix.
6. Applied one-hot encoding to the race feature.
7. Mapped parental education levels to numerical categories (1-4).
8. Scaled features to a standard normal distribution.
9. Box-plotted features to check for outliers.
10. Visualized histograms of feature distributions.
11. Created box plots for average score distribution across parental education levels and races.
  
 ![image](https://github.com/user-attachments/assets/5c9b0f48-a256-4c21-bdac-c76c6cadfd6d)
 ![image](https://github.com/user-attachments/assets/4b55938a-62d3-4ba5-8366-a8822811fd8b)
 ![image](https://github.com/user-attachments/assets/3dfc2fee-1342-4219-ae44-c7fe59be28f7)
 ![image](https://github.com/user-attachments/assets/71c8adc0-42b6-4fd1-91ac-0b6785f87595)
 ![image](https://github.com/user-attachments/assets/21da3924-f3b5-4cbc-b4fb-5a1dadbe08d5)

## 3. Conclusions from EDA
- Grades (math, reading, writing) are highly positively correlated.
- Gender is negatively correlated with grades but not with lunch or test prep.
- Lunch is positively correlated with grades but not with test prep.
- Test prep is positively correlated with grades.
- No significant outliers were identified.
- Students with parents holding master's degrees are underrepresented, requiring data imputation.
- Grades show some variation across races but vary more so with parental education levels.
## 4. Research Question
**Objective:** Determine the most influential features for predicting the quantile of math grades.
**Significance:** Math grades are a known predictor of future STEM success and salary potential. Understanding influential features can guide resource allocation (e.g., subsidizing lunch or test prep) or indicate the need for other interventions.
## 5. Model Selection
- **Primary focus:** Feature importance
- **Base Model:** Multiclass Logistic Regression for interpretability of coefficients
- **Advanced Model:** XGBoost for feature importance insights
- **Additional Analysis:** Clustering using K-Means and DBSCAN to identify hidden patterns
## 6. Logistic Regression
### Description:
A linear model for classification tasks that predicts the probability of a given class using a logistic (sigmoid) function, making it suitable for binary and multi-class problems.
### Adjustments:
- Duplicated master's education level instances in training to balance representation.
- Removed correlated grade features to avoid redundancy.
### Results:
- Accuracy: 0.38
- Confusion matrix: Better classification of extreme quantiles (1 and 4).
- Feature importance: Lunch is dominant, followed by gender and test prep.

![image](https://github.com/user-attachments/assets/673814be-5bb5-4aa3-a11d-0cac41ac4e2e)
![image](https://github.com/user-attachments/assets/2ff02d5f-64bf-42bc-b00d-d136ae3e0a56)

## 7. XGBoost
### Description:
An advanced gradient boosting algorithm that builds decision trees sequentially, with each tree improving upon the errors of the previous ones by focusing more on the data points that were misclassified or poorly predicted.
### CV parameters:
- Cross-validated the parameters: max depth, min child weight, learning rate.
### Results:
- Accuracy: 0.41 (slight improvement)
- Confusion: Reduced to adjacent quantiles (e.g., mistaking between either 1 and 2 or between 3 and 4).
- Feature importance: Lunch remains dominant, followed by gender and race group E.

![image](https://github.com/user-attachments/assets/560ea2e1-1b71-4778-b4b8-008b64519926)
![image](https://github.com/user-attachments/assets/7a73c5a1-468b-4cbd-b2ee-0ead150f0e79)
![image](https://github.com/user-attachments/assets/0ba0899a-169a-4fb5-95e3-197cf0331ee3)
 
## 8. Analysis of Results From Supervised Models
1. **Lunch:** Strongly influential, likely linked to socio-economic factors, with perhaps also intrinsic benefits like increased social engagement or cognitive health.
2. **Gender:** Significant influence in both models as well, suggesting tailored interventions for female students like special class exercises and teacher attention for participation.
3. **Other Features:** Limited standalone influence.
4. **Classification Limits:** Data Sufficient for binary math grade classes, but not four classes.
5. **Data Constraints:** Small dataset and feature space limit predictive power; math grades likely influenced by additional factors.
## 9. K-Means Clustering
### Description:
A partition-based clustering algorithm that divides data points into a fixed number of clusters (K) by iteratively assigning points to the nearest cluster center and updating the centroids to minimize the within-cluster variance.
### Method:
- Metrics: Elbow method (identifying the point where Within-Cluster-Sum of Squares decreases slowly) and silhouette (a measure balancing cohesion and separation bw clusters) scores.
- Optimal clusters found: 5
### Results:
- Visualized only in 3 principal components.
- Feature silhouette scores indicate clusters are race-based.
 
![image](https://github.com/user-attachments/assets/16c0dd90-5532-4a67-9002-f10eef1a600b)
![image](https://github.com/user-attachments/assets/b9dec0ee-2a11-46d9-9d1d-acb4962dd03f) ![image](https://github.com/user-attachments/assets/a67331b1-d9ed-4ab2-9f91-e162605555dd)
![image](https://github.com/user-attachments/assets/b8dbdd17-4e95-4d2d-8074-6771db9eece5)
 
## 10. DBSCAN Clustering
### Description: Density-Based Spatial Clustering of Applications with Noise. A density-based clustering algorithm that groups data points into clusters based on dense regions of data, allowing for clusters of arbitrary shapes and identifying sparse, isolated points as outliers.
### Method:
- Applied PCA to blur race feature dominance (one-hot encoded).
### Results:
- Similar to K-Means; clusters remain race-based.

![image](https://github.com/user-attachments/assets/a51c1aff-cf4c-4ad2-9cda-b95f8ed11f5d)
![image](https://github.com/user-attachments/assets/e0e99b9f-0306-4227-a985-f77c6e81eda9)

## 11. K-Means Clustering Without Race
### Adjustments:
- Removed race feature.
### Results:
- Found 2 clusters corresponding to pass/fail feature.
![image](https://github.com/user-attachments/assets/6cfb162d-956b-4608-bf19-16bcd1f04bf3)


## 12. Conclusions from Clustering
- Lunch, test prep, and parental education levels are dispersed across clusters.
- Specific groups cannot be targeted for these features, highlighting a need for broader interventions.
