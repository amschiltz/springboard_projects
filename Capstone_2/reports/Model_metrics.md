# Final Model Metrics Summary

**Project:** Telco Customer Churn Prediction  
**Author:** Allison Schiltz  
**Date:** February 15, 2026

---

## Model Selection

**Selected Model:** Logistic Regression (All Add-Ons Feature Set)

---

## Feature Set

**Feature Count:** 28 features (pruned from original dataset)

**Features Used:**

| Category | Features |
|----------|----------|
| **Demographics** | `SeniorCitizen_Yes`, `Partner_Yes`, `Dependents_Yes` |
| **Internet Service** | `InternetService_Fiber_optic` |
| **Add-On Services** | `OnlineSecurity_Yes`, `OnlineSecurity_No_internet_service`, `OnlineBackup_Yes`, `OnlineBackup_No_internet_service`, `DeviceProtection_Yes`, `DeviceProtection_No_internet_service`, `TechSupport_Yes`, `TechSupport_No_internet_service`, `StreamingTV_Yes`, `StreamingTV_No_internet_service`, `StreamingMovies_Yes`, `StreamingMovies_No_internet_service` |
| **Contract & Billing** | `Contract_One_year`, `Contract_Two_year`, `PaperlessBilling_Yes`, `PaymentMethod_Credit_card_(automatic)`, `PaymentMethod_Electronic_check`, `PaymentMethod_Mailed_check` |
| **Engineered Features** | `tenure_group_eng_7-12`, `tenure_group_eng_13-24`, `tenure_group_eng_25-48`, `tenure_group_eng_49+`, `monthly_charges_bin_eng_Low`, `monthly_charges_bin_eng_Medium` |

**Feature Source:** `data/processed/pruned_features_all_add_ons.csv`

---

## Model Architecture & Hyperparameters

**Algorithm:** Logistic Regression with L1 Regularization

**Hyperparameters (tuned via GridSearchCV):**
- **Regularization Strength (C):** 10
- **Penalty:** L1
- **Solver:** liblinear
- **Class Weight:** balanced (adjusts for imbalanced target)
- **Max Iterations:** 1000
- **Random State:** 42

**Hyperparameter Tuning:**
- **Method:** GridSearchCV with 5-fold Stratified Cross-Validation
- **Scoring Metric:** Average Precision (PR-AUC)
- **Search Space:** C ∈ [0.01, 0.1, 1, 10, 100]; penalty ∈ [l1, l2]; solver ∈ [liblinear, saga]
- **Selected Threshold:** 0.4856 (optimized to maximize recall subject to precision ≥ 0.52)

---

## Performance Metrics

### Out-of-Fold (OOF) Performance on Training Data

| Metric | Value |
|--------|-------|
| **Recall** | 0.8167 |
| **Precision** | 0.5202 |
| **ROC-AUC** | 0.8463 |
| **Average Precision (PR-AUC)** | 0.6500 |
| **True Positive Rate** | 0.8167 |
| **True Negative Rate** | 0.7274 |
| **False Positive Rate** | 0.2726 |
| **False Negative Rate** | 0.1833 |

**Confusion Matrix (OOF, threshold = 0.4856):**

|             | Predicted Negative | Predicted Positive |
|-------------|--------------------|--------------------|
| **Actual Negative** | 3004 | 1126 |
| **Actual Positive** | 274 | 1221 |

---

### Test Set Performance (Final Holdout Evaluation)

| Metric | Value |
|--------|-------|
| **Recall** | 0.8155 |
| **Precision** | 0.4864 |
| **ROC-AUC** | 0.8342 |
| **Average Precision (PR-AUC)** | 0.6273 |
| **True Positive Rate** | 0.8155 |
| **True Negative Rate** | 0.6883 |
| **False Positive Rate** | 0.3117 |
| **False Negative Rate** | 0.1845 |

**Confusion Matrix (Test Set, threshold = 0.4856):**

|             | Predicted Negative | Predicted Positive |
|-------------|--------------------|--------------------|
| **Actual Negative** | 711 | 322 |
| **Actual Positive** | 69 | 305 |

---

