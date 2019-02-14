## Laboratory Measurements and Procedures

1. Fill out the attached spreadsheet. Make sure that all calculated values are entered in the spreadsheet as equations. All remaining analysis for the course will be done in Atom using Python!
2. Create a graph of absorbance vs. concentration of red dye \#40 in Atom/Markdown using the exported data file. Does absorbance increase linearly with concentration of the red dye? Remove data points from the graph that are outside of the linear region.
3. What is the value of the extinction coefficient, ε?
4. Did you use interpolation or extrapolation to get the concentration of the unknown?
5. What measurement controls the accuracy of the density measurement for the NaCl solution?
6. What density did you expect (see prelab 2)?
7. Approximately what should the accuracy be for the density measurement?
8. Don’t forget to write a brief paragraph on conclusions and on suggestions using Markdown.
9. Verify that your report and graphs meet the requirements as outlined on the course website.

```Python
from aguaclara.core.units import unit_registry as u
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from scipy import stats
#The data file path is the raw data url on github. Happily python can read directly from a web page.
data_file_path = "https://raw.githubusercontent.com/jiwon0206/4530/master/abs_and_conc.txt"

#Now we create a pandas dataframe with the data in the file
df = pd.read_csv(data_file_path,delimiter='\t')
#if you want to see what is in the dataframe you can print it!
print(df)
# The column headers can be access by using the list command
list(df)
columns = df.columns
print(columns)
#Below are three equally fine methods of extracting a column of data from the pandas dataframe.

# 1) We can select a column by using the column header. Here we use the column header by selecting one array element from the list command.
x = df[list(df)[0]].values * u.mg/u.L
x
# 2) We can use the loc command to select all of the rows (: command) and the column with the label given by list(df)[0].
x = df.loc[:, list(df)[0]].values * u.mg/u.L
x
# 3) We can use the iloc command and select all of the rows in column 0.
x = df.iloc[:,0].values * u.mg/u.L
x
#The iloc method is simple and efficient, so I'll use that to get the y values.
y = df.iloc[:,1].values * u.mg/u.L

# We will use the stats package to do the linear regression.
# It is important to note that the units are stripped from the x and y arrays when processed by the stats package.
slope, intercept, r_value, p_value, std_err = stats.linregress(x,y)

#We can add the units to intercept by giving it the same units as the y values.
intercept = intercept * y.units
# Note that slope is dimensionless for this case, but not in general!
# For the general case we can attach the correct units to slope.
slope = slope * y.units/x.units

# Now create a figure and plot the data and the line from the linear regression.
fig, ax = plt.subplots()
# plot the data as red circles
ax.plot(x, y, 'ro', )

#plot the linear regression as a black line
ax.plot(x, slope * x + intercept, 'k-', )

# Add axis labels using the column labels from the dataframe
ax.set(xlabel=list(df)[0])
ax.set(ylabel=list(df)[1])
ax.legend(['Measured', 'Linear regression'])
ax.grid(True)
# Here I save the file to my local harddrive. You will need to change this to work on your computer.
# We don't need the file type (png) here.
plt.savefig('C:/Users/Jiwon Lee/github/4530/abs_vs_conc_image')
plt.show()
print(intercept)
print(slope)

# calculate absorbance
# A = ebc
# e = A/bc
