# Time Series Analysis of Bank Data

This repository contains an R script for performing time series analysis on bank loan data. The analysis includes model estimation, stationarity testing, parameter estimation, residual analysis, and forecasting.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Script Overview](#script-overview)
  - [Step 1: Install Packages and Import Dataset](#step-1-install-packages-and-import-dataset)
  - [Step 2: Model Estimation](#step-2-model-estimation)
  - [Step 3: Test for Stationarity](#step-3-test-for-stationarity)
  - [Step 4: Parameter Estimation](#step-4-parameter-estimation)
  - [Step 5: Fitted Values](#step-5-fitted-values)
  - [Step 6: Residual Analysis](#step-6-residual-analysis)
  - [Step 7: Forecasting](#step-7-forecasting)
- [Notes](#notes)
- [License](#license)

## Installation

To run this script, you need to have R installed on your system. Additionally, you need to install the following R packages:

- `tseries`
- `forecast`

You can install these packages using the following commands in R:

```R
install.packages("tseries")
install.packages("forecast")
```

## Usage

1. **Clone the repository or download the script.**

2. **Ensure you have installed the required packages:**

    ```R
    install.packages("tseries")
    install.packages("forecast")
    ```

3. **Download the dataset and place it in the specified location:**

    Make sure the `bank_case.txt` file is located at `C:\\Users\\hakhamanesh\\Downloads\\`.

4. **Run the script:**

    ```R
    source('path_to_your_script.R')
    ```

## Script Overview

### Step 1: Install Packages and Import Dataset

The script installs the necessary packages and imports the dataset as a time series object.

```R
install.packages("tseries")
install.packages("forecast")

library(tseries)
library(forecast)

bank_case <- as.ts(scan("C:\\Users\\hakhamanesh\\Downloads\\bank_case.txt"))
print(bank_case)
```

### Step 2: Model Estimation

The script plots the time series data, autocorrelation function (ACF), and partial autocorrelation function (PACF).

```R
par(mfrow=c(1,3))
plot(bank_case, main = "Time Series of Loans")
acf(bank_case, main = "ACF of Loans", lag.max = 10, ylim = c(-1,1))
pacf(bank_case, main = "PACF of Loans", lag.max = 10, ylim = c(-1,1))
```

### Step 3: Test for Stationarity

The script tests the time series for stationarity using the Augmented Dickey-Fuller (ADF) test and performs differencing if necessary.

```R
adf.test(bank_case)
bank_case_d1 <- diff(bank_case)
adf.test(bank_case_d1)
bank_case_d2 <- diff(bank_case_d1)
adf.test(bank_case_d2)
par(mfrow=c(1,3))
plot(bank_case_d2, main = "SOD Time Series of Loans")
acf(bank_case_d2, main = "ACF of SOD Loans", lag.max = 10, ylim = c(-1,1))
pacf(bank_case_d2, main = "PACF of SOD Loans", lag.max = 10, ylim=c(-1,1))
```

### Step 4: Parameter Estimation

The script estimates the parameters of the ARIMA model.

```R
bank_fit <- arima(x=bank_case, order = c(0,2,1))
bank_fit
```

### Step 5: Fitted Values

The script calculates and prints the fitted values of the ARIMA model.

```R
fitted(bank_fit)
```

### Step 6: Residual Analysis

The script performs residual analysis to check the adequacy of the model.

```R
par(mfrow = c(1,3))
plot(bank_fit$residuals, ylab = "Residuals")
acf(bank_fit$residuals, ylim = c(-1,1))
pacf(bank_fit$residuals, ylim = c(-1,1))

checkresiduals(bank_fit)
```

### Step 7: Forecasting

The script forecasts future values using the ARIMA model and plots the forecast.

```R
bank_pred <- forecast(bank_fit, h=24)
bank_pred
par(mfrow = c(1,1))
plot(bank_pred)
```

## Notes

- Ensure that the dataset path in the script matches the location of your dataset.
- The script assumes the dataset is a univariate time series stored in a text file.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
