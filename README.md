    # HealthConnect Clinic – Appointment No-Show Analysis

## Overview

This project analyses appointment attendance patterns for **HealthConnect Clinic**, a fictional outpatient healthcare provider, and develops a machine-learning approach for predicting patient no-shows.

The project is being developed progressively through the **AnalystLab Africa Experience Lab**. The work has progressed from business and data understanding to data preparation, exploratory analysis, baseline modelling, error analysis, cross-track integration, feature refinement, model comparison, testing, re-testing and validation.

> **Project status: Week 8 - Data Science Final Integration, Reproducibility Review, Documentation and Project Showcase completed.**

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
23. Final candidate reproduction
24. Final cross-track handoff documentation
25. Stakeholder communication and project showcase

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

# Week 8 – Final integration, reproducibility and project showcase

Week 8 moved the project from structured testing into final Data Science integration, reproducibility confirmation, stakeholder communication and presentation readiness. No new model was introduced. The focus remained on confirming and documenting the strongest candidate already supported by the available evidence.

## Week 8 objectives

The final Data Science stage included:

* Re-establishing the retained Week 6/7 candidate model.
* Recreating the engineered features required by the final model.
* Reproducing the final train/test workflow using the established stratified split.
* Confirming the final model metrics and confusion matrix.
* Comparing the Week 5 baseline with the final candidate.
* Consolidating final feature and model-selection decisions.
* Integrating validated findings from the Data Analytics track.
* Preparing the Data Science → ML Engineering handoff specification.
* Documenting limitations, responsible use and business suitability.
* Completing a full notebook reproducibility review.
* Preparing a non-technical stakeholder summary and final project presentation.

## Final candidate reproduction

The final **Refined Logistic Regression** was reproduced using the established 13-feature representation and the same stratified 80/20 split with `random_state = 42`.

The final test-set performance was:

| Metric | Final result |
| --- | ---: |
| Accuracy | **61.50%** |
| Precision | **60.87%** |
| Recall | **69.28%** |
| F1 Score | **64.80%** |
| ROC-AUC | **66.47%** |

The reproduced confusion matrix was:

```text
[[247, 216],
 [149, 336]]
```

The model therefore correctly identified **336 of 485 actual no-shows**, or approximately seven out of ten actual no-shows in the held-out test set.

The Week 8 reproduction matched the established Week 6/7 candidate results exactly within the same project environment. This confirms internal reproducibility of the workflow, but it is not independent external validation.

## Baseline vs final candidate

| Metric | Week 5 baseline | Final candidate | Change |
| --- | ---: | ---: | ---: |
| Accuracy | 61.39% | **61.50%** | +0.11 pp |
| Precision | 60.84% | **60.87%** | +0.03 pp |
| Recall | 68.87% | **69.28%** | +0.41 pp |
| F1 Score | 64.60% | **64.80%** | +0.20 pp |
| ROC-AUC | 66.42% | **66.47%** | +0.05 pp |

The final model produced modest rather than dramatic metric improvements. The main value of the later project stages came from stronger validation, error analysis, cross-track testing, evidence-based refinement decisions and reproducibility rather than optimisation of a single metric.

## Final model and feature decisions

The retained final candidate uses 13 predictors:

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

The final workflow retains:

* `previous_no_show_group` with categories 0, 1, 2 and 3+.
* `lead_no_show_interaction` as the established booking lead-time × previous no-show representation.
* StandardScaler for numerical features.
* OneHotEncoder with unknown-category handling for categorical features.
* Logistic Regression as the final classifier.

The Week 7 `booking_lead_log` refinement and collaborative additive `lead_no_show_score` were not adopted because neither produced meaningful and consistent improvement over the retained candidate.

## Final cross-track integration status

Validated Data Analytics findings supported the interpretation of booking lead time and previous no-show history as important factors associated with no-show behaviour. These findings were used to motivate and evaluate Data Science refinements rather than being automatically incorporated without predictive testing.

From the Data Science side, the final handoff specification for ML Engineering was prepared. It documents:

* The 13 required model inputs.
* Engineered feature requirements.
* Numerical and categorical preprocessing.
* Target definition: 1 = No-Show and 0 = Attended.
* Final Logistic Regression classifier.
* Expected predicted class and no-show probability outputs.

Final ML Engineering implementation and validation could not be completed before submission because the required cross-track collaboration was unavailable. This remains a **documented pending dependency** and is not presented as a completed integration.

## Final model suitability and responsible use

The final candidate demonstrates moderate and reasonably stable predictive performance within the current project framework. It should be treated as a **decision-support prototype**, not a production healthcare prediction system.

