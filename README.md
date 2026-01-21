# Breast-Cancer-Prediction
Overview

This project presents a comprehensive survival analysis framework for predicting breast cancer prognosis using population-level data from the SEER (Surveillance, Epidemiology, and End Results) database. The study integrates classical statistical survival modeling with modern machine learning and deep learning approaches to capture both linear and non-linear risk patterns.
The implemented models include:
Cox Proportional Hazards (Cox PH)
Random Survival Forest (RSF)
Deep Neural Network for Survival Analysis (DeepSurv)
Ensemble Risk Model combining Cox and RSF
Explainability using SHAP for DeepSurv
Model performance is evaluated using concordance index (C-index), time-dependent AUC, Kaplan–Meier survival curves, and bootstrap confidence intervals. Feature importance is analyzed through permutation importance, Cox coefficients, and SHAP values.

Dataset

The analysis uses the SEER breast cancer registry containing:
Demographic variables (age, sex, race, marital status)
Tumor characteristics (tumor size, grade, AJCC T/N/M stage, summary stage)
Hormone receptor status (ER, PR, HER2)
Treatment information (surgery, radiation, chemotherapy)
Survival time and censoring indicators

A consistent set of 22 clinically relevant features is used across Cox, RSF, and DeepSurv models.

Methodology

Preprocessing
Survival time extraction and censoring definition
Median and mode imputation
Label encoding and standardization
Stratified train–test split
Model-specific feature transformation

Modeling
Cox PH: Regularized partial likelihood estimation
RSF: 200-tree ensemble with depth and leaf constraints
DeepSurv: Fully connected neural network trained using Cox partial likelihood loss
Ensemble: Min-max normalized risk averaging of Cox and RSF

Evaluation
Concordance Index (C-index)
Time-dependent AUC at 1, 3, 5, and 10 years
Kaplan–Meier risk group separation
Bootstrap confidence intervals
SHAP-based interpretability

Key Results
RSF achieved the highest C-index among individual models.
DeepSurv captured complex non-linear survival patterns and provided interpretable explanations using SHAP.
Ensemble risk stratification produced clear separation of Kaplan–Meier curves.
The most influential prognostic factors across models include:
Age group
AJCC T, N, and M stage
Tumor size
Lymph node involvement
Summary stage
Surgery type
Hormone receptor status

Significance
This work demonstrates the effectiveness of combining statistical survival analysis, machine learning, deep learning, and explainable AI to improve prognostic modeling in oncology. The framework supports reproducible research, interpretable risk stratification, and robust performance evaluation.

Technologies:
Python
scikit-survival
lifelines
PyTorch
SHAP
scikit-learn
NumPy, Pandas, Matplotlib
