# HealthConnect Clinic – Appointment Analysis

## Overview

The project focuses on analysing appointment attendance patterns and developing a baseline machine learning approach to better understand and predict patient no-shows.

The project is being developed progressively across the weekly Experience Lab activities. Week 4 established the project foundation, while Week 5 moved into practical data preparation, exploratory analysis, feature engineering, model development, and initial evaluation.

> Project status: Week 5 – Data Science Development and Initial Modelling completed.

---

## Project Context

HealthConnect is a fictional outpatient healthcare provider experiencing missed patient appointments.

Missed appointments can affect:

* Clinic scheduling and resource utilisation
* Healthcare staff planning
* Patient access to available appointment slots
* Operational efficiency
* Service delivery

The purpose of this project is to use data science techniques to identify patterns associated with missed appointments and establish a baseline predictive modelling approach that can be improved in later stages.

This project is intended as an analytical and learning exercise and is not a production healthcare prediction system.

---

## Project objective

The main objective is to investigate:

> Which appointment and patient-related factors are associated with patient no-shows, and can these patterns be used to develop an initial model for identifying appointments with a higher likelihood of being missed?

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
10. Model evaluation
11. Interpretation
12. Identification of limitations
13. Recommendations for further development

---

# Week 4 – Foundation stage

Week 4 focused on understanding the problem, reviewing the available dataset, assessing data quality, defining the machine learning problem, and planning the modelling approach.

Week 4 did not include a completed predictive model.

### Week 4 objectives

The Week 4 work focused on:

* Understanding the HealthConnect business problem
* Inspecting the available appointment dataset
* Assessing missing values and data quality
* Understanding the appointment outcome categories
* Identifying potential predictors
* Defining the target variable
* Considering data leakage risks
* Determining an initial modelling strategy
* Documenting assumptions, limitations, and risks

### Original dataset

The original HealthConnect dataset contained:

* 5,000 appointment records
* 18 variables

The original appointment outcomes were:

| Appointment outcome | Records | Percentage |
| ------------------- | ------: | ---------: |
| No-Show             |   2,423 |     48.46% |
| Attended            |   2,314 |     46.28% |
| Cancelled           |     263 |      5.26% |

### Missing values identified in Week 4

| Variable              | Missing values |
| --------------------- | -------------: |
| reminder_channel      |          1,366 |
| distance_to_clinic_km |             90 |
| waiting_time_minutes  |             60 |

Week 4 identified appointment_outcome as the initial target concept, with No-Show considered the positive class and Attended the negative class. Cancelled appointments were treated separately rather than as attendance failures.

---

# Week 5 – Data Science development and initial modelling

Week 5 moved the project from planning into practical implementation.

The main focus was preparing the dataset, investigating patterns in the data, engineering useful features, selecting modelling variables, developing a baseline classification model, and evaluating its initial performance.

---

## 1. Data preparation

The original dataset was preserved and was not overwritten.

A separate processed dataset was created for modelling:

HealthConnect_Processed_Dataset.csv

### Appointment outcome handling

Cancelled appointments were excluded from the modelling dataset because they represent a different outcome from both attendance and no-show behaviour.

After removing the 263 cancelled appointments:

Final modelling dataset: 4,737 records

The target variable was transformed into:

* 1 = No-Show
* 0 = Attended

### Identifiers removed

The following identifiers were excluded from modelling:

* appointment_id
* patient_id

These variables were not considered meaningful predictive features.

### Variables excluded from modelling

The following variables were excluded because they could introduce data leakage or were not appropriate predictors available before the appointment outcome:

* appointment_outcome
* waiting_time_minutes
* reminder_sent
* reminder_channel

The original date variables were retained for reference but were not directly used as predictors.

### Missing values

Missing distance_to_clinic_km values were handled using the median distance of 8.7 km.

After preprocessing, the prepared modelling dataset contained:

0 missing values

---

# 2. Exploratory data analysis

Exploratory analysis was conducted to understand the distribution of the target variable and identify relationships between appointment characteristics and no-show behaviour.

### Target distribution

After excluding cancelled appointments:

* No-Show: 2,423
* Attended: 2,314

This corresponds to approximately:

* 51.15% No-Show
* 48.85% Attended

