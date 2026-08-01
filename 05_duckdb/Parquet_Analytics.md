# Parquet Analytics With DuckDB

## Overview

Apache Parquet is a columnar storage file format designed for efficient analytical processing.

It is widely used in modern data platforms because it provides:

- Faster analytical queries
- Lower storage requirements
- Better compression
- Efficient data transfer

DuckDB works extremely well with Parquet files because both are optimized for analytical workloads.

---

# What Is Parquet?

Parquet is a file format that stores data by columns rather than rows.

Traditional CSV:

```
Row-based storage

Customer | Product | Revenue
```

Parquet:

```
Column-based storage

Customer Column

Product Column

Revenue Column
```

---

# Row Storage vs Column Storage

## Row-Oriented Storage

Example:

```
Customer 1
Name: John
Country: Ghana
Revenue: 500

Customer 2
Name: Sarah
Country: Kenya
Revenue: 700
```

Good for:

- Transaction systems
- Individual record lookups

Examples:

- PostgreSQL
- MySQL

---

## Column-Oriented Storage

Example:

```
Customer Column:

John
Sarah


Revenue Column:

500
700
```

Good for:

- Aggregations
- Analytics
- Reporting

Examples:

- Parquet
- Data warehouses

---

# Why Parquet Is Important in Analytics Engineering

Modern analytics workflows often use:

```
Data Sources

      ↓

Parquet Files

      ↓

Warehouse / DuckDB

      ↓

dbt Models

      ↓

BI Dashboard
```

---

# Advantages of Parquet

## 1. Columnar Storage

Only required columns are scanned.

Example:

Query:

```sql
SELECT

revenue

FROM sales;
```

DuckDB only reads:

```
revenue column
```

instead of the entire dataset.

---

# 2. Compression

Parquet compresses similar values efficiently.

Example:

Country column:

```
Ghana
Ghana
Ghana
Ghana
```

can be compressed significantly.

---

# 3. Schema Storage

Parquet stores metadata about:

- Column names
- Data types
- File structure

Example:

```
customer_id → INTEGER

revenue → DOUBLE

date → DATE
```

---

# 4. Faster Query Performance

Because of:

- Column pruning
- Compression
- Metadata filtering

---

# Creating Parquet Files

## From CSV Using DuckDB

Example:

```sql
COPY (

SELECT *

FROM read_csv_auto(
'sales.csv'
)

)

TO 'sales.parquet'

(FORMAT PARQUET);
```

---

Workflow:

```
sales.csv

↓

sales.parquet
```

---

# Reading Parquet Files

DuckDB can query Parquet directly.

Example:

```sql
SELECT *

FROM read_parquet(
'sales.parquet'
);
```

---

# Selecting Columns

Instead of:

```sql
SELECT *

FROM read_parquet(
'sales.parquet'
);
```

Use:

```sql
SELECT

customer_id,

revenue

FROM read_parquet(
'sales.parquet'
);
```

This improves performance.

---

# Querying Multiple Parquet Files

DuckDB supports reading multiple files.

Example:

```sql
SELECT *

FROM read_parquet(
'data/*.parquet'
);
```

---

Example folder:

```
data/

sales_2024.parquet

sales_2025.parquet

sales_2026.parquet
```

DuckDB combines them.

---

# Partitioned Parquet Data

Large datasets are often partitioned.

Example:

```
sales/

    year=2024/

        data.parquet

    year=2025/

        data.parquet
```

---

Query:

```sql
SELECT *

FROM read_parquet(
'sales/*/*.parquet'
);
```

---

# Partition Pruning

DuckDB can avoid scanning unnecessary files.

Example:

Query:

```sql
SELECT *

FROM sales

WHERE year = 2025;
```

DuckDB reads:

```
year=2025
```

and ignores:

```
year=2024
```

---

# Parquet With Python

Example:

```python
import duckdb


result = duckdb.sql(
"""
SELECT *

FROM read_parquet(
'sales.parquet'
)
"""
)

print(result)
```

---

# Parquet With Pandas

Convert DataFrame:

```python
import pandas as pd


df = pd.read_csv(
"sales.csv"
)


df.to_parquet(
"sales.parquet"
)
```

---

# Parquet and Analytics Engineering

Parquet is commonly used for:

## Data Lakes

Example:

```
Raw Data

↓

Parquet Files

↓

Analytics Engine
```

---

## Data Transformation

Example:

```
Raw Parquet

↓

DuckDB

↓

dbt Models
```

---

## Data Exchange

Parquet is used between:

- Python
- Spark
- DuckDB
- Cloud warehouses

---

# Parquet vs CSV

|Feature|Parquet|CSV|
|-|-|-|
|Storage|Columnar|Text-based|
|Compression|Excellent|Limited|
|Schema|Stored|Not stored|
|Query Speed|Fast|Slower|
|Analytics Workloads|Excellent|Limited|
|Human Readable|No|Yes|

---

# Parquet vs Database Tables

|Feature|Parquet|Database Table|
|-|-|-|
|Storage|Files|Managed storage|
|Schema|Embedded|Database controlled|
|Query Engine|External|Built-in|
|Best Use|Analytics|Applications + warehouses|

---

# Example: Customer Support Analytics

Raw data:

```
tickets.csv
```

Convert:

```
tickets.parquet
```

Query:

```sql
SELECT

priority,

COUNT(*) AS tickets

FROM read_parquet(
'tickets.parquet'
)

GROUP BY priority;
```

Result:

```
High       5000

Medium     12000

Low        8000
```

---

# Optimizing Parquet Performance

## 1. Choose Good File Sizes

Avoid:

Too many tiny files:

```
100,000 files
```

Prefer:

```
larger optimized files
```

---

## 2. Partition Large Datasets

Example:

```
year

month

country
```

---

## 3. Store Correct Data Types

Avoid:

```
Everything as text
```

Use:

```
INTEGER

DATE

BOOLEAN

DOUBLE
```

---

## 4. Compress Data

Common compression methods:

- Snappy
- ZSTD
- GZIP

---

# Parquet in Modern Data Stack

Example:

```
APIs

↓

Python Pipeline

↓

Parquet Data Lake

↓

DuckDB / Warehouse

↓

dbt

↓

Power BI
```

---

# Interview Questions

## What is Parquet?

Parquet is a columnar storage file format optimized for analytical workloads.

---

## Why is Parquet faster than CSV?

Because it supports column pruning, compression, and metadata storage.

---

## Why is Parquet used in data lakes?

Because it provides efficient storage and fast analytical processing.

---

## How does DuckDB work with Parquet?

DuckDB can directly query, transform, and create Parquet files using SQL.

---

# Key Takeaway

Parquet is one of the most important formats in modern analytics engineering.

Combined with DuckDB:

```
Parquet

+

DuckDB

+

SQL

+

dbt

=

Lightweight Analytics Platform
```

This combination enables engineers to build scalable analytical workflows locally before moving to cloud platforms.