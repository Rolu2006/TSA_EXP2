# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 31-10-2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
A - LINEAR TREND ESTIMATION
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('/content/airline(1).csv')

data['Airline passengers carried'] = (
    data['Airline passengers carried']
    .astype(str).str.replace(',', '', regex=False).astype(float)
)

resampled_data = data.groupby('Year')['Airline passengers carried'].sum().reset_index()

years = resampled_data['Year'].tolist()
passengers = resampled_data['Airline passengers carried'].tolist()

X = [i - years[len(years)//2] for i in years]
x2 = [i**2 for i in X]
xy = [i*j for i, j in zip(X, passengers)]

n = len(years)

b = (n*sum(xy) - sum(passengers)*sum(X)) / (n*sum(x2) - sum(X)**2)
a = (sum(passengers) - b*sum(X)) / n

linear_trend = [a + b*X[i] for i in range(n)]

print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")

resampled_data['Linear Trend'] = linear_trend
resampled_data.set_index('Year', inplace=True)

resampled_data['Airline passengers carried'].plot(
    color='blue', marker='o', label='Passengers'
)
resampled_data['Linear Trend'].plot(
    color='black', linestyle='--', label='Linear Trend'
)

plt.title('Linear Trend Estimation')
plt.xlabel('Year')
plt.ylabel('Number of Passengers')
plt.legend()
plt.grid(True)
plt.show()
```


B- POLYNOMIAL TREND ESTIMATION
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('/content/airline(1).csv')

data['Airline passengers carried'] = (
    data['Airline passengers carried']
    .astype(str).str.replace(',', '', regex=False).astype(float)
)

resampled_data = data.groupby('Year')['Airline passengers carried'].sum().reset_index()

years = resampled_data['Year'].tolist()
passengers = resampled_data['Airline passengers carried'].tolist()

X = [i - years[len(years)//2] for i in years]
x2 = [i**2 for i in X]
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
xy = [i*j for i, j in zip(X, passengers)]
x2y = [i*j for i, j in zip(x2, passengers)]

n = len(years)

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(passengers), sum(xy), sum(x2y)]

A = np.array(coeff, dtype=float)
B = np.array(Y, dtype=float)

solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly*X[i] + c_poly*X[i]**2
    for i in range(n)
]

print(
    f"Polynomial Trend: y = {a_poly:.2f} "
    f"+ {b_poly:.2f}x + {c_poly:.2f}x²"
)

resampled_data['Polynomial Trend'] = poly_trend
resampled_data.set_index('Year', inplace=True)

resampled_data['Airline passengers carried'].plot(
    color='blue', marker='o', label='Passengers'
)
resampled_data['Polynomial Trend'].plot(
    color='black', marker='o', label='Polynomial Trend'
)

plt.title('Polynomial Trend Estimation')
plt.xlabel('Year')
plt.ylabel('Number of Passengers')
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="1335" height="662" alt="image" src="https://github.com/user-attachments/assets/13e33a4f-94a6-4f40-91b9-0dc9bf8ffb09" />

B- POLYNOMIAL TREND ESTIMATION

<img width="1336" height="642" alt="image" src="https://github.com/user-attachments/assets/adfff38f-8038-420d-bdc6-5d85638b0601" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