The target variable was therefore relatively balanced, although no-show appointments were slightly more common.

### Key patterns observed

The exploratory analysis indicated several potentially important patterns:

* Longer booking lead times were associated with higher levels of no-show behaviour.
* Previous no-show history showed a strong relationship with the current appointment outcome.
* Distance to the clinic appeared to increase the relative no-show rate across distance groups, although some of the furthest groups contained relatively few observations.
* Numerical correlations between most variables were weak.
* previous_appointments and previous_no_shows showed a moderate positive relationship of approximately 0.461.
* Plausible extreme values were retained where they did not appear to represent data entry errors.

These findings informed the feature engineering and feature selection stages.

---

# 3. Feature engineering

Two additional categorical features were created to represent appointment characteristics in groups.

### Booking lead-time groups

The booking_lead_days variable was grouped into:

* 0–7 days
* 8–14 days
* 15–30 days
* 31–45 days
* 46–60 days

### Distance groups

The distance_to_clinic_km variable was grouped into:

* 0–5 km
* 6–10 km
* 11–15 km
* 16–20 km
* 21–30 km
* 31–50 km

The engineered variables were:

* booking_lead_group
* distance_group

Validation confirmed that the engineered groups contained no missing values.

---

# 4. Selected modelling features

The final feature set contained 12 modelling variables:

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

The selection was based on:

* Relevance to the prediction problem
* Availability before the appointment outcome
* Findings from exploratory analysis
* Avoidance of identifiers
* Avoidance of potential data leakage

---

# 5. Train/Test strategy

The prepared dataset was divided into training and testing datasets using a stratified 80/20 split.

random_state = 42

### Dataset sizes

| Dataset      | Records | Features |
| ------------ | ------: | -------: |
| Full dataset |   4,737 |       12 |
| Training set |   3,789 |       12 |
| Testing set  |     948 |       12 |

Stratification was used to maintain a similar proportion of No-Show and Attended appointments in both the training and testing datasets.

---

# 6. Baseline model

A Logistic Regression classifier was developed as the Week 5 baseline model.

The modelling workflow used a preprocessing pipeline containing:

* StandardScaler for numerical variables
* OneHotEncoder for categorical variables
* LogisticRegression as the classification model

The categorical encoder was configured to handle previously unseen categories during prediction.

The baseline model provides a starting point against which more advanced models can be compared in later stages.

---

# 7. Baseline model evaluation

The baseline Logistic Regression model was evaluated using the test dataset.

### Performance results

| Metric    | Result |
| --------- | -----: |
| Accuracy  | 61.39% |
| Precision | 60.84% |
| Recall    | 68.87% |
| F1 Score  | 64.60% |
| ROC-AUC   | 66.42% |

### Confusion matrix

The model produced:

|                 | Predicted Attended | Predicted No-Show |
| --------------- | -----------------: | ----------------: |
| Actual Attended |                248 |               215 |
| Actual No-Show  |                151 |               334 |

### Interpretation

The baseline model demonstrates moderate predictive ability, but there is substantial room for improvement.

The model correctly identified 334 of the 485 actual no-show appointments, resulting in a recall of 68.87%.

However, it also incorrectly classified some attended appointments as no-shows. The precision of 60.84% indicates that not every appointment predicted as a no-show was actually missed.

The ROC-AUC score of 0.6642 indicates that the model has some ability to distinguish between attended and no-show appointments, but the separation is still limited.

Therefore, the model should be treated as a baseline for further development rather than a deployment-ready solution.

---

# 8. Data Science findings

The Week 5 analysis produced several useful findings:

### Booking lead time

Appointments booked further in advance showed a tendency towards higher no-show behaviour.

This suggests that the amount of time between booking and appointment date may be useful when assessing appointment attendance risk.

### Previous No-Show history

Previous no-show behaviour showed an important relationship with the current appointment outcome.

This indicates that historical attendance behaviour may provide useful information for future modelling.

### Distance to clinic

No-show rates appeared to increase across some distance groups.

However, the furthest distance group contained relatively few observations, so this finding should be interpreted cautiously.

### Target balance

The modelling target was reasonably balanced after cancelled appointments were removed. Therefore, the baseline model was evaluated using standard classification metrics without relying on extreme class-imbalance assumptions.

---

