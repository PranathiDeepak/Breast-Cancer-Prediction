# Breast Cancer Survival Prognosis
### Leveraging SEER Cancer Registry Data for Survival Prediction

> Master's Project — MS Computer Science · California State University, Dominguez Hills · Fall 2025  
> **Author:** Pranathi Deepak · **Advisor:** Dr. Liudong Zuo

---

## Overview

This project develops and compares a suite of survival analysis models — Cox Proportional Hazards, Random Survival Forest, and DeepSurv — applied to **117,000+ breast cancer patients** from the SEER registry (2000–2017). The goal is a reproducible, interpretable ensemble risk-scoring pipeline that can meaningfully stratify patients into low, medium, and high risk groups.

**Live Demo →** Open `app/index.html` in your browser for an interactive risk predictor.

---

## Results at a Glance

| Model | Test C-index | Bootstrap Mean | 95% CI | AUC @ 12mo |
|---|---|---|---|---|
| Cox Proportional Hazards | 0.7900 | 0.7918 | [0.7808, 0.8008] | 0.8710 |
| **Random Survival Forest** | **0.8000** | **0.8035** | **[0.7928, 0.8128]** | **0.9123** |
| DeepSurv | 0.7900 | — | — | — |

**Top prognostic features (RSF permutation importance):** age group, nodes positive, AJCC M stage, summary stage, surgery type, tumor size.

---

## Repository Structure

```
breast-cancer-survival/
├── notebooks/
│   ├── breast_cancer_survival_analysis.ipynb   # Main analysis (Cox, RSF, DeepSurv, SHAP)
│   └── exploratory_analysis.ipynb              # EDA and cohort exploration
├── data/
│   ├── Breast_Cancer.csv                       # Processed patient dataset
│   ├── df_encoded_final.csv                    # Model-ready encoded features
│   └── seer_query_config.txt                   # SEER*Stat export configuration
├── models/
│   └── deepsurv_best.pth                       # Trained DeepSurv weights (PyTorch)
├── results/
│   ├── cox_model_summary.csv                   # Cox coefficient table
│   ├── cox_feature_importance.csv              # Penalized Cox |coef| ranking
│   └── rsf_permutation_importance.csv          # RSF permutation importance scores
├── app/
│   └── index.html                              # Interactive web app (no server needed)
├── FinalReport_PranathiDeepak.pdf              # Full MS project report
├── requirements.txt
└── README.md
```

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/breast-cancer-survival.git
cd breast-cancer-survival

# 2. Set up environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run the main notebook
jupyter notebook notebooks/breast_cancer_survival_analysis.ipynb

# 5. Open the interactive app
open app/index.html             # or just drag it into your browser
```

---

## Data

**Source:** [SEER Program](https://seer.cancer.gov) — Incidence Database, 17 Registries, Nov 2024 submission (2000–2022 diagnoses).

**Cohort:** Female breast cancer patients diagnosed 2000–2017, with complete AJCC T/N/M staging.

**22 clinical features across 6 categories:**

| Category | Features |
|---|---|
| Demographics | Age group, sex, race, marital status, year of diagnosis |
| Tumor | Primary site, histology, grade, laterality, summary stage, tumor size |
| Node / Metastatic | Nodes examined, nodes positive, AJCC T, AJCC N, AJCC M |
| Receptor Status | ER status, PR status, HER2 status |
| Treatment | Surgery type, radiation, chemotherapy |

---

## Methods

### Preprocessing Pipeline

- **Numeric columns** (tumor size, node counts, year): median imputation → StandardScaler
- **Categorical columns**: mode imputation → OneHotEncoding (Cox) / LabelEncoding (RSF, DeepSurv)
- **TNM missing**: rows dropped (stage is non-negotiable for survival modeling)
- **Train/test split**: stratified 70/30 on event status

### Models

**Cox Proportional Hazards**
- Ridge regularization (α = 1×10⁻⁴) to handle correlated AJCC variables
- Variance threshold to remove near-constant OHE columns
- Provides interpretable hazard ratios

**Random Survival Forest**
- 200 trees, max depth 20, min samples per leaf 10
- Label-encoded categorical inputs (tree-compatible)
- Batch CHF prediction to handle memory constraints at scale

**DeepSurv**
- Architecture: 22 → 128 → 64 → 1 (ReLU + Dropout 0.2)
- Loss: Cox negative log partial likelihood
- Optimizer: Adam (lr=1×10⁻³), 20 epochs, batch size 2048
- Interpretability via SHAP DeepExplainer

**Ensemble**
- Min-max normalize Cox and RSF risk scores to [0, 1]
- Average → ensemble risk score
- Patients stratified into Low / Medium / High risk by tercile
- KM curves show monotonic survival separation across all follow-up windows

---

## Evaluation

- **Concordance Index (C-index)** — primary discrimination metric
- **Bootstrap CI** — 50 resamples on 5,000-patient subset for stability estimates
- **Time-dependent AUC** — at 12, 36, 60, 120 months
- **Kaplan-Meier stratification** — visual survival separation by risk group
- **SHAP** — global and local feature importance for DeepSurv

---

## Key Findings

1. **RSF outperforms Cox** on both C-index (0.80 vs 0.79) and time-dependent AUC, especially at longer horizons (120 months).
2. **Age group is the single strongest predictor** in the RSF permutation importance ranking, followed by nodes positive and AJCC M stage.
3. **Ensemble improves risk stratification** over any single model — KM curves show cleaner separation between Low/Medium/High risk groups.
4. **SHAP reveals nonlinear patterns**: tumor size shows a threshold effect; year of diagnosis shows improving survival over time, consistent with advances in screening and targeted therapy.
5. **DeepSurv is competitive** (C-index 0.79) and successfully captures nonlinear hazard structure, validated by SHAP dependence plots.

---

## Dependencies

```
numpy==1.26.4
pandas>=2.0
scikit-learn>=1.3
scikit-survival>=0.22
torch>=2.0
lifelines>=0.27
shap>=0.43
matplotlib>=3.7
seaborn>=0.12
jupyter
```

See `requirements.txt` for pinned versions.

---

## Cite / Reference

```
Deepak, P. (2025). Breast Cancer Prognosis: Leveraging Cancer Registry Data for 
Survival Prediction. MS Computer Science Project, California State University, 
Dominguez Hills.

Data: Surveillance, Epidemiology, and End Results (SEER) Program 
(www.seer.cancer.gov), National Cancer Institute, DCCPS, Surveillance Research 
Program, released April 2025, based on the November 2024 submission.
```

---

## Acknowledgements

Dr. Liudong Zuo (advisor), Dr. Jianchao Han, Dr. Pei-Chen Peng — CSUDH Computer Science Department.

---

*Research use only. Not validated for clinical decision-making.*
