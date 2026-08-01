# SQL Query Optimization

## Overview

SQL query optimization is the process of improving query performance while maintaining accurate results.

For Analytics Engineers, query optimization is important because inefficient SQL can cause:

- Slow dashboards
- Expensive warehouse usage
- Long-running data pipelines
- Delayed business decisions

Optimization focuses on:

- Writing efficient SQL
- Understanding query execution
- Reducing unnecessary data processing
- Designing scalable analytical models

---

# 1. How SQL Queries Execute

A simplified SQL execution flow:

```
FROM
 ↓
JOIN
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT
```

Understanding this helps identify performance issues.

Example:

```sql
SELECT

customer_id,

SUM(amount)

FROM orders

WHERE order_date >= '2026-01-01'

GROUP BY customer_id;
```

The database first:

1. Reads the orders table
2. Filters rows
3. Groups customers
4. Calculates totals

---

# 2. Select Only Required Columns

## Bad Practice

```sql
SELECT *

FROM tickets;
```

Problems:

- Reads unnecessary data
- Increases memory usage
- Slows transformations

---

## Better Practice

```sql
SELECT

ticket_id,

customer_id,

status,

created_at

FROM tickets;
```

Benefits:

- Faster queries
- Lower storage costs
- Cleaner models

---

# 3. Filter Data Early

Filtering before joins reduces the amount of data processed.

## Inefficient

```sql
SELECT *

FROM orders o

JOIN customers c

ON o.customer_id = c.customer_id

WHERE o.order_date >= '2026-01-01';
```

---

## Better

```sql
WITH filtered_orders AS (

SELECT *

FROM orders

WHERE order_date >= '2026-01-01'

)

SELECT *

FROM filtered_orders o

JOIN customers c

ON o.customer_id = c.customer_id;
```

---

# 4. Avoid SELECT DISTINCT When Possible

DISTINCT removes duplicate rows.

Example:

```sql
SELECT DISTINCT

customer_id

FROM orders;
```

However:

- It requires additional processing
- It can hide data quality issues

Before using DISTINCT, investigate:

- Duplicate records
- Incorrect joins
- Source system problems

---

# 5. Optimize Joins

Joins are one of the biggest causes of slow queries.

---

## Avoid Joining Unnecessary Tables

Bad:

```sql
SELECT *

FROM orders

JOIN customers

JOIN products

JOIN payments

JOIN employees;
```

Only join tables required for the analysis.

---

## Join Using Keys

Preferred:

```sql
SELECT *

FROM orders o

JOIN customers c

ON o.customer_id = c.customer_id;
```

Avoid:

```sql
ON LOWER(o.email)=LOWER(c.email)
```

because transformations during joins can slow execution.

---

# 6. Understand Join Types

## INNER JOIN

Returns matching records.

```sql
SELECT *

FROM tickets t

INNER JOIN customers c

ON t.customer_id=c.customer_id;
```

---

## LEFT JOIN

Keeps all records from the left table.

Common in analytics:

```sql
SELECT *

FROM customers c

LEFT JOIN tickets t

ON c.customer_id=t.customer_id;
```

Example:

Find customers without tickets:

```sql
WHERE t.ticket_id IS NULL
```

---

# 7. Avoid Functions on Filter Columns

## Slow

```sql
WHERE YEAR(order_date)=2026
```

The database must calculate YEAR for every row.

---

## Better

```sql
WHERE order_date >= '2026-01-01'

AND order_date < '2027-01-01';
```

---

# 8. Use Appropriate Aggregations

Avoid unnecessary calculations.

Bad:

```sql
SELECT

customer_id,

COUNT(*)

FROM orders

GROUP BY customer_id

ORDER BY COUNT(*) DESC;
```

Better:

```sql
WITH customer_orders AS (

SELECT

customer_id,

COUNT(*) AS orders

FROM orders

GROUP BY customer_id

)

SELECT *

FROM customer_orders

ORDER BY orders DESC;
```

Benefits:

- Easier debugging
- Reusable logic

---

# 9. Optimize Window Functions

Window functions can be expensive.

Example:

```sql
ROW_NUMBER()

OVER(

PARTITION BY customer_id

ORDER BY created_at

)
```

Optimization:

- Reduce rows before applying window functions
- Select only required columns

---

# 10. Avoid Repeated Calculations

## Inefficient

