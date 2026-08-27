# HealthConnect Clinic – Week 4 Data Science

## Overview

Week 4 focused on establishing the foundation for a potential machine learning solution rather than developing the complete predictive model.

The main objective was to understand the appointment data, assess its quality and suitability, define the machine learning problem, identify a proposed target variable and potential input features, and establish an initial modelling approach.

---

## Project context

HealthConnect Clinic is a fictional outpatient healthcare provider offering appointment-based services to adult patients.

A key challenge considered in this project is missed appointments (no-shows), which can affect appointment scheduling and the efficient use of clinic resources.

As a Data Science intern, my role in Week 4 was to assess whether the available appointment data could support a machine learning solution for predicting appointment no-shows.

---

## Week 4 objectives

1. Reviewing the appointment dataset and relevant variables.
2. Assessing the quality and suitability of the available data.
3. Defining the machine learning problem.
4. Identifying the proposed target variable.
5. Identifying potential input features.
6. Defining how appointment cancellations would be handled.
7. Developing an initial modelling approach.
8. Identifying key assumptions, limitations and modelling risks.

---

## Dataset

The HealthConnect appointment dataset contains:

- 5,000 appointment records
- 18 variables

The variables include information relating to:

- Patient characteristics
- Appointment details
- Booking information
- Previous appointment history
- Reminder information
- Distance to the clinic
- Waiting time
- Appointment outcomes

The dataset is fictional and anonymised for the Experience Lab.

---

## Initial data assessment

The Week 4 assessment included:

- Dataset structure and dimensions
- Variable names and data types
- Missing value assessment
- Duplicate record assessment
- Appointment outcome categories
- Outcome distribution
- Initial assessment of data suitability

Missing values were identified in:

| Variable | Missing values |
|---|---:|
| reminder_channel | 1,366 |
| distance_to_clinic_km | 90 |
| waiting_time_minutes | 60 |

No completely duplicated records were identified during the initial assessment.

### Appointment outcomes

| Outcome | Count | Percentage |
|---|---:|---:|
| No-Show | 2,423 | 48.46% |
| Attended | 2,314 | 46.28% |
| Cancelled | 263 | 5.26% |

---

## Machine learning problem

The proposed machine learning problem is a supervised binary classification problem.

The aim is to predict whether a scheduled appointment will result in:

- No-Show
- Attended

The proposed target variable is:

appointment_outcome

For the initial binary classification approach:

- No-Show → positive class
- Attended → negative class
- Cancelled → handled separately

Cancelled appointments are treated separately because cancellation represents a different outcome from failing to attend a scheduled appointment.

---

## Potential input features

Potential input features identified during Week 4 include:

- gender
- age
- age_group
- appointment_type
- appointment_day
- appointment_time
- booking_lead_days
- previous_appointments
- previous_no_shows
- reminder_sent
- reminder_channel
- distance_to_clinic_km
- waiting_time_minutes

Identifier variables such as appointment_id and patient_id are not proposed as predictive features.

Potential data leakage will also be considered when determining whether information would realistically be available at the time a prediction is made.

---

## Initial modelling approach

The proposed approach is to develop a supervised binary classification solution.

The planned later stage workflow includes:

1. Preparing the target variable.
2. Assessing and treating missing values.
3. Preparing categorical and numerical features.
4. Assessing potential data leakage.
5. Splitting the data appropriately for modelling.
6. Developing suitable classification models.
7. Comparing model performance.
8. Evaluating the models using appropriate classification metrics.

Potential evaluation measures include:

- Precision
- Recall
- F1-score
- Confusion matrix
- Accuracy as a supporting metric

---

## Key considerations

Several considerations were identified during Week 4.

### Missing data

Missing values will require appropriate treatment during the data preparation stage. The meaning of missing values will be considered before selecting a suitable handling method.

### Data leakage

Features should only be used if the relevant information would be available at the time a no-show prediction is made. In particular, waiting_time_minutes requires further assessment.

### Class distribution

The appointment outcome contains three categories. The No-Show and Attended outcomes will form the initial binary prediction target, while Cancelled appointments will be treated separately.

### Assumptions

The initial approach assumes that the available appointment information represents information that can reasonably be available before the scheduled appointment. It also assumes that the recorded appointment outcomes are sufficiently reliable for use as the prediction target.

### Limitations

The dataset is fictional and anonymised. Therefore, patterns identified in the dataset may not fully represent real-world patient behaviour or clinic operations.

---

## Week 4 scope

Week 4 was intentionally limited to:

- Problem understanding
- Resource review
- Initial data assessment
- Problem definition
- Target definition
- Feature identification
- Initial modelling planning
- Identification of assumptions, limitations and risks

**No predictive model was trained during Week 4.**

Model development and further data preparation are planned for subsequent stages of the project.

---

## Repository Contents

```text
HealthConnect-Week4-Data-Science/
│
├── README.md
├── Week_4_HealthConnect_Data_Science.ipynb
├── Week_4_HealthConnect_Project_Summary.docx
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   └── HealthConnect_Data_Dictionary.xlsx
│
└── .gitignore

---

## Author

Samuel Makobe

Data Science Intern | AnalystLab Africa Experience Lab
