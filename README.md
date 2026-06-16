# 🌍 Tanzania Tourism Expenditure Prediction
### Zindi Challenge | Ngao Labs Data Science Programme
 
![Python](https://img.shields.io/badge/Python-3.10-blue)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-orange)
![RandomForest](https://img.shields.io/badge/Model-RandomForest-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
 
 
## 📌 About This Project
 
This project was completed as part of the **Ngao Labs Data Science Programme — Module 3.**
 
The goal was to build a machine learning model that predicts the total amount of money a tourist will spend when visiting Tanzania, based on information such as their country of origin, age group, purpose of visit, number of nights stayed, and travel arrangements.
 
The dataset was provided by the **National Bureau of Statistics (NBS) of Tanzania** and contained 6,476 rows of real tourist survey data.
 
 
## 👩‍💻 Author
 
**Faith Njoroge**
Ngao Labs Data Science Programme
June 2026
 
 
## 🎯 Challenge Details
 
| | |
|---|---|
| **Platform** | [Zindi Africa](https://zindi.africa) |
| **Challenge** | Tanzania Tourism Expenditure Prediction |
| **Type** | Regression — predicting a number (spending amount) |
| **Metric** | RMSE (Root Mean Squared Error) — lower is better |
| **Duration** | 2 days |
 
 
## 📊 My Results
 
| Submission | Score (RMSE) | Improvement |
|---|---|---|
| First submission | 5,193,136 TZS | Baseline |
| Added target encoding | 5,131,407 TZS | ✅ Better |
| Added model blending | 5,107,144 TZS | ✅ Better |
| Final optimised model | Submitted | ✅ Best |
 
 
## 🗂️ Dataset
 
The dataset contains information about tourists visiting Tanzania including:
 
| Column | Description |
|---|---|
| `country` | Country the tourist came from |
| `age_group` | Age group of the tourist |
| `travel_with` | Who they travelled with |
| `purpose` | Reason for visiting Tanzania |
| `main_activity` | Main activity during visit |
| `night_mainland` | Nights spent on mainland Tanzania |
| `night_zanzibar` | Nights spent in Zanzibar |
| `tour_arrangement` | Package Tour or Independent |
| `total_cost` | **Target variable** — total spending in TZS |
 
 
## ⚙️ My Approach
 
### 1. Data Cleaning
- Filled missing values in `travel_with` with `'Unknown'`
- Filled missing values in `most_impressing` with `'None'`
- Filled missing numeric values (`total_female`, `total_male`) with the median
### 2. Target Encoding
Instead of using One Hot Encoding for all categorical columns, I replaced high-cardinality columns with the **average spending of each category.** This gives the model real spending information instead of meaningless 0s and 1s.
 
Columns target encoded:
- `country` → average spending per country
- `purpose` → average spending per purpose
- `age_group` → average spending per age group
- `main_activity` → average spending per activity
- `info_source` → average spending per information source
### 3. Feature Engineering
Created new columns to give the model more useful information:
 
| New Feature | How it was created |
|---|---|
| `total_nights` | night_mainland + night_zanzibar |
| `total_people` | total_female + total_male |
| `is_solo` | 1 if travelling alone, 0 if not |
| `is_first_timer` | 1 if first visit to Tanzania, 0 if not |
| `package_count` | Number of package services included |
| `cost_per_night` | total_cost / total_nights |
 
### 4. Encoding
- **Label Encoding** for Yes/No columns (package services, tour arrangement)
- **One Hot Encoding** for `travel_with` and `payment_mode`
### 5. Scaling
Applied `StandardScaler` to numeric columns so no single column dominates the model unfairly.
 
### 6. Log Transformation
Tourist spending ranged from **49,000 to 99,000,000 TZS** — a huge range. I applied `np.log1p()` to compress this range before training and `np.expm1()` to convert predictions back to real TZS values.
 
### 7. Cross Validation
Used **5-fold cross validation** to get a more reliable estimate of model performance instead of relying on a single 80/20 split.
 
### 8. Model Training & Blending
Trained two models on the full dataset:
- **XGBoost** — learns from its own mistakes tree by tree
- **Random Forest** — 300 trees working independently then voting
Combined them using **weighted blending:**
```
Final prediction = (60% × XGBoost) + (40% × Random Forest)
```
 
 
## 📁 Files in This Repository
 
```
├── Tanzania_Tourism_Final.ipynb  ← Main notebook with all code
├── submission.csv                ← Final predictions submitted to Zindi
└── README.md                     ← This file
```
 
 
## 🧠 What I Learned
 
This was my second data science projects and I learned a lot:
 
- **How to work with real datasets** — understanding what each column means and how to clean the data before using it
- **The difference between XGBoost and Random Forest** — XGBoost learns from its mistakes one tree at a time while Random Forest uses many independent trees that vote together
- **How to improve model performance** — through better feature engineering, target encoding and blending multiple models
- **The importance of cell order in a notebook** — knowing which steps must come before others and why
The hardest part was understanding how each cell connects to the next and knowing what code belongs in each step. But by the end I could build the full pipeline myself from scratch.
 
 
## 🚀 How to Run This Project
 
1. Open `Tanzania_Tourism_Final.ipynb` in **Google Colab**
2. Upload `Train.csv` and `Test.csv` from the Zindi challenge page
3. Run all cells from top to bottom
4. Download `submission.csv` and upload to Zindi
 
## 🛠️ Tools Used
 
| Tool | Purpose |
|---|---|
| Python | Programming language |
| Pandas | Data manipulation |
| NumPy | Numerical operations |
| Matplotlib | Data visualisation |
| Scikit-learn | Preprocessing and evaluation |
| XGBoost | Primary prediction model |
| Google Colab | Development environment |
 
 
*Ngao Labs Data Science Programme | June 2026*
 
