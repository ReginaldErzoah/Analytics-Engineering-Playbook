# SQL Analytics With DuckDB

## Overview

DuckDB is designed primarily for analytical workloads, making it a powerful tool for performing SQL-based analysis.

Analytics engineers use DuckDB to:

- Explore datasets
- Transform raw data
- Build analytical models
- Generate business metrics
- Prepare data for BI tools

This makes DuckDB a valuable component in modern analytics engineering workflows.

---

# Analytical SQL Workflow

A typical analytics workflow:

```
Raw Data

    ↓

Data Exploration

    ↓

Data Cleaning

    ↓

Transformation

    ↓

Aggregation

    ↓

Business Metrics

    ↓

Dashboard
```

---

# Loading Analytical Data

Example dataset:

```
sales.csv
```

Columns:

|Column|Description|
|-|-|
|order_id|Unique order identifier|
|customer_id|Customer reference|
|product_id|Product reference|
|quantity|Items purchased|
|price|Order amount|
|order_date|Transaction date|

---

Load into DuckDB:

```sql
CREATE TABLE sales AS

SELECT *

FROM read_csv_auto(
'sales.csv'
);
```

---

# Data Exploration Queries

Before analysis, understand the dataset.

---

## View Sample Records

```sql
SELECT *

FROM sales

LIMIT 10;
```

---

## Count Records

```sql
SELECT

COUNT(*) AS total_records

FROM sales;
```

---

## Check Columns

```sql
DESCRIBE sales;
```

---

# Filtering Data

## WHERE Clause

Example:

Find high-value orders:

```sql
SELECT *

FROM sales

WHERE price > 500;
```

---

## Multiple Conditions

```sql
SELECT *

FROM sales

WHERE price > 500

AND quantity >= 2;
```

---

# Sorting Data

Example:

Highest revenue orders:

```sql
SELECT *

FROM sales

ORDER BY price DESC;
```

---

# Aggregation Analytics

Aggregations summarize large datasets.

Common functions:

- COUNT
- SUM
- AVG
- MIN
- MAX

---

# Total Sales

```sql
SELECT

SUM(price) AS total_sales

FROM sales;
```

---

# Average Order Value

```sql
SELECT

AVG(price) AS average_order_value

FROM sales;
```

---

# Number of Orders

```sql
SELECT

COUNT(order_id)

FROM sales;
```

---

# GROUP BY Analytics

GROUP BY creates summaries by category.

---

Example:

Sales by product:

```sql
SELECT

product_id,

SUM(price) AS revenue

FROM sales

GROUP BY product_id;
```

---

Result:

|product_id|revenue|
|-|-|
|101|50000|
|102|72000|

---

# HAVING Clause

HAVING filters aggregated results.

Example:

Find products with revenue above 50,000:

```sql
SELECT

product_id,

SUM(price) AS revenue

FROM sales

GROUP BY product_id

HAVING SUM(price) > 50000;
```

---

# Customer Analytics

## Customer Revenue

Question:

> Which customers generate the most revenue?

Query:

```sql
SELECT

customer_id,

SUM(price) AS revenue

FROM sales

GROUP BY customer_id

ORDER BY revenue DESC;
```

---

# Top Customers

```sql
SELECT

customer_id,

SUM(price) AS revenue

FROM sales

GROUP BY customer_id

ORDER BY revenue DESC

LIMIT 10;
```

---

# Product Analytics

## Best Selling Products

```sql
SELECT

product_id,

SUM(quantity) AS units_sold

FROM sales

GROUP BY product_id

ORDER BY units_sold DESC;
```

---

# Time-Based Analytics

DuckDB provides strong date functions.

---

## Monthly Revenue

```sql
SELECT

DATE_TRUNC(
'month',
order_date
) AS month,

SUM(price) AS revenue

FROM sales

GROUP BY month

ORDER BY month;
```

---

## Yearly Sales

```sql
SELECT

EXTRACT(
YEAR FROM order_date
) AS year,

SUM(price)

FROM sales

GROUP BY year;
```

