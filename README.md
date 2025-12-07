###  IBM stock price forecast

In this exercise, I analyzed the IBM stock adjusted close price. I checked which model returned the best MASE and RMSE results.

The IBM stock adjusted prices are from <a href="https://finance.yahoo.com/quote/IBM/history/" target="_blank">Yahoo Finance</a> .

### Models
- **Naïve Model**
- **Seasonal Naïve Model**
- **ETS (Error, Trend, Seasonal) Model**
- **ARIMA Model**
- **Neural Network Autoregression (NNAR)**

Performance was evaluated using **RMSE (Root Mean Squared Error)** and **MASE (Mean Absolute Scaled Error)**.


## Libraries Used

- `readr` – for reading CSV data  
- `forecast` – for time series forecasting models  
- `ggplot2` – for visualization  

---

## Methodology

1. **Data Loading**  
   - IBM adjusted close prices were imported from Yahoo Finance (`IBM.csv`).

2. **Data Preparation**  
   - Converted into a time series object (`ts`) with monthly frequency.  
   - Split into:
     - **Training set:** Jan 2016 – May 2021  
     - **Testing set:** Jun 2021 – Dec 2021 (7 months)

3. **Model Training & Forecasting**  
   - Each model was trained on the training set.  
   - Forecasts were generated for the 7‑month test period.  
   - Residuals were checked, and accuracy metrics were computed.

4. **Model Comparison**  
   - RMSE and MASE values were compared across models.  
   - Visual plots were generated to compare forecasts against actual test data.

---
