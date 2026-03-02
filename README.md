# US-Car-Accidents — Machine Learning Analysis and Modeling

This project explores the **US Car Accidents** dataset through data preparation, feature engineering,  
modeling, explainability techniques, calibration, and error analysis.  
The goal is to predict whether an accident reaches the **highest severity level (Severity = 4)** using  
only information available **at the moment the accident occurs** (t0).

---

## Project Overview

### 1. Data Preparation
The original dataset contains ~7.7M rows.  
To work efficiently, I used a **1M sampled subset** and loaded only the columns available at t0.

To avoid temporal leakage:
- Post-event fields (e.g., `End_Time`, `Distance(mi)`) were removed.
- Weather data was filtered to include only `Weather_Timestamp ≤ Start_Time`.

Feature engineering included:
- hour, day of week, month, weekend flag  
- binned latitude/longitude to approximate spatial regions  
- boolean POI indicators (amenity, junction, traffic signal, etc.)

A **time‑based split** was used for validation:  
the **last 90 days** of data were held out as the test period.

---

## Models

### Baseline — Logistic Regression
Implemented within a scikit‑learn pipeline:

- median/mode imputation  
- `OneHotEncoder(min_frequency=100)` to reduce cardinality  
- `class_weight="balanced"` to counter class imbalance

**Performance on the test set:**
- **Average Precision (PR‑AUC): ~0.127**
- **F1-score: ~0.143**

Given the rarity of severity‑4 accidents, these values are aligned  
with expectations for rare-event modeling.  
This model is stored as the baseline:  
`artifacts/baseline_logreg_ohe.joblib`.

---

### LightGBM (Advanced Model)
A gradient‑boosted tree model was trained using the same preprocessed features.

**Performance on the test set:**
- **PR‑AUC: ~0.209**
- **F1-score: ~0.171**

LightGBM provides a clear improvement in both ranking quality and  
classification performance.

Additionally, probabilities were calibrated using **Isotonic Regression**,  
resulting in **well-aligned, trustworthy predicted probabilities**, as shown  
by calibration curves across several binning settings.

---

## What Influences the Predictions?

### • Permutation Importance (raw-level)
The most influential features are:

1. **Geographical context**  
   (`State`, `City`, `County`, and spatial bins)  
2. **Seasonality** (`month`)  
3. **Weather**  
   (temperature, visibility, precipitation)  
4. **Time-of-day**  
   (`hour`) — important but secondary compared to geography and seasonality

Infrastructure-related POI variables (traffic signals, junctions, roundabouts)  
showed **minimal importance**.

### • SHAP (post-OHE, local/global explainability)
SHAP confirms and refines these findings:

- Several **state-level categories** (`State_CA`, `State_MN`, etc.) strongly  
  influence predictions.
- Winter months, freezing temperatures, low visibility, and evening/night  
  hours tend to **increase predicted severity**.
- Some **rare geographic categories** (e.g., infrequent counties/cities)  
  produce large SHAP values due to their heterogeneous and highly variable risk.

Overall, the model relies on a combination of **geography**, **seasonality**,  
**weather**, and **temporal patterns** to estimate accident severity.

---

## Additional Analyses

### • Probability Calibration
Isotonic Regression produced **excellent calibration**, with predicted  
probabilities closely matching the true fraction of positives across  
different binning granularities.

### • Recall@Top‑k (operational metric)
Selecting only the **top 1% highest‑risk predictions** recovers:
- **Recall@Top‑278: ~10.3%**

This is meaningful for real-world prioritization where only a small  
fraction of incidents can be reviewed or flagged.

### • Error Analysis
The model performs unevenly across:

- **States**: some low-data states (NM, WY, WV…) show very high error rates,  
  highlighting **geographical dataset shift**.
- **Hours**: performance is worse during **night-time hours**,  
  better during morning/daytime.
- **Weather conditions**: rare and extreme weather scenarios  
  (freezing rain, sleet, blowing snow) yield the highest error rates due  
  to limited representation in the dataset.

These findings identify concrete opportunities to improve model robustness  
(e.g., geographic cross-validation, richer weather features, stratified sampling).

---

## Dataset Challenges

Key difficulties addressed during the project:

- **Temporal leakage** → removed post-event features, filtered weather timestamps  
- **Missing values** → simple imputations + LightGBM’s native NaN handling  
- **Severe class imbalance** → class weighting + PR‑AUC and Recall@k  
- **High cardinality categorical features** → grouped with  
  `min_frequency=100` and geographic binning  
- **Time-aware evaluation** → avoids overly optimistic estimates  

---

## Repository Structure
US-Car-Accidents/  
├── data/  
&ensp; &ensp; └── README.md  
├── 01_US_accidents_notebook.ipynb  
├── README.md  
└── requirements.txt  

---

## How to run

1. Create a virtual environment  
2. Install the required packages using:

   ```bash
   python -m pip install -r requirements.txt
   
3. Run the notebook in order

---