# 9. Cross-Track collaboration and project dependencies

The Data Science work is connected to the other project tracks, particularly Data Analytics.

Analytical findings and visual exploration are important inputs into the Data Science workflow because they help identify:

* Relevant variables
* Potential relationships
* Patterns requiring further investigation
* Candidate features for modelling
* Questions that should be tested statistically or through machine learning

The Week 5 workflow therefore treats Data Analytics outputs as an important project dependency for refining the predictive modelling approach.

No unsupported claims of formal meetings or direct data exchange are made where such interaction has not been documented.

---

# 10. Limitations

Several limitations were identified during the Week 5 implementation.

### Dataset limitations

The dataset is limited to the variables provided in the HealthConnect dataset.

Additional factors that may influence appointment attendance may not be represented.

### Baseline model performance

The Logistic Regression model achieved only moderate performance.

The results indicate that additional modelling approaches and feature development should be investigated.

### Distance groups

Some distance categories, particularly the furthest group, contain relatively few observations. Results for these groups should therefore be interpreted cautiously.

### Feature availability

Variables that may have strong relationships with appointment outcomes were excluded when they were considered unsuitable for prediction because they could introduce leakage or may only become available after the appointment process.

### Generalisation

The dataset represents a specific project scenario. Model performance should not be assumed to generalise to other clinics, populations, or healthcare settings without further validation.

### Healthcare context

This project is an educational data science exercise. The baseline model should not be used to make automated decisions about individual patients.

---

# 11. Week 6 direction

The Week 5 baseline establishes a foundation for further development.

The next stage should focus on:

* Comparing additional classification algorithms
* Evaluating whether model performance improves
* Investigating feature importance or model coefficients appropriately
* Further validating engineered features
* Examining model errors
* Investigating threshold selection
* Considering precision-recall trade-offs
* Improving interpretation of model outputs
* Strengthening visual communication of findings
* Documenting reproducibility and modelling decisions
* Connecting modelling results with the broader project analysis

The goal is to improve the baseline while maintaining a clear distinction between analytical findings and assumptions.

---

# Repository structure

The repository is organised to keep original data, processed data, notebooks, and reports separate.

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
│   └── Week_5_HealthConnect_Data_Science.ipynb
│
├── reports/
│   ├── Week 4 project summary-HealthConnect Clinic Experience Lab.pdf
│   └── HealthConnect_Week_5_Project_Summary.docx
│
└── .gitignore
```

This structure ensures that the original dataset is preserved separately from processed and derived data.

---

# Project files

### Data

* data/raw/HealthConnect_Appointment_Data.csv – Original HealthConnect appointment dataset
* data/raw/HealthConnect_Data_Dictionary.xlsx – Dataset data dictionary
* data/processed/HealthConnect_Processed_Dataset.csv – Prepared dataset used for Week 5 analysis and modelling

### Notebooks

* notebooks/Week_4_HealthConnect_Data_Science.ipynb – Week 4 foundation work
* notebooks/Week_5_HealthConnect_Data_Science.ipynb – Week 5 data preparation, analysis, feature engineering, modelling, and evaluation

### Reports

* reports/Week 4 project summary-HealthConnect Clinic Experience Lab.pdf – Week 4 project summary
* reports/HealthConnect_Week_5_Project_Summary.docx – Week 5 project summary

---

# Tools and technologies

The project uses:

* Python
* Jupyter Notebook
* pandas
* NumPy
* Matplotlib
* Seaborn
* scikit-learn
* Anaconda
* Git
* GitHub

---

# Reproducibility and data management

The project follows a reproducible workflow by keeping the original data separate from processed data.

The original dataset should not be overwritten.

All transformations and derived features should be documented in the Week 5 notebook and reflected in the processed dataset.

The modelling workflow uses a fixed random state of 42 for the train/test split to support reproducibility.

---

# Project status

| Week   | Stage                                                             | Status     |
| ------ | ----------------------------------------------------------------- | ---------- |
| Week 4 | Business understanding, data inspection and project foundation    | Completed  |
| Week 5 | Data preparation, EDA, feature engineering and baseline modelling | Completed  |
| Week 6 | Model improvement, deeper evaluation and refinement               | Next stage |

---

# Author

Samuel Makobe
Data Science Intern
