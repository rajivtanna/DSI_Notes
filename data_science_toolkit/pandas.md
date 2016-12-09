# Data Science Immersive Notes

# Contents
  * [Panda Joins](#### __Panda_Joins__)
  * [Useful Panda Operations](#### __Useful Pandas Options__)


[Cheat Sheet](cheatsheet.jpg)




## Pandas
##### [__Documentation Link__](http://pandas.pydata.org/pandas-docs/stable/#)


##### __Indexing and Referencing__
[__Documentation Link__](http://pandas.pydata.org/pandas-docs/stable/indexing.html)
* Loc allows you to return rows based on the index
* iloc allows you to return rows based on the row numbers
    * _so in the case you have rearranged the index you can still call data by the row number_

## Below is an example of how to use loc and index referencing
```
# The below two return the same result. Loc is the recommended method and has better performance.
s.loc[1]
s[1]

# Can also use index slicing like  lists
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

#### Pandas Logic

We can also use logic statement to return new pandas dataframes
```
[(df['name']=='Tina') & (df['name']=='Jake')] # AND STATEMENT
[(df['name']=='Tina') | (df['name']=='Jake')] # OR STATEMENT
```

We can also use conditionals with strings to find names between certain characters
```
df.loc[(df["name"]>"B"]&(df["name"]<"M")]
```

#### Getting Information about the Data Frame
```
#Get the shape of the dataframe
df.shape # returs a tuple with rows and columns

# All the Data Types in the data
df.dtypes()

# A count of all the columns with that data type
df.get_dtype_counts()

# Create summary statistics for each of the columns in the data  
df.describe()
```

#### Testing for integer values in a column
```
locate missing entries
i = 0
for ps in df.horsepower:
   try:
       int(ps)
   except:
       print ps, i
   i += 1
```

##### __Data Munging Tips__

```
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
```
df.apply(pd.Series.value_counts)
```

What is the purpose of dummy variable

##### Dealing with date and time elements
```
# Converts to a Time Delta Object
pd.to_timedelta(df)

# Converts to a Date Time Object
pd.to_datetime(df)
```

##### Convert a list of data to also include all the missing elements
```
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

##### Converting data to numeric and changing the type

###### Category type  - WHY?
* The category type is useful as it uses less memory
* Also help to let other functions know what columns as category data
* Also when sorting, max, min will use the logical order and not the lexical order
* Also useful in linear regression work

```
pandas.to_numeric('Series')

# Can set to a category type - used to store numerical data that is really a category
sales_funnel_df['Status'].astype("category")

# Can also pass pandas a categorical object:
raw_cat = pd.Categorical(["a","b","c","a"], categories=["b","c","d"],
                          ordered=False)
```

###### Dummy Variables
* A way of creating a dummy variable so that it can be passed into machine learning models
* Encodes categories into dummy numeric variables that can be used for linear regression
```
# First create a sample df
df = pd.DataFrame({'key': list('bbacab'), 'data1': range(6)})

# Create dummy variables
pd.get_dummies(df['key'])
```

##### Useful na functions
```
.fillna(0)
null_values = df.loc[df["column_of_interest"].isnull(),:]
non_null = df.loc[df["column_of_interest"].notnull(),:]
df_clean = df.dropna(subset=["column1", "column2"])
```

#### __Melt Function__
* Allows users to quickly convert table style data to database style data so that it can be used for further analysis

```
pd.melt(billboard_clean,id_vars=['year','artist.inverted','track','genre','date.entered','date.peaked','time','days_to_peak','days_to_peak_int','time_in_seconds'],value_vars=ranking_cols,value_name='ranking')
```


#### __Pivot Tables or Group by__
##### Pivot Tables
* Really powerful function to quickly review and analyse a dataset

Simple Example
```
pd.pivot_table(sales_funnel_df, index=["Manager", "Rep"], values=['Price'])

# Alternative Syntax
salex_funnel_df.pivot_table(index=["Manager", "Rep"], values=['Price'])

# Apply a sort on a particular column
pd.pivot_table(sales_funnel_df,index=["Name"]).sort_values(["Price"], ascending=False)

# Aggregate Function Parameter - used to add whatever aggregate functions on the data
pd.pivot_table(sales_funnel_df, index=["Manager", "Rep"], values=['Price'],
              aggfunc=[np.sum,np.mean,np.max,np.min, len])
# Can pass a dictionary to the Aggregate Function to apply different aggregations to each column
aggfunc={'Quantity': len, 'Price': np.sum}

# Margin function allows you to see a totals columns
pd.pivot_table(sales_funnel_df, index=["Manager", "Rep"], columns=['Product'],
               values=['Price'],
               aggfunc=[np.sum],
               fill_value=0,
               margins=True)
```

A more complicated pivot example
```
pd.pivot_table(sales_funnel_df, index=["Manager", "Rep"], columns=['Product'], values=['Price'],
              aggfunc=[np.sum], fill_value=0)
```

Can use query to filter the pivot table output
```
table.query('Manager == ["Debra Henley"]')
```

##### Group by
* Allows you to group by category columns and then apply aggregation functions

Simple example
```
df_group = df.groupby('Region')
df_group.sum()
```

Note the group by function returns the grouped columns as an index. Can use the reset_index function to reset_index. However this has to be done after an aggreate function has been applied to the group object.
```
df_group = df.groupby(['Region','Rep'])['Units'].mean()
df_group.reset_index()
```

Can use the .groups to iterate through the groupings:
```
df_test = df.groupby(['Region','Rep'])
for c in df_test.groups:
    print c
```

Cleaning columns headers
```
dates.columns = [' '.join(col).strip() for col in dates.columns.values]
```

#### __Panda_Joins__
* Like SQL functions
* [link to docs](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html)

##### Join Functions (Merge)
* Options of how:
  * LEFT
  * RIGHT
  * INNER
  * OUTER

```
pd.merge(df1, df2, on=['subject_id'], how='left')
```

##### Other Functions
```
# CONCAT
result = pd.concat([df_concat_1, df_concat_2])

# if index not unique best to drop
result = pd.concat([df_concat_1, df_concat_2], ignore_index=True)
```

#### __Plotting in Pandas using matplotlib__

##### Simple Bar Charts
```
df_bar.plot(kind="bar")

# Horizontal Bar + Stacked
df_bar.plot(kind="barh", stacked=True)
```

##### Scatter Plots
```
df_scatter.plot(kind="scatter", x='a', y='b')

# Multiple scatter plots
fig = plt.figure(figsize=(20,10))
ax1 = fig.add_subplot(111)
ax2 = fig.add_subplot(111)
ax3 = fig.add_subplot(111)
ax4 = fig.add_subplot(111)
df_scatter.plot(kind="scatter", x='a', y='b',ax=ax1, color='DarkBlue', label='Group 1')
df_scatter.plot(kind="scatter", x='c', y='d',ax=ax2, color='DarkRed', label='Group 2')
df_scatter.plot(kind="scatter", x='a', y='c',ax=ax3, color='DarkGreen', label='Group 3')
df_scatter.plot(kind="scatter", x='b', y='d',ax=ax4, color='Yellow', label='Group 4')
```

##### Time Series Plots

```
# Convert date into the index for the df
time_series_df.index = time_series_df['date']

# Remove the date column
del time_series_df['date']

# Simple plot and then rotate the labels.
time_series_df.plot(rot=90)

# Can also resample to get by day etc.
time_series_df.resample('D').sum().plot()

```



---
#### __Tips for using Pandas in real world__
* Generally you should reassign dataframes and new calculations to a new columns

#### __Useful Pandas Tips__
* Can set the number of columns that can be displayed
* In pandas when you assign a variable to a df it is just a pointer to that df --> need to be careful to copy dataframes when doing operations.

```
pd.set_option("display.max_columns", 200)
```
