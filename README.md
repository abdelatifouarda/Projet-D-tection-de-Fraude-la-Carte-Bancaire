# Projet-D-tection-de-Fraude-la-Carte-Bancaire
Fraud Detection System using Data Analysis and Machine Learning | Focus on Threshold Optimization &amp; Business Decision-Making


# 💳 Credit Card Fraud Detection

### Data Analysis & Business-Oriented Machine Learning System

---

## 🚀 Key Impact

* Detected **83% of fraudulent transactions**
* Maintained **91% precision** (low false alerts)
* Identified **high-risk hours (2–5 AM)**
* Built a **validation-based decision threshold (0.96)**

> This project focuses on **decision quality**, not just model performance.

---

## 📌 Overview

This project simulates a real-world fraud detection system using a highly imbalanced dataset (0.17% fraud).

The goal is to build a system that balances:

* Detecting fraud (recall)
* Reducing customer friction (precision)

---

## 📦 Dataset

* 284,807 transactions
* 30 features (PCA + Time + Amount)
* Fraud ratio: ~578:1

Source: Kaggle (ULB)

---

## 🔍 Key Insights (EDA)

* 🕐 Fraud peaks during **low-activity hours (2–5 AM)**
* 💰 Fraud transactions are mostly **low amounts (~$20)**
* 📊 Features like **V14, V17, V12** carry strong signals
* ⚖️ Fraud *rate* ≠ fraud *volume* → important business distinction

---

## ⚙️ Approach

### 1. Data Analysis

* Distribution analysis
* Temporal patterns (Hour extraction)
* Correlation analysis

### 2. Modeling

* Logistic Regression (baseline)
* XGBoost (final model)
* Random Forest & LightGBM tested but underperformed

### 3. Imbalance Handling

* Class weighting (no SMOTE to avoid leakage)

### 4. Threshold Optimization

* Tuned on validation set (not default 0.5)
* Optimal threshold: **0.96**

---

## 📊 Results

| Modèle                | PR-AUC    | ROC-AUC   | F1-Score  | Threshold
| --------------------- | --------- | --------- | --------- | ------------- |
| **XGBoost**           | **0,86** | **0,98** | **0,87** | **0,96**      |
| Régression Logistique | 0,75     | 0,97    | 0,78     | 0,99          |


---

## 💼 Business Interpretation

* Out of 100 flagged transactions → **91 are real fraud**
* System detects **83% of fraud cases**
* ~17% fraud remains undetected (trade-off decision)

✔️ Configured as a **precision-oriented system**
✔️ Can be adjusted depending on risk tolerance

---

## 🛠️ Tech Stack

* Python
* Pandas, NumPy
* Scikit-learn
* XGBoost
* Matplotlib, Seaborn

---

## 🧠 Key Takeaways

* PR-AUC is critical for imbalanced problems
* Threshold tuning is essential in production
* EDA drives modeling decisions
* Model ≠ system → decision layer matters

---

## 👨‍💻 Author

ABDELATIF  OUARDA
Data Analyst | Machine Learning

LinkedIn:  www.linkedin.com/in/abdelatif-ouarda-790a2b36a

