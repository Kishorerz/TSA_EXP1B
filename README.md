# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: s


### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.stattools import adfuller

data = pd.read_csv('TSLA.csv')

data['Date'] = pd.to_datetime(data['Date'])
data.set_index('Date', inplace=True)

tsla = data['Close'].dropna()

tsla_log = np.log(tsla)

tsla_diff = tsla.diff()
tsla_diff2 = tsla.diff().diff()
tsla_log_diff = tsla_log.diff()

try:
    decomposition = seasonal_decompose(tsla, model='additive', period=30)  # monthly seasonality approx
    seasonal = decomposition.seasonal
    trend = decomposition.trend
    residual = decomposition.resid
    seasonally_adjusted = tsla - seasonal
except Exception as e:
    seasonal = trend = residual = seasonally_adjusted = pd.Series(np.nan, index=tsla.index)

def adf_test(series, name):
    result = adfuller(series.dropna())
    return {
        "Series": name,
        "ADF Statistic": result[0],
        "p-value": result[1]
    }

adf_results = []
adf_results.append(adf_test(tsla, "Original"))
adf_results.append(adf_test(tsla_log, "Log Transformed"))
adf_results.append(adf_test(tsla_diff, "1st Differenced"))
adf_results.append(adf_test(tsla_diff2, "2nd Differenced"))
adf_results.append(adf_test(tsla_log_diff, "Log + Differenced"))
if seasonally_adjusted.notna().any():
    adf_results.append(adf_test(seasonally_adjusted, "Seasonally Adjusted"))

plt.figure(figsize=(14,14))

plt.subplot(611)
plt.plot(tsla, label="Original (Close Price)")
plt.legend()

plt.subplot(612)
plt.plot(tsla_log, label="Log Transformed")
plt.legend()

plt.subplot(613)
plt.plot(tsla_diff, label="1st Differenced")
plt.legend()

plt.subplot(614)
plt.plot(tsla_diff2, label="2nd Differenced")
plt.legend()

plt.subplot(615)
plt.plot(tsla_log_diff, label="Log + Differenced")
plt.legend()

plt.subplot(616)
if seasonally_adjusted.notna().any():
    plt.plot(seasonally_adjusted, label="Seasonally Adjusted")
    plt.legend()

plt.tight_layout()
plt.show()

adf_results

```

### OUTPUT:
ORIGINAL:
<img width="1387" height="228" alt="Screenshot 2025-08-19 133059" src="https://github.com/user-attachments/assets/a2f823ec-1359-4752-8ebe-4533b9ab9fa3" />


REGULAR DIFFERENCING:
<img width="1389" height="692" alt="Screenshot 2025-08-19 133301" src="https://github.com/user-attachments/assets/4603b017-acb1-4ac0-befa-e2dde8b434ad" />


SEASONAL ADJUSTMENT:
<img width="1384" height="236" alt="Screenshot 2025-08-19 133327" src="https://github.com/user-attachments/assets/053f7858-b7bf-4c47-a54b-0f77ad33b266" />


LOG TRANSFORMATION:
<img width="1358" height="228" alt="Screenshot 2025-08-19 133205" src="https://github.com/user-attachments/assets/cd7d16f8-b55f-47ad-807a-caac568db151" />



### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
