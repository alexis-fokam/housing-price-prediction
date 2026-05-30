# 🏠 Housing Price Prediction

## Overview
Machine learning project predicting house sale prices based on 
property features using regression techniques.

**Best model: Random Forest — R² score: 0.87+**

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
| Model | R² Score | RMSE |
|-------|----------|------|
| Linear Regression | 0.75 | ~35,000 |
| Random Forest | 0.87 | ~22,000 |

## Key Features Used
- GrLivArea (living area square feet)
- OverallQual (overall material quality)
- TotalBsmtSF (basement area)
- YearBuilt (construction year)
- BedroomAbvGr (number of bedrooms)
- FullBath (number of bathrooms)

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