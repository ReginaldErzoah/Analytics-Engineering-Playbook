# Data Processing With Python

## Overview

Data processing is the process of collecting, cleaning, transforming, and preparing raw data into a format suitable for analysis, reporting, and machine learning.

In analytics engineering, data processing ensures that data becomes:

- Accurate
- Consistent
- Usable
- Reliable

A typical workflow:

```
Raw Data

    ↓

Data Ingestion

    ↓

Data Cleaning

    ↓

Data Transformation

    ↓

Data Validation

    ↓

Analytics Dataset
```

---

# Importance of Data Processing

Raw data is rarely ready for analysis.

Common problems include:

- Missing values
- Duplicate records
- Incorrect data types
- Inconsistent formats
- Invalid values
- Data quality issues

Example:

Raw customer data:

|Customer|Age|Country|
|-|-|-|
|John|25|GH|
|Mary|null|ghana|
|John|25|GH|

Problems:

```
Missing age

Different country formats

Duplicate customer
```

---

# Data Processing Stages

## 1. Data Collection

The first stage is gathering data from different sources.

Sources include:

- Databases
- APIs
- CSV files
- Excel files
- Cloud storage
- Applications

Example:

```
CRM System

+

Sales Database

+

Website Events

=

Analytics Dataset
```

---

# 2. Data Loading

Python can load data from multiple sources.

Example:

```python
import pandas as pd

sales = pd.read_csv(
"sales.csv"
)
```

---

Database loading:

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
"postgresql://database"
)

df = pd.read_sql(

"SELECT * FROM sales",

engine

)
```

---

# 3. Data Exploration

Before transforming data, understand the dataset.

Important checks:

```python
df.head()

df.info()

df.describe()

df.shape
```

Questions to answer:

```
How many rows exist?

What are the columns?

What are the data types?

Are values missing?
```

---

# Data Cleaning

Data cleaning improves data quality.

Common operations:

- Removing duplicates
- Handling missing values
- Fixing data types
- Standardizing formats
- Removing invalid records

---

# Handling Missing Values

## Detect Missing Values

```python
df.isnull().sum()
```

Example output:

```
Age          50

Email        20

Revenue       5
```

---

## Remove Missing Rows

```python
df.dropna()
```

Use when:

- Missing values are insignificant
- Records cannot be recovered

---

## Fill Missing Values

Example:

```python
df["Age"] = df["Age"].fillna(

df["Age"].median()

)
```

---

Common strategies:

|Data Type|Method|
|-|-|
|Numeric|Mean/Median|
|Category|Mode|
|Date|Forward fill|
|Text|Unknown value|

---

# Removing Duplicates

Check duplicates:

```python
df.duplicated().sum()
```

---

Remove duplicates:

```python
df = df.drop_duplicates()
```

---

Example:

Before:

```
Customer_ID

1001

1001

1002
```

After:

```
Customer_ID

1001

1002
```

---

# Data Type Cleaning

Incorrect data types cause problems.

Example:

Wrong:

```
Revenue

"$1000"

"$2000"
```

Correct:

```
Revenue

1000

2000
```

---

Convert:

```python
df["Revenue"] = pd.to_numeric(

df["Revenue"]

)
```

---

# Text Cleaning

Common problems:

```
Ghana

ghana

 GHANA
