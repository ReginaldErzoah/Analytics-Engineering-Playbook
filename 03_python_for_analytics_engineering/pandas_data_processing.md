# Pandas Data Processing for Analytics Engineering

## Overview

Pandas is one of the most important Python libraries for analytics engineering because it provides powerful tools for loading, cleaning, transforming, validating, and analyzing structured data.

In analytics engineering workflows, Pandas is commonly used during the early stages of the data lifecycle:

- Extracting data from source files
- Cleaning raw datasets
- Standardizing data formats
- Performing exploratory data analysis
- Preparing data before loading into analytical databases
- Building lightweight data transformation pipelines

In the SupportOps Intelligence Analytics project, Pandas was used during data profiling and cleaning before loading the processed customer support dataset into DuckDB.

---

# Why Pandas Matters in Analytics Engineering

Modern analytics engineering workflows often follow an ELT pattern:

```

Extract → Load → Transform → Analyze

```

Python and Pandas are especially useful before the warehouse layer.

Example workflow:

```

CSV Files
|
↓
Pandas Data Cleaning
|
↓
DuckDB Storage
|
↓
dbt Transformations
|
↓
Power BI Dashboard

````

---

# Core Pandas Concepts

## DataFrames

A DataFrame is Pandas' primary data structure.

It represents tabular data similar to:

- Excel tables
- SQL tables
- Database relations

Example:

```python
import pandas as pd

df = pd.read_csv("customers.csv")

print(df.head())
````

Example output:

| customer_id | name       | email                                   |
| ----------- | ---------- | --------------------------------------- |
| 1           | John Smith | [john@email.com](mailto:john@email.com) |

---

# Loading Data

## Reading CSV Files

```python
df = pd.read_csv(
    "customer_support_tickets.csv"
)
```

Common parameters:

```python
df = pd.read_csv(
    "file.csv",
    encoding="utf-8",
    low_memory=False
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

## Reading Parquet Files

Parquet is a columnar storage format commonly used in modern data platforms.

```python
df = pd.read_parquet(
    "tickets.parquet"
)
```

Benefits:

* Faster reading
* Smaller storage size
* Better analytics performance

---

# Data Inspection

Before transforming data, always understand the dataset.

## Preview Data

```python
df.head()
```

View bottom rows:

```python
df.tail()
```

---

## Dataset Information

```python
df.info()
```

Shows:

* Column names
* Data types
* Missing values
* Memory usage

---

## Statistical Summary

```python
df.describe()
```

Useful for:

* Detecting outliers
* Understanding distributions
* Checking numerical ranges

---

# Data Cleaning

Data cleaning is one of the most important analytics engineering responsibilities.

Common cleaning tasks:

* Removing duplicates
* Handling missing values
* Fixing incorrect data types
* Standardizing categories
* Removing invalid records

---

# Handling Missing Values

## Detect Missing Values

```python
df.isnull().sum()
```

Example:

```
customer_email    25
resolution_time    5
```

---

## Removing Missing Records

```python
df.dropna()
```

---

## Filling Missing Values

```python
df["priority"].fillna(
    "Unknown",
    inplace=True
)
```

---

# Removing Duplicates

Check duplicates:

```python
df.duplicated().sum()
```

Remove duplicates:

```python
df.drop_duplicates(
    inplace=True
)
```

In analytics pipelines, duplicate records can create:

* Incorrect KPIs
* Duplicate customers
* Incorrect revenue calculations

---

# Data Type Management

Correct data types are critical.

Check types:

```python
df.dtypes
```

---

Convert dates:

```python
df["submission_date"] = pd.to_datetime(
    df["submission_date"]
)
```

Convert numbers:

```python
df["resolution_hours"] = (
    df["resolution_hours"]
    .astype(float)
)
```

---

# Data Transformation

## Creating New Columns

Example:

```python
df["resolution_days"] = (
    df["resolution_hours"] / 24
)
```

---

## Conditional Logic

Similar to SQL CASE statements:

```python
df["sla_status"] = (
    df["resolution_hours"]
    .apply(
        lambda x:
        "Met SLA"
        if x <= 48
        else "Missed SLA"
    )
)
```

Equivalent SQL:

```sql
CASE
    WHEN resolution_hours <= 48
    THEN 'Met SLA'
    ELSE 'Missed SLA'
