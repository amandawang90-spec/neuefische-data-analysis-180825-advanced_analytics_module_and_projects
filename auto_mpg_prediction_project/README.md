# 🚗 Car Fuel Efficiency Prediction (MPG)

A machine learning project that predicts car fuel efficiency (miles per gallon) using regression models. Four models are built, evaluated, and compared: **Lasso Regression**, **Ridge Regression**, **Principal Component Regression (PCR)**, and **Random Forest Regression**.

---

## 📋 Project Overview

| | |
|---|---|
| **Target Variable** | `mpg` — miles per gallon (continuous) |
| **Task** | Supervised Regression |
| **Dataset** | Auto MPG Dataset (392 records) |
| **Models** | Lasso CV, Ridge CV, PCA + Regression, Random Forest |
| **Notebook** | [auto_mpg_prediction.ipynb](https://github.com/amandawang90-spec/neuefische-data-analysis-180825-advanced_analytics_module_and_projects/blob/main/auto_mpg_prediction_project/auto_mpg_prediction.ipynb) |

---

## 📁 Project Structure

```
auto_mpg_model_comparison/
│
├── cars_multivariate,csv.           # Auto MPG dataset
│                        
├── auto_mpg_prediction.ipynb        # Main notebook
│
└── README.md                        # Project documentation
```

---

## 📊 Dataset

The dataset contains information about cars from the 1970s and 1980s.

| Feature | Type | Description |
|---|---|---|
| `mpg` | Numerical (target) | Miles per gallon |
| `weight` | Numerical | Car weight (lbs) |
| `horsepower` | Numerical | Engine horsepower |
| `displacement` | Numerical | Engine displacement (cc) |
| `acceleration` | Numerical | Acceleration |
| `model` | Numerical | Model year (e.g. 70 = 1970) |
| `cylinders` | Categorical | Number of cylinders (4, 6, 8) |
| `origin` | Categorical | 1 = American, 2 = European, 3 = Japanese |

---

## 🔄 Workflow

### Phase 0: Data Cleaning (Already Done)
- Handle missing values, duplicates, and data types

---

### Phase 1: Exploratory Data Analysis (EDA)

1. **Explore Target Variable (mpg)**
   - Mean = 23.45, Median = 22.75, Skewness = 0.46
   - Mild positive skew → no log transform needed on target

2. **Explore Feature Distributions**
   - Histograms and skewness check for all features
   - Outlier detection using IQR method
   - horsepower (skew=1.09) → log transform required
   - weight (skew=0.52) and displacement (skew=0.70) → borderline

3. **Check Relationships with Target**
   - Scatter plots and Pearson correlation
   - weight (-0.83), displacement (-0.81), horsepower (-0.78) → strong linear
   - All features show significant relationship with mpg

4. **Check Multicollinearity Between Features**
   - Correlation heatmap revealed severe multicollinearity
   - cylinders ↔ displacement: 0.95, weight ↔ displacement: 0.93
   - VIF analysis confirmed severe multicollinearity across engine features
   - Handled by Lasso (zeroing), Ridge (shrinking), PCA (compression)

5. **Linear Model Assumption Check (Part 1)**
   - Assumption 1: Linearity ✅ confirmed by scatter plots
   - Assumption 2: Multicollinearity ⚠️ severe but handled by models
   - Assumptions 3 & 4 require model residuals → checked in Phase 2

---

### Phase 2: Feature Engineering, Preprocessing & Model Building
*(Repeated for each model: Lasso, Ridge, PCA, Random Forest)*

#### 2a. Feature Engineering & Preprocessing
- **Identify feature types** — numerical vs categorical
- **Log transformation** applied to `horsepower` only (skew=1.09)
  - weight and displacement not transformed (skew < 1)
- **StandardScaler** applied to all numerical features
- **One-Hot Encoding** applied to categorical features (origin, cylinders)
- **Train/Test Split** — 80% train, 20% test, random_state=42
- **Pipeline** — preprocessor and model combined into sklearn Pipeline

#### 2b. Model Building
- **Hyperparameter Tuning**
  - Lasso → LassoCV auto finds best alpha via 5-fold CV
    → alpha search visualised: alpha vs mean CV R²
  - Ridge → RidgeCV auto finds best alpha via CV
    → alpha search visualised: alpha vs mean CV R²
  - Random Forest → GridSearchCV for max_depth,
    n_estimators, min_samples_split, min_samples_leaf
- **Fit Pipeline on X_train**
  - Cross Validation verifies performance on training data
  - Best hyperparameters selected automatically
  - Final model trained on ALL X_train

#### 2c. Model Evaluation
- **Evaluate on X_test**
  - Calculate Train R², Test R², Gap, MAE, MSE, RMSE
  - Gap between Train R² and Test R² indicates overfitting
- **Linear Model Assumption Check (Part 2)**
  - Applies to: Lasso, Ridge, PCA only
  - Assumption 3: Normality of residuals
    → Histogram + Q-Q plot + Shapiro-Wilk test
    → p > 0.05 → ✅ normally distributed
  - Assumption 4: Homoscedasticity
    → Residual plot + Levene test
    → p > 0.05 → ✅ constant variance confirmed
  - ✅ All assumptions confirmed for Lasso, Ridge and PCA
- **Residual Plot**
  - Train and test residuals vs fitted values
  - ✅ Random scatter → assumptions met
  - ⚠️ Curve → non-linearity present
  - ⚠️ Funnel → heteroscedasticity present
- **Model Interpretation**
  - Lasso/Ridge → coefficients + equation
  - Random Forest → feature importance scores
  - PCA → component loadings + variance explained
- **Predicted vs Actual**
  - Scatter plot of predicted vs actual on test set

---

### Phase 3: Model Comparison
- All models compared side by side: Train R², Test R², Gap, MAE, MSE, RMSE
- Models ranked by predictive performance and stability
- Final model selected

---

### Phase 4: Final Prediction
New car specifications used for prediction:

```python
new_car = pd.DataFrame({
    'weight':         [3000],          # raw value — not logged
    'log_horsepower': [np.log(100)],   # logged — pipeline expects this
    'displacement':   [200],           # raw value — not logged
    'acceleration':   [15.0],
    'model':          [80],            # model year 1980
    'origin':         [3],             # Japanese
    'cylinders':      [4]
})
```

| Specification | Value |
|---|---|
| Weight | 3000 lbs |
| Horsepower | 100 hp |
| Displacement | 200 cc |
| Acceleration | 15.0 |
| Model Year | 1980 |
| Origin | Japan (3) |
| Cylinders | 4 |

---

## 🤖 Models

### 1. Lasso Regression (L1 Regularization)
$$\text{Loss} = \sum(y_i - \hat{y}_i)^2 + \alpha \sum|\beta_j|$$

- Auto-selects best alpha via 5-fold cross-validation (LassoCV)
- Best alpha = **0.0133**
- Features kept: **10**, Features removed: **1**
- Removed feature: `cylinders_6` (coefficient = 0)

**Lasso Coefficients:**

| Feature | Coefficient | Interpretation |
|---|---|---|
| `log_horsepower` | -3.4477 | Strongest effect — more power = much less mpg |
| `weight` | -2.9183 | Heavier car = less mpg |
| `cylinders_4` | +2.8874 | 4 cylinders most fuel efficient |
| `origin_3 (Japan)` | +2.6427 | Japanese cars most fuel efficient |
| `model year` | +2.5696 | Newer cars = more fuel efficient |
| `origin_2 (Europe)` | +1.6799 | European cars more efficient than American |
| `cylinders_8` | +1.5580 | Positive vs baseline |
| `cylinders_5` | +1.2782 | Rare category — positive effect |
| `displacement` | +1.1796 | Mild positive after controlling other features |
| `acceleration` | -0.8025 | Mild negative effect |
| `cylinders_6` | 0.0000 | ❌ Removed by Lasso — not useful |

---

### 2. Ridge Regression (L2 Regularization)
$$\text{Loss} = \sum(y_i - \hat{y}_i)^2 + \alpha \sum\beta_j^2$$

- Auto-selects best alpha via cross-validation (RidgeCV)
- Best alpha = **5.8570**
- Shrinks all coefficients but never removes them
- Handles multicollinearity well

---

### 3. Principal Component Regression (PCR)
$$X \xrightarrow{\text{PCA}} \text{Components} \xrightarrow{\text{Linear Regression}} \hat{y}$$

- PCA reduces features to **5 components** (96% variance retained)

**Component Breakdown:**

| Component | Variance | Cumulative | Main Theme | Effect on MPG |
|---|---|---|---|---|
| PC1 | 62.52% | 62.52% | Engine size & power (weight, horsepower, displacement) | ❌ Less mpg |
| PC2 | 14.57% | 77.10% | Model year effect | ✅ More mpg |
| PC3 | 12.23% | 89.32% | Acceleration profile | ❌ Less mpg |
| PC4 | 3.83% | 93.15% | 6-cylinder distinction | ❌ Weak negative |
| PC5 | 2.86% | 96.01% | Geographic origin (Japan vs Europe) | ✅ More mpg |

**PCA Equation:**
```
MPG = 23.5994
      - 3.6162 × PC1
      + 0.1213 × PC2
      - 2.6683 × PC3
      - 0.4061 × PC4
      + 0.2157 × PC5
```

**Component Composition:**
- **PC1** — dominated by weight (+0.47), log_horsepower (+0.50), displacement (+0.49)
  → large, powerful, old American cars → less mpg
- **PC2** — dominated by model year (+0.86)
  → newer cars → more mpg
- **PC3** — dominated by acceleration (+0.81)
  → fast but older cars → less mpg
- **PC4** — dominated by cylinders_6 (+0.77)
  → 6-cylinder distinction → weak negative effect
- **PC5** — dominated by origin_3 Japan (+0.72) vs origin_2 Europe (-0.65)
  → Japanese vs European origin → more mpg

---

### 4. Random Forest Regression
$$\hat{y} = \frac{1}{T}\sum_{t=1}^{T} f_t(x)$$

- Ensemble of decision trees — captures non-linear relationships
- Tuned with GridSearchCV:
  - `max_depth=15`
  - `min_samples_split=5`
  - `min_samples_leaf=3`
  - `n_estimators=100`

---

## 📈 Results

| Rank | Model | Train R² | Test R² | Gap | MAE | RMSE | Verdict |
|---|---|---|---|---|---|---|---|
| 🥇 1st | Random Forest | 0.9513 | 0.8909 | 0.0605 | 1.7117 | 2.3601 | Best prediction |
| 🥈 2nd | Lasso CV | 0.8603 | 0.8317 | 0.0286 | 2.2976 | 2.9313 | Best linear, most stable |
| 🥉 3rd | Ridge CV | 0.8581 | 0.8301 | 0.0279 | 2.2869 | 2.9444 | Very close to Lasso |
| 4th | PCA + Regression | 0.8325 | 0.8068 | 0.0258 | 2.3654 | 3.1404 | Weakest |

---

## 🔍 Key Findings

### Linear Model Assumptions

| Assumption | Status | Test Used |
|---|---|---|
| Linearity | ✅ Confirmed | Pearson correlation |
| Multicollinearity | ⚠️ Severe — handled by models | VIF |
| Normality of Residuals | ✅ Confirmed | Shapiro-Wilk (p=0.51) |
| Homoscedasticity | ✅ Confirmed | Levene Test |

### Key Observations
- **Horsepower and weight** are the strongest predictors of fuel efficiency
- **Model year** confirms cars became more fuel efficient over time (70s → 80s)
- **Japanese and European cars** significantly more fuel efficient than American
- **Random Forest** outperforms linear models — confirms non-linear relationships
- **Lasso** correctly removed `cylinders_6` as a non-contributing feature
- **Log transform on horsepower only** — justified by EDA skewness and outlier analysis
- **PC1 dominates PCA** with 62.52% — engine characteristics are
  the strongest driver of fuel efficiency

---

## 🛠️ Requirements

```
python >= 3.8
pandas
numpy
scikit-learn
matplotlib
seaborn
scipy
statsmodels
```

Install dependencies:
```bash
pip install pandas numpy scikit-learn matplotlib seaborn scipy statsmodels
```

---

## 🚀 How to Run

1. Clone the repository:
```bash
git clone https://github.com/amandawang90-spec/neuefische-data-analysis-180825-advanced_analytics_module_and_projects.git
cd neuefische-data-analysis-180825-advanced_analytics_module_and_projects/auto_mpg_prediction_project
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Open the notebook:
```bash
jupyter notebook auto_mpg_prediction.ipynb
```

---

## 📌 Final Recommendation

| Priority | Recommended Model |
|---|---|
| **Best prediction accuracy** | Tuned Random Forest (Test R²=0.8909, RMSE=2.3601) |
| **Interpretability** | Lasso CV /Ridge CV (clear feature coefficients) |
| **Stability** | Lasso CV /Ridge CV |
| **Production deployment** | Tuned Random Forest |

