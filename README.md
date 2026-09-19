# HealthConnect Clinic – Appointment No-Show Analysis

## Overview

This project analyses appointment attendance patterns for **HealthConnect Clinic**, a fictional outpatient healthcare provider, and develops a machine-learning approach for predicting patient no-shows.

The project is being developed progressively through the **AnalystLab Africa Experience Lab**. The work has progressed from business and data understanding to data preparation, exploratory analysis, baseline modelling, error analysis, cross-track integration, feature refinement, model comparison, testing, re-testing and validation.

> **Project status: Week 7 - Data Science Testing, Refinement, Cross-Track Validation and Re-testing completed.**

The project is an educational data science exercise and is **not a production healthcare prediction system**.

---

# Project context

HealthConnect Clinic is experiencing missed patient appointments.

Missed appointments can affect:

* Clinic scheduling and resource utilisation
* Healthcare staff planning
* Patient access to available appointment slots
* Operational efficiency
* Service delivery

The purpose of this project is to investigate patterns associated with missed appointments and determine whether these patterns can support an initial predictive modelling approach.

---

# Project objective

The main objective is to investigate:

> **Which appointment and patient-related factors are associated with patient no-shows, and can these patterns be used to develop a model for identifying appointments with a higher likelihood of being missed?**

The project follows a structured data science workflow:

1. Business and problem understanding
2. Dataset inspection
3. Data quality assessment
4. Data preparation
5. Exploratory data analysis
6. Feature engineering
7. Feature selection
8. Train/test strategy
9. Baseline model development
10. Error analysis
11. Cross-track integration
12. Feature refinement
13. Model comparison
14. Cross-validation
15. Model selection
16. Testing and validation
17. Targeted refinement
18. Re-testing
19. Segment performance analysis
20. Robustness and generalisation assessment
21. Model suitability assessment
22. Limitations and recommendations

---

# Week 4 – Project foundation

Week 4 focused on establishing the foundation of the HealthConnect project.

## Objectives

The work included:

* Understanding the HealthConnect business problem
* Inspecting the appointment dataset
* Assessing data quality
* Understanding appointment outcome categories
* Identifying potential predictors
* Defining the modelling target
* Considering data leakage risks
* Planning the initial modelling strategy
* Documenting assumptions and limitations

## Original dataset

The original HealthConnect dataset contained:

* **5,000 appointment records**
* **18 variables**

## Original appointment outcomes

| Appointment outcome | Records | Percentage |
| ------------------- | ------: | ---------: |
| No-Show             |   2,423 |     48.46% |
| Attended            |   2,314 |     46.28% |
| Cancelled           |     263 |      5.26% |

## Missing values identified

| Variable              | Missing values |
| --------------------- | -------------: |
| reminder_channel      |          1,366 |
| distance_to_clinic_km |             90 |
| waiting_time_minutes  |             60 |

Week 4 established the initial prediction problem, with **No-Show** treated as the positive class and **Attended** as the negative class. Cancelled appointments were treated separately.

---

# Week 5 – Data preparation, EDA and baseline modelling

Week 5 moved the project from planning into practical Data Science implementation.

The main focus was:

* Data preparation
* Data quality assessment
* Exploratory data analysis
* Feature engineering
* Feature selection
* Train/test splitting
* Baseline Logistic Regression
* Model evaluation

## Data preparation

The original dataset was preserved and a separate processed dataset was created.

### Cancelled appointments

The 263 cancelled appointments were excluded from the binary attended-versus-no-show modelling task.

This resulted in:

> **4,737 modelling records**

The target variable was transformed into:

* 1 = No-Show
* 0 = Attended

### Identifiers removed

The following identifiers were excluded from modelling:

* appointment_id
* patient_id

### Variables excluded

The following variables were excluded because they were potentially unavailable at prediction time or could introduce data leakage:

* appointment_outcome
* waiting_time_minutes
* reminder_sent
* reminder_channel

Date variables were retained for reference but were not directly used as predictors.

### Missing values

Missing distance_to_clinic_km values were handled using the median distance of **8.7 km**.

The prepared modelling dataset contained:

> **0 missing values**

---

# Exploratory data analysis

The Week 5 EDA investigated:

