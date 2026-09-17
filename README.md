Loan Default / Credit Risk Prediction — Real Data ML Project

Overview

This project builds a Loan Default / Credit Risk Prediction system using real loan datasets. The goal is to predict whether a loan applicant is likely to default or repay, helping lenders assess credit risk more consistently.

The notebook follows this complete pipeline:

Filtering → Dataset Creation → Realistic Check → Cleaning & Preprocessing → Feature Engineering → Feature Selection → Train/Test Split → Model Training → Evaluation → Cross-Validation → Class Imbalance Handling → Feature Importance → New Applicant Prediction → Cost-Benefit Analysis → Risk Tiering

Datasets

Two publicly available loan datasets are combined:

credit_risk_dataset.csv

32,581 borrowers

12 columns

Includes age, income, loan amount, loan grade, interest rate and default status.

Loan_Default_csv.xlsx

148,670 loan/mortgage applicants

34 columns

Contains financial and credit-risk information with missing values and categorical age bands.

The datasets are mapped to a common schema before being combined. Some variables, such as credit score and debt-to-income ratio, require approximate mappings because the original datasets use different definitions.

Machine Learning Models

Three binary-classification models are trained and compared:

Logistic Regression — interpretable statistical baseline.

Decision Tree — rule-based model that is easy to understand and visualize.

Random Forest — ensemble of decision trees designed to capture more complex patterns.

Data Preprocessing

The notebook performs:

Removal of impossible or unusable records.

Missing-value flagging for employment length.

Median imputation for missing numerical values.

Outlier clipping at the 1st and 99th percentiles.

Standardization for Logistic Regression.

Feature Engineering

Additional features are created:

loan_to_income_ratio = loan amount / annual income

credit_score_normalized = credit score converted to a 0–100 scale

high_dti_flag = 1 when debt-to-income ratio is above 40%

age_group = applicant age divided into meaningful age ranges

Feature Selection

Candidate features are examined using:

Correlation with the default target.

Random Forest feature importance.

The top 8 features by Random Forest importance are selected for modeling. The original source column is excluded so the models do not simply learn which dataset an applicant came from.

Model Training

The data is divided using a 70/30 stratified train/test split:

70% → Training data

30% → Held-out testing data

The same split is used for all three models so their performance can be compared fairly.

Evaluation Metrics

The models are evaluated using:

Accuracy — percentage of total predictions that are correct.

Precision — among applicants predicted as defaulters, how many actually defaulted.

Recall — among actual defaulters, how many were identified.

F1-score — balance between precision and recall.

ROC-AUC — ability to distinguish defaulters from non-defaulters across different thresholds.

Confusion Matrix — shows true positives, true negatives, false positives and false negatives.

ROC curves are also generated to compare the three models visually.

Cross-Validation

A 5-fold Stratified Cross-Validation check is performed. Each model is retrained across five different folds, and the mean and standard deviation of ROC-AUC are reported to assess model stability.

Class Imbalance

Approximately 24% of applicants in the combined dataset are defaulters.

To address this imbalance, balanced versions of all three models use:

class_weight="balanced"

This gives more importance to the minority default class and can improve recall, although it may reduce precision. In lending, this trade-off is important because missing a genuine defaulter can be more costly than incorrectly flagging a safe applicant.

New Applicant Prediction

The notebook includes a raw-input prediction function accepting:

Age

Annual income

Loan amount

Interest rate

Credit score

Debt-to-income percentage

Employment length

Engineered features are calculated automatically before the applicant is passed to the trained models.

An interactive widget demo is also included so users can change applicant values and view predictions from all three models.

Cost-Benefit Analysis

The project translates prediction errors into estimated business costs.

False Negative: Model predicts a safe applicant, but the applicant defaults. This is treated as the more expensive error because the lender may lose the loan principal.

False Positive: Model predicts risk, but the applicant would have repaid. The estimated cost is based on lost interest income.

Formula:

Total Estimated Cost = (False Negatives × Cost per False Negative) + (False Positives × Cost per False Positive)

Lower estimated cost is better from the business perspective.

Risk Tiering

Applicants are grouped according to predicted probability of default:

Risk Tier

Probability of Default

Low Risk

< 20%

Medium Risk

20%–<50%

High Risk

≥ 50%

These tiers can support different actions such as standard pricing, risk-based pricing, or manual underwriting review.

Responsible AI & Limitations

This notebook is a prototype/demonstration, not a deployment-ready lending system.

Important limitations include:

The two datasets use different definitions and scales for some variables.

Some features are approximated when creating the common schema.

Monthly income from one dataset is annualized.

Letter loan grades from one dataset are mapped to approximate numeric credit scores.

Fairness/disparate-impact testing has not been performed.

Real-world lending would require consistent data pipelines, model validation, privacy controls, regulatory review and human oversight.

The model should support human decision-making rather than completely replace it.

Technologies Used

Python

NumPy

Pandas

Matplotlib

Seaborn

Scikit-learn

OpenPyXL

IPyWidgets

Google Colab / Jupyter Notebook

How to Run

Open the notebook in Google Colab or Jupyter Notebook.

Upload credit_risk_dataset.csv and Loan_Default_csv.xlsx.

Run the installation/import cell.

Run the notebook cells from top to bottom.

Review model comparison, confusion matrices, ROC curves and cross-validation results.

Use the interactive applicant demo to test hypothetical applicants.

Project Outputs

The notebook produces:

Cleaned and combined real dataset

Exploratory visualizations

Selected predictive features

Three trained ML models

Model performance comparison

Confusion matrices

ROC curves

Cross-validation results

Class-balanced model comparison

Feature-importance analysis

New applicant predictions

Cost-benefit comparison

Risk-tier analysis

Executive summary

Regulatory/compliance considerations

Key Takeaway

Loan-default prediction is not only a machine-learning problem. A useful lending model must also consider recall, financial cost, risk tiers, fairness, explainability, human oversight and regulatory requirements.
