# 🔐 UNSW-NB15: Binary and Multiclass Intrusion Detection using Traditional ML

This repository presents a machine learning pipeline for **Network Intrusion Detection** using the **UNSW-NB15** dataset. The goal is to classify network traffic as either **normal** or an **attack** using traditional machine learning models.

---

## 📂 Dataset Description

The **UNSW-NB15** dataset was developed by the Australian Centre for Cyber Security (ACCS). It simulates realistic network traffic including modern attack scenarios. The dataset is composed of:

- 4 CSV files: `UNSW-NB15_1.csv` to `UNSW-NB15_4.csv`
- `UNSW-NB15_features.csv`: Describes all 49 features
- `UNSW-NB15_LIST_EVENTS.csv`: Describes attack types

Each record consists of 49 features representing flow-level network activity.

---

### 🧬 Feature Summary

| Feature Type | Count | Examples |
|--------------|--------|----------|
| Nominal      | 7      | `proto`, `state`, `service`, `srcip`, `dstip`, `attack_cat`, `Label` |
| Integer      | 30+    | `sport`, `dsport`, `sbytes`, `dbytes`, `Spkts`, `Dpkts` |
| Float        | 10+    | `dur`, `Sload`, `Dload`, `tcprtt`, `synack`, `ackdat` |
| Binary       | 3      | `Label`, `is_ftp_login`, `is_sm_ips_ports` |
| Timestamp    | 2      | `Stime`, `Ltime` |

---

### 🏷️ Labels

- `Label`: Binary classification
  - `0` = Normal
  - `1` = Attack

- `attack_cat`: Multiclass classification with 9 attack categories:
  - `Analysis`, `Backdoor`, `DoS`, `Exploits`, `Fuzzers`, `Generic`, `Reconnaissance`, `Worms`, `Normal`

Two label versions were created:
- `bin`: Binary label (0 = Normal, 1 = Attack)
- `multi`: Multiclass label (attack categories)

---

## ⚙️ Pipeline Overview

### 1. Data Loading & Merging  
All four dataset parts are combined into a single DataFrame.

### 2. Preprocessing
- Dropped null and redundant columns
- Applied **correlation analysis** to remove highly correlated features
- Label encoding of categorical features
- Normalized features using `MinMaxScaler`

### 3. Feature Selection  
Used **Correlation-based Feature Selection**:
- Removed 9 highly correlated features
- Retained most informative and uncorrelated columns

### 4. Classification Tasks
- **Binary Classification:** Predict if a record is `attack` or `normal`
- **Multiclass Classification:** Predict the specific type of attack from 9 categories

### 5. Model Training  
Trained on both tasks using:
- Logistic Regression
- Decision Tree
- Naive Bayes
- LightGBM
- KNN
- Random Forest

Each model was trained using **50%, 60%, 70% training data splits** and exported as `.pkl` files for reuse.

---

## 📊Classification Tasks
Binary Classification: Predict if a record is attack or normal

Multiclass Classification Results

| Class          
|---------------- 
| Analysis       
| Backdoor       
| DoS             
| Exploits        
| Fuzzers         
| Generic         
| Normal         
| Reconnaissance  
| Worms           

### 📊 Model Performance Comparison

| Rank | Model               | Accuracy (%) | Macro F1-Score |
|------|---------------------|---------------|----------------|
| 1    | Decision Tree       | 92.45         | 0.9242         |
| 2    | LightGBM            | 73.18         | 0.7205         |
| 3    | CatBoost            | 63.13         | 0.6154         |
| 4    | Logistic Regression | 48.41         | 0.4465         |
| 5    | Naive Bayes         | 41.38         | 0.3141         |


> 🔍 Multiclass classification is more challenging due to class imbalance and overlapping behavior among attacks.

---

## 🚀 How to Run

```bash
git clone https://github.com/yourusername/unsw-nb15-ids.git
cd unsw-nb15-ids

