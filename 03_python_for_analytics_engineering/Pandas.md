# Pandas

## Overview

Pandas is a Python library designed for data manipulation and analysis.

It provides powerful data structures and functions for working with structured data such as:

- CSV files
- Excel files
- Databases
- APIs
- Parquet files

Pandas is one of the most widely used tools in:

- Data Analytics
- Data Science
- Analytics Engineering
- Data Engineering

---

# Why Pandas Matters in Analytics

Pandas allows analysts and engineers to:

- Load datasets
- Explore data
- Clean data
- Transform data
- Aggregate information
- Prepare data for modeling

Typical workflow:

```
Raw Data

    ↓

Pandas Processing

    ↓

Clean Dataset

    ↓

Analysis / Visualization / Storage
```

---

# Installing Pandas

Install using pip:

```bash
pip install pandas
```

Import:

```python
import pandas as pd
```

The common convention is:

```python
pd
```

as the alias.

---

# Core Pandas Data Structures

Pandas has two main structures:

```
Series

DataFrame
```

---

# Series

A Series is a one-dimensional labeled array.

Example:

```python
import pandas as pd

sales = pd.Series(
[100,200,300]
)

print(sales)
```

Output:

```
0    100
1    200
2    300
```

---

# DataFrame

A DataFrame is a two-dimensional table.

It is similar to:

- SQL tables
- Excel spreadsheets
- Database views

Example:

```python
data = {

"Product": [
"Laptop",
"Phone"
],

"Price": [
1000,
500
]

}

df = pd.DataFrame(data)
```

Result:

|Product|Price|
|-|-|
|Laptop|1000|
|Phone|500|

---

# Loading Data

## Reading CSV Files

```python
df = pd.read_csv(
"sales.csv"
)
```

---

## Reading Excel Files

```python
df = pd.read_excel(
"sales.xlsx"
)
```

---

## Reading JSON

```python
df = pd.read_json(
"data.json"
)
```

---

## Reading SQL Data

Using SQLAlchemy:

```python
import pandas as pd

df = pd.read_sql(

query,

connection

)
```

---

# Exploring Data

## View First Rows

```python
df.head()
```

Default:

```
First 5 rows
```

---

## View Last Rows

```python
df.tail()
```

---

## Dataset Shape

```python
df.shape
```

Returns:

```
(rows, columns)
```

Example:

```
(10000, 15)
```

---

## Column Information

```python
df.info()
```

Shows:

- Column names
- Data types
- Missing values

---

## Statistical Summary

```python
df.describe()
```

Provides:

- Count
- Mean
- Standard deviation
- Min
- Max

---

# Selecting Data

## Select Column

```python
df["Revenue"]
```

---

## Select Multiple Columns

```python
df[
[
"Product",
"Revenue"
]
]
```

---

## Select Rows

Using position:

```python
df.iloc[0]
```

---

Using labels:

```python
df.loc[0]
```

---

# Filtering Data

Example:

Find expensive products:

```python
df[
df["Price"] > 500
]
```

---

Multiple conditions:

```python
df[
(df["Price"] > 500)

&

(df["Category"]=="Laptop")
]
```

---

# Sorting Data

Sort ascending:

```python
df.sort_values(
"Revenue"
)
```

---

Sort descending:

```python
df.sort_values(

"Revenue",

ascending=False

)
```

---

# Adding Columns

Create new column:

```python
df["Profit"] = (

df["Revenue"]

-

df["Cost"]

)
```

---

# Removing Columns

```python
df.drop(

"Column_Name",

axis=1

)
```

---

Remove permanently:

```python
df.drop(

"Column_Name",

axis=1,

inplace=True

)
```

---

# Handling Missing Data

## Detect Missing Values

```python
df.isnull()
```

---

Count missing values:

```python
df.isnull().sum()
```

---

# Removing Missing Values

```python
df.dropna()
```

---

# Filling Missing Values

Example:

Replace missing values with zero:

```python
df.fillna(0)
```

---

Example:

Replace missing salary with average:

```python
df["Salary"].fillna(

df["Salary"].mean()

)
```

---

# Removing Duplicates

Check duplicates:

```python
df.duplicated()
```

---

