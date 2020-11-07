### Name: Christopher Phillips
### Date: November 4, 2020
### Country: United States


## Udacity Data Analysis Nanodegree Weather Trends Project:


## Exploring Weather Trends

## Summary
The goal of this project is to analyze local and global temperature data and compare the temperature trends where I live to overall global temperature trends.


## SQL Data Extraction

A Udacity workspace with access to SQL data tables was provided.

SQL Queries were used to download (CSV) files that contain yearly average temperature for ‘New York City’ and also the global average temperatures.

```
/*
SQL Code used to download global temperature data by year.
*/
SELECT *
FROM global_data;


/*
SQL Code used to download City temperature data by year.
*/
SELECT year, city, country, avg_temp
FROM city_data
WHERE city IN ('New York')
ORDER BY year;
```


## Initial Analysis

The Global data was analyzed using Python Programming Language using Jupyter Notebook. Temperatures are in Celsius.

Python Code


```python
# Importing needed libraries for analysis
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly.graph_objects as go

```


```python
# Importing data files
#Global Average Temperature data by year
global_avg_temp = pd.read_csv('global_results.csv')

#City Average Temperature data by year
city_avg_temp = pd.read_csv('New_York_results.csv')

#Global vs City Average Temperature data by year 1750-2015
glob_vs_city_avg_temp = pd.read_csv('New_York_vs_global_results.csv')
```


```python
# Global Average Temperature dataframe first 5 rows
df = global_avg_temp.head(5)
fig =  go.Figure(data=[go.Table(
    header=dict(values=list(df.columns),
                fill_color='paleturquoise',
                align='left'),
    cells=dict(values=[df.year, df.avg_temp],
               fill_color='lavender',
               align='left'))
])
fig.update_layout(width=500, height=300)
fig.write_image("images/table_global_avg_temp.png")
fig.show()
```


