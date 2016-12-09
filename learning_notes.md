# Data Science Immersive Notes

# Contents
* Command Line
* [Markdown](## Markdown)
* Python
* Numpy
* [Pandas](## Markdown)
  * [Test](#### __Useful Pandas Options__)


## Overall Process Map for Data Science

* Data Loading
  * CSV
  * Excel
  * APIs
  * Web Scraping
  * Databases
  * ETL
  * Streaming

* EDA (Exploratory Data Analysis)
  * Descriptive Stats
  * Data Cleaning
  * Plotting
  * Feature Selection

* Modeling
  * Regression
    * LR, MLR
    * Lasso, Ridge

  * Classification
    * KNN
    * Logistic Regression
      * Lasso, Ridge

  * Assessment
    * MSE
    * R2, Adj. R2
    * Accuracy
      * Recall, Precision



---
BELOW TO BE ORGANISED


## Command Line

Interrupting a process
Ctrl-z

Closing a process
Ctrl-c

## Typical Ipython Notebook Setup
```python
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```


## Markdown
* [Markdown Cheat Sheet #1](https://help.ghost.org/hc/en-us/articles/224410728-Markdown-Guide#headers)
* [Markdown Cheat Sheet #3](https://markable.in/file/aa191728-9dc7-11e1-91c7-984be164924a.html)


## Pandas
##### [__Documentation Link__](http://pandas.pydata.org/pandas-docs/stable/#)


##### __Indexing and Referencing__
[__Documentation Link__](http://pandas.pydata.org/pandas-docs/stable/indexing.html)
* Loc allows you to return rows based on the index
* iloc allows you to return rows based on the row numbers
    * _so in the case you have rearranged the index you can still call data by the row number_

Below is an example of how to use loc and index referencing
```python
# The below two return the same result. Loc is the recommended method and has better performance.
s.loc[1]
s[1]


# Can also use index slicing like python lists
s.loc[:1]
s[:1]

# With a dataframe we can also use loc to reference the columns
s.loc[rows,columns]

# We can also use the isin function to return all rows that match a list
df.loc[df["name".isin(["Molly", "Jason"]), :]

# which is equivelant to
df.loc[(df-"name"]=="molly")|(df["name"]=="Jason"),:]

# The iloc statement below allows you to return rows based on row number.
df[['name','age']].iloc[2:4]
```

We can also use logic statement to return new pandas dataframes
```python
[(df['name']=='Tina') & (df['name']=='Jake')] # OR STATEMENT
[(df['name']=='Tina') | (df['name']=='Jake')] # OR STATEMENT

```

We can also use conditionals with strings to find names between certain characters
```python
df.loc[(df["name"]>"B"]&(df["name"]<"M")]
```

#### Getting Information about the Data Frame
```python
#Get the shape of the dataframe
df.shape # returs a tuple with rows and columns

# All the Data Types in the data
df.dtypes()

# A count of all the columns with that data type
df.get_dtype_counts()

# Create summary statistics for each of the columns in the data  
df.describe()


```

#### Data Cleaning using Pandas

```python
# Copying Data Types
dft2=dft.copy()

# Replacing Data
df[col].replace('find_value','replace_value')

# Using the map function and a dictionary to replace values in a column
mapping = {existing_value1 : new_value2, existing_value1 : new_value2}
df[new_col] = df[old_col].map(mapping)


# Returns a count of all the values in a particular column
df.value_counts()

# Applying a function to all values in a row (axis=1) or column
df.apply(applied_function,axis=0)
```

Using apply and value_counts() function allow you return results for multiple columns
```python
df.apply(pd.Series.value_counts)
```

##### Dealing with date and time elements
```python
# Converts to a Time Delta Object
pd.to_timedelta

# Converts to a Date Time Object
pd.to_datetime()
```

##### Convert a list of data to also include all the missing elements
```python
# STEP 1 - Create a dataframe with all the dates using the min and max of the relevant column in the original data set
dates = pd.date_range(billboard_clean['date.entered'].min(), billboard_clean['date.entered'].max(), freq='D')

# STEP 2 - Pivot the original data set on the relevant date column and use the appropriate aggregate function
billboard_entered_dates = billboard_clean.pivot_table(index='date.entered', values='track', aggfunc='count')

# STEP 3 - Reindex the pivot table using the dates date frame created in step 1
billboard_entered_dates = billboard_entered_dates.reindex(index=days)

# STEP 4 - Replace the values that are not found with 0
billboard_entered_dates = billboard_entered_dates.fillna(0)

# STEP5 5 - Convert data to the right type
billboard_entered_dates.astype(int)

# STEP 6 - Can then create a cumulative sum column
billboard_entered_dates.cumsum()

```

##### Converting str data to numeric
```python
pandas.to_numeric('Series')
```

##### Useful na functions
```python
.fillna(0)
null_values = df.loc[df["column_of_interest"].isnull(),:]
non_null = df.loc[df["column_of_interest"].notnull(),:]
df_clean = df.dropna(subset=["column1", "column2"])
```


---
#### __Tips for using Pandas in real world__
* Generally you should reassign dataframes and new calculations to a new columns


#### __Useful Pandas Options__
Can set the number of columns that can be displayed
```python
pd.set_option("display.max_columns", 200)
```