```

Standardize:

```python
df["Country"] = (

df["Country"]

.str

.lower()

)
```

---

Remove spaces:

```python
df["Name"] = (

df["Name"]

.str.strip()

)
```

---

# Date Processing

Dates are critical for analytics.

Convert:

```python
df["Order_Date"] = pd.to_datetime(

df["Order_Date"]

)
```

---

Extract components:

Year:

```python
df["Year"] = (

df["Order_Date"]

.dt.year

)
```

---

Month:

```python
df["Month"] = (

df["Order_Date"]

.dt.month

)
```

---

Quarter:

```python
df["Quarter"] = (

df["Order_Date"]

.dt.quarter

)
```

---

# Data Transformation

Transformation converts cleaned data into analytical structures.

Examples:

- Creating calculated fields
- Aggregating data
- Reshaping data
- Combining datasets

---

# Creating New Columns

Example:

Calculate profit:

```python
df["Profit"] = (

df["Revenue"]

-

df["Cost"]

)
```

---

Calculate margin:

```python
df["Margin"] = (

df["Profit"]

/

df["Revenue"]

)
```

---

# Filtering Data

Example:

Only completed orders:

```python
completed = df[

df["Status"]=="Completed"

]
```

---

# Aggregation

Similar to SQL GROUP BY.

Example:

Revenue by country:

```python
df.groupby(

"Country"

)[

"Revenue"

].sum()
```

---

# Reshaping Data

## Wide Format

Example:

|Customer|2025|2026|
|-|-|-|
|John|100|200|

---

## Long Format

Example:

|Customer|Year|Sales|
|-|-|-|
|John|2025|100|
|John|2026|200|

---

Convert:

```python
df.melt()
```

---

# Combining Data

## Concatenation

Used when datasets have similar structures.

Example:

```
Sales_January

+

Sales_February

=

All_Sales
```

Python:

```python
pd.concat(
[
jan,
feb
]
)
```

---

## Merge

Used when combining related datasets.

Example:

```
Customers

+

Orders
```

Python:

```python
customers.merge(

orders,

on="customer_id"

)
```

---

# Data Validation

Before using processed data, validate it.

Checks include:

## Row Count

Example:

```
Expected:

10000 rows

Received:

9998 rows
```

---

## Column Validation

Check:

```
Required columns exist
```

---

## Data Type Validation

Example:

Expected:

```
Revenue = Float
```

Received:

```
Revenue = Text
```

---

## Business Rule Validation

Example:

Invalid:

```
Quantity < 0
```

---

# Automating Data Processing

A reusable pipeline:

```python
def process_sales_data(file):

    df = pd.read_csv(file)

    df = clean_data(df)

    df = transform_data(df)

    validate_data(df)

    return df
```

---

# Data Processing In ETL Pipelines

Example architecture:

```
API

 ↓

Python Extraction

 ↓

Pandas Processing

 ↓

Data Warehouse

 ↓

dbt Transformation

 ↓

BI Dashboard
```

---

# Performance Considerations

## Avoid Loading Unnecessary Data

Bad:

```python
SELECT *
```

Better:

```sql
SELECT

customer_id,

revenue

FROM sales;
```

---

## Use Vectorized Operations

Avoid:

```python
for row in dataframe:
```

Prefer:

```python
df["profit"]

=

df["sales"]

-

df["cost"]
```

---

## Process Large Files in Chunks

Example:

```python
pd.read_csv(

"large_file.csv",

chunksize=50000

)
```

---

# Common Data Processing Challenges

## Inconsistent Data

Example:

```
USA

United States

US
```

Solution:

Create standard mappings.

---

## Missing Data

Solution:

Investigate why values are missing before filling.

---

## Duplicate Records

Solution:

Identify unique keys.

---

## Changing Schemas

Solution:

Use validation frameworks.

---

# Interview Questions

## What is data processing?

Data processing is the transformation of raw data into clean, structured, and usable information.

---

## Why is data cleaning important?

Because poor-quality data leads to inaccurate analysis and incorrect business decisions.

---

## Difference between transformation and cleaning?

Cleaning fixes data quality issues, while transformation converts data into a useful analytical structure.

---

## How do you handle large datasets in Python?

Using techniques such as:

- Chunk processing
- Efficient data types
- Vectorized operations
- Distributed processing tools

---

# Key Takeaway

Effective data processing creates the foundation for reliable analytics.

A strong processing workflow combines:

```
Data Cleaning

+

Transformation

+

Validation

+

Automation

=

Trusted Data
```

Python provides the flexibility needed to build scalable analytical data workflows.