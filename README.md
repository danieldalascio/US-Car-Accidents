# US-Car-Accidents — ML Analysis and Modeling

This project explores the *US Car Accidents* dataset using a mix of data analysis, feature engineering and machine learning.  
The goal is to predict whether an accident reaches the highest severity level (Severity = 4) using only the information available at the moment the accident happens.

The work was done as preparation for a thesis assignment.

---

## Project Overview

### 1. Data preparation
The original dataset is quite large (7.7M rows), so I selected only the columns that are actually known at time *t0* (when the accident begins).  
I also synchronized the weather table to avoid temporal leakage, keeping only weather observations with timestamps ≤ Start_Time.

A few simple features were added:
- hour of day, day of week  
- binned latitude/longitude  
- boolean flags extracted from some categorical fields  

The final train/test split was **time‑based**: the most recent 90 days were held out for testing.

---

## Models

### Baseline: Logistic Regression  
Implemented using a scikit‑learn pipeline with:
- imputation  
- `OneHotEncoder(min_frequency=100)` for high-cardinality categorical features  
- `class_weight="balanced"`  

On the held‑out test period, the model reaches:
- **Average Precision ≈ 0.09**
- **F1 ≈ 0.20**

Given the rarity of severe accidents, both precision and recall remain low.  
This logistic model is saved as the baseline:  
`artifacts/baseline_logreg_ohe.joblib`.

### LightGBM  
A second model (LightGBM) was tested afterwards.  
It provides slightly better metrics while using the same features and preprocessing.

---

## What influences the predictions?

To understand which factors the models rely on, I used:

### • Permutation Importance  
Showed that the most relevant factors are:
- hour of day  
- weather conditions (visibility, precipitation)  
- geographic location (lat/lng bins)

Infrastructure-related variables (signals, roundabouts, etc.) had very low importance.

### • SHAP (TreeExplainer)  
Confirmed the ranking and provided directionality:  
lower visibility, bad weather and peak hours tend to increase the predicted probability of a severe accident.

These results make sense physically and match known traffic patterns.

---

## Dataset Challenges

A few issues required attention:

- **Temporal leakage** (columns like `End_Time` or future weather) → removed or filtered  
- **High number of missing values** in weather features → simple imputations + LightGBM’s handling of NaNs  
- **Severe class imbalance** → class weights and PR‑AUC as main metric  
- **Very high cardinality** in city/ZIP/weather → `min_frequency` encoding + coordinate binning  
- **Time‑aware validation** → avoids overly optimistic estimates

---

## Repository Structure
US-Car-Accidents/  
├── data/  
&ensp; &ensp; └── README.md  
├── accidents_us_notebook.ipynb  
├── README.md  
└── requirements.txt  

---

## How to run

1. Create a virtual environment  
2. Install the required packages using:

   ```bash
   python -m pip install -r requirements.txt
   
3. Run the notebooks in order

---