* Target distribution
* Numerical variables
* Categorical variables
* Feature-target relationships
* Correlations
* Potential outliers
* Appointment characteristics associated with no-show behaviour

After cancelled appointments were excluded:

| Target   | Records | Percentage |
| -------- | ------: | ---------: |
| No-Show  |   2,423 |     51.15% |
| Attended |   2,314 |     48.85% |

The target was therefore relatively balanced.

### Key Week 5 observations

The analysis indicated:

* Longer booking lead times were associated with higher no-show levels.
* Previous no-show history showed an important relationship with current no-show behaviour.
* Distance to the clinic showed a tendency towards higher no-show levels across some distance groups.
* Most numerical relationships were weak.
* previous_appointments and previous_no_shows showed a moderate positive correlation of approximately **0.461**.
* Plausible extreme values were retained rather than automatically removed.

---

# Week 5 feature engineering

Two grouped features were created.

### Booking lead-time groups

booking_lead_days was grouped into:

* 0–7 days
* 8–14 days
* 15–30 days
* 31–45 days
* 46–60 days

### Distance groups

distance_to_clinic_km was grouped into:

* 0–5 km
* 6–10 km
* 11–15 km
* 16–20 km
* 21–30 km
* 31–50 km

The engineered variables were:

* booking_lead_group
* distance_group

---

# Week 5 baseline features

The baseline model used 12 predictors:

```text
gender
age
age_group
appointment_type
appointment_day
appointment_time
booking_lead_days
booking_lead_group
previous_appointments
previous_no_shows
distance_to_clinic_km
distance_group
```

Feature selection considered:

* Prediction-time availability
* Exploratory analysis findings
* Relevance to the no-show problem
* Avoidance of identifiers
* Avoidance of potential data leakage

---

# Week 5 train/test strategy

A stratified 80/20 train/test split was used.

```text
random_state = 42
```

| Dataset      | Records | Features |
| ------------ | ------: | -------: |
| Full dataset |   4,737 |       12 |
| Training set |   3,789 |       12 |
| Testing set  |     948 |       12 |

Stratification was used to maintain a similar class distribution in the training and testing datasets.

---

# Week 5 baseline model

A **Logistic Regression** classifier was developed as the baseline model.

The modelling pipeline used:

* StandardScaler for numerical variables
* OneHotEncoder for categorical variables
* LogisticRegression as the classifier

### Baseline performance

| Metric    | Result |
| --------- | -----: |
| Accuracy  | 61.39% |
| Precision | 60.84% |
| Recall    | 68.87% |
| F1 Score  | 64.60% |
| ROC-AUC   | 66.42% |

### Confusion matrix

|                 | Predicted Attended | Predicted No-Show |
| --------------- | -----------------: | ----------------: |
| Actual Attended |                248 |               215 |
| Actual No-Show  |                151 |               334 |

The baseline correctly identified 334 actual no-shows but missed 151 no-show appointments.

The model demonstrated moderate predictive ability and was retained as the reference point for Week 6 improvement.

---

# Week 6 – Model refinement, error analysis and validation

Week 6 focused on understanding where the baseline model made errors, integrating findings from the Data Analytics track, refining the feature representation, comparing modelling approaches, and validating the final candidate model.

The Week 6 workflow consisted of:

1. Re-establishing the Week 5 baseline
2. False-positive and false-negative error analysis
3. Data Analytics → Data Science cross-track integration
4. Feature refinement
5. Refined Logistic Regression
6. Random Forest comparison
7. Five-fold stratified cross-validation
8. Final model selection
9. Documentation of limitations and remaining issues

---

# Week 6 error analysis

The baseline model produced:

* **151 false negatives**
* **215 false positives**
* **248 true negatives**
* **334 true positives**

The error analysis investigated model performance across:

* Booking lead-time groups
* Previous no-show history
* Appointment type
* Age groups
* Appointment day
* Appointment time
* Distance to clinic

### Main error-analysis findings

Booking lead time showed the clearest variation in model errors.

The **15–30 day** and **31–45 day** booking groups showed relatively high error rates.

The 15–30 day group contained the largest number of false negatives, while the 31–45 day group contained the largest number of false positives.

Previous no-show history was also important, with errors occurring across different levels of previous no-show behaviour.

