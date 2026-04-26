# Part B: Business Case Analysis

## Scenario: Promotion Effectiveness at a Fashion Retail Chain

A fashion retailer operates 50 stores across different locations and uses multiple promotion strategies. The objective is to determine which promotion should be deployed in each store each month to maximize the number of items sold.

---

# B1. Problem Formulation

### (a) Machine Learning Problem

The problem can be formulated as a **supervised machine learning regression problem**.

- **Target Variable:**  
  `items_sold` (number of items sold per store per month)

- **Input Features:**  
  - Store attributes: store_size, location_type  
  - Promotion details: promotion_type  
  - Time features: month, year, day_of_week, is_weekend, is_festival  
  - External factors: competition_density  
  - Historical trends (if available)

- **Type of Problem:**  
  Regression, because the target variable is continuous (sales volume).

- **Justification:**  
  The goal is to predict numerical sales outcomes based on historical data and features, making regression the most appropriate approach.

---

### (b) Why Items Sold is Better than Revenue

Using **items_sold** instead of total revenue is more reliable because:

- Revenue is affected by pricing, discounts, and promotions.
- Different promotions may reduce prices but increase volume.
- Revenue may hide true customer demand.

**Broader Principle:**  
This illustrates the importance of **choosing the correct target variable** in machine learning. The target should directly reflect the business objective — here, maximizing sales volume rather than monetary value.

---

### (c) Alternative Modeling Strategy

Instead of using a single global model across all stores, a better approach would be:

- **Cluster-based models (segmentation):**
  - Group stores based on location, size, and customer behavior.
  - Train separate models for each cluster.

OR

- **Hierarchical / multi-level modeling:**
  - Capture both global trends and store-level variations.

**Justification:**  
Different stores behave differently due to location, demographics, and competition. A single model may fail to capture these variations.

---

# B2. Data and EDA Strategy

### (a) Data Joining Strategy

Data is available in four tables:
- Transactions
- Store attributes
- Promotion details
- Calendar data

**Steps:**
1. Join transactions with store attributes using `store_id`
2. Join with promotion data using `promotion_id`
3. Join with calendar data using `date`

**Final Dataset Grain:**  
- One row per **store per month**

**Aggregations:**
- Total items sold per store per month
- Average competition density
- Promotion applied in that month
- Monthly footfall (if available)

---

### (b) EDA (Exploratory Data Analysis)

At least four analyses:

1. **Sales Distribution Plot**
   - Understand spread of `items_sold`
   - Detect skewness and outliers

2. **Promotion vs Sales (Boxplot)**
   - Compare performance of each promotion type
   - Identify which promotions drive higher sales

3. **Time Trend Analysis**
   - Monthly sales trends
   - Detect seasonality patterns

4. **Correlation Heatmap**
   - Identify relationships between variables
   - Helps in feature selection

**Impact on Modeling:**
- Helps in feature engineering
- Identifies important variables
- Detects anomalies and data issues

---

### (c) Handling Promotion Imbalance

Since 80% of transactions have no promotion:

**Impact:**
- Model may become biased towards “no promotion”
- Poor learning of promotion effects

**Solutions:**
- Use stratified sampling (if classification-based framing)
- Add interaction features (promotion × store type)
- Use weighted models or resampling techniques
- Ensure evaluation metrics consider minority cases

---

# B3. Model Evaluation and Deployment

### (a) Train-Test Split Strategy

- Use **time-based split**:
  - Train on older data
  - Test on recent data

**Why not random split?**
- Breaks time order
- Causes data leakage
- Unrealistic evaluation

**Evaluation Metrics:**
- RMSE (Root Mean Squared Error): penalizes large errors
- MAE (Mean Absolute Error): interpretable average error
- R² Score: explains variance

**Interpretation:**
- Lower RMSE/MAE → better predictions
- Higher R² → better explanatory power

---

### (b) Feature Importance Explanation

If the model gives different recommendations for the same store:

**Steps to investigate:**
1. Check feature importance (e.g., using Random Forest)
2. Analyze key factors:
   - Seasonality (December vs March)
   - Festival effects
   - Promotion effectiveness

**Explanation to business:**
- Customer behavior changes across months
- Some promotions work better during festive periods
- Model adapts to these patterns dynamically

---

### (c) Deployment Strategy

**Steps:**

1. **Model Saving**
   - Save model using `joblib` or `pickle`

2. **Monthly Data Pipeline**
   - Collect latest store + promotion + calendar data
   - Preprocess using same pipeline

3. **Prediction**
   - Generate recommendations for each store monthly

4. **Monitoring**
   - Track prediction vs actual sales
   - Monitor metrics like RMSE

5. **Retraining**
   - Retrain model periodically (e.g., monthly or quarterly)
   - Trigger retraining if performance drops

---

## Final Note

This approach ensures:
- Data-driven promotion decisions
- Better store-level personalization
- Continuous model improvement over time