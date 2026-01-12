# Demand Prediction for Service Availability
*Time-Series Regression & Gradient Boosting*

## Executive Summary

In dynamic, service-oriented environments, anticipating short-term demand is critical to reducing wait times and ensuring reliable user experiences. This project focuses on building a time-series forecasting system to predict hourly taxi demand at busy airports, enabling proactive capacity planning and better service availability.

By combining rigorous validation practices with interpretable predictive modeling, the final solution achieved a Root Mean Squared Error (RMSE) of 40.03, significantly outperforming the project benchmark of 48, while remaining efficient enough for real-time decision support.

## Business Problem

Sweet Lift Taxi needed a data-driven way to anticipate demand spikes and encourage driver availability during peak hours. The challenge was to transform historical, timestamped order data into a reliable forecasting tool that could support operational decisions without relying on future information.

### Key constraints:

- Temporal integrity: No data leakage from future observations.

- Reliability: RMSE ≤ 48 on unseen data.

- Efficiency: Low training latency suitable for near–real-time use.

## Why This Matters
Accurate short-term demand forecasting enables:

- Reduced user wait times.

- Better service availability during high-demand periods.

- More efficient and fair resource allocation.

These challenges closely mirror capacity planning and scheduling problems in digital health and telehealth platforms, where reliability and trust in predictions are essential.

## Demand Patterns: Hourly & Weekly Intensity

<p align="center">
  <img src="taxi-heatmap.png" width="650"/>
</p>

This heatmap visualizes average hourly demand across the week, revealing strong temporal patterns and predictable demand cycles.

Clear peaks emerge during late-night and early-morning hours on weekends, as well as during late afternoon hours on weekdays. Conversely, early morning hours consistently show consistently low demand across all days.

These insights informed both feature engineering decisions and operational assumptions, enabling the model to anticipate demand surges while avoiding unnecessary over-allocation during low-activity periods.

## Methodology Overview
### Validation Strategy

To preserve chronological integrity and avoid data leakage, I implemented a TimeSeriesSplit cross-validation strategy, ensuring that all validation data strictly followed the training data in time.

### Feature Engineering

To capture daily usage patterns and short-term trends:

- Seasonality: Hour-of-day and day-of-week features.

- Autocorrelation: 24-hour lag features to model daily cycles.

- Trend smoothing: 10-hour rolling mean features.

## Model Benchmarking & Selection
I benchmarked three primary architectures:

| Model | Test RMSE | Training Time | Notes |
| ----- | --------- | ------------- | ------- |
| Random Forest | 44.80 | 3.37s | Overfitting observed |
| XGBoost | 42.26 | 4.64s | Strong performance, slower training |
| LightGBM (Optimized) | 40.03 | 0.19s | Best balance of accuracy & efficiency |

Using **Permutation Importance**, I reduced the feature set from 31 to 7, improving generalization and reducing training time by approximately 68%.

## Key Insights & Results

- Interpretable models scale better: Simpler feature sets improved stability and performance.

- Gradient boosting excels in volatile demand: LightGBM effectively captured sharp, non-linear demand spikes.

- Production readiness matters: Low latency and robust validation were prioritized alongside accuracy.

## Technical Stack

- Language: Python

- Libraries: Pandas, scikit-learn, LightGBM

- Validation: TimeSeriesSplit

- Optimization: RandomizedSearchCV, Permutation Importance
  
## Author
Sebastian Méndez Ramírez

## Acknowledgments
This project was developed as a graduation requirement for a Data Science Bootcamp. The dataset was provided by the instructional team. All feature engineering, validation strategy, model optimization, and analysis were performed independently.