Appointment type and distance showed secondary patterns, while appointment day and appointment time showed comparatively modest differences.

These findings were used to guide feature refinement rather than being treated as causal relationships.

---

# Week 6 cross-track collaboration

The Data Science track collaborated with the Data Analytics track.

The Data Analytics findings provided additional evidence about:

* Booking lead time
* Previous no-shows
* Distance
* Appointment type
* Age
* Appointment day
* Appointment time
* Booking lead time × previous no-show interaction

### Key Data Analytics finding

Booking lead time was identified as one of the strongest individual predictors of no-show behaviour.

Previous no-show history also showed an increasing no-show pattern.

The interaction between booking lead time and previous no-show history suggested that these variables may provide useful combined information.

Appointment time showed very little variation in no-show rates and was therefore reconsidered during feature refinement.

---

# Week 6 feature refinement

Based on the combined Data Analytics findings and Data Science error analysis, the feature representation was refined.

### Previous no-show group

Previous no-shows were transformed into:

```text
0
1
2
3+
```

The resulting groups contained:

| Previous no-show group | Records |
| ---------------------- | ------: |
| 0                      |   2,745 |
| 1                      |   1,482 |
| 2                      |     419 |
| 3+                     |      91 |

The 3+ grouping reduced the instability associated with very small individual categories.

### Booking lead-time × previous no-show interaction

A combined categorical interaction feature was created:

```text
lead_no_show_interaction
```

It combines:

```text
booking_lead_group
```

with:

```text
previous_no_show_group
```

This produced 20 possible combinations across the five booking lead-time groups and four previous-no-show groups.

### Appointment time

The appointment_time was removed from the refined feature set because the Data Analytics findings showed that its no-show differences were relatively flat.

---

# Week 6 refined feature set

The refined model used 13 features:

```text
gender
age
age_group
appointment_type
appointment_day
booking_lead_days
booking_lead_group
previous_appointments
previous_no_shows
previous_no_show_group
distance_to_clinic_km
distance_group
lead_no_show_interaction
```

The refined feature set contained:

* **5 numerical features**
* **8 categorical features**

The same stratified 80/20 split and random_state = 42 were retained to support a direct comparison with the Week 5 baseline.

---

# Week 6 refined Logistic Regression

The refined Logistic Regression model produced:

| Metric    | Week 5 Baseline | Week 6 Refined |
| --------- | --------------: | -------------: |
| Accuracy  |          61.39% |     **61.50%** |
| Precision |          60.84% |     **60.87%** |
| Recall    |          68.87% |     **69.28%** |
| F1 Score  |          64.60% |     **64.80%** |
| ROC-AUC   |          66.42% |     **66.47%** |

### Improvement

The improvements were modest:

* Accuracy: **+0.11 percentage points**
* Precision: **+0.03 percentage points**
* Recall: **+0.41 percentage points**
* F1 Score: **+0.20 percentage points**
* ROC-AUC: **+0.05 percentage points**

The refined model reduced false negatives:

```text
151 → 149
```

and increased true positives:

```text
334 → 336
```

The feature refinement therefore produced a small improvement rather than a major performance increase.

---

# Week 6 Random Forest comparison

A Random Forest Classifier was evaluated using the same refined feature set.

### Random Forest performance

| Metric    | Refined Logistic Regression | Random Forest |
| --------- | --------------------------: | ------------: |
| Accuracy  |                  **61.50%** |        59.92% |
| Precision |                  **60.87%** |        60.44% |
| Recall    |                  **69.28%** |        62.68% |
| F1 Score  |                  **64.80%** |        61.54% |
| ROC-AUC   |                  **66.47%** |        63.74% |

The Random Forest produced fewer false positives:

```text
216 → 199
```

but produced more false negatives:

```text
149 → 181
```

It also identified fewer true no-show appointments:

```text
336 → 304
```

The Random Forest therefore did not outperform the refined Logistic Regression model.

---

# Week 6 cross-validation

Five-fold Stratified Cross-Validation was performed to assess whether the observed model differences were reasonably consistent across multiple training/testing folds.

### Cross-validation results

