# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
# Date:28-04-2024
### AIM:
To Implement Linear and Polynomial Trend Estimation Using Python.

### Software Required:
Google colab

### Dataset:
stock_data.csv

### ALGORITHM:
1.Import necessary libraries (NumPy, Matplotlib)

2.Load the dataset

3.Calculate the linear trend values using least square method

4.Calculate the polynomial trend values using least square method

5.End the program

### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('/content/stock_data (1).csv')

data['Date'] = pd.to_datetime(data['Date'], dayfirst=True)
data.set_index('Date', inplace=True)

stock = data['Stock_1']

resampled_data = stock.resample('ME').mean().to_frame()

resampled_data.index = resampled_data.index.strftime('%Y-%m')
resampled_data.reset_index(inplace=True)

resampled_data.columns = ['Month', 'Price']

X = list(range(1, len(resampled_data) + 1))
prices = resampled_data['Price'].tolist()

n = len(X)

x2 = [i**2 for i in X]
xy = [i*j for i, j in zip(X, prices)]

b = (n*sum(xy) - sum(prices)*sum(X)) / (n*sum(x2) - (sum(X)**2))
a = (sum(prices) - b*sum(X)) / n

linear_trend = [a + b*i for i in X]

x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i, j in zip(x2, prices)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(prices), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)

a_poly, b_poly, c_poly = solution

poly_trend = [a_poly + b_poly*i + c_poly*(i**2) for i in X]

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

print("Linear Trend: y =", round(a,2), "+", round(b,2), "x")
print("Polynomial Trend: y =", round(a_poly,2), "+", round(b_poly,2), "x +", round(c_poly,2), "x²")

plt.figure(figsize=(12,6))
plt.plot(resampled_data['Month'], resampled_data['Price'], marker='o', label='Stock Price')
plt.plot(resampled_data['Month'], resampled_data['Linear Trend'], linestyle='--', label='Linear Trend')
plt.title('Linear Trend Analysis of Stock_1')
plt.xlabel('Month')
plt.ylabel('Price')
plt.xticks(rotation=90)
plt.legend()
plt.grid(True)
plt.show()

plt.figure(figsize=(12,6))
plt.plot(resampled_data['Month'], resampled_data['Price'], marker='o', label='Stock Price')
plt.plot(resampled_data['Month'], resampled_data['Polynomial Trend'], label='Polynomial Trend')
plt.title('Polynomial Trend Analysis of Stock_1')
plt.xlabel('Month')
plt.ylabel('Price')
plt.xticks(rotation=90)
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="1242" height="720" alt="image" src="https://github.com/user-attachments/assets/58193c5f-a22d-4ea4-9a37-b060f195e35d" />

B- POLYNOMIAL TREND ESTIMATION

<img width="1258" height="668" alt="image" src="https://github.com/user-attachments/assets/89abe91c-c7de-43b7-b4b7-4bcfaee9a0e1" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estimation has been executed successfully.
