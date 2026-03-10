# Project Overview
The goal was to predict **miles per gallon (mpg)** using car features: cylinders, weight, horsepower, displacement, acceleration, model year, and origin.

## Data Preprocessing
Several important steps were taken before modelling:
- **Log transformation** applied to `weight`, `horsepower`, and `displacement` as they were right-skewed
- **One-Hot Encoding** applied to `origin` and `cylinders` as categorical features
- **StandardScaler** applied to all numerical features
- **model** was treated as a numerical feature representing the year

## Models Compared

| Model | Train R² | Test R² | Gap |
|---|---|---|---|
| PCA + Regression | 0.8487 | 0.8141 | 0.0346 |
| Lasso CV | 0.8611 | 0.8309 | 0.0302 |
| **Random Forest (Tuned)** | 0.9513 | **0.8912** | 0.0601 |

## Key Findings

### 1. Best Performing Model: Tuned Random Forest
- Achieved the highest Test R² of **0.8912**
- Explains **89% of variance** in mpg on unseen data
- Captures **non-linear relationships** between features and mpg that linear models cannot

### 2. Most Stable Model: Lasso CV
- Smallest gap of **0.0302** — most consistent between train and test
- Test R² of **0.8309** — strong performance
- Most **interpretable** — shows exactly which features matter
- Alpha of **0.0385** — mild regularization was sufficient

### 3. PCA + Regression
- Compressed 7 features into **5 components** explaining 95% of variance
- Weakest Test R² of **0.8141**
- Least interpretable — components are abstract combinations of features
- Not ideal here since only 7 features — PCA adds little value with few features

## What Drives MPG?

Based on Lasso and Random Forest findings, the most influential features are likely:
- **Weight** — heavier cars consume more fuel
- **Horsepower** — more power = more fuel
- **Displacement** — larger engine = more fuel
- **Model year** — newer cars tend to be more fuel efficient
- **Origin** — Japanese/European cars tend to be more fuel efficient than American

## Residual Analysis
The Lasso residual plot confirmed:
- ✅ Residuals centered around 0 — unbiased predictions
- ✅ No curve — linear relationship is appropriate
- ✅ No funnel shape — constant variance
- ⚠️ A few large positive residuals — model slightly under-predicts high mpg cars

## Final Recommendation

| Scenario | Recommended Model |
|---|---|
| **Best prediction accuracy** | Tuned Random Forest |
| **Interpretability needed** | Lasso CV |
| **Production deployment** | Tuned Random Forest |
| **Explaining to stakeholders** | Lasso CV |

## Conclusion

- Overall, the **Tuned Random Forest** is the best model for predicting mpg with a Test R² of 0.8912. 
- However, if interpretability is important — for example, understanding which car features most affect fuel efficiency — **Lasso CV** is the recommended choice with a strong Test R² of 0.8309 and the smallest overfitting gap of 0.0302.