# 🏠 Housing Price Prediction

## Overview
Machine learning project predicting house sale prices based on 
property features using regression techniques.

**Best model: Linear Regression + StandardScaler — R² score: 0.90+**

## Problem Statement
Real estate investors and agencies need accurate price estimates 
to make informed decisions. This model predicts house prices 
based on key features like size, quality, location and age.

## Tech Stack
- Python 3.x
- Pandas & NumPy (data processing)
- Scikit-learn (ML models)
- Matplotlib & Seaborn (visualizations)
- Jupyter Notebook

## Results
| Model | R² Score (CV 5-fold) | R² Score (test) | RMSE |
|-------|---------------------|-----------------|------|
| Linear Regression + StandardScaler | **0.9048 ± 0.007** | **0.8881** | ~21,811 $ |
| Random Forest | 0.8751 ± 0.008 | 0.8618 | ~25,043 $ |

## Key Features Used
- TotalSF (total surface = GrLivArea + TotalBsmtSF) — feature engineerée
- OverallQual (overall material quality)
- GrLivArea (living area square feet)
- TotalBath (total bathrooms — feature engineerée)
- HouseAge (age at sale — feature engineerée)
- GarageCars (garage capacity)

## Project Structure
housing-price-prediction/
├── data/
│   └── train.csv
├── notebooks/
│   └── housing_prediction.ipynb
├── model.pkl
├── requirements.txt
└── README.md

## How to Run
# Clone the repository
git clone https://github.com/alexis-fokam/housing-price-prediction

# Install dependencies
pip install -r requirements.txt

# Launch notebook
jupyter notebook notebooks/housing_prediction.ipynb

## Dataset
Kaggle House Prices - Advanced Regression Techniques
https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques

## Author
Alexis Fokam — Data Scientist & ML Engineer
Upwork: https://www.upwork.com/freelancers/~01b522970676cfcfb6
LinkedIn: www.linkedin.com/in/alexis-fokam



main                    # branche principale (code final propre)
├── dev                 # développement général
├── feature/data-exploration      # jour 1
├── feature/data-cleaning         # jour 1
├── feature/feature-engineering   # jour 2
├── feature/model-training        # jour 2
├── feature/model-evaluation      # jour 2
└── feature/visualizations        # jour 3