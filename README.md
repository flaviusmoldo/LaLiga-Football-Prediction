# La Liga Match Outcome Prediction

Machine learning pipeline for predicting La Liga football match results (Home Win / Draw / Away Win) using in-game statistics. Built for the Intelligent Systems course.

## Dataset

Historical La Liga match data from seasons 2005/06 onwards (~7600 matches), sourced from an Excel file. Each row is one match with statistics: shots, shots on target, fouls, corners, yellow/red cards, half-time goals, and the full-time result (FTR) as the target variable.

Full-time goals (`FTHG`, `FTAG`) are excluded from features — they directly determine the result and would be unavailable at prediction time.

## Models Compared

| Model | Notes |
|---|---|
| Decision Tree | Interpretable baseline |
| Random Forest | Ensemble; handles non-linearity |
| SVM (RBF) | Strong on scaled tabular data |
| XGBoost | Gradient boosting; state-of-the-art tabular |
| CatBoost | Gradient boosting; few hyperparams to tune |

Evaluation: **weighted F1-score** via **Stratified 5-Fold Cross-Validation**. Test set (15%) held out and used only once for final evaluation.

## Key Results

- Gradient boosting methods (XGBoost, CatBoost) achieve the best performance
- Draws are the hardest outcome to predict across all models — consistent with football analytics literature
- Strongest predictors: `HTGoalDiff` (half-time goal difference), `ShotOnTDiff`, `HTR` (half-time result)

## Feature Engineering

Difference-based features capture relative team performance instead of raw counts:

```
ShotDiff, ShotOnTDiff, FoulDiff, CornerDiff,
YellowDiff, RedDiff, HTGoalDiff, HShotAcc, AShotAcc
```

`HTR` (half-time result) is one-hot encoded. All features are standardized with `StandardScaler`.

## Pipeline Overview

1. EDA — class distribution, correlations, half-time vs full-time result transitions
2. Data cleaning — drop leaky and metadata columns
3. Feature engineering + encoding + normalization
4. PCA — variance analysis and 2D visualization
5. Stratified K-Fold CV — model comparison with error bars
6. Final evaluation — confusion matrices, classification reports, feature importances

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
catboost
openpyxl
```

Install with:

```bash
pip install -r requirements.txt
```

## Usage

Place `Football_LaLiga_DataSets_From0506.xlsx` in the project root and run `LaLiga_Project.ipynb` end-to-end.

## Author

Moldovan Flavius Cosmin — Technical University of Cluj-Napoca, Intelligent Systems Course