| Metric    | Refined Logistic Regression | Random Forest |
| --------- | --------------------------: | ------------: |
| Accuracy  |                  **61.87%** |        61.20% |
| Precision |                  **62.47%** |        61.97% |
| Recall    |                  **63.72%** |        62.61% |
| F1 Score  |                  **63.09%** |        62.28% |
| ROC-AUC   |                  **67.45%** |        64.78% |

The refined Logistic Regression model performed better on all five cross-validation metrics.

This provided additional support for selecting Logistic Regression as the strongest model evaluated during Week 6.

---

# Final Week 6 model selection

The final selected model was:

> **Refined Logistic Regression**

### Test-set performance

* Accuracy: **61.50%**
* Precision: **60.87%**
* Recall: **69.28%**
* F1 Score: **64.80%**
* ROC-AUC: **66.47%**

### Five-fold cross-validation

* Accuracy: **61.87%**
* Precision: **62.47%**
* Recall: **63.72%**
* F1 Score: **63.09%**
* ROC-AUC: **67.45%**

The refined Logistic Regression model was selected because it consistently outperformed Random Forest on the available test-set and cross-validation results.

Importantly, the Week 6 findings demonstrate that greater model complexity does not automatically result in better predictive performance.

---

# Week 7 – Testing, refinement and validation

Week 7 focused on testing the Week 6 candidate model rather than rebuilding the modelling workflow.

The main objectives were:

* Re-establish the Week 6 candidate model.
* Test overall predictive performance.
* Conduct false-positive and false-negative analysis.
* Evaluate performance across important segments.
* Assess robustness and generalisation.
* Test a targeted feature refinement.
* Re-test the retained candidate.
* Complete meaningful cross-track testing with Data Analytics.
* Assess model suitability.
* Document remaining limitations and Week 8 requirements.

The Week 7 progression was:

```text
Week 6 Candidate
      ↓
Structured Testing
      ↓
Error Analysis
      ↓
Segment Performance Testing
      ↓
Robustness & Generalisation Testing
      ↓
Targeted Refinement
      ↓
Re-testing
      ↓
Cross-Track Validation
      ↓
Model Suitability Assessment
      ↓
Week 8 Preparation
```

---

# Week 7 candidate model testing

The Week 6 Refined Logistic Regression model was successfully re-established using the same modelling structure and stratified 80/20 split.

The candidate reproduced the Week 6 test-set performance:

| Metric    | Week 7 Test |
| --------- | ----------: |
| Accuracy  |  **61.50%** |
| Precision |  **60.87%** |
| Recall    |  **69.28%** |
| F1 Score  |  **64.80%** |
| ROC-AUC   |  **66.47%** |

The confusion matrix was:

```text
[[247, 216],
 [149, 336]]
```

This reproduced the Week 6 candidate results exactly.

---

# Week 7 error and segment testing

The candidate model was evaluated across:

* Booking lead-time groups
* Previous no-show history
* Appointment type
* Age
* Appointment day
* Appointment time
* Distance to clinic

### Main testing finding

Booking lead time continued to show the clearest variation in model performance.

In particular, shorter booking lead-time groups showed relatively low recall.

Previous no-show history also remained an important predictive feature.

Other tested segments showed variation but did not provide sufficient evidence to justify independent feature changes.

The results therefore supported **targeted refinement rather than broad model restructuring**.

---

# Week 7 robustness and generalisation testing

The retained model was evaluated using:

* Held-out test-set performance
* Training-versus-test comparison
* Existing 5-fold cross-validation results
* Cross-validation variability

The results showed:

* No obvious severe overfitting.
* Reasonably close training and test performance.
* Relatively small variation across cross-validation folds.
* Moderate but reasonably stable predictive performance.

The model nevertheless requires further validation using independent or temporally separated data before any production use could be considered.

---

# Week 7 targeted feature refinement

Based on the strong ordered relationship between booking lead time and no-show behaviour, a new logarithmic representation was tested:

```python
df["booking_lead_log"] = np.log1p(df["booking_lead_days"])
```

This created a non-linear representation of booking lead time while retaining the original `booking_lead_days` and `booking_lead_group` features.

The Week 7 refined model used **14 features**.

---

# Week 7 refinement results

The new booking lead-time representation produced:

