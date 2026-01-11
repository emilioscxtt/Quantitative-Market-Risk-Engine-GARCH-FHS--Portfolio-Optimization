# 📈 Quantitative Market Risk Engine: GARCH-FHS & Portfolio Optimization

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)
![Financial Modeling](https://img.shields.io/badge/Financial_Modeling-GARCH_%26_VaR-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Validated-success?style=for-the-badge)

## 📌 Executive Summary
This repository implements a robust **Market Risk Engine** designed to quantify extreme losses in Latin American equity portfolios (BMV/IPC). Unlike traditional parametric models that assume Normality (Gaussian distribution), this engine addresses **Volatility Clustering** and **Leptokurtosis** (Heavy Tails) —characteristics inherent to emerging markets.

The core core methodology utilizes **GARCH(1,1)** for volatility forecasting combined with **Filtered Historical Simulation (FHS)**, following the theoretical framework established in *Statistics of Financial Markets* (Franke, Härdle & Hafner).

## 🧬 Theoretical Framework & Methodology

The model rejects the Homoscedasticity assumption. Instead, it models returns ($r_t$) as a stochastic process dependent on time-varying volatility ($\sigma_t$), based on **SFM Chapter 14 (Financial Time Series)**.

### 1. GARCH(1,1) Process
Volatility is modeled using a Generalized Autoregressive Conditional Heteroskedasticity process to capture shocks:

$$\sigma^2_t = \omega + \alpha r^2_{t-1} + \beta \sigma^2_{t-1}$$

### 2. Heavy-Tailed Distribution (t-Student)
To account for "Black Swan" events, the standardized residuals ($\varepsilon_t$) are modeled using a Student's t-distribution ($\nu$ degrees of freedom) rather than a Normal distribution:

$$r_t = \mu + \sigma_t \varepsilon_t, \quad \varepsilon_t \sim t_\nu(0,1)$$

### 3. Filtered Historical Simulation (FHS)
We employ a semi-parametric approach:
1.  **Devolatilization:** Standardization of historical returns ($z_t = r_t / \sigma_t$) to obtain i.i.d. residuals.
2.  **Bootstrapping:** Simulation of future scenarios using the empirical distribution of $z_t$.
3.  **Revolatilization:** Scaling simulated residuals by the forecasted GARCH volatility ($\sigma_{t+1}$).

## 🛠️ Key Features

* **Dynamic Portfolio Construction:** Automated weighting and handling of data gaps for assets like `WALMEX`, `CEMEX`, `AMX`, `GFNORTE`.
* **Risk Metrics:** Calculation of **Value at Risk (VaR)** and **Expected Shortfall (ES/CVaR)** at 95% and 99% confidence levels.
* **Backtesting Engine:** "Reality check" visualizing exceptions (breaches) where actual losses exceeded VaR estimates.
* **Model Diagnostics:** Statistical validation via **ACF (Autocorrelation Function)** of squared residuals and **QQ-Plots** to ensure the model successfully filters volatility clusters.

## 📊 Visual Validation Results

### 1. Backtesting (Dynamic VaR vs. Actual Returns)
*The red line represents the dynamic risk threshold calculated by the GARCH model. Magenta points indicate market anomalies.*
![Backtesting Results](img/backtesting_result.png)

### 2. Statistical Diagnostics
*Validation of "White Noise" behavior in standardized residuals, confirming model robustness.*
![Diagnostics](img/model_diagnostics.png)

## 🚀 Installation & Usage

```bash
# Clone the repository
git clone [https://github.com/YourUsername/Market-Risk-GARCH-FHS.git](https://github.com/YourUsername/Market-Risk-GARCH-FHS.git)

# Install quantitative libraries
pip install numpy pandas yfinance arch matplotlib seaborn statsmodels
