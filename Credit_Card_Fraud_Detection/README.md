# 💳 Credit Card Fraud Detection

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-red?logo=scikit-learn&logoColor=white)
![imbalanced-learn](https://img.shields.io/badge/imbalanced--learn-SMOTE-purple)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

A complete data science project focused on detecting fraudulent credit card transactions. It includes exploratory data analysis, anomaly detection with Isolation Forest, class imbalance handling (SMOTE and `class_weight`), and predictive modeling with Random Forest, evaluated using metrics specifically suited for highly imbalanced problems (ROC AUC, PR-AUC).

---

## 📌 Table of Contents

- [Context](#-context)
- [Objective](#-objective)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Key Results](#-key-results)
- [Tech Stack](#️-tech-stack)
- [Installation and Usage](#-installation-and-usage)
- [Project Structure](#-project-structure)
- [Author](#-author)
- [License](#-license)

---

## 📌 Context

Credit card fraud represents a constant threat to financial institutions and users. The main technical challenge here is not purely predictive but statistical: fraudulent transactions are extremely rare (in this dataset, just **0.17%** of the total), which calls for specific class imbalance techniques and evaluation metrics different from the usual ones (accuracy is not reliable in this scenario).

---

## 🎯 Objective

Build a model capable of identifying fraudulent credit card transactions, prioritizing a balance between **recall** (catching as many real frauds as possible) and **precision** (minimizing false alarms), evaluated through metrics that are robust to class imbalance.

---

## 📊 Dataset

The **`creditcard.csv`** dataset (source: [Kaggle — mlg-ulb/creditcardfraud](https://www.kaggle.com/mlg-ulb/creditcardfraud)) contains **284,807 transactions** made by European cardholders over two days in September 2013, with **31 columns**:

| Column | Description |
|---|---|
| `Class` | Target variable — 0 = legitimate transaction, 1 = fraud |
| `Time` | Seconds elapsed since the first transaction in the dataset |
| `Amount` | Transaction amount |
| `V1`...`V28` | Anonymized numerical components obtained via PCA (for confidentiality reasons, the original meaning of these variables is not available) |

**Target distribution:**

| Class | Count | Percentage |
|---|---|---|
| 0 — Not fraud | 284,315 | 99.83% |
| 1 — Fraud | 492 | 0.17% |

The dataset has no missing values.

---

## 🔧 Methodology

### 1. Loading and initial inspection
Dataset downloaded from Kaggle (`kagglehub`), followed by a review of dimensions, data types (all features are numerical), and a check for null values (none found).

### 2. Exploratory Data Analysis (EDA)
- **Descriptive statistics** for all variables (mean, median, standard deviation, percentiles).
- **Univariate outlier detection (IQR)**: applied to the 30 numerical variables to get a quick first reference on dispersion.
- **Multivariate outlier detection (Isolation Forest)**: prioritized over IQR because it evaluates variables jointly (not column by column), makes no assumptions about the underlying distribution, and avoids the false-positive inflation that comes from applying IQR separately to 30 columns. With `contamination=0.01`, it flags 1% of the sample as outliers.
  - Cross-tabulating the detected outliers against the real class showed that Isolation Forest captured **58.7%** of actual frauds without ever seeing the label during training, with a **~59x fraud enrichment** inside the outlier group compared to the baseline rate. This supports the hypothesis that fraud tends to manifest as anomalous behavior.
  - The `is_outlier_IF` variable (Isolation Forest output) is added as an **extra feature** for the supervised model.
- **Correlation with the target**: point-biserial correlation calculated for numerical variables against `Class`. The variables with the strongest association were `V17`, `V14`, `V12`, `is_outlier_IF`, `V10`, `V16`, `V3`, and `V7`.
- **Distribution visualization**: histograms per variable segmented by class, and scatter plots of the most correlated variables against the target, with trend lines and correlation coefficients.

### 3. Preprocessing for modeling
- **Train/test split**: 70% training / 30% test, with **stratification** to preserve the fraud ratio in both sets.
- **Scaling**: `RobustScaler`, chosen for its robustness to outliers (it uses the median and interquartile range instead of the mean and standard deviation).

### 4. Handling class imbalance
Two strategies were explored:
- **SMOTE (Synthetic Minority Over-sampling Technique)**: applied only to the already-scaled training set (never to the test set, to avoid data leakage), generating synthetic samples of the minority class until both classes were balanced (199,020 vs. 199,020).
- **`class_weight='balanced'`**: penalizes errors on the minority class more heavily during Random Forest training, without needing to generate synthetic samples.

### 5. Predictive modeling
- **Random Forest Classifier** (`n_estimators=100`, `max_depth=15`, `min_samples_split=10`, `min_samples_leaf=5`, `class_weight='balanced'`).
- Trained on the `RobustScaler`-scaled data, using all 31 available features (`V1`-`V28`, `Time`, `Amount`, `is_outlier_IF`).

### 6. Evaluation
Given the strong imbalance, **accuracy is not a reliable metric** (a model that always predicts "no fraud" would already score ~99.8%). The focus is placed on:
- **Precision / Recall** for the fraud class.
- **ROC AUC**.
- **PR-AUC (Precision-Recall AUC)**, considered the primary metric due to its sensitivity to false positives in scenarios with extreme positive-class rarity.
- **Confusion matrix**, with special attention to **false negatives** (undetected frauds), the costliest type of error in this domain.

### 7. Prediction function
A `predict_fraud()` function was implemented, which takes a new transaction, scales it using the same `scaler` fitted on the training data, and returns the fraud probability, the binary prediction, and a risk level (`LOW` / `MEDIUM` / `HIGH`) based on the probability threshold.

---

## 📈 Key Results

Final model: **Random Forest with `class_weight='balanced'`**, evaluated on the test set (85,443 transactions, 148 real frauds).

| Metric | Value |
|---|---|
| Accuracy | 99.9% |
| Precision (fraud) | 86.9% |
| Recall / Sensitivity (fraud) | 76.4% |
| F1-Score | 81.3% |
| ROC AUC | 0.952 |
| **PR-AUC** | **0.808** |

**Confusion matrix:**

| | Predicted: No Fraud | Predicted: Fraud |
|---|---|---|
| **Actual: No Fraud** | 85,278 (TN) | 17 (FP) |
| **Actual: Fraud** | 35 (FN) | 113 (TP) |

- The model correctly detects **76.4%** of real frauds, with **86.9%** precision on the transactions flagged as fraud.
- The **PR-AUC of 0.808** confirms a good balance between catching real fraud and limiting false alarms, making it the most representative metric given the dataset's extreme imbalance.
- The **35 undetected fraudulent transactions** (false negatives) represent the model's most critical area for improvement.
- The variables `V17`, `V14`, `V12`, `V10`, `V16`, `V3`, and `V7`, along with the `is_outlier_IF` anomaly signal, show the strongest association with fraud.

---

## 🛠️ Tech Stack

| Library | Use |
|---|---|
| `pandas` | Data manipulation |
| `numpy` | Numerical operations |
| `matplotlib` / `seaborn` | Visualization |
| `scikit-learn` | Preprocessing, Isolation Forest, Random Forest, metrics |
| `imbalanced-learn` | SMOTE for class balancing |
| `scipy` | Statistical tests (point-biserial, chi-squared) |
| `kagglehub` | Dataset download from Kaggle |
| `jupyter` | Interactive environment |

---

## 🚀 Installation and Usage

### Prerequisites
* All dependencies are listed in [requirements.txt](requirements.txt).
* A configured Kaggle account (for automatic download via `kagglehub`), or a manual download of the [creditcard.csv](https://www.kaggle.com/mlg-ulb/creditcardfraud) dataset.

### Setup steps

```bash
# 1. Clone the repository
git clone https://github.com/christianirshool-glitch/My-projects.git
cd My-projects

# 2. Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate       # On Linux/macOS
venv\Scripts\activate          # On Windows

# 3. Install the dependencies
pip install -r requirements.txt

# 4. Launch the interactive environment
jupyter notebook "Project_1_Credit_Fraud_Detection.ipynb"
```

> 📌 The notebook downloads the dataset automatically via `kagglehub.dataset_download("mlg-ulb/creditcardfraud")`. If you don't have Kaggle credentials configured, download `creditcard.csv` manually and adjust the loading path in the notebook.

---

## 📁 Project Structure

```
credit-fraud-detection/
├── Project_1_Credit_Fraud_Detection.ipynb   # Main notebook with the full pipeline
├── requirements.txt                          # Project dependencies
├── LICENSE                                    # MIT license
└── README.md                                  # Project documentation
```

---

## 👤 Author

**Christian Méndez Giraldo**
Data Scientist · MSc in Data Science
[GitHub](https://github.com/christianirshool-glitch)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
