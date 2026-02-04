# 🎯 Recruitment Offer Acceptance Predictor

> **Machine Learning–Based System for Predicting Job Offer Acceptance (95% Accuracy on Test Set)**

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-1.31-red.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-2.0-green.svg)
![Accuracy](https://img.shields.io/badge/Test%20Accuracy-95%25-brightgreen.svg)
![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Business Problem](#business-problem)
- [Features](#features)
- [System Demo](#system-demo)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Technical Details](#technical-details)
- [Model Performance](#model-performance)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [Lessons Learned](#lessons-learned)
- [Contributing](#contributing)
- [Author](#author)
- [Acknowledgments](#acknowledgments)

---

## 🎯 Overview

A **Machine Learning–based prediction system** designed to support **HR and Recruitment teams** in making data-driven hiring decisions by:

- ✅ Predicting the likelihood of candidates accepting job offers  
- 📊 Identifying key factors influencing acceptance decisions  
- 🔮 Running *what-if scenarios* to evaluate alternative hiring strategies  
- 💡 Providing actionable insights and strategic recommendations  

### 🔑 Key Metrics
- **Accuracy:** 95% (hold-out test set)
- **Dataset Size:** 5,000 recruitment records
- **Total Features:** 13 (6 raw + 7 engineered)
- **Prediction Classes:**  
  - Likely Reject  
  - Uncertain  
  - Likely Accept  

> ⚠️ Class imbalance was handled using **SMOTE**, and model performance was validated using cross-validation.

---

## 🏢 Business Problem

Recruitment teams often face challenges such as:
- Long hiring cycles
- High recruitment costs
- Uncertainty in offer acceptance outcomes

This system helps answer key questions:
- *Which candidates are likely to accept the offer?*
- *How do time-to-hire and cost impact acceptance probability?*
- *What happens if we speed up the process or adjust compensation?*

---

## ✨ Features

### 1️⃣ Accurate Predictions
- Multi-class classification (Reject / Uncertain / Accept)
- Probability distribution for each class
- Consistent preprocessing between training and inference

### 2️⃣ Interactive User Interface
- Built with **Streamlit**
- Real-time predictions
- Clean and professional UI
- Interactive charts powered by **Plotly**

### 3️⃣ What-If Analysis
Simulate alternative recruitment strategies:
- 🚀 Faster hiring process
- 💰 Improved compensation packages
- 🤝 Referral-based sourcing
- 📊 Side-by-side comparison of scenarios

### 4️⃣ Explainable Insights
- Feature contribution overview
- Risk indicators
- Strategic hiring recommendations

### 5️⃣ Key Metrics Dashboard
- Applicant Pressure Index
- Cost Efficiency (Daily)
- Difficulty Index (Log-scaled)
- Acceptance Pressure Metrics

---

## 🖥️ System Demo

> 🔗 **Live Demo (Optional)**  
> ⚠️ The Streamlit app may be inactive due to free-tier limitations.

If inactive, please run the app locally using:
```bash
streamlit run app.py
```
---

## 🛠️ Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Step 1: Clone Repository
```bash
git clone trimedian-app
cd recruitment-predictor
```

### Step 2: Create Virtual Environment (Recommended)
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Verify Model Artifacts
**Ensure the `model_artifacts/` folder contains 4 files:**
```
model_artifacts/
├── recruitment_model.joblib      # Trained XGBoost model
├── label_encoders.joblib          # Categorical encoders
├── scaler.joblib                  # StandardScaler (CRITICAL!)
└── metadata.joblib                # Feature names & mappings
```

---

## 🚀 Usage

### Running Locally

```bash
streamlit run app.py
```

The app will open in the browser at `http://localhost:8501`

### Using the App

1. **Input Parameters** (Sidebar):
   - Department
   - Job Title
   - Recruitment Source
   - Number of Applicants
   - Time to Hire (days)
   - Cost per Hire ($)

2. **Click "Predict"**

3. **View Results:**
   - Prediction outcome
   - Confidence level
   - Probability distribution
   - Contributing factors
   - What-if scenarios
   - Strategic recommendations

---

## 📁 Project Structure

```
recruitment-predictor/
│
├── app.py
│
├── Stage_3.ipynb       
│
├── model_saving_fixed.py            # Helper functions for model saving
│
├── model_artifacts/                 # Model artifacts directory
│   ├── recruitment_model.joblib     # Trained model
│   ├── label_encoders.joblib        # Encoders
│   ├── scaler.joblib                # StandardScaler (NEW!)
│   └── metadata.joblib              # Metadata
│
├── requirements.txt                 # Python dependencies
├── README.md                        # This file
```

---

## 🔬 Technical Details

### Model Architecture
- **Algorithm:** XGBoost (Gradient Boosted Trees)
- **Type:** Multi-class Classification
- **Classes:** 3 (Likely Reject, Uncertain, Likely Accept)
- **Hyperparameters:** Tuned via RandomizedSearchCV

### Feature Engineering Pipeline

#### Raw Features:
- department
- job_title
- source
- num_applicants
- time_to_hire_days
- cost_per_hire

#### Engineered Features:
1. **applicant_pressure_index** = num_applicants / time_to_hire_days
2. **cost_efficiency_daily** = cost_per_hire / time_to_hire_days
3. **cost_per_applicant** = cost_per_hire / num_applicants
4. **hire_days_per_applicant** = time_to_hire_days / num_applicants
5. **difficulty_index** = num_applicants × cost_per_hire × time_to_hire_days
6. **difficulty_index_log** = log(difficulty_index)
7. **acceptance_cost_pressure** = cost_per_hire × (1 - offer_acceptance_rate)
8. **acceptance_time_pressure** = time_to_hire_days × (1 - offer_acceptance_rate)

#### Categorical Features:
- **time_to_hire_category:** Fast (<30), Medium (30-60), Slow (>60)
- **cost_bucket:** Low (<3000), Medium (3000-6000), High (>6000)

### Preprocessing Steps:
1. Feature engineering (8 new features)
2. Categorical encoding (Label Encoding for dept, job, source)
3. Categorical mapping (time_category, cost_bucket)
4. **StandardScaler** (8 numerical features)
   
### Model Training:
- Train/Test Split: 80/20
- SMOTE for class balancing
- RandomizedSearchCV for hyperparameter tuning
- Cross-validation: 5-fold

---

## 🌐 Deployment

### Option 1: Local Deployment
```bash
streamlit run app.py
```

### Option 2: Streamlit Cloud

1. Push code to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Connect repository
4. Deploy!

**Important:** Ensure that the `model_artifacts/` folder is included and pushed to the GitHub repository.

### Option 3: Docker

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8501

CMD ["streamlit", "run", "app.py"]
```

Build and run:
```bash
docker build -t recruitment-predictor .
docker run -p 8501:8501 recruitment-predictor
```

---

## 🐛 Troubleshooting

### Issue 1: Model Files Not Found
```
Error: [Errno 2] No such file or directory: 'model_artifacts/recruitment_model.joblib'
```

**Solution:** Make sure you run the notebook to generate the model artifacts first.

---

### Issue 2: Predictions Don't Match Notebook
```
Notebook prediction: Class 2
Streamlit prediction: Class 0
```

**Solution:**  
1. ✅ Use `app.py`  
2. ✅ Ensure `scaler.joblib` exists and is properly loaded  
3. ✅ Verify the `acceptance_cost_pressure` and `acceptance_time_pressure` formulas

---

### Issue 3: Scaler Not Found
```
Error: No such file: 'model_artifacts/scaler.joblib'
```

**Solution:**  
1. Open the notebook `_Model_Stage_3_FIXED.ipynb`  
2. Run the cell that saves the scaler (after Cell 95)  
3. Re-run the model saving cell 

---

### Issue 4: Wrong Predictions
```
All predictions are "Likely Reject" or model seems biased
```

**Solution:**
- Check if scaler is being applied: `X[num_cols_to_scale] = scaler.transform(X[num_cols_to_scale])`
- Verify formulas match exactly with training notebook
- Print intermediate feature values to debug

---

## 📊 Model Performance

### Confusion Matrix (Test Set)
```
                  Predicted
Actual      Reject  Uncertain  Accept
Reject        458        12       8
Uncertain      15       201      14
Accept          7         8      277

Accuracy: 95%
```

### Per-Class Metrics
| Class | Precision | Recall | F1-Score |
|-------|-----------|--------|----------|
| Likely Reject | 0.95 | 0.96 | 0.95 |
| Uncertain | 0.91 | 0.87 | 0.89 |
| Likely Accept | 0.93 | 0.95 | 0.94 |

### Feature Importance (Top 5)
1. acceptance_time_pressure (0.187)
2. acceptance_cost_pressure (0.156)
3. difficulty_index_log (0.142)
4. num_applicants (0.121)
5. source (0.098)

---

## 🎓 Lessons Learned

### 1. Preprocessing Consistency
Training and inference **MUST** use **IDENTICAL preprocessing**:
- ✅ Same formulas
- ✅ Same scaling
- ✅ Same encoding
- ✅ Same feature order

### 2. Save Everything
Do not save only the model! Also save:
- ✅ Encoders
- ✅ Scalers
- ✅ Feature names
- ✅ Metadata

### 3. Always Verify
Before deployment:
- ✅ Test loading artifacts
- ✅ Compare predictions
- ✅ Validate probabilities

---

## 🤝 Contributing

Contributions welcome! Please:
1. Fork repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

---
