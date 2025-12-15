# Fraud-Analytics-UAE-Mastercard
A cost-sensitive fraud detection system tailored for the UAE market using XGBoost and Anomaly Detection.

# Fraud Analytics for the UAE Market 🛡️

### 📋 Project Overview
This project presents a Proof of Concept (PoC) for a fraud detection system specifically tailored for the **UAE market**.
Starting from a publicly available dataset, I simulated a realistic business scenario for **Mastercard Middle East** by engineering domain-specific features.

The core philosophy of this project is **"Cost-Sensitive Learning"**: instead of simply maximizing accuracy, the model is optimized to minimize the actual **financial loss** caused by fraud, weighing the cost of missed frauds against the operational cost of false alarms.

### 🚀 Key Results
* **Recall:** **93%** (Overall fraud detection rate)
* **Financial Impact:** Estimated prevention of **~1.2 Million AED** in yearly fraud losses based on a simulation of 12M transactions.
* **Architecture:** Two-layer hybrid pipeline (Supervised + Unsupervised).

---

### 🧠 Methodology & Architecture

The detection pipeline consists of two distinct layers to maximize recall while managing costs.

#### 1. Data Adaptation & Feature Engineering
To anchor the analysis to the UAE context, I injected synthetic domain-specific features into the original dataset:
* **Transaction Types:** B2C vs B2B (higher risk).
* **Geolocation:** Mapped synthetic coordinates to real Emirati zones (e.g., Dubai Marina, Sharjah).
* **Customer Profiles:** Separation between EXPAT and LOCAL cardholders.
* **Security Flags:** Tokenization status and 3-D Secure usage.
* **Cost Function:** Defined `cost_if_fraud = 4.19 * Amount` based on regional "True Cost of Fraud" studies.

#### 2. Layer 1: Cost-Sensitive XGBoost 🤖
* **Model:** XGBoost Classifier trained on a stratified 75/25 split.
* **Handling Imbalance:** Applied **SMOTE** to lift the minority class to 5% for better model learning.
* **Optimization Strategy:** Instead of maximizing AUC, the decision threshold (0.465) was tuned to minimize economic loss, valuing a False Negative at ~4,190 AED and a False Positive at 50 AED.

#### 3. Layer 2: Anomaly Detection Safety Net 🔍
* **Objective:** Catch borderline cases that XGBoost judged as "Legitimate".
* **Models:** Applied **Isolation Forest** and **Local Outlier Factor (LOF)** on transactions below 50k AED.
* **Result:** This secondary screen recovered an additional **117 frauds**, pushing the overall recall to 93%.

---

### 🛠️ Technologies Used
* **Language:** Python 3.10+
* **Data Manipulation:** Pandas, NumPy
* **Machine Learning:** XGBoost, Scikit-Learn (IsolationForest, LocalOutlierFactor), Imbalanced-learn (SMOTE)
* **Visualization:** Matplotlib, Seaborn

---

### ⚠️ Data Disclaimer

This project uses a simulation based on the publicly available "Credit-Card Fraud Detection" dataset from Kaggle. Domain-specific features (Geo-location, Customer Type, etc.) were synthetically generated to demonstrate business logic and adaptation capabilities for the UAE market. In a production environment, the model would require retraining on real transaction streams to account for actual concept drift.

--- 

🔮 Future Work

    Integration of Time-series velocity features.

    Inclusion of Device Fingerprinting data.

    Testing LightGBM or Graph-based models to improve Precision (currently ~6%).

    SHAP analysis for Responsible AI and explainability.

    Potential deployment within Brighterion AI Studio.

Author: Lorenzo D'Amico

### 📂 Repository Structure
```text
.
├── data/                   # Contains dataset (or link to source)
├── notebooks/              # Jupyter Notebooks for EDA and Modeling
├── src/                    # (Optional) Source code for preprocessing scripts
├── images/                 # Charts and plots generated during analysis
├── requirements.txt        # Python dependencies
└── README.md               # Project documentation

