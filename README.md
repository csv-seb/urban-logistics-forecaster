# Predictive Modeling for Fleet Optimization

## Technical Deep-Dive: Time-Series Regression & Gradient Boosting

## Executive Summary

In the high-stakes environment of airport logistics, managing driver supply to meet volatile passenger demand is critical. This project develops a high-performance forecasting engine to predict taxi orders for the upcoming hour. By transitioning from a baseline Random Forest to an optimized LightGBM architecture, I reduced model complexity while improving generalization. The final solution achieves a Root Mean Squared Error (RMSE) of 40.03, significantly outperforming the project benchmark of 48.

## Business Problem

Sweet Lift Taxi required a data-driven approach to attract more drivers during peak hours at airports. The challenge was to transform historical timestamped data into a proactive scheduling tool. 

### Key Project Constraints:

- Temporal Integrity: Shuffling was disabled to avoid information leakage from the future into the past.
- Accuracy Target: RMSE ≤ 48 on the test set.
- Computational Efficiency: Low training latency for real-time deployment.

## Technical Stack

- Language: Python
- Libraries: Scikit-learn, LightGBM, XGBoost, Pandas, Seaborn
- Validation: TimeSeriesSplit Cross-Validation
- Optimization: RandomizedSearchCV, Permutation Importance

## Methodology

### 1. Feature Engineering (Temporal Logic)
To capture the underlying cyclicality of airport arrivals, I engineered features that allow the model to "perceive" time:
- Seasonal Markers: Extraction of day of the week and hour of the day from the datetime index.
- Autocorrelation (Lags): 24-hour lag features to capture daily cycles.
- Trend Analysis: 10-hour rolling mean to smooth short-term fluctuations and capture local trends.
  
### 2. Validation Strategy
To ensure the model is robust, I implemented TimeSeriesSplit. This ensures that the validation set always follows the training set chronologically, preventing data leakage.

### 3. Model Comparison & Optimization
I benchmarked three primary architectures:

| Model | Test RMSE | Training Time | Verdict |
| ----- | --------- | ------------- | ------- |
| Random Forest | 44.80 | 3.37s | Overfitted | baseline |
| XGBoost | 42.26 | 4.64s |Robust but slower training |
| LightGBM (Optimized) | 40.03 | 0.19s | Selected Production Model |

## Visual Insights
### Fleet Allocation Matrix

<img width="790" height="375" alt="taxi-heatmap" src="https://github.com/user-attachments/assets/1b1bb43a-8c9e-467a-8171-a22ca5384777" />

This heatmap identifies "Red Zones"—specifically midnight on weekends and late afternoon—where fleet availability must be maximized.
### Forecast Reliability
The plot above compares actual orders against LightGBM predictions, confirming the model effectively captures sharp evening rush-hour spikes.

## Key Takeaways
- Model Simplicity Wins: Using Permutation Importance, I reduced the feature set from 31 to 7, decreasing training time by more than 65% while maintaining an RMSE of 40.03.
- Gradient Boosting Superiority: LightGBM’s leaf-wise growth strategy proved significantly more effective than Random Forest at modeling non-linear temporal spikes.

Author: Sebastian Méndez

## Acknowledgments
This project was developed as a graduation requirement for a Data Science Bootcamp. The dataset was provided by the instructional team; feature engineering, optimization logic, and final analysis were performed independently.
