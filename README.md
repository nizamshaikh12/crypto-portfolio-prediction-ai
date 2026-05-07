# Cryptocurrency Portfolio Prediction & Adaptive Management

**Module:** AI for Finance  
**Course:** MSc in FinTech (MSCFTD1)  
**Student ID:** 24198170

---

## Overview

This project builds an **AI‑driven cryptocurrency portfolio management pipeline** using the **Top 100 Cryptocurrency (2020–2025)** dataset from Kaggle [file:56][file:55]. The goal is to predict next‑period returns across a large, dynamic universe of coins and convert those forecasts into an adaptive long‑only portfolio strategy with realistic transaction cost adjustments.

Instead of focusing only on single‑asset price prediction, the pipeline emphasises:

- **Scale‑invariant feature engineering** so that BTC, ETH and low‑priced altcoins can be modelled in a single dataset
- **Lightweight time‑series modelling** using both classical ML and deep sequence architectures
- **Portfolio‑level backtesting**, including transaction costs, rather than just regression metrics
- **Explainability** via SHAP to understand which technical and volatility features drive the models

---

## Objectives

- Load and clean the full **Top 100 Cryptocurrency (2020–2025)** dataset (≈211k rows, 100 symbols) [file:56][file:55]
- Engineer **ratio‑normalised** technical features that are comparable across assets regardless of absolute price level
- Transform the target into **log returns** with winsorisation to control extreme outliers
- Build and compare five forecasting models:
  - Random Forest
  - XGBoost
  - 1D CNN
  - LSTM
  - GRU
- Evaluate models using RMSE, MAE, R² and **Directional Accuracy** on next‑period returns
- Convert model forecasts into a simple **Top‑k equal‑weight portfolio** with transaction cost deductions
- Analyse portfolio performance vs an equal‑weight benchmark (total return, Sharpe, drawdown, volatility)
- Use **SHAP** to interpret which features most strongly influence predictions

---

## Dataset

- Source: Kaggle — *Top 100 Cryptocurrency (2020–2025)* [file:56][file:55]
- Raw columns include:
  - `symbol`, `date`, `open`, `high`, `low`, `close`, `network`
- After cleaning and feature engineering, the working dataset includes:
  - **Log returns** for prediction target
  - **Moving average distances** (e.g., price / MA)
  - **Volatility regime indicators**
  - **Technical indicators** (e.g., RSI, Bollinger Band position, MACD‑style ratios)
- All **100 symbols** are used to create ~240k training rows, which is critical for deep learning stability [file:56]

Key design decisions:

- Raw OHLC prices are **removed** to avoid scale dominance (BTC at ~35,000 vs DOGE at ~0.11) [file:56]
- Target is log return with **winsorisation at ±3σ** to reduce noise‑driven extremes [file:56][file:55]
- Features are **ratio‑normalised** so that patterns are comparable across all assets
- Sequence models use a **lookback window of 20 days** to capture richer temporal context [file:56]

---

## Methods & Models

### Modelling pipeline

- Data loaded from an Excel workbook (e.g., `top_100_cryptocurrency_2020_2025 - 24198170.xlsm`) [file:56]
- Per‑symbol processing and feature engineering
- Train/validation/test splits respecting time order to minimise look‑ahead bias

### Models

All models are trained on the same feature set to allow a fair comparison [file:56]:

- **Random Forest Regressor**
- **XGBoost Regressor**
- **1D Convolutional Neural Network (CNN)** for local pattern extraction
- **LSTM** for sequence‑based temporal dependencies
- **GRU** as a lighter recurrent alternative

Evaluation metrics:

- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- R‑squared (R²)
- **Directional Accuracy** — how often the model correctly predicts the sign of the next return [file:55]

---

## Portfolio Construction & Backtesting

The project goes beyond prediction by converting forecasts into a simple prototype trading strategy [file:55]:

- For each rebalancing date, predicted returns are ranked across assets
- A **Top‑3 equal‑weight portfolio** is constructed from the highest‑ranked coins
- A fixed **transaction cost (e.g., 10 bps per trade)** is deducted on each rebalance cycle
- Performance is compared against an **equal‑weight benchmark** that allocates uniformly across all available assets

Reported portfolio metrics include [file:55]:

- Total return
- Sharpe ratio
- Maximum drawdown
- Annualised volatility

This highlights that in non‑stationary crypto markets, portfolio‑level outcomes and directional signals matter more than maximising R² alone [file:55].

---

## Explainability (SHAP)

To ensure the Random Forest model does not act as a pure black box, **SHAP** is used to identify the most influential features [file:55][file:56]:

- Top drivers include:
  - Return dynamics over recent windows
  - Moving average distance features
  - Volatility regime indicators
  - Technical indicators such as RSI, Bollinger Band position and MACD‑style signals

SHAP summary plots show that the model leverages meaningful market structure rather than noise, improving interpretability for a risk or portfolio manager audience [file:55].

---

## Files in this Repository

- `Mohd-Nizam-Shaikh-Cryptocurrency-24198170.ipynb` — main AI pipeline notebook for cryptocurrency portfolio prediction [file:56]
- `Mohd-Nizam-Shaikh-CA2-AI-for-Finance-24198170.pdf` — full project report with literature review, methodology, experiments and portfolio analysis [file:55]
- `CA1-AI-for-Finance-24198170.pdf` — critical review of deep reinforcement learning‑based portfolio management (Betancourt & Chen, 2021) [file:57]
- `top_100_cryptocurrency_2020_2025 - 24198170.xlsm` — dataset workbook (if included)

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Python 3** | Core programming language |
| **pandas / NumPy** | Data wrangling and numerical operations |
| **scikit‑learn** | Random Forest, metrics, preprocessing |
| **XGBoost** | Gradient boosting baseline |
| **TensorFlow / Keras** | CNN, LSTM and GRU sequence models |
| **ta** | Technical analysis indicators (RSI, Bollinger Bands, etc.) |
| **SHAP** | Model explainability |
| **Matplotlib / Seaborn** | Visualisation of returns, errors and portfolio curves |
| **Jupyter Notebook** | Interactive experimentation and documentation |

---

## How to Run

1. Clone this repository:

   ```bash
   git clone https://github.com/nizamshaikh12/crypto-portfolio-prediction-ai.git
   ```

2. Open the main notebook in **Jupyter**, **VS Code**, or **Google Colab**:

   ```text
   Mohd-Nizam-Shaikh-Cryptocurrency-24198170.ipynb
   ```

3. Place the cryptocurrency dataset file (e.g., `top_100_cryptocurrency_2020_2025 - 24198170.xlsm`) in the expected path used inside the notebook.

4. Run the notebook cells sequentially to:
   - Engineer features and targets
   - Train and compare Random Forest, XGBoost, CNN, LSTM and GRU models
   - Evaluate forecasting metrics and directional accuracy
   - Backtest the Top‑3 portfolio strategy with transaction costs
   - Generate SHAP explainability plots

> **Requirements:** Python 3.x, pandas, numpy, scikit‑learn, xgboost, tensorflow / keras, ta, shap, matplotlib, seaborn, jupyter

---

## Author

**Mohd Nizam Mohd Nasir Shaikh**  
MSc in FinTech — National College of Ireland