```sql
SELECT

sales * tax_rate,

sales * tax_rate * discount

FROM orders;
```

---

## Better

```sql
WITH calculations AS (

SELECT

sales * tax_rate AS tax

FROM orders

)

SELECT *

FROM calculations;
```

---

# 11. Use Incremental Processing

Analytics pipelines often process millions of rows.

Instead of rebuilding everything:

```
Full Refresh

10 million rows every day
```

Use:

```
Incremental Load

Only new/changed rows
```

Example:

```sql
WHERE updated_at >

(
SELECT MAX(updated_at)

FROM target_table
)
```

Used heavily in:

- dbt models
- Data warehouses
- Production pipelines

---

# 12. Query Optimization in dbt

dbt models should be designed for performance.

Example:

```sql
{{ config(
    materialized='table'
) }}
```

Creates a physical table.

Useful for:

- Frequently used dashboards
- Large transformations

---

View:

```sql
{{ config(
    materialized='view'
) }}
```

Useful for:

- Lightweight models
- Rapid development

---

Incremental:

```sql
{{ config(
    materialized='incremental'
) }}
```

Useful for:

- Large datasets
- Daily pipelines

---

# 13. Data Warehouse Optimization

Modern warehouses optimize queries using:

## Partitioning

Splitting data by a column.

Example:

Partition by:

```
date
```

Instead of scanning:

```
2020-2026
```

Query scans:

```
Only January 2026
```

---

## Clustering

Organizing similar data together.

Example:

Cluster by:

```
customer_id
```

Useful for:

- Customer analysis
- Filtering

---

# 14. Query Execution Plans

Databases provide execution plans.

Example:

```sql
EXPLAIN

SELECT *

FROM orders;
```

Shows:

- Table scans
- Join operations
- Sorting
- Aggregations

Look for:

- Full table scans
- Expensive joins
- Large intermediate datasets

---

# 15. Indexing

Indexes improve lookup speed.

Example:

```sql
CREATE INDEX idx_customer

ON orders(customer_id);
```

Useful for:

- Transactional databases
- Frequent lookups

Less important in:

- Columnar analytical warehouses

---

# 16. Avoid Data Explosion From Joins

A common analytics problem.

Example:

Customers:

|customer_id|
|-|
|1|

Orders:

|customer_id|order|
|-|-|
|1|A|
|1|B|

Joining:

```
Customer × Orders

Creates multiple rows
```

Always validate:

- Join keys
- Relationship cardinality

---

# 17. SQL Optimization Checklist

Before deploying a query:

## Columns

☑ Select only required columns

## Filters

☑ Filter early

## Joins

☑ Validate join keys

☑ Avoid unnecessary joins

## Aggregations

☑ Aggregate only required data

## Windows

☑ Reduce data before window functions

## Production

☑ Test execution time

☑ Review query plan

---

# 18. Analytics Engineering Example

A slow dashboard query:

```
Raw Tickets Table

50 million rows

        ↓

Complex joins

        ↓

Multiple calculations

        ↓

Power BI Dashboard
```

Better architecture:

```
Raw Data

        ↓

dbt Staging Models

        ↓

Intermediate Transformations

        ↓

Aggregated Mart Tables

        ↓

Power BI Dashboard
```

Benefits:

- Faster dashboards
- Cleaner logic
- Easier maintenance
- Reliable metrics

---

# Interview Questions

## Q1. How do you optimize a slow SQL query?

Answer:

1. Review execution plan
2. Identify expensive operations
3. Reduce scanned data
4. Optimize joins
5. Remove unnecessary calculations
6. Add appropriate indexing or partitioning

---

## Q2. Why is SELECT * discouraged?

Because it:

- Reads unnecessary columns
- Increases processing
- Makes models less stable

---

## Q3. What causes duplicate rows after joins?

Usually:

- Incorrect join keys
- Many-to-many relationships
- Missing filtering conditions

---

## Q4. How do you optimize analytics dashboards?

Use:

- Pre-aggregated tables
- Efficient SQL models
- Incremental pipelines
- Proper data modeling

---

# Key Takeaway

SQL optimization is essential for building scalable analytics systems.

A strong Analytics Engineer does not only write SQL that works.

They write SQL that:

✅ Runs efficiently  
✅ Scales with data growth  
✅ Produces trusted metrics  
✅ Supports reliable dashboards  