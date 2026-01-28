# Porto Seguro Safe Driver Prediction — Insurance Claim Risk Ranking

This project was developed as part of an AI Project Management Bootcamp, with a focus on both ML performance and product decision-making.

## Goal
Predict whether a driver will file an insurance claim in the next year, with the goal of improving risk ranking and enabling fairer pricing.


## Project Overview

Insurance pricing relies heavily on accurately ranking drivers by risk.
When models fail to properly separate low- and high-risk drivers:

- Safe drivers may subsidize risky ones
- Pricing becomes less competitive
- Customer trust can decrease

Our goal is to build a model that improves risk ranking, not just raw prediction accuracy, to support fairer and more efficient pricing strategies.

## Problem Framing

- **Task:** Binary classification — predict probability of a claim in the next year
- **Primary Objective:** Risk ranking, not yes/no classification
- **Primary Metric:** Normalized Gini Coefficient (industry standard in insurance)
- **Secondary Metric:** ROC-AUC (robust for rare-event prediction)

The dataset is highly imbalanced (~3.6% claim rate), so accuracy is not an appropriate metric.

## Dataset

We use the Porto Seguro Safe Driver Prediction dataset from Kaggle.

- ~595k training rows with labels
- ~892k test rows without labels
- 59 anonymized features grouped by:
    - ps_ind_* (driver-related)
    - ps_reg_* (region-related)
    - ps_car_* (vehicle-related)
    - ps_calc_* (computed features)
- Missing values are encoded as -1

📎 Dataset source:
https://www.kaggle.com/competitions/porto-seguro-safe-driver-prediction


## Approach Summary

### Models Tested
- Logistic Regression
- KNN
- Random Forest
- Decision Tree
- LightGBM
- CatBoost
- XGBoost

### Key Improvements
- Alternative categorical feature encodings (native vs one-hot)
- Removal of noisy ps_calc_* features
- Hyperparameter tuning with cross-validation
- Model ensembling and stacking

Final Model:
- A stacked ensemble combining:
    - CatBoost
    - LightGBM
    - Multiple diversified XGBoost models

Each model uses different feature encodings, reducing error correlation and improving overall ranking performance.

## Results

- Baseline Gini: ~0.27
- Final Ensemble Gini: ~0.29 (cross-validated)
- Kaggle Public Score: ~0.28

In insurance risk modeling, small improvements in ranking metrics can translate into meaningful business impact through better pricing segmentation and reduced cross-subsidization.

## Ethical Considerations & Risks

Key risks identified:
- Regional features as socioeconomic proxies, raising fairness concerns
- Feature dominance, where small data shifts could impact predictions
- Cold-start problem for new car models or regions

Mitigation strategies discussed in the project:
- Disparate impact testing across regional clusters
- Data drift monitoring
- Human-in-the-loop review for extreme pricing changes

This project is intended for educational purposes and does not represent a production-ready underwriting system.


## Set up your Environment

The added [requirements file](requirements.txt) contains all libraries and dependencies we need to execute the notebooks.

### **`macOS`** type the following commands : 

- Install the virtual environment and the required packages by following commands:

    ```BASH
    pyenv local 3.11.3
    python -m venv .venv
    source .venv/bin/activate
    pip install --upgrade pip
    pip install -r requirements.txt
    ```
### **`WindowsOS`** type the following commands :

- Install the virtual environment and the required packages by following commands.

   For `PowerShell` CLI :

    ```PowerShell
    pyenv local 3.11.3
    python -m venv .venv
    .venv\Scripts\Activate.ps1
    python -m pip install --upgrade pip
    pip install -r requirements.txt
    ```

    For `Git-Bash` CLI :
    ```
    pyenv local 3.11.3
    python -m venv .venv
    source .venv/Scripts/activate
    python -m pip install --upgrade pip
    pip install -r requirements.txt
    ```


*Note: If there are errors during environment setup, try removing the versions from the failing packages in the requirements file.*
