# Data Analysis Report on Australian Weather Database to predict Rainfall

## Introduction

The data analysis project is concerned with exploring the data set AustralianWeather2020. It is a subset of the data set weatherAUS contained in the R package rattle which we have converted to a CSV file as we have worked on the project using Python Programming. 

## Understanding the Dataset (Exploratory Analysis)

To begin with, Let’s import the required Python Libraries and load the dataset for further analysis.
```
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plot
import matplotlib.cm
import plotly.graph_objects as go
```

From Pandas Dataframe, we use the “read_csv” function to load, read and analyse the data.
```
weather=pd.read_csv("WeatherAustralia.csv")
```

Dataframe is a Pandas data structure that has a wide range of functions that allow us to understand our dataset better. Some of the functions include columns, shape, size, info, dtypes, axes, ndim, value_counts, etc.“shape” function from Pandas Dataframe gives the number of rows and columns:
```
print(weather.shape)
```
Output: (9759, 22)

The “column” function from Pandas Dataframe gives all the columns available.
```
print(weather.columns)
```
Output: Index(['Date', 'Location', 'MinTemp', 'MaxTemp', 'Rainfall', 'Evaporation', 'Sunshine', ‘WindGustSpeed', 'WindSpeed9am', 'WindSpeed3pm', 'Humidity9am', 'Humidity3pm', 'Pressure9am', ‘Pressure3pm', 'Cloud9am', 'Cloud3pm', 'Temp9am', 'Temp3pm',  'RainToday', 'RainTomorrow', 'latitude', 'longitude'],  dtype='object')

If we want to know how many times the rainfall happened during the duration of time we have considered for analysis, we can use value_counts functions.
```
weather['RainToday'].value_counts()
```
Output: No 7619<br/>
   Yes    2140<br/>
   Name: RainToday, dtype: int64

Let’s have a look at all the numerical variables that are present in the data set. We can use the following code to know the numerical variables.
```
numerical = [var for var in weather.columns if weather[var].dtype!='O']
print('There are {} numerical variables\n'.format(len(numerical)))
print('The numerical variables are :', numerical)
```
Output: There are 18 numerical variables
The numerical variables are : ['MinTemp', 'MaxTemp', 'Rainfall', 'Evaporation', 'Sunshine', 'WindGustSpeed', 'WindSpeed9am', 'WindSpeed3pm', 'Humidity9am', 'Humidity3pm', 'Pressure9am', 'Pressure3pm', 'Cloud9am', 'Cloud3pm', 'Temp9am', 'Temp3pm', 'latitude', 'longitude']                                    

Similar to the numerical variable, let’s also have a look at the ‘Categorical variables’ in the dataset.
```
categorical = [var for var in df.columns if df[var].dtype=='O']
print('There are {} categorical variables\n'.format(len(categorical)))
print('The categorical variables are :', categorical)
```
Output: There are 4 categorical variables
   The categorical variables are : ['Date', 'Location', 'RainToday', 'RainTomorrow']

From this, we can have a summary of Categorical variables:
- There is a date variable. It is denoted by the Date column.
- There are 3 categorical variables. These are given by Location, RainToday and RainTomorrow.
- There are two binary categorical variables - RainToday and RainTomorrow.
- RainTomorrow is the target variable.

Before we start plotting the data let us use the Date column to create a Year column for a time series plotting that will help us evaluate the data on a lot of factors including missing data.
```
weather['year']=pd.to_datetime(weather['Date'])  #Creates a column with datetime[ns] data type
weather['Year']=weather['Year'].dt.year                 #Creates a new column Year
weather['Month'] = weather['Month'].dt.month    #Creates a new column Month
weather[‘Day’] = weather[‘Day’].dt.day                  #Creates a new column Day
```

We can drop the Date column now as we don’t have the requirement of it anymore.
```
weather.drop('Date', axis=1, inplace = True)
```

We can check the count of data we have from each year from the new column.
We can use the value_counts() function like earlier to have a numerical understanding of this.
```
weather['Year'].value_counts()
```
Output: 2014    2480  
   2015    2366  
   2013    2324  
   2017    1470  
   2018     980  
   2016     139  
   Name: Year, dtype: int64

Here, we can clearly see that in the year 2016 we only have 139 rows associated which indicate that there is indeed a lot of missing data.
Let us have a last look at the Data before we start the Exploratory Data Analysis.

                              

## Graphical Data Analysis

Graphical Data Analysis helps to learn about the nature of the process, enables clarity of communication and provides a focus for further analysis. It is an important tool for understanding sources of variation in the data and thereby helping to better understand the process and where root causes might be.  Before proceeding with the plots and other visual representations of the data, let us have a better understanding of the Data. 
We used Jupyter Notebook for the analysis of data and carried down some initial code to understand the data and drop some unnecessary columns such as Unnamed: 0.

This task consists of investigating how strong the relationship of the individual predictors with the response is, identifying (the most) useful/informative predictors, investigating whether there are relationships between the different predictors (e.g. correlations), and visualize the spatial as well as temporal behaviour/properties of the predictors.
We can also specify this by doing a time-series plot which is one of the most common plots that will help us understand if there is any missing data and if yes at what location we can identify that visually.
Using the new column that we have created that is Year, we will be able to analyse a lot of other aspects in the data as well including “ Plotting a specific Variable over Time”

For example, let’s take Sunshine and plot the values of sunshine over the year from the data we have:
```
weather.plot(kind='scatter', x='Year', y='Sunshine')
plot.show() 
```
Output:
                       

From this plot, We can easily distinguish the other years from 2016 as it has less density comparatively. We have used Sunshine because when compared to other columns the visual representation of Sunshine data with the Respect to time gave a better picture of missing data. 