| Metric    | Week 6 Candidate | Week 7 Refinement |
| --------- | ---------------: | ----------------: |
| Accuracy  |           61.50% |            61.50% |
| Precision |           60.87% |            60.91% |
| Recall    |           69.28% |            69.07% |
| F1 Score  |           64.80% |            64.73% |
| ROC-AUC   |           66.47% |            66.52% |

The overall changes were marginal.

The booking lead-time segment comparison also showed no consistent improvement in the weaker groups.

### Refinement decision

The `booking_lead_log` feature was therefore **rejected**.

The Week 6 Refined Logistic Regression model remained the retained candidate.

This was an evidence-based refinement decision: the feature was tested but not adopted because it did not demonstrate meaningful or consistent improvement.

---

# Week 7 cross-track testing

The Data Science track continued its collaboration with the **Data Analytics track**.

The Data Analytics findings identified:

* Booking lead time as a strong predictor.
* Previous no-show history as another important predictor.
* 15–30 day and 31–45 day booking lead-time groups as important segments.
* An additive rather than multiplicative relationship between booking lead time and previous no-show history.

The Data Science track tested this recommendation by creating:

```text
lead_no_show_score
```

The score combined ordinal booking lead-time groups with previous no-show groups.

---

# Collaborative model test

The additive `lead_no_show_score` model produced:

| Metric    | Week 6 Candidate | Collaborative Model |
| --------- | ---------------: | ------------------: |
| Accuracy  |           61.50% |              61.18% |
| Precision |           60.87% |              60.73% |
| Recall    |           69.28% |              68.25% |
| F1 Score  |           64.80% |              64.27% |
| ROC-AUC   |           66.47% |              66.39% |

The additive representation did not improve the candidate model.

### Cross-track decision

The `lead_no_show_score` representation was therefore **not adopted**.

The existing `lead_no_show_interaction` representation was retained.

The unsuccessful collaborative test was documented as evidence of model refinement and decision-making rather than discarded.

---

# Week 7 re-testing

After rejecting the proposed refinements, the retained Week 6 candidate was re-tested.

The re-test reproduced:

* Accuracy: **61.50%**
* Precision: **60.87%**
* Recall: **69.28%**
* F1 Score: **64.80%**
* ROC-AUC: **66.47%**

The confusion matrix was again:

```text
[[247, 216],
 [149, 336]]
```

The exact reproduction confirmed the candidate model's results within the same evaluation framework.

This should not be interpreted as independent validation because the same held-out test set was used.

---

# Week 7 model suitability assessment

The retained Logistic Regression model was assessed against:

* Overall predictive performance
* Error behaviour
* Segment performance
* Robustness
* Business relevance
* Operational readiness

The model demonstrated:

* Moderate predictive performance.
* Reasonable stability across the current validation framework.
* Meaningful predictive information about no-show behaviour.
* Continued false-positive and false-negative errors.
* Segment-level variation, particularly across booking lead-time groups.

The model is therefore considered:

> **Suitable for continued testing and development, but not yet suitable for production deployment.**

---

# Week 7 key outcomes

Week 7 achieved several important outcomes:

### 1. Candidate model testing

The Week 6 candidate was successfully re-established and its performance was reproduced.

### 2. Error testing

False-positive and false-negative patterns were investigated across relevant segments.

### 3. Segment performance testing

Booking lead time was confirmed as the clearest area requiring continued attention.

### 4. Robustness testing

Training/test and cross-validation evidence showed reasonable stability without obvious severe overfitting.

### 5. Targeted refinement

A logarithmic booking lead-time transformation was tested but rejected.

### 6. Cross-track validation

The Data Analytics additive recommendation was converted into a testable modelling feature and evaluated.

### 7. Evidence-based model retention

Neither tested refinement improved the candidate sufficiently to justify replacing the Week 6 model.

### 8. Re-testing

The retained candidate was re-tested and reproduced the Week 6 results.

### 9. Model suitability

The candidate was assessed as suitable for continued development but not production-ready.

---

# Important Week 7 modelling decisions

The project maintained the following principles:

