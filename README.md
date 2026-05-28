# 📦 E-Commerce Demand Forecasting & Inventory Optimization Engine

## 🎯 Business Objective
In e-commerce supply chains, overstocking ties up critical capital, while understocking leads to stockouts and lost revenue. This project builds a robust time-series forecasting engine to predict monthly product demand, allowing the warehouse to optimize inventory levels mathematically.

By transitioning from a baseline Linear Regression model to an optimized Extreme Gradient Boosting (XGBoost) architecture, the average prediction error was reduced by **73%**, translating to a highly predictable inventory variance.

## 🛠 Tech Stack & Architecture
* **Language:** Python
* **Data Processing Pipeline:** Pandas, NumPy
* **Machine Learning Engine:** XGBoost, Scikit-Learn
* **Diagnostic Visualization:** Seaborn, Matplotlib

## 🧠 Key AI Engineering & Pipeline Decisions

To ensure this model is robust enough for a production environment, several advanced data engineering techniques were implemented:

* **Strict Temporal Aggregation:** Raw minute-by-minute transactional data was chronologically bounded into strict `Month_Year` periods to accurately map macroeconomic growth without data leakage.
* **Accounting Anomaly Filtering:** Identified and stripped extreme negative quantities (representing bulk B2B cancellations and warehouse write-offs) to prevent the algorithm from artificially lowering consumer demand forecasts.
* **Autoregressive Feature Engineering:** Machine learning models do not natively understand time. The pipeline transforms the data to satisfy the Markov property by engineering `Lag_1`, `Lag_2`, and `Lag_3` features, alongside a `Rolling_Mean_3`, allowing the model to learn short-term momentum and velocity.
* **Algorithmic Outlier Capping:** E-commerce data is heavily skewed by massive one-off wholesale orders. The pipeline calculates and caps target variables at the 99th percentile, forcing the loss function to optimize for daily operational variance rather than chasing extreme noise.

## 📊 Exploratory Data Analysis (EDA) Insights
The EDA phase uncovered critical business dynamics that dictated the ML feature strategy:
1. **The 80/20 Rule:** A highly concentrated subset of products drives the vast majority of volume, prioritizing the need for exact forecasting on top-tier SKUs.
2. **Cyclical Seasonality:** Aggregate monthly volume proves a massive spike in Q4 (November/December). The integer `Month` feature was explicitly passed to the XGBoost model to mathematically trigger seasonal weighting.

## 🚀 Model Evaluation & Impact

* **Baseline Linear Regression MAE:** 120.21 units
* **Random Forest Regressor MAE:** 89.38 units
* **Optimized XGBoost MAE:** 23.78 units

**Business Translation:** When procurement orders inventory for the upcoming month, the XGBoost engine will, on average, accurately predict the necessary stock within a ~24 unit variance. This allows the business to safely lower its safety stock thresholds, freeing up liquidity while maintaining high fulfillment rates.

## 💻 How to Run

```bash
git clone [https://github.com/your-username/ecommerce-demand-forecasting.git](https://github.com/your-username/ecommerce-demand-forecasting.git)
cd ecommerce-demand-forecasting
pip install -r requirements.txt
jupyter notebook E_Commerce_Demand_Prediction.ipynb
