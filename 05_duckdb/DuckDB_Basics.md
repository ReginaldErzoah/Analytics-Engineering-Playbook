# DuckDB Basics

## Overview

DuckDB is an analytical database designed for fast data processing directly on local machines.

Unlike traditional databases that require a running server, DuckDB can run inside:

- Python applications
- Data analysis notebooks
- Command line environments
- Analytics workflows

It provides SQL capabilities while being optimized for analytical queries.

---

# Installing DuckDB

## Python Installation

Install using pip:

```bash
pip install duckdb
```

Verify installation:

```python
import duckdb

print(duckdb.__version__)
```

---

# DuckDB Command Line

Start DuckDB:

```bash
duckdb
```

Create or open a database:

```bash
duckdb analytics.duckdb
```

This creates:

```
analytics.duckdb
```

which stores tables and metadata.

---

# Connecting With Python

Basic connection:

```python
import duckdb


connection = duckdb.connect(
"analytics.duckdb"
)


connection.sql(
"""
SELECT 'Hello DuckDB';
"""
)
```

---

# Running SQL Queries

DuckDB supports standard SQL.

Example:

```sql
SELECT

1 + 1;
```

Result:

```
2
```

---

# Creating Tables

## Create Table

```sql
CREATE TABLE customers (

customer_id INTEGER,

customer_name VARCHAR,

email VARCHAR

);
```

---

## View Tables

```sql
SHOW TABLES;
```

---

## Describe Table

```sql
DESCRIBE customers;
```

Output:

|column|type|
|-|-|
|customer_id|INTEGER|
|customer_name|VARCHAR|
|email|VARCHAR|

---

# Inserting Data

## Single Record

```sql
INSERT INTO customers

VALUES

(
1,
'John Mensah',
'john@email.com'
);
```

---

## Multiple Records

```sql
INSERT INTO customers

VALUES

(1,'John','john@email.com'),

(2,'Sarah','sarah@email.com'),

(3,'Michael','michael@email.com');
```

---

# Querying Data

Basic query:

```sql
SELECT *

FROM customers;
```

---

Selecting Columns:

```sql
SELECT

customer_name,

email

FROM customers;
```

---

Filtering Data:

```sql
SELECT *

FROM customers

WHERE customer_id = 1;
```

---

# Updating Data

Example:

```sql
UPDATE customers

SET email='new@email.com'

WHERE customer_id=1;
```

---

# Deleting Data

Example:

```sql
DELETE FROM customers

WHERE customer_id=3;
```

---

# Loading Data From Files

One of DuckDB's biggest advantages is querying files directly.

---

# CSV Files

Example:

```sql
SELECT *

FROM read_csv_auto(
'customers.csv'
);
```

DuckDB automatically detects:

- Column names
- Data types
- Formatting

---

# Parquet Files

Example:

```sql
SELECT *

FROM read_parquet(
'sales.parquet'
);
```

---

# JSON Files

Example:

```sql
SELECT *

FROM read_json_auto(
'events.json'
);
```

---

# Creating Tables From Files

Example:

```sql
CREATE TABLE sales AS

SELECT *

FROM read_csv_auto(
'sales.csv'
);
```

---

Now:

```
sales.csv

        ↓

DuckDB Table
```

---

# Data Types in DuckDB

Common types:

|Type|Example|
|-|-|
|INTEGER|100|
|BIGINT|100000|
|DOUBLE|99.50|
|VARCHAR|'Customer'|
|DATE|'2026-01-01'|
|BOOLEAN|TRUE|

---

# Working With Dates

Example:

```sql
SELECT

DATE '2026-01-01';
```

---

Extract year:

```sql
SELECT

EXTRACT(
YEAR FROM order_date
)

FROM orders;
```

---

# Aggregations

DuckDB supports standard SQL aggregations.

Example:

```sql
SELECT

COUNT(*) AS total_orders

FROM orders;
```

---

## SUM

```sql
SELECT

SUM(amount)

FROM sales;
```

---

## AVG

```sql
SELECT

AVG(amount)

FROM sales;
```

---

## GROUP BY

Example:

```sql
SELECT

product,

SUM(amount)

FROM sales

GROUP BY product;
```

---

# Joins in DuckDB

DuckDB supports:

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN

---

Example:

Customers:

```
customer_id
```

Orders:

```
customer_id
```

Query:

```sql
SELECT

c.customer_name,

o.order_id

FROM customers c

JOIN orders o

ON c.customer_id=o.customer_id;
```

---

# Creating Views

Views store reusable queries.

Example:

```sql
CREATE VIEW customer_orders AS

SELECT *

FROM orders;
```

Query:

```sql
SELECT *

FROM customer_orders;
```

---

# Exporting Data

DuckDB can export query results.

---

## Export CSV

```sql
COPY (

SELECT *

FROM customers

)

TO 'customers_output.csv'

WITH (
HEADER TRUE
);
```

---

## Export Parquet

```sql
COPY (

SELECT *

FROM sales

)

TO 'sales.parquet'

(FORMAT PARQUET);
```

---

# DuckDB Performance Features

## Vectorized Execution

Processes data in batches instead of one row at a time.

---

## Parallel Query Execution

Uses multiple CPU cores for analytical workloads.

---

## Columnar Processing

Reads only required columns.

Example:

Query:

```sql
SELECT revenue

FROM sales;
```

DuckDB only scans:

```
revenue column
```

---

# DuckDB Extensions

DuckDB supports extensions.

Example:

```sql
INSTALL httpfs;

LOAD httpfs;
```

Allows querying:

- Cloud storage
- Remote files

---

# Example Analytics Workflow

```
CSV Files

     ↓

DuckDB

     ↓

SQL Cleaning

     ↓

Analytics Tables

     ↓

Power BI Dashboard
```

---

# Example: Customer Support Analytics

Raw file:

```
tickets.csv
```

Load:

```sql
CREATE TABLE tickets AS

SELECT *

FROM read_csv_auto(
'tickets.csv'
);
```

Analyze:

```sql
SELECT

priority,

COUNT(*) AS tickets

FROM tickets

GROUP BY priority;
```

Output:

```
High       5000

Medium     12000

Low        8000
```

---

# Best Practices

## Use Parquet For Large Data

Parquet provides:

- Compression
- Faster queries
- Efficient storage

---

## Keep Transformations In SQL

SQL is easier to:

- Review
- Test
- Maintain

---

## Use DuckDB For Development

Great for:

- Learning analytics engineering
- Building prototypes
- Testing dbt models

---

## Separate Raw and Clean Data

Example:

```
raw_data

    ↓

staging

    ↓

analytics
```

---

# Interview Questions

## How do you create a DuckDB database?

```bash
duckdb database_name.duckdb
```

---

## Can DuckDB query files directly?

Yes. It can query CSV, Parquet, and JSON files without loading them first.

---

## Why is DuckDB fast?

Because it uses:

- Columnar storage
- Vectorized execution
- Parallel processing

---

## Is DuckDB a replacement for Snowflake?

Not completely.

DuckDB is excellent for local analytics and development, while cloud warehouses are designed for large-scale enterprise workloads.

---

# Key Takeaway

DuckDB provides a lightweight but powerful analytical environment.

It enables analytics engineers to practice:

```
SQL Transformations

+

Data Modeling

+

Warehouse Concepts

+

BI Analytics

```

without requiring expensive infrastructure.