* The Week 6 candidate remained the reference model.
* The same evaluation framework was maintained for direct comparison.
* Booking lead time remained a priority feature.
* Previous no-show history remained a relevant feature.
* The `booking_lead_log` refinement was rejected.
* The additive `lead_no_show_score` was rejected.
* The existing `lead_no_show_interaction` was retained.
* No leakage-prone variables were reintroduced.
* Segment variation was interpreted cautiously.
* Small subgroup sizes were not treated as definitive evidence.
* Predictive associations were not interpreted as causal relationships.
* Unsuccessful experiments were documented as evidence.
* The model was not presented as production-ready.

---

# Limitations and remaining risks

The current model should not be considered deployment-ready.

Important limitations include:

### Dataset

The HealthConnect dataset is a fictional/synthetic project dataset and may not represent real-world healthcare populations or clinic behaviour.

### Model performance

The selected model demonstrates moderate predictive performance rather than high predictive accuracy.

### False negatives

Some actual no-show appointments remain unidentified.

### False positives

Some attended appointments are incorrectly classified as potential no-shows.

### Segment variation

Model performance varies across booking lead-time groups and other segments.

### Generalisation

Performance should not be assumed to generalise to other clinics, patient populations or healthcare environments.

### Independent validation

The model has not yet been independently validated using a separate external or temporally separated dataset.

### Threshold and calibration

Alternative classification thresholds and probability calibration have not yet been fully evaluated.

### Operational use

Model predictions should not be used to make automated decisions about individual patients.

---

# Week 8 direction

The Week 8 focus should move from Week 7 testing into **final integration and validation**.

Potential priorities include:

* Final candidate model confirmation.
* Independent or temporal validation where possible.
* Probability calibration.
* Classification-threshold analysis.
* Precision-recall trade-off analysis.
* Continued segment-level testing.
* Final error analysis.
* ML Engineering integration.
* Final model interface validation.
* Documentation of the final candidate model.
* Final limitations and risk documentation.
* Preparation of final project presentation materials.

The focus should remain on producing an interpretable and evidence-based predictive workflow rather than optimising a single performance metric.

---

# Repository Structure

The repository is organised to separate raw data, processed data, notebooks, reports, and supporting project outputs.

```text
HealthConnect-Clinic-Experience-Lab-Appointment-Analysis/
│
├── README.md
│
├── data/
│   ├── raw/
│   │   ├── HealthConnect_Appointment_Data.csv
│   │   └── HealthConnect_Data_Dictionary.xlsx
│   │
│   └── processed/
│       └── HealthConnect_Processed_Dataset.csv
│
├── notebooks/
│   ├── Week_4_HealthConnect_Data_Science.ipynb
│   ├── Week_5_HealthConnect_Data_Science.ipynb
│   ├── Week_6_HealthConnect_Integration,_Advanced _Development _& _Validation.ipynb
│   └── HealthConnect_Week7_Data_Science.ipynb
│
├── reports/
│   ├── Week 4 project summary-HealthConnect Clinic Experience Lab.pdf
│   ├── HealthConnect_Week_5_Project_Summary.pdf
│   ├── HealthConnect_Week6_Project_Summary.pdf
│   ├── HealthConnect_Week6_DataScience_to_DataAnalytics_CrossTrack_Handoff.pdf
│   ├── HealthConnect_Week7_Data_Science_to_Data_ Analytics_Cross-track_handoff.pdf
│   └── HealthConnect_Week7_Project_Summary.pdf
│
└── .gitignore
```

---

# Project deliverables

## Week 4

* Project foundation notebook
* Initial dataset inspection
* Business/problem understanding
* Data quality assessment
* Initial modelling plan
* Week 4 project summary

## Week 5

* Data preparation
* Cleaned/processed dataset
* Exploratory data analysis
* Feature engineering
* Feature selection
* Baseline Logistic Regression
* Baseline model evaluation
* Week 5 project summary

## Week 6

* False-positive/false-negative error analysis
* Cross-track Data Analytics → Data Science integration
* Data Analytics findings incorporated into feature refinement
* Refined feature set
* Previous no-show grouping
* Booking lead-time × previous no-show interaction
* Refined Logistic Regression
* Random Forest comparison
* Confusion-matrix comparison
* Five-fold stratified cross-validation
* Final model selection
* Week 6 Project Summary
* Data Science → Data Analytics Cross-Track Handoff
* Week 6 notebook and supporting outputs

