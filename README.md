# SimpleDataAnalysis

Simple data analysis with python libraries

## Python packages for Data Science

- Scientific Computing Libraries
  - **Pandas:** Data structures and tools
  - **NumPy:** Array and matrices
  - **SciPy:** Integrals, solving differential equations, optimization
- Visualization Libraries
  - **Matplotlib:** Plots and graphs
  - **Seaborn:** Plots:Heatmaps, time seris, violin plots
- Algorithmic Libraries
  - **Scikit-learn:** Machine learning: Regression, classification.
  - **Statsmodel:** Explore data, estimate statistical models, perform statistical tests

## Importing & Exporting Data

```python
import pandas as pd
url = 'url of a dataset'
# import data in a csv to a dataframe
# if dataset already has column names as the first row
df = pd.read_csv(url)

# if there is no header in the dataset, it is given default integer values
df = pd.read_csv(url, header=None)
# assign column names
df.columns = ['col1', 'col2', 'col3']

# get quick view of the first n rows
df.head(n)

# get quick view of the last n rows
df.tail(n)

# export data in a dataframe to a csv
df.to_csv('filename.csv')
```

### Data formats supported by python

| Data Format | Read            | Save        |
| ----------- | --------------- | ----------- |
| csv         | pd.read_csv()   | df.to_csv   |
| json        | pd.read_json()  | df.to_json  |
| excel       | pd.read_excel() | df.to_excel |
| sql         | pd.read_sql()   | df.to_sql   |

## Pandas

### Data Types

| Pandas type | Python type | Description            |
| ----------- | ----------- | ---------------------- |
| int64       | int         | integers               |
| float64     | float       | floating point numbers |
| bool        | bool        | boolean values         |
| object      | str         | strings                |

### Simple Pandas methods

- Check data type

```python
df.dtypes
```

- Check data type of a column

```python
df['column_name'].dtypes
```

- Check data type of a row

```python
df.iloc[0].dtypes
```

- Check data type of a cell

```python
df.iloc[0, 0].dtypes
```

- Check dataframe statistical summary

```python
df.describe()
```

- Check dataframe statistical summary and include data types

```python
df.describe(include='all')
```

- Check dataframe for a quick inspection

```python
df.info()
```

### DB API functions

Set of standardized Python functions used to interact with relational databases.

- **Connection objects:** Connect to a db abd query objects.
- **Cursor objects:** Scan through the results of a database.
  - **Methods:** cursor(), commit(), rollback(), close(), execute(), fetchone(), fetchall()

```python
from dbmodule import connect
conn = connect('databasename', 'username', 'password')
cursor = conn.cursor()
cursor.execute('SELECT * FROM table')
result = cursor.fetchall()
cursor.close()
conn.close()
```

### Dealing with missing values

Usually a missing value in a dataset appears as ?, N/A, 0 or a blank. The most common way to deal with missing values is to drop the row or column that contains the missing value.

- **Drop rows:** Drop rows with missing values
- **Drop columns:** Drop columns with missing values

#### dropna(subset = ['column name'], axis = 0, inplace = True)

axis = 0 -> drop the row
axis = 1 -> drop the column
to modify the dataframe inplace=True is necessary

```python
df.dropna()
```

- **Impute:** Replace missing values with a value
  - Average
  - Most common if average is not applicable
  - Based on other functions

#### replace(missing_value, new_value)

```python
mean = df.['column name'].mean()
df['column name'].replace(np.nan, mean)
```

### Data formatting

- Converting a column to a different data type
  - To get data types:

```python
df.dtypes()
```

- To convert data types:

```python
df['column name'] = df['column name'].astype('data type')
```

- Renaming a column name

```python
df.rename(columns = {'old name': 'new name'}, inplace = True)
```

### Data normalization

- Normalization is the process of scaling the data to a common range, in order to eliminate bias to the very big/small values.
- Normalization is important because it allows for easier comparison of data across different features.
- There are several methods for normalizing data,
  - min-max scaling: scaling the data to a range of 0 to 1
  - z-score normalization: scaling the data to a range of -1 to 1
  - log transformation:
  - square root transformation:
  - cube root transformation:

| height | weight | age |
| ------ | ------ | --- |
| 174.6  | 45     | 22  |
| 180.1  | 65     | 34  |
| 164.3  | 54     | 22  |
| ...    | ...    | ... |

```python
# feature scaling
df['height'] = df['height'] / df['height'].max()
```

| height | weight | age |
| ------ | ------ | --- |
| 0.96   | 45     | 22  |
| 1      | 65     | 34  |
| 0.91   | 54     | 22  |
| ...    | ...    | ... |

```python
# min-max scaling
df['height'] = (df['height'] - df['height'].min()) / (df['height'].max() - df['height'].min())
```

| height | weight | age |
| ------ | ------ | --- |
| 0.65   | 45     | 22  |
| 1      | 65     | 34  |
| 0      | 54     | 22  |
| ...    | ...    | ... |

```python
# z-score normalization
df['height'] = (df['height'] - df['height'].mean()) / df['height'].std()
```

### Binning