---

# Window Functions

Window functions calculate values across related rows.

Unlike GROUP BY:

```
GROUP BY

reduces rows
```

Window functions:

```
keep original rows
```

---

# Ranking Customers

Example:

```sql
SELECT

customer_id,

SUM(price) AS revenue,

RANK() OVER(

ORDER BY SUM(price) DESC

) AS ranking

FROM sales

GROUP BY customer_id;
```

---

Output:

|customer|revenue|ranking|
|-|-|-|
|101|90000|1|
|205|70000|2|

---

# Running Totals

Example:

```sql
SELECT

order_date,

price,

SUM(price) OVER(

ORDER BY order_date

) AS running_total

FROM sales;
```

---

# Common Table Expressions (CTEs)

CTEs improve query readability.

Example:

```sql
WITH customer_sales AS (

SELECT

customer_id,

SUM(price) revenue

FROM sales

GROUP BY customer_id

)


SELECT *

FROM customer_sales

WHERE revenue > 10000;
```

---

# Analytical Data Modeling

Analytics engineers often transform raw tables into:

```
Raw Tables

↓

Staging Models

↓

Intermediate Models

↓

Mart Tables
```

---

Example:

Raw:

```
orders
```

Transformation:

```
stg_orders
```

Final:

```
fact_sales
```

---

# Customer Support Analytics Example

Dataset:

```
tickets
```

Columns:

|Column|Description|
|-|-|
|ticket_id|Ticket identifier|
|customer_id|Customer|
|priority|Ticket priority|
|created_at|Creation time|
|resolved_at|Resolution time|

---

## Ticket Volume

```sql
SELECT

COUNT(*) AS ticket_volume

FROM tickets;
```

---

## Tickets By Priority

```sql
SELECT

priority,

COUNT(*) AS tickets

FROM tickets

GROUP BY priority;
```

---

## Average Resolution Time

```sql
SELECT

AVG(

resolved_at - created_at

)

FROM tickets;
```

---

# DuckDB Analytical Patterns

## Pattern 1: Extract → Transform → Analyze

```
CSV Files

↓

DuckDB Tables

↓

SQL Models

↓

Metrics
```

---

## Pattern 2: Query Without Loading

```
Parquet Files

↓

SQL Query

↓

Result
```

---

## Pattern 3: Build Local Warehouse

```
Raw Layer

↓

Staging Layer

↓

Mart Layer

```

---

# DuckDB With BI Tools

DuckDB outputs can feed:

- Power BI
- Tableau
- Looker Studio

Example:

```
DuckDB

↓

Clean Analytics Table

↓

Power BI Dashboard
```

---

# Performance Tips

## Select Only Required Columns

Avoid:

```sql
SELECT *
```

Use:

```sql
SELECT

customer_id,

revenue

FROM sales;
```

---

## Filter Early

Reduce data before expensive operations.

---

## Use Parquet

Benefits:

- Faster scans
- Lower storage
- Better compression

---

## Optimize Joins

Use:

- Indexed keys where applicable
- Correct join conditions
- Smaller datasets first

---

# Interview Questions

## Why is DuckDB useful for analytics?

Because it provides fast SQL analytics on local data without requiring a server.

---

## Difference between GROUP BY and Window Functions?

GROUP BY aggregates rows; window functions calculate values while keeping individual rows.

---

## How would you build an analytics workflow using DuckDB?

I would:

1. Load raw data
2. Clean and transform using SQL
3. Create analytical models
4. Validate data quality
5. Connect outputs to BI tools

---

## Why use Parquet with DuckDB?

Because Parquet provides efficient column-based storage optimized for analytical queries.

---

# Key Takeaway

DuckDB enables modern SQL analytics workflows by combining:

```
Fast Query Execution

+

Flexible File Access

+

SQL Transformations

+

Analytics Engineering Practices
```

It is an excellent tool for building and testing production-style analytics systems locally.