![Global Avg Temp Dataframe](https://github.com/cphillips103/weather-exploration/blob/main/images/table_global_avg_temp.png)


```python
# New York City Average Temperature dataframe first 5 rows
df2 = city_avg_temp.head(5)
fig2 =  go.Figure(data=[go.Table(
    header=dict(values=list(df2.columns),
                fill_color='paleturquoise',
                align='left'),
    cells=dict(values=[df2.year, df2.avg_temp, df2.country, df2.avg_temp],
               fill_color='lavender',
               align='left'))
])
fig2.update_layout(width=800, height=300)
fig2.write_image("images/table_NYC_avg_temp.png")
fig2.show()
```


![NYC Avg Temp Dataframe](https://github.com/cphillips103/weather-exploration/blob/main/images/table_NYC_avg_temp.png)


```python
# New York City vs Global Average Temperature dataframe first 5 rows
#glob_vs_city_avg_temp.head(5)

df3 = glob_vs_city_avg_temp.head(5)
fig3 =  go.Figure(data=[go.Table(
    header=dict(values=list(df3.columns),
                fill_color='paleturquoise',
                align='left'),
    cells=dict(values=[df3.year, df3.nyc_avg_temp, df3.g_avg_temp, df3['diff']],
               fill_color='lavender',
               align='left'))
])
fig3.update_layout(width=800, height=300)
fig.write_image("images/table_NYC_vs_glob_avg_temp.png")
fig3.show()
```


![NYC vs Global Avg Temp Dataframe](https://github.com/cphillips103/weather-exploration/blob/main/images/table_NYC_vs_glob_avg_temp.png)


```python
#Moving Averages:
#Rolling Average has been calculated to smooth out the data and
#make it easier to observe the trends when presented
#graphically.

#Rolling Averages have been calculated for every 10 years
#to each single data period.

#Python was used for calculating the moving average using
#the built in function "rolling" and "mean."

#Calculation of moving averages to smooth out the data

global_mov_avg = global_avg_temp['avg_temp'].rolling(window=10).mean()
city_mov_avg = city_avg_temp['avg_temp'].rolling(window=10).mean()
```

```python
#Line Chart for the data:
plt.plot(city_avg_temp['year'],city_mov_avg,label='New York City 10 Yr MA', color='#4b0082')
plt.plot(global_avg_temp['year'],global_mov_avg,label='Global 10 Yr MA', color='lightcoral')
plt.legend()
plt.xlabel("Year")
plt.ylabel("Temperature (°C)") 
plt.title("Average Annual Temperature for Global and New York City")
plt.savefig('images/NYCavgtemp.png', dpi=100)
plt.show()
```


![NYC vs Global Ann Temp Graph](https://github.com/cphillips103/weather-exploration/blob/main/images/gGlob_NYC_MA_temp.png)


```python
#Two charts showing raw and smooth 10 year MA data
fig, (ax1, ax2) = plt.subplots(1, 2, constrained_layout=True)
fig.set_size_inches(18.5, 8.5)
fig.suptitle('Average Annual Temperatures Global and New York City')


ax1.plot(global_avg_temp['year'],global_avg_temp['avg_temp'],label='Global',color='lightcoral')
ax1.plot(city_avg_temp['year'],city_avg_temp['avg_temp'],label='New York City', color='#4b0082')

ax2.plot(global_avg_temp['year'],global_mov_avg,label='Global 10 Yr MA')
ax2.plot(city_avg_temp['year'],city_mov_avg,label='New York City 10 Yr MA')


ax1.legend()
ax1.set_xlabel("Year")
ax1.set_ylabel("Temperature (°C)") 
ax1.set_title("Average Annual Temperatures Global and New York City")
ax2.legend()
ax2.set_title("10 Year Moving Averages Global and New York City")
ax2.set_xlabel("Year")
ax2.set_ylabel("Temperature (°C)") 
plt.savefig('images/AvgAnnTempMA.png', dpi=100)
plt.show()
```

![NYC vs Global Ann Temp Graph](https://github.com/cphillips103/weather-exploration/blob/main/images/Raw_MA_Temp.png)


```
#Observations:

*Looking at the initial charts, they show that average annual
temperatures have been rising over the available data periods.

*The Global temperatures started to increase in the 1800s,
which coincides with the start of the industrial revolution.

*On average, it appears that increases in average temperatures have
accelerated since the 1970 period.

*On average, New York City temperatures are slightly higher
than Global temperatures.

*As with Global temperatures, New York City temperatures have
also been on the rise during the available data periods.


##Notes regarding data sets. Data provided by the Berkley Project through Kaggle.
##Some early data periods are missing, creating large variances
##in the Global average annual temperatures.
##
##Through furture research, the provided data is also for land temperatures only,
##so Global doesn't include ocean surface temperatures.
```

```python
#Detailed look at New York City average temperature data vs the globe
#Global vs New York City Average Annual Temperatures combined using excel based on the
#individual Global and New York City CSV files.

#Rolling mean created using Python built in functions "rolling" and "mean."
glob_vs_city_mov_avg = glob_vs_city_avg_temp['diff'].rolling(window=10).mean()


plt.plot(glob_vs_city_avg_temp['year'],glob_vs_city_avg_temp['diff'],label='New York City vs Global', color='#4b0082')
plt.plot(glob_vs_city_avg_temp['year'],glob_vs_city_mov_avg,label='New York City vs Global 10 Yr MA', color='lightcoral')

plt.legend()
plt.xlabel("Year")
plt.ylabel("Temperature (°C)") 
plt.title("New York City vs Global Average Annual Temperature")
plt.savefig('images/globvsNYCavgtemp.png', dpi=100)
plt.show()
```

![NYC vs Global Ann Temp Graph](https://github.com/cphillips103/weather-exploration/blob/main/images/globvsNYCavgtemp.png)


```
#Additioning Observation
##Overall, the variance between New York City reported average annual temperatures
##and the Global temperatures is within a narrow range of 1-2 degrees Celsius.
```


```
## Future thoughts:
It would be interesting to look at seasonality variances and ocean surface temperatures
vs Global land and total Global temperatures.
```


```
#Credits:
#Global and New York City Average Annual Temperatures from Udacity via Kaggle and Berkeley Earth:
#        "https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data"
```


```
#Ploty charting documentation: "https://plotly.com/python/"
#My GitHub Repository "https://github.com/cphillips103/weather-exploration"
#Udacity Data Analyst Program: "https://www.udacity.com/course/data-analyst-nanodegree--nd002"
```