- Binning is the process of dividing the data into discrete intervals, or bins.
- Binning is useful for visualizing data and for identifying patterns.
- There are several methods for binning data,
  - _equal-width binning:_ Dividing the data into equal-width intervals
  - _equal-frequency binning:_ Dividing the data into equal-frequency intervals
  - _quantile binning:_ Dividing the data into equal-sized intervals based on the quantiles of the data
- After binning use histograms to visualize the data

| age |
| --- |
| 10  |
| 15  |
| 5   |
| 67  |
| 12  |
| 45  |
| 13  |

```python
# equal-width binning
bins = np.linspace(df['age'].min(), df['age'].max(), 5)
age_groups = ['kid','teenage','adult','elderly']
df['age_bin'] = pd.cut(df['age'], bins, labels = age_groups, include_lowest = True)
```

| age | age_bin |
| --- | ------- |
| 10  | kid     |
| 15  | teenage |
| 5   | kid     |
| 67  | elderly |
| 12  | kid     |
| 45  | adult   |
| 13  | teenage |

### Converting categorical variables into quantitative variables

Most statistical models cannot take object as input needs to be converted into quantitative values

- One-hot encoding
- Label encoding
- Ordinal encoding
- Target encoding
- Mean encoding
- Frequency encoding
- Count encoding
- Rank encoding
- Discretization
- Binning

| Car | Fuel Type |
| --- | --------- |
| A   | Gas       |
| B   | Diesel    |
| C   | Gas       |
| D   | Gas       |
| E   | Gas       |
| F   | Diesel    |

```python
# one-hot encoding
df = pd.get_dummies(df, columns = ['Fuel Type'])
```

| Car | Gas | Diesel |
| --- | --- | ------ |
| A   | 1   | 0      |
| B   | 0   | 1      |
| C   | 1   | 0      |
| D   | 1   | 0      |
| E   | 1   | 0      |
| F   | 0   | 1      |

```python
# label encoding
df['Fuel Type'] = df['Fuel Type'].map({'Gas': 0, 'Diesel': 1})
```

| Car | Gas | Diesel |
| --- | --- | ------ |
| A   | 0   | 1      |
| B   | 1   | 0      |
| C   | 0   | 1      |
| D   | 0   | 1      |
| E   | 0   | 1      |
| F   | 1   | 0      |

# Exploratory Data Analysis

Understand the underlying patterns, structures and relationships within the dataset before applying modeling and hypothesis testing.

- Understanding the data structure
- Detecting outliers and anomalies
- Uncovering patterns and relationships
- Generating hypothesis

## Descriptive statistics:

- Basic features of the data
- Short summaries and measures of the data
- describe(): generate summary statistics of data to get a quick overview of the distribution and key metrics of numerical columns

  - **count:** number of non-null observations
  - **mean:** average of the vakues
  - **std:** standard deviation
  - **25%:** first quartile
  - **50%:** median
  - **75%:** third quartile
  - **max:** maximum value

- value_counts(): count the unique values in a series. Useful to understand the distribution of the values.

- Box plot: also known as Whisker plot. Useful for identifying the outliers and understanding the spread and skeweness of the data. Visualize the distribution of data based on a five number summary
  1. Minimum
  2. First quartile(Q1)
  3. Median(Q2)
  4. Third quartile(Q3)
  5. Maximum
- Scatter plot: Good to understand the relationship between two data.
  predictor: independent variable on x-axis
  target:dependent variable on y-axis

## Grouping data

- Use groupby() function to group the data based on some criteria
- Apply some operations on the grouped data
- Use the agg() function to apply multiple operations on the grouped data
- Use the transform() function to apply operations on the grouped data and return the result as a new column
- Use the apply() function to apply a function on the grouped data and return the result as a new column
- Heatmap Visualize the relationship of a variable against multiple variables. Used to view the data in a binned form.

## Some useful methods:

- Use the pipe() function to chain multiple operations together
- Use the sort_values() function to sort the data based on a column
- Use the nlargest() function to get the top n values of a column
- Use the nsmallest() function to get the bottom n values of a column
- Use the cumsum() function to get the cumulative sum of a column
- Use the cumprod() function to get the cumulative product of a column
- Use the cummin() function to get the cumulative minimum of a column
- Use the cummax() function to get the cumulative maximum of a column
- Use the diff() function to get the difference between consecutive values of a column
- Use the shift() function to shift the values of a column by a certain number of positions
- Use the rolling() function to apply a function on a rolling window of data
- Use the expanding() function to apply a function on an expanding window of data
- Use the ewm() function to apply a function on an exponentially weighted moving average of data
- Use the resample() function to resample the data at a different frequency
- Use the interpolate() function to interpolate missing values in the data
- Use the fillna() function to fill missing values in the data
- Use the dropna() function to drop missing values in the data
- Use the isna() function to check if a value is missing
- Use the notna() function to check if a value is not missing
- Use the isnull() function to check if a value is missing
- Use the notnull() function to check if a value is not missing
- Use the isin() function to check if a value is in a list of values
- Use the notin() function to check if a value is not in a list of values
- Use the isfinite() function to check if a value is finite
- Use the isinf() function to check if a value is infinite
- Use the isnan() function to check if a value is NaN