Remove duplicates:

```python
df.drop_duplicates()
```

---

# Data Type Conversion

Check types:

```python
df.dtypes
```

---

Convert:

```python
df["Date"] = pd.to_datetime(

df["Date"]

)
```

---

Convert numeric values:

```python
df["Revenue"] = pd.to_numeric(

df["Revenue"]

)
```

---

# Grouping Data

Similar to SQL GROUP BY.

SQL:

```sql
SELECT

category,

SUM(revenue)

FROM sales

GROUP BY category;
```

Pandas:

```python
df.groupby(

"Category"

)[

"Revenue"

].sum()
```

---

# Aggregations

Multiple aggregations:

```python
df.groupby(

"Category"

).agg(

{

"Revenue":"sum",

"Price":"mean"

}

)
```

---

# Joining DataFrames

Similar to SQL JOIN.

Example:

Customers:

```
customer_id
name
```

Orders:

```
customer_id
amount
```

---

Merge:

```python
df = customers.merge(

orders,

on="customer_id"

)
```

---

# Join Types

## Inner Join

```python
how="inner"
```

Returns matching rows.

---

## Left Join

```python
how="left"
```

Keeps all left table rows.

---

## Right Join

```python
how="right"
```

Keeps all right table rows.

---

## Outer Join

```python
how="outer"
```

Keeps everything.

---

# Concatenating DataFrames

Similar to SQL UNION.

Example:

```python
combined = pd.concat(

[

sales_2025,

sales_2026

]

)
```

---

# Working With Dates

Extract year:

```python
df["Year"] = (

df["Date"]

.dt.year

)
```

---

Extract month:

```python
df["Month"] = (

df["Date"]

.dt.month

)
```

---

# Pivot Tables

Similar to Excel pivot tables.

Example:

```python
df.pivot_table(

values="Revenue",

index="Region",

aggfunc="sum"

)
```

---

# Exporting Data

Save CSV:

```python
df.to_csv(

"output.csv",

index=False

)
```

---

Save Excel:

```python
df.to_excel(

"output.xlsx",

index=False

)
```

---

# Pandas Performance Tips

## Use Appropriate Data Types

Example:

Convert:

```
Object

↓

Category
```

---

## Avoid Loops

Bad:

```python
for row in df:
    process(row)
```

Prefer:

```python
vectorized operations
```

---

## Select Only Needed Columns

Avoid:

```python
df
```

when you need:

```python
df[
[
"sales",
"date"
]
]
```

---

## Use Chunk Processing

For large files:

```python
pd.read_csv(

"large.csv",

chunksize=10000

)
```

---

# Pandas in Analytics Engineering

Common use cases:

## Data Cleaning

Example:

```
Remove duplicates

Handle missing values
```

---

## Data Validation

Example:

```
Check schema

Check data types
```

---

## ETL Pipelines

Example:

```
Extract API Data

↓

Transform with Pandas

↓

Load Warehouse
```

---

# Pandas vs SQL

|Pandas|SQL|
|-|-|
|Works in memory|Runs in database|
|Great for complex processing|Great for querying|
|Python ecosystem|Database ecosystem|
|Good for analysis|Good for large-scale storage|

---

# Pandas vs Polars

|Pandas|Polars|
|-|-|
|Older ecosystem|Modern ecosystem|
|Very popular|High performance|
|Eager execution|Lazy execution|
|Large community|Growing adoption|

---

# Common Pandas Interview Questions

## What is Pandas?

Pandas is a Python library used for data manipulation and analysis using Series and DataFrames.

---

## Difference between Series and DataFrame?

A Series is one-dimensional, while a DataFrame is a two-dimensional table.

---

## Difference between merge and concat?

Merge combines datasets using keys like SQL JOIN, while concat stacks datasets vertically or horizontally.

---

## How do you handle missing data?

Using methods such as:

- dropna()
- fillna()
- interpolation
- default values

---

# Key Takeaway

Pandas is a foundational analytics tool.

Mastering Pandas allows you to:

```
Load Data

+

Clean Data

+

Transform Data

+

Analyze Data

+

Build Pipelines
```

For analytics engineers, Pandas bridges the gap between raw data and reliable analytical datasets.