Important considerations include:

* False-positive rate: **46.65%**.
* False-negative rate: **30.72%**.
* The HealthConnect data are fictional/synthetic and educational.
* Predictive associations should not be interpreted as causal relationships.
* Performance has not been independently validated on real operational healthcare data.
* Classification-threshold optimisation and probability calibration remain future work.
* Predictions should not be used as the sole basis for decisions about individual patients.

## Week 8 final outputs

The final Data Science stage produced:

* Final Week 8 Data Science notebook.
* Reproduced final candidate model and evaluation.
* Baseline-versus-final comparison.
* Final error and suitability interpretation.
* Consolidated feature and model decisions.
* Data Analytics → Data Science integration evidence.
* Data Science → ML Engineering handoff specification.
* Documented pending ML Engineering integration dependency.
* Non-technical stakeholder model summary.
* Final project presentation and presentation script.
* Final reproducibility review.

## Final Data Science outcome

The final project outcome is a reproducible **Refined Logistic Regression** prototype that converts appointment and patient-history information into a structured no-show risk signal. The project demonstrates an end-to-end Data Science process that includes data preparation, exploratory analysis, feature engineering, model development, model comparison, cross-validation, error analysis, segment testing, cross-track validation, targeted refinement, evidence-based rejection of unsuccessful experiments, reproducibility and responsible model communication.

The model is suitable as a tested educational decision-support prototype and portfolio project, but further real-world validation would be required before operational use.

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
│   ├── HealthConnect_Week7_Data_Science.ipynb
│   └── HealthConnect_Week8_Data_Science.ipynb
│
├── reports/
│   ├── Week 4 project summary-HealthConnect Clinic Experience Lab.pdf
│   ├── HealthConnect_Week_5_Project_Summary.pdf
│   ├── HealthConnect_Week6_Project_Summary.pdf
│   ├── HealthConnect_Week6_DataScience_to_DataAnalytics_CrossTrack_Handoff.pdf
│   ├── HealthConnect_Week7_Data_Science_to_Data_ Analytics_Cross-track_handoff.pdf
│   ├── HealthConnect_Week7_Project_Summary.pdf
│   ├── HealthConnect_Non_Technical_Model_Summary_Report.docx
│   └── HealthConnect_Week8_Data_Science_Showcase_FINAL.pptx
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

## Week 8

* Final candidate model reproduction
* Final baseline-versus-candidate comparison
* Final model evaluation and confusion-matrix confirmation
* Final feature and model-selection documentation
* Data Analytics findings integrated into final interpretation
* Data Science → ML Engineering technical handoff specification
* Pending ML Engineering dependency documented transparently
* Final limitations and responsible-use assessment
* End-to-end Data Science walkthrough
* Final reproducibility review
* Non-technical stakeholder model summary
* Final project presentation and speaker script

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
| Week 8 | Final candidate reproduction, integration documentation, reproducibility review and project showcase       |  Completed  |

---

# Overall project progress

The HealthConnect project progressed from initial business and data understanding to a fully documented and reproducible Data Science prototype for appointment no-show risk.

The final evidence supports **Refined Logistic Regression** as the retained candidate model evaluated during the project. Its final held-out test performance is **61.50% accuracy, 60.87% precision, 69.28% recall, 64.80% F1 score and 66.47% ROC-AUC**.

The project demonstrates that meaningful Data Science progress is not limited to increasing a model metric. Later stages strengthened the solution through model comparison, cross-validation, false-positive and false-negative analysis, segment testing, robustness assessment, cross-track validation, targeted feature experiments, evidence-based rejection of unsuccessful refinements and exact workflow reproduction.

The final project progression was:

```text
Business Understanding
        ↓
Data Preparation & EDA
        ↓
Baseline Modelling
        ↓
Error Analysis & Feature Refinement
        ↓
Model Comparison & Cross-Validation
        ↓
Candidate Model Selection
        ↓
Structured Testing & Segment Analysis
        ↓
Robustness & Generalisation Assessment
        ↓
Targeted Refinement & Re-testing
        ↓
Cross-Track Validation
        ↓
Final Candidate Reproduction
        ↓
Integration Handoff & Responsible-Use Documentation
        ↓
Stakeholder Communication & Project Showcase
```

The final model remains a **decision-support prototype rather than a production-ready healthcare system**. Future work should include validation on real operational data, threshold optimisation, probability calibration, external or temporal validation and completion of downstream ML Engineering integration.

---

# Author

Samuel Makobe

Data Science Intern

    
