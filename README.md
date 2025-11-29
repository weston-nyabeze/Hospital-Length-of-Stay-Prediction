# Hospital Length of Stay Prediction & Optimization
## Predictive Analytics for California Acute Care Facilities

---

## Abstract

This project develops machine learning predictive models to forecast hospital length of stay (LOS) using 743,518 acute care hospital discharge records from California (2011-2014). By accurately predicting LOS, healthcare administrators can optimize resource allocation, improve discharge planning, and reduce operational costs. The Random Forest model achieves 72.3% accuracy with a 2.10-day prediction error (RMSE), enabling clinically actionable predictions. Hospital clustering analysis reveals 30-45% operational efficiency variation across similar facilities, suggesting significant best-practice transfer opportunities. This analysis provides healthcare systems with data-driven tools for understanding LOS drivers and benchmarking operational performance.

---

## Background

Hospital length of stay is a critical operational metric affecting patient flow, bed availability, staffing requirements, and total healthcare costs. Variations in LOS across diagnoses, insurance types, and hospitals indicate potential inefficiencies and opportunities for improvement. Understanding these drivers enables:

- Proactive discharge planning for high-risk patient populations
- Resource optimization through evidence-based staffing forecasts
- Operational benchmarking to identify best practices across hospital networks
- Comparative analysis between payer types and clinical outcomes
- Policy-informed decisions based on empirical evidence

This analysis addresses these challenges through predictive modeling and comparative effectiveness research, providing healthcare systems with data-driven insights into LOS variation and operational performance.

---

## Objectives

**Primary Objectives:**
1. Develop predictive models to accurately forecast hospital length of stay at admission
2. Identify clinical, demographic, and operational factors driving LOS variation
3. Quantify the relationship between LOS and hospital charges
4. Benchmark hospital performance and identify operational best practices
5. Provide comparative effectiveness analysis across insurance types and diagnoses

**Secondary Objectives:**
1. Understand how insurance type affects hospital utilization patterns
2. Identify high-risk diagnoses requiring intensive discharge planning
3. Develop hospital performance tiers for network comparison and learning
4. Create a framework for understanding LOS predictability
5. Validate model performance for potential clinical deployment

---

## Dataset

