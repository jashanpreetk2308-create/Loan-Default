# Loan Default / Credit Risk Prediction

## 📌 Project Overview

This project uses Machine Learning to predict whether a loan applicant is likely to **default on a loan or repay it**.

The project is designed as a credit-risk assessment system that can help lenders make more consistent and data-driven lending decisions.

The project uses **one real publicly available Kaggle loan dataset** containing approximately **148,000 loan applications** and multiple financial attributes.

---

## 🎯 Business Problem

Loan default is a major risk for lending institutions. If a lender approves a loan for an applicant who later defaults, the lender may suffer a significant financial loss.

The objective of this project is to build a classification model that can:

* Predict loan default risk
* Compare different Machine Learning models
* Identify important factors related to default
* Estimate the probability of default
* Divide applicants into Low, Medium, and High-risk categories
* Support better lending decisions

---

## 📊 Dataset

**Dataset:** `Loan_Default_csv.xlsx`

The dataset contains approximately **148,670 mortgage/loan applicants** and 34 original columns.

Important variables include:

* Age
* Income
* Loan Amount
* Interest Rate
* Credit Score
* Debt-to-Income Ratio
* Default Status

The target variable is:

* `1` = Default
* `0` = No Default

---

## 🔄 Project Workflow

The complete Machine Learning pipeline is:

```text
Filtering
     ↓
Dataset Creation
     ↓
Realistic Data Check
     ↓
Data Cleaning & Preprocessing
     ↓
Feature Engineering
     ↓
Feature Selection
     ↓
Train/Test Split (70/30)
     ↓
Train 3 ML Models
     ↓
Model Evaluation
     ↓
Cross-Validation
     ↓
Class Imbalance Handling
     ↓
Feature Importance
     ↓
New Applicant Prediction
     ↓
Cost-Benefit Analysis
     ↓
Risk Tiering
     ↓
Executive Summary
```

---

## 🧹 Data Cleaning & Preprocessing

The following preprocessing steps are performed:

1. Relevant columns are selected.
2. Invalid rows such as non-positive income are removed.
3. Age ranges are converted into numerical midpoints.
4. Monthly income is converted into annual income.
5. Missing numerical values are replaced using the **median**.
6. Extreme values are clipped between the **1st and 99th percentiles**.
7. Additional validity checks are performed for age, income, and loan amount.

---

## ⚙️ Feature Engineering

New features are created to improve the model:

### 1. Loan-to-Income Ratio

```text
loan_to_income_ratio = loan_amount / annual_income
```

This represents the loan amount relative to the applicant's annual income.

### 2. Normalized Credit Score

The credit score is rescaled to a 0–100 range.

### 3. High DTI Flag

```text
high_dti_flag = 1 if DTI > 40%
                0 otherwise
```

This identifies applicants with relatively high debt compared with their income.

### 4. Age Group

Applicants are divided into different age groups for analysis and visualization.

---

## 🔍 Feature Selection

Two approaches are used:

* **Correlation with the target:** identifies how candidate variables move with default.
* **Random Forest Feature Importance:** measures the predictive usefulness of features.

The top 7 features based on Random Forest importance are selected for model training.

---

## 🤖 Machine Learning Models

Three binary classification models are trained:

### 1. Logistic Regression

Used as an interpretable statistical baseline.

### 2. Decision Tree

Uses decision rules and splits to classify applicants.

### 3. Random Forest

Uses multiple decision trees together to capture more complex patterns.

All three models use the same **70% training / 30% testing split**, allowing a fair comparison.

---

## 📈 Model Evaluation

The models are compared using:

### Accuracy

Measures the overall percentage of correct predictions.

### Precision

Of the applicants predicted as defaulters, precision tells us how many actually defaulted.

### Recall

Of the applicants who actually defaulted, recall tells us how many were correctly identified.

### F1-Score

Provides a balance between precision and recall.

### ROC-AUC

Measures how well the model distinguishes between defaulters and non-defaulters across different classification thresholds.

---

## 📉 Confusion Matrix & ROC Curve

The project generates:

* Confusion matrices for all three models
* Classification reports
* ROC curves
* ROC-AUC comparison

These visualizations help understand model performance beyond simple accuracy.

---

## 🔁 Cross-Validation

A **5-fold Stratified Cross-Validation** is performed.

Each model is trained and evaluated across five different data splits. The mean and standard deviation of ROC-AUC are calculated.

This provides a more reliable estimate of model performance than relying only on one train/test split.

---

## ⚖️ Class Imbalance Handling

Loan defaults represent a minority class in the dataset.

To reduce the tendency of models to simply predict "No Default", the project also trains balanced versions of the models using:

```python
class_weight="balanced"
```

This gives greater importance to the minority default class and generally helps improve recall for actual defaulters.

---

## 🔎 Feature Importance

Feature importance is examined using:

* Logistic Regression coefficients
* Decision Tree feature importance
* Random Forest feature importance

This helps identify which applicant characteristics contribute most to the model's predictions.

---

## 👤 New Applicant Prediction

The project can accept raw applicant information such as:

* Age
* Annual Income
* Loan Amount
* Interest Rate
* Credit Score
* Debt-to-Income Ratio

The system automatically calculates the engineered features and generates predictions using all three models.

Example output:

```text
Model                 Prediction          Probability of Default
----------------------------------------------------------------
Logistic Regression   DEFAULT RISK        XX.X%
Decision Tree         LIKELY TO REPAY     XX.X%
Random Forest         DEFAULT RISK        XX.X%
```

---

## 🖥️ Interactive Demo

An interactive widget is included where users can change:

* Age
* Annual Income
* Loan Amount
* Interest Rate
* Credit Score
* DTI

The predictions from all three models update automatically.

---

## 💰 Cost-Benefit Analysis

The project considers the financial impact of incorrect predictions.

### False Negative

The model predicts **safe**, but the applicant actually defaults.

This is treated as a high-cost error because the lender may lose the loan principal.

### False Positive

The model predicts **risky**, but the applicant would actually repay.

The estimated cost is based on the interest income that the lender could have earned.

The project calculates:

```text
Total Estimated Cost
=
(False Negatives × Cost per False Negative)
+
(False Positives × Cost per False Positive)
```

Therefore, model selection is considered from a **business perspective**, not only from an accuracy perspective.
