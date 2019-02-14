## Acid Precipitation and Remediation of Acid Lakes

# Group 1

K1=10−6.3, K2=10−10.3, KH=10−1.5molLatm, PCO2=10−3.5atm, and Kw=10−14.

1. Plot measured pH of the lake versus dimensionless hydraulic residence time (t/θ).
2. Assuming that the lake can be modeled as a completely mixed flow reactor and that ANC is a conservative parameter, equation (25) can be used to calculate the expected ANC in the lake effluent as the experiment proceeds. Graph the expected ANC in the lake effluent versus the hydraulic residence time (t/θ) based on the completely mixed flow reactor equation with the plot labeled (in the legend) as conservative ANC.
3. If we assume that there are no carbonates exchanged with the atmosphere during the experiment, then we can calculate ANC in the lake effluent by using equation (14) describing the ANC of a closed system. Calculate the ANC under the assumption of a closed system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as closed ANC.
4. If we assume that there is exchange with the atmosphere and that carbonates are at equilibrium with the atmosphere, then we can calculate ANC in the lake effluent by using equation (18) describing the ANC of an open system. Calculate the ANC under the assumption of an open system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as open ANC.
5. Analyze the data from the second experiment and graph the data appropriately. What did you learn from the second experiment?

```python
from aguaclara.core.units import unit_registry as u
import numpy as np
import math
import matplotlib.pyplot as plt
import pandas as pd
from scipy import stats

# Load data from file
data_file_path = ""

df = pd.read_csv(data_file_path,delimiter='\t')
print(df)
list(df)
columns = df.columns

Conc = df.iloc[:,0].values * u.mg/u.L
Voltage = df.iloc[:,1].values * u.V

#constants from measurements in ProCoDa, dark voltage and blank voltage
dark_V =-1.307035 * u.V
blank_V	= 3.563451 * u.V

# Calculate absorbance from voltage
A = -np.log10((Voltage-dark_V)/(blank_V-dark_V))

# Linear regression
slope, intercept, r_value, p_value, std_err = stats.linregress(Conc,A)

# Intercept is unitless
print(intercept)
slope = slope /Conc.units
print(slope)

# Now create a figure and plot the data and the line from the linear regression.
fig, ax = plt.subplots()
# plot the data as red circles
ax.plot(Conc, A, 'ro', )

#plot the linear regression as a black line
ax.plot(Conc, slope * Conc + intercept, 'k-', )

# Add axis labels using the column labels from the dataframe
ax.set(xlabel=list(df)[0])
ax.set(ylabel='Absorbance')
ax.legend(['Measured', 'Linear regression'])
ax.grid(True)
ax.set(title='Absorbance of light as a function of concentration of red dye')

# Save file
plt.savefig('/Users/Rosie/github/CEE4530/Lab1_plot')
plt.show()

```
