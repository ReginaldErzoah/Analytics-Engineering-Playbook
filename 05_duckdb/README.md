# DuckDB

## Overview

DuckDB is an open-source analytical database designed for fast local analytics and data processing.

It is often described as:

> "The SQLite for analytics"

because it provides the simplicity of an embedded database while being optimized for analytical workloads.

DuckDB is commonly used in:

- Analytics engineering
- Data exploration
- Local data warehouses
- Data transformation workflows
- Data science projects

---

# Why DuckDB Matters

Traditional databases are often optimized for transactional workloads.

Example:

```
Insert customer record

Update account balance

Process payment
```

DuckDB is optimized for analytical workloads.

Example:

```
Analyze millions of sales records

Calculate customer metrics

Aggregate large datasets
```

---

# DuckDB Characteristics

## 1. Embedded Database

DuckDB runs inside applications.

No server required.

Example:

```
Python Application

        +

DuckDB Engine

        +

Analytics Queries
```

---

## 2. Columnar Analytics Engine

DuckDB uses column-based processing.

Example:

Sales table:

```
customer_id

product_id

amount

date
```

If calculating total sales:

DuckDB only reads:

```
amount
```

instead of scanning every column.

---

## 3. SQL Interface

DuckDB supports standard SQL.

Example:

```sql
SELECT

product,

SUM(revenue)

FROM sales

GROUP BY product;
```

---

## 4. Local Data Processing

DuckDB can query files directly.

Supported formats:

- CSV
- Parquet
- JSON
- Pandas DataFrames
- Arrow tables

---

# DuckDB in Analytics Engineering

Modern analytics workflows often look like:

```
Raw Files

(CSV, JSON, Parquet)

        ↓

DuckDB

        ↓

SQL Transformations

        ↓

Analytics Models

        ↓

BI Dashboard
```

---

# DuckDB vs Traditional Databases

|Feature|DuckDB|PostgreSQL|
|-|-|-|
|Primary Use|Analytics|Transactional applications|
|Architecture|Embedded|Client-server|
|Workload|OLAP|OLTP|
|Setup|No server required|Requires database server|
|SQL Support|Yes|Yes|
|Large Aggregations|Excellent|Good|
|Local Analytics|Excellent|Limited|

---

# DuckDB vs SQLite

DuckDB and SQLite are both embedded databases, but they serve different purposes.

|Feature|DuckDB|SQLite|
|-|-|-|
|Purpose|Analytics|Application storage|
|Optimization|OLAP|OLTP|
|Data Processing|Large analytical queries|Small transactions|
|Columnar Execution|Yes|No|
|Parallel Processing|Yes|Limited|

---

# Installing DuckDB

## Python Installation

```bash
pip install duckdb
```

---

## Command Line Installation

After installation:

```bash
duckdb
```

opens the DuckDB shell.

---

# Basic DuckDB Usage

## Create Database

```sql
CREATE TABLE customers (

customer_id INTEGER,

name VARCHAR

);
```

---

## Insert Data

```sql
INSERT INTO customers

VALUES

(1, 'John'),

(2, 'Sarah');
```

---

## Query Data

```sql
SELECT *

FROM customers;
```

---

# Querying Files Directly

One of DuckDB's strongest features is querying files without loading them first.

---

# Query CSV Files

```sql
SELECT *

FROM read_csv_auto(
'customers.csv'
);
```

---

# Query Parquet Files

```sql
SELECT *

FROM read_parquet(
'sales.parquet'
);
```

---

# Query JSON Files

```sql
SELECT *

FROM read_json_auto(
'events.json'
);
```

---

# DuckDB and Parquet

DuckDB works extremely well with Parquet files.

Workflow:

```
CSV Data

    ↓

Convert to Parquet

    ↓

Query Using DuckDB

    ↓

Build Analytics Models
```

Benefits:

- Smaller storage
- Faster queries
- Column-based processing

---

# DuckDB and Python

DuckDB integrates easily with Python.

Example:

```python
import duckdb

result = duckdb.sql(
"""
SELECT *

FROM 'sales.parquet'
"""
)

print(result)
```

---

# DuckDB with Pandas

Example:

```python
import duckdb
import pandas as pd


df = pd.read_csv(
"sales.csv"
)


result = duckdb.sql(
"""
SELECT

product,

SUM(amount)

FROM df

GROUP BY product
"""
)

print(result)
```

---

# DuckDB in Analytics Engineering Projects

Common use cases:

## Local Data Warehouse

Build warehouse-style projects without cloud infrastructure.

Example:

```
Raw Data

↓

DuckDB

↓

dbt

↓

Power BI
```

---

## Data Transformation

Perform SQL transformations locally.

Example:

```
staging tables

↓

fact tables

↓

dimension tables
```

---

## Data Exploration

Analyze large datasets without loading into a traditional database.

---

# DuckDB + dbt

DuckDB is commonly paired with dbt for local analytics engineering.

Architecture:

```
Source Files

        ↓

DuckDB Warehouse

        ↓

dbt Models

        ↓

Data Marts

        ↓

BI Tools
```

---

# Example Customer Support Analytics Stack

```
support_tickets.csv

        ↓

DuckDB

        ↓

dbt staging models

        ↓

fact_ticket_metrics

        ↓

Power BI Dashboard
```

---

# Advantages of DuckDB

## Developer Friendly

No database server required.

---

## Fast

Excellent performance for analytical queries.

---

## Portable

Database files can be moved easily.

---

## Cost Effective

Useful for:

- Learning
- Prototyping
- Small analytics workloads

---

# Limitations of DuckDB

## Not Designed for High-Concurrency Applications

Many users writing simultaneously is not its main purpose.

---

## Not a Cloud Data Warehouse Replacement

Large enterprises may still use:

- Snowflake
- BigQuery
- Redshift

---

## Limited Operational Features

It is not designed for:

- User management
- Application backends
- Transaction processing

---

# DuckDB in Modern Data Stack

Example:

```
Data Sources

     ↓

Python / Airbyte

     ↓

DuckDB

     ↓

dbt

     ↓

Power BI / Looker
```

---

# Interview Questions

## What is DuckDB?

DuckDB is an embedded analytical database optimized for OLAP workloads.

---

## Why use DuckDB?

Because it provides fast local analytical processing without requiring a database server.

---

## Difference between DuckDB and PostgreSQL?

DuckDB is optimized for analytics; PostgreSQL is optimized for transactional workloads.

---

## Why is DuckDB useful with dbt?

It provides a lightweight local warehouse environment for developing and testing analytics models.

---

# Key Takeaway

DuckDB is an important tool in modern analytics engineering because it enables:

```
Fast Local Analytics

+

SQL Transformations

+

Data Warehouse Concepts

+

Low-Cost Development
```

It allows analysts and analytics engineers to build production-style workflows without requiring expensive cloud infrastructure.