END
```

---

# Filtering Data

Pandas filtering is similar to SQL WHERE clauses.

SQL:

```sql
SELECT *
FROM tickets
WHERE priority='High';
```

Pandas:

```python
high_priority = df[
    df["priority"] == "High"
]
```

---

# Aggregations

Similar to SQL GROUP BY.

SQL:

```sql
SELECT
priority,
COUNT(*)
FROM tickets
GROUP BY priority;
```

Pandas:

```python
df.groupby(
    "priority"
).size()
```

---

# Joining Data

Similar to SQL JOIN operations.

Example:

```python
merged = customers.merge(
    tickets,
    on="customer_id",
    how="left"
)
```

Supported joins:

* inner
* left
* right
* outer

---

# Exporting Data

Save cleaned datasets:

```python
df.to_csv(
    "cleaned_data.csv",
    index=False
)
```

Save Parquet:

```python
df.to_parquet(
    "cleaned_data.parquet"
)
```

---

# Pandas in the SupportOps Intelligence Project

Pandas was used for:

## Data Profiling

Notebook:

```
notebooks/
01_data_profiling.ipynb
```

Activities:

* Dataset exploration
* Missing value analysis
* Duplicate detection
* Column inspection

## Data Cleaning

Notebook:

```
notebooks/
02_data_cleaning.ipynb
```

Activities:

* Data type correction
* Standardization
* Cleaning inconsistent values
* Exporting cleaned CSV

## Data Pipeline

Workflow:

```
Raw CSV
 |
 ↓
Pandas Cleaning
 |
 ↓
Clean CSV
 |
 ↓
DuckDB Loading
 |
 ↓
dbt Transformation
 |
 ↓
Power BI
```

---

# Best Practices

## 1. Keep Raw Data Untouched

Never modify source files.

Recommended:

```
data/

raw/
    original_file.csv

cleaned/
    cleaned_file.csv
```

---

## 2. Use Functions Instead of Long Scripts

Avoid:

```python
100 lines of transformation code
```

Prefer:

```python
def clean_customers(df):
    return df.drop_duplicates()
```

---

## 3. Validate After Transformations

Example:

```python
assert df["customer_id"].notnull().all()
```

---

## 4. Document Transformations

Every transformation should answer:

* Why was this change needed?
* What business problem does it solve?
* How does it affect downstream analytics?

---

# Topics to Master

To become strong in analytics engineering with Pandas, learn:

* DataFrame operations
* Vectorized transformations
* GroupBy operations
* Merge and joins
* Date/time manipulation
* Missing data strategies
* Data validation
* Performance optimization
* Working with large datasets
* Integration with databases

---

# Recommended Resources

## Documentation

* Pandas Official Documentation
  [https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)

---

## Books

### Python for Data Analysis

Author:
Wes McKinney

Focus:

* Pandas fundamentals
* Data cleaning
* Data manipulation
* Real-world analytics workflows

---

## Courses

### Data Analysis with Pandas and Python

Author:
Boris Paskhaver

Platform:
Udemy

---

## YouTube

### Corey Schafer - Pandas Tutorials

[https://www.youtube.com/@coreyms](https://www.youtube.com/@coreyms)

Excellent for:

* Beginners
* Practical examples
* Data manipulation concepts

---

# Summary

Pandas is the bridge between raw data and analytical systems.

A strong analytics engineer should be comfortable using Pandas to:

1. Understand incoming datasets
2. Clean unreliable data
3. Create reusable transformations
4. Validate data quality
5. Prepare datasets for analytical databases

In combination with SQL, DuckDB, dbt, and BI tools, Pandas forms a complete analytics engineering workflow.