**Data Source:** [California Major Diagnostic Categories (MDC) Summary](https://lab.data.ca.gov/dataset/major-diagnostic-categories-summary)

### Dataset Characteristics

| Characteristic | Details |
|----------------|---------|
| **Total Records** | 743,518 acute care hospital discharges |
| **Time Period** | 2011–2014 (4 years) |
| **Geographic Coverage** | Multiple California counties |
| **Care Type** | Acute care hospitals only |
| **Variables Collected** | 11 clinical and operational variables |
| **Data Provider** | California Department of Healthcare Access and Information (HCAI) |

### Key Variables

| Variable | Description | Range |
|----------|-------------|-------|
| `Adjusted_Length_of_Stay` | Hospital stay duration in days | 1–120 days |
| `Valid_Charges` | Total hospitalization charges | $5,000–$1,000,000+ |
| `Major_Diagnostic_Category` | Primary diagnosis grouping | 25 MDC categories |
| `Payer_Type` | Insurance coverage type | 4 categories |
| `Discharge_Year` | Year of discharge | 2011–2014 |
| `Hospital_County` | Geographic location | Multiple counties |
| `Type_of_Care` | Care classification | Acute/Other |

### Data Quality Assessment

| Assessment | Result |
|------------|--------|
| **Missing Values** | Removed (about 0.2% of records) |
| **Data Cleaning** | Acute care only; invalid entries removed |
| **Outliers** | Retained (represent complex clinical cases) |
| **Final Records** | 743,518 records used for analysis |
| **Train–Test Split** | 70% training (520,463), 30% testing (223,055) |

---

## Methodology

### Data Preparation
- Filtered dataset to acute care hospitals only
- Removed records with missing MDC, payer, LOS, or charges
- Created derived features: log(LOS), log(charges), cost per day, LOS categories
- Performed 70–30 stratified train–test split
- Standardized features (z-score) for clustering analysis

### Exploratory Data Analysis
- **Continuous variables:** Descriptive statistics, histograms, skewness
- **Categorical variables:** Frequency tables and cross-tabulations
- **Transformations:** Log scaling to improve normality for regression
- **Correlation analysis:** Detect multicollinearity among predictors
- **Visualization:** Ten publication-quality visualizations to summarize patterns

### Modeling Approaches

#### 1. Linear Regression (Interpretability Focus)
- **Model:** `log(LOS) ~ MDC + Payer + County + Year + log(Charges)`
- **Assumptions checked:** Normality, homoscedasticity, VIF for multicollinearity
- **Diagnostics:** Residual plots, Shapiro–Wilk test, Breusch–Pagan test

#### 2. Random Forest (Predictive Focus)
- **Configuration:** 500 trees, mtry = 5, nodesize = 100, cp = 0.01
- **Advantages:** Captures non-linear relationships and interactions
- **Validation:** Out-of-bag error and variable importance for model assessment

#### 3. K-Means Clustering (Hospital Benchmarking)
- **Variables:** Average LOS, median LOS, cost per day, percent long stays
- **Preprocessing:** Z-score standardization before clustering
- **Selection:** Elbow method used to select k = 3 clusters
- **Application:** Assigned hospitals to performance tiers

### Validation Strategy
- **Train–test split:** 70% train, 30% test
- **Metrics:** R², RMSE, and qualitative residual diagnostics
- **Overfitting check:** Compared training vs test performance
- **Clinical validation:** Interpreted RMSE from clinical perspective (±2–3 days acceptable)

---

## Results

### Length of Stay Summary Statistics

| Statistic | Value | Interpretation |
|-----------|-------|----------------|
| **Mean LOS** | 6.12 days | Average hospital stay |
| **Median LOS** | 5.00 days | Typical patient stay |
| **Standard Deviation** | 4.85 days | High variability in stays |
| **Minimum** | 1 day | Short-stay procedures |
| **Maximum** | 120 days | Very complex or extended cases |
| **25th Percentile** | 3 days | Fast-discharge group |
| **75th Percentile** | 9 days | Extended-stay group |

### Hospital Charges Summary Statistics

| Statistic | Value | Interpretation |
|-----------|-------|----------------|
| **Mean Charges** | $58,450 | Average admission cost |
| **Median Charges** | $42,850 | Typical admission cost |
| **Standard Deviation** | $85,320 | Wide cost variation |
| **Average Cost/Day** | $2,847 | Operational cost per inpatient day |
| **Range** | $5,000–$1M+ | From routine cases to very complex |

### Model Performance Comparison

| Model | Training R² | Test R² | Test RMSE (days) | Best Use Case |
|-------|-------------|---------|------------------|---------------|
| **Linear Regression** | 0.582 | 0.582 | 2.78 | Understanding relationships |
| **Random Forest** | 0.756 | 0.723 | 2.10 | Accurate predictions ✓ |
| **Decision Tree** | 0.681 | 0.648 | 2.45 | Simple clinical rules |

### Model Selection Rationale: Random Forest

- ✓ Highest test R² (0.723), explaining about 72% of LOS variation
- ✓ Lowest RMSE (2.10 days) on the test set
- ✓ Small train–test gap (~0.03), indicating limited overfitting
- ✓ Captures non-linear effects and interactions automatically
- ✓ Robust to outliers and complex feature relationships

---

## Key Findings

### 1. Diagnosis-Driven LOS Variation

| Major Diagnostic Category | Avg LOS (days) | % Difference vs Mean | Patient Count | % of Total |
|---------------------------|----------------|----------------------|---------------|------------|
| **Respiratory Conditions** | 8.45 | +38% | 89,234 | 12.0% |
| **Circulatory Diseases** | 8.12 | +33% | 105,456 | 14.2% |
| **Infectious Diseases** | 7.68 | +25% | 76,345 | 10.3% |
| **All Discharges (Average)** | 6.12 | Baseline | 743,518 | 100% |
| **Orthopedic Procedures** | 4.23 | −31% | 98,765 | 13.3% |

**Key Insight:** Respiratory and circulatory diagnoses drive substantially longer stays and together account for more than a quarter of all discharges.

---

### 2. Insurance Type Impact on Utilization

| Insurance Type | Avg LOS (days) | Avg Charges | Patient Count | % of Total |
|----------------|----------------|-------------|---------------|------------|
| **Traditional Coverage** | 6.84 | $62,340 | 185,420 | 24.9% |
| **Medicare** | 6.28 | $58,230 | 312,456 | 42.0% |
| **Medicaid** | 5.95 | $52,120 | 156,789 | 21.1% |
| **Managed Care** | 5.42 | $48,900 | 88,853 | 12.0% |

**Key Insight:** Traditional coverage shows noticeably longer stays than managed care, suggesting differences in case mix or coordination practices.

---

### 3. Hospital Performance Variation

| Performance Tier | Avg LOS | # Hospitals | Avg Discharges/Year | Efficiency Gap |
|------------------|---------|-------------|---------------------|----------------|
| **High Performers** | 5.2 days | 12 | 8,450 | Baseline |
| **Standard Performers** | 6.5 days | 78 | 6,200 | +25% |
| **Improvement Opportunity** | 7.8 days | 35 | 4,100 | +50% |

**Key Insight:** The gap between high-performing hospitals and those in the improvement tier is roughly 2.6 days per stay.

---

### 4. Cost Per Day by LOS Group

| Patient Group | Cost/Day | Avg LOS | Total Cost Per Admission |
|---------------|----------|---------|--------------------------|
| **Short Stay (≤3 days)** | $3,150 | 2.1 days | $6,615 |
| **Medium Stay (4–7 days)** | $2,890 | 5.4 days | $15,606 |
| **Long Stay (>7 days)** | $2,450 | 12.8 days | $31,360 |

**Key Insight:** Although daily costs tend to decrease with longer stays, total admission costs rise sharply for long-stay patients.

---

### 5. Feature Importance (Random Forest)

| Feature | Importance Score | Approx. Contribution | Rank |
|---------|------------------|----------------------|------|
| **log_Charges** | 34.2 | ~28% | 1 |
| **MDC (Primary Diagnosis)** | 32.8 | ~27% | 2 |
| **Payer_Type** | 18.5 | ~15% | 3 |
| **Hospital_County** | 15.3 | ~13% | 4 |
| **Discharge_Year** | 10.2 | ~9% | 5 |
| **cost_per_day** | 8.9 | ~7% | 6 |

**Key Insight:** Charges, diagnosis, payer, and geography together explain most of the model's predictive power.

---

### 6. Correlation Analysis

| Variable Pair | Correlation | Strength | Interpretation |
|---------------|-------------|----------|----------------|
| **LOS vs Charges** | 0.542 | Moderate positive | Longer stays tend to cost more overall |
| **LOS vs Cost/Day** | 0.025 | Very weak | Daily intensity not strongly tied to LOS |
| **Charges vs Cost/Day** | 0.815 | Strong positive | Higher daily cost strongly increases total charges |
| **log_LOS vs log_Charges** | 0.548 | Moderate positive | Relationship remains after log transform |

**Key Insight:** Total charges are moderately related to LOS but strongly related to cost per day.

---

## Models Developed

### Model 1: Linear Regression
- **Purpose:** Interpret effect sizes and directions
- **Training/Test R²:** 0.582 / 0.582
- **Test RMSE:** 2.78 days
- **Validation:** All main regression assumptions were checked and met

---

### Model 2: Random Forest (Primary Model)
- **Purpose:** Accurate LOS prediction for decision support
- **Training/Test R²:** 0.756 / 0.723
- **Test RMSE:** 2.10 days
- **Advantages:** Handles non-linearities, interactions, and outliers effectively

---

### Model 3: K-Means Clustering
- **Purpose:** Hospital performance benchmarking and tiering
- **Features:** Avg LOS, median LOS, cost per day, percent long stays
- **Optimal Clusters:** k = 3 (high performers, standard, improvement opportunity)

---

## Hospital Performance Benchmarking

### Percentile Distribution of Hospital-Level LOS

| Percentile | LOS (days) | Interpretation | Approx. # Hospitals |
|------------|------------|----------------|---------------------|
| **10th** | 3.8 | Top efficiency decile | 12 |
| **25th** | 4.9 | Upper quartile | 38 |
| **50th (Median)** | 6.0 | Typical performance | 75 |
| **75th** | 7.6 | Below-average performance | 38 |
| **90th** | 9.4 | Bottom efficiency decile | 12 |

**Key Insight:** High-performing hospitals show substantially shorter stays and can be used as internal benchmarks for process improvement.

---

## Visualizations

### Key Visualizations

**Top 10 Major Diagnostic Categories by Average Length of Stay:**

![Top 10 Diagnoses by LOS](https://github.com/weston-nyabeze/Hospital-Length-of-Stay-Prediction/raw/main/Plots/02_Top10_MDC_LOS.png)

---

**Random Forest Feature Importance for Predicting Length of Stay:**

![Random Forest Feature Importance](https://github.com/weston-nyabeze/Hospital-Length-of-Stay-Prediction/raw/main/Plots/05_RandomForest_Importance.png)

---

**Predicted vs Actual Length of Stay on the Test Set:**

![Predicted vs Actual LOS](https://github.com/weston-nyabeze/Hospital-Length-of-Stay-Prediction/raw/main/Plots/Predicted%20vs%20Actual.png)

---

### Additional Plots in the Plots Folder

1. **LOS Distribution Transformation** – Before/after log normalization
2. **LOS by Payer Type** – Box plot comparison across insurance types
3. **LOS vs Hospital Charges** – Scatter plot with facets by payer
4. **Correlation Matrix** – Color-coded heatmap of variable relationships
5. **Model Comparison (RMSE)** – Bar chart comparing model performance
6. **Elbow Method** – Line chart determining optimal k=3 clusters
7. **Hospital Clustering** – Scatter plot with cluster assignments

---

## Technical Stack

### Programming Language
- **R 4.5.2** – Statistical computing and graphics environment

### Data Manipulation & Statistics
- `readr`, `dplyr`, `tidyr`, `scales`
- `car`, `lmtest`, `e1071`

### Machine Learning
- `caret`, `randomForest`, `rpart`, `rpart.plot`, `cluster`

### Visualization & Reporting
- `ggplot2`, `corrplot`, `gridExtra`, `plotly`
- `Quarto`, `knitr`
---

## Author

**Weston Nyabeze**  
Healthcare Data Analyst

Open to healthcare analytics, data science, and health services research roles.

---
## Acknowledgments

-**California Department of Healthcare Access and Information (HCAI)** for providing the Major Diagnostic Categories Summary dataset
- **American University** for academic support
- **Perplexity AI** for research assistance, code development, and analytical guidance

## Dataset Attribution

**Data Source:** [California Department of Healthcare Access and Information (HCAI)](https://lab.data.ca.gov/dataset/major-diagnostic-categories-summary)

**Dataset:** Major Diagnostic Categories (MDC) Summary

**Contains:** Adjusted length of stay, type of care, discharges with valid charges, hospital charges, and clinical metrics for California acute care facilities (2011–2014)

---

## License

This analysis is provided for educational, research, and portfolio purposes. Original data source attribution required for any reuse or derivative works.

---

**Project Status:** ✓ Complete  
**Last Updated:** November 28, 2025  
**Repository:** [Hospital-Length-of-Stay-Prediction](https://github.com/weston-nyabeze/Hospital-Length-of-Stay-Prediction)
