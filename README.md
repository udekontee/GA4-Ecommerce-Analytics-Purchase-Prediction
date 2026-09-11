# GA4-Ecommerce-Analytics-Purchase-Prediction
End-to-end GA4 ecommerce analysis using BigQuery, SQL, Python, statistical testing, and machine learning to analyze customer behavior and predict future purchases.

## Project Overview

This project analyzes ecommerce customer behavior using the Google Analytics 4 (GA4) public ecommerce dataset from the Google Merchandise Store.

The project combines **BigQuery, SQL, Python, statistical analysis, and machine learning** to examine the ecommerce conversion funnel, compare customer behavior across devices and traffic sources, analyze user engagement, and predict future purchase behavior.

The analysis was completed in **Google Colab**, with BigQuery used to query the public GA4 dataset.

---

## Business Questions

This project addresses the following questions:

1. Where are the major drop-off points in the ecommerce conversion funnel?
2. Does purchase activity differ across device categories?
3. Does traffic source have a relationship with purchase activity?
4. How does engagement differ between purchasers and non-purchasers?
5. Can earlier user engagement behavior help predict future purchases?
6. Which machine learning model performs better for future purchase prediction?

---

## Data Source

The project uses the public GA4 ecommerce sample dataset:

`bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`

The dataset contains obfuscated Google Merchandise Store ecommerce activity covering:

**November 1, 2020 – January 31, 2021**

### Dataset Summary

- **4,295,584 events**
- **270,154 users**
- **92 days of activity**

---

## Tools and Technologies

- Google BigQuery
- SQL
- Google Colab
- Python
- Pandas
- NumPy
- SciPy
- Scikit-learn
- Matplotlib

---

## Analysis Workflow

The project follows an end-to-end analytics and machine learning workflow:

**Data Source → Exploratory Analysis → Funnel Analysis → Device Analysis → Traffic Source Analysis → Statistical Testing → User Engagement Analysis → Feature Engineering → Machine Learning → Model Evaluation → Business Recommendations**

---

## Ecommerce Funnel Analysis

The primary ecommerce funnel analyzed was:

**View Item → Add to Cart → Begin Checkout → Purchase**

| Funnel Stage | Events |
|---|---:|
| View Item | 386,068 |
| Add to Cart | 58,543 |
| Begin Checkout | 38,757 |
| Purchase | 5,692 |

The event-level analysis identified substantial drop-off between product views and cart activity and between checkout and purchase.

These percentages represent aggregate event-level progression indicators rather than exact user-level conversion probabilities.

---

## Device Analysis

Observed event-level purchase rates were:

| Device | Purchase Rate |
|---|---:|
| Mobile | 1.54% |
| Desktop | 1.43% |
| Tablet | 1.33% |

A chi-square test found a statistically significant association between device category and purchase activity:

- **p-value:** 0.0130
- **Cramér's V:** 0.0047

Although statistically significant, the extremely small Cramér's V indicates that the practical relationship is negligible.

---

## Traffic Source Analysis

Purchase activity also differed across traffic sources.

The chi-square analysis produced:

- **p-value:** approximately 4.51 × 10⁻⁸³
- **Cramér's V:** 0.0318

Traffic source showed a stronger relationship with purchase activity than device category, although the practical association remained weak.

---

## User Engagement Analysis

Purchasers demonstrated substantially higher engagement than non-purchasers.

| Purchase Status | Engagement Events | Page Views | Product Views |
|---|---:|---:|---:|
| Non-Purchasers | 3.20 | 4.22 | 1.08 |
| Purchasers | 47.40 | 51.67 | 22.63 |

This descriptive analysis showed a strong relationship between engagement behavior and purchase status.

---

## Leakage Prevention and Feature Engineering

To reduce target leakage, behavioral features and purchase outcomes were separated by time.

### Feature Period

**November 1 – December 30, 2020**

Features:

- Engagement events
- Page views
- Product views

### Prediction Period

**December 31, 2020 – January 31, 2021**

Target:

- Future purchase (`0` or `1`)

The resulting modeling dataset contained:

- **176,942 users**
- **176,684 non-purchasers**
- **258 purchasers**

Only approximately **0.15%** of users were future purchasers, creating a highly imbalanced classification problem.

---

## Machine Learning Models

Two classification models were evaluated:

- Logistic Regression
- Random Forest

Class weighting was used to address the severe target imbalance.

### Model Performance

| Model | Precision | Recall | F1-Score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.0118 | **0.6346** | **0.0231** | **0.8880** | **0.0197** |
| Random Forest | 0.0006 | 0.0192 | 0.0012 | 0.3472 | 0.0013 |

**Logistic Regression substantially outperformed Random Forest.**

The Logistic Regression model identified approximately **63% of actual future purchasers** and achieved a ROC-AUC of approximately **0.89**.

Because precision remains low, the model is better suited for **ranking and prioritizing higher-potential users** than making definitive individual purchase predictions.

---

## Final Model Interpretation

The standardized Logistic Regression coefficients were:

| Feature | Coefficient | Odds Ratio |
|---|---:|---:|
| Engagement Events | 1.7106 | 5.5325 |
| Product Views | 0.4622 | 1.5876 |
| Page Views | -0.6491 | 0.5225 |

**Engagement events were the strongest positive predictor of future purchase activity**, followed by product views.

The negative page-view coefficient should be interpreted cautiously because page views overlap with the other engagement variables and the coefficients represent relationships while holding the other predictors constant.

---

## Business Recommendations

Based on the analysis:

1. **Investigate major funnel drop-off points**, particularly product-view-to-cart and checkout-to-purchase activity.

2. **Prioritize highly engaged users** for further marketing analysis and audience segmentation.

3. **Use the Logistic Regression model as a prioritization tool**, rather than a definitive purchase predictor.

4. **Use traffic source as a supporting segmentation signal**, while recognizing that its practical relationship with purchase activity is weak.

5. **Use A/B testing to evaluate interventions.** Predictive and observational results identify patterns but do not establish that a marketing action will cause higher conversion.

---

## Limitations

The project uses an obfuscated public ecommerce sample covering approximately three months.

Funnel and device/traffic-source analyses are primarily based on aggregate event counts rather than complete user-level conversion journeys.

The exploratory chi-square analyses use repeated event-level observations, which may not fully satisfy the independence assumption.

The predictive model uses only three behavioral features. Additional behavioral, acquisition, session, and ecommerce features could potentially improve performance.

The feature and target periods were separated to reduce leakage, but the train/test split was performed across users within the same historical feature and future prediction windows rather than using a complete forward-in-time validation design.

The results identify statistical associations and predictive relationships, not causal effects.

---

## Conclusion

This project demonstrates an end-to-end ecommerce analytics workflow combining **SQL, BigQuery, Python, statistical analysis, data visualization, and machine learning**.

The analysis found substantial event-level funnel drop-off, limited practical differences across device categories, weak differences across traffic sources, and a strong relationship between user engagement and purchase behavior.

Logistic Regression substantially outperformed Random Forest for future purchase prediction. The results suggest that behavioral engagement data can provide useful information for prioritizing customers with relatively higher future purchase potential.

---

## Project Notebook

The complete analysis, SQL queries, Python code, statistical testing, machine learning models, visualizations, and interpretations are available in:

**`GA4_Ecommerce_Analytics_and_Purchase_Prediction.ipynb`**

---

## Author

**U Dekontee Kun**

Data Analytics | Data Science | Business Intelligence