## Week 7

* Week 6 candidate model re-establishment
* Candidate model testing
* False-positive/false-negative testing
* Segment performance testing
* Booking lead-time performance analysis
* Previous no-show segment analysis
* Robustness and generalisation testing
* Training-versus-test evaluation
* Cross-validation stability assessment
* `booking_lead_log` refinement
* Refinement rejection based on evidence
* Data Analytics → Data Science cross-track testing
* `lead_no_show_score` collaborative experiment
* Re-testing of retained candidate
* Model suitability assessment
* Limitations and risk assessment
* Week 8 recommendations
* Week 7 Project Summary
* Week 7 supporting outputs

---

# Tools and technologies

The project uses:

* Python
* Jupyter Notebook
* Anaconda
* pandas
* NumPy
* Matplotlib
* Seaborn
* scikit-learn
* Git
* GitHub

### Machine learning techniques

* Logistic Regression
* Random Forest Classification
* One-Hot Encoding
* Feature scaling
* Stratified train/test splitting
* Five-fold stratified cross-validation
* Classification metrics
* Confusion-matrix analysis
* ROC-AUC evaluation
* Error analysis
* Segment-level performance analysis
* Feature engineering
* Model refinement
* Model re-testing

---

# Reproducibility and data management

The project follows a reproducible workflow.

Key principles include:

* The original dataset is preserved separately.
* Processed datasets are stored separately from raw data.
* Modelling transformations are implemented through pipelines.
* Numerical variables are standardised within the modelling pipeline.
* Categorical variables are one-hot encoded within the pipeline.
* A fixed random_state = 42 is used for reproducible train/test splitting.
* The same train/test split is maintained when comparing models where direct comparison is required.
* Model evaluation is performed using multiple complementary metrics.
* Cross-validation is used to assess model consistency.
* Segment-level performance is examined during validation.
* Modelling decisions and limitations are documented.
* Unsuccessful refinements are retained as evidence rather than removed from the project record.
* Cross-track recommendations are tested before being incorporated into the model.
* The project does not make causal claims from predictive associations.

---

# Project status

| Week   | Stage                                                                                                     | Status        |
| ------ | --------------------------------------------------------------------------------------------------------- | ------------- |
| Week 4 | Business understanding, data inspection and project foundation                                            |  Completed   |
| Week 5 | Data preparation, EDA, feature engineering and baseline modelling                                         |  Completed   |
| Week 6 | Error analysis, cross-track integration, feature refinement, model comparison and validation              |  Completed   |
| Week 7 | Testing, segment analysis, robustness testing, targeted refinement, re-testing and cross-track validation |  Completed   |
| Week 8 | Final integration, additional validation and final presentation                                           |  Next Stage |

---

# Overall project progress

The HealthConnect project has progressed from an initial understanding of the appointment problem to a structured and tested predictive modelling workflow.

The current evidence supports **Refined Logistic Regression** as the retained candidate model evaluated during the project.

Week 7 strengthened the project by testing the candidate model beyond the initial Week 6 validation stage.

The Week 7 work demonstrated the importance of:

* Testing rather than assuming model performance.
* Understanding false-positive and false-negative behaviour.
* Evaluating performance across relevant segments.
* Assessing robustness and generalisation.
* Refining features only when supported by evidence.
* Testing cross-track recommendations.
* Rejecting refinements that do not demonstrate meaningful improvement.
* Re-testing the retained candidate.
* Clearly documenting model limitations and operational risks.
* Preparing the model for final integration rather than presenting it as production-ready.

The project has therefore progressed through:

```text
Business Understanding
        ↓
Data Preparation
        ↓
Exploratory Analysis
        ↓
Baseline Modelling
        ↓
Error Analysis
        ↓
Cross-Track Integration
        ↓
Feature Refinement
        ↓
Model Comparison
        ↓
Cross-Validation
        ↓
Model Selection
        ↓
Testing
        ↓
Segment & Robustness Validation
        ↓
Targeted Refinement
        ↓
Re-testing
        ↓
Cross-Track Validation
        ↓
Final Integration & Validation
```

---

# Author

Samuel Makobe

Data Science Intern
