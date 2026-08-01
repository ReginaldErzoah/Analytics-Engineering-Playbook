# SQL Performance Optimization for Analytics Engineering

## Overview

SQL performance optimization is the process of improving query execution speed, reducing resource consumption, and designing efficient analytical systems.

Analytics engineers often work with large datasets where inefficient SQL can cause:

- Slow dashboards
- Expensive warehouse usage
- Long dbt execution times
- Poor user experience

A strong analytics engineer understands not only how to write correct SQL, but also how to write efficient SQL.

---

# Why SQL Performance Matters

A query can return the correct answer but still be inefficient.

Example:

A dashboard query takes:

```
5 seconds
```

For one user.

With:

```
1,000 users
```

The impact becomes significant.

Performance affects:

- BI dashboard loading
- Data pipeline execution
- Cloud computing costs
- Developer productivity

---

# Understanding Query Execution

A database does not simply read SQL from top to bottom.

The database engine:

1. Parses SQL
2. Creates an execution plan
3. Optimizes operations
4. Executes the query

Example:

```sql
SELECT *

FROM tickets

WHERE priority_level = 'High';
```

The database decides:

- How to scan the table
- Whether to use indexes
- Join order
- Filtering strategy

---

# Query Execution Plans

Execution plans show how databases execute queries.

Example:

DuckDB:

```sql
EXPLAIN

SELECT *

FROM tickets;
```

Output includes:

- Table scans
- Joins
- Filters
- Estimated operations

---

# Selecting Only Required Columns

## Bad Practice

```sql
SELECT *

FROM tickets;
```

Problems:

- Reads unnecessary data
- Increases memory usage
- Makes models fragile

---

## Better Practice

```sql
SELECT

ticket_id,

priority_level,

resolution_time_hours

FROM tickets;
```

Benefits:

- Faster execution
- Clearer models
- Smaller datasets

---

# Filtering Data Early

Filtering reduces the amount of data processed.

## Inefficient

```sql
SELECT *

FROM orders o

JOIN customers c

ON o.customer_id = c.customer_id

WHERE c.country='Ghana';
```

The database may join unnecessary rows first.

---

## Better

```sql
WITH customers AS (

SELECT *

FROM customers

WHERE country='Ghana'

)

SELECT *

FROM orders o

JOIN customers c

ON o.customer_id=c.customer_id;
```

---

# Optimizing JOIN Operations

JOINs are expensive operations.

Common causes of slow queries:

- Joining huge tables unnecessarily
- Joining on non-unique columns
- Missing filters
- Duplicate records

---

# Join Best Practices

## Use Proper Join Keys

Good:

```sql
ON customer_id
```

Avoid:

```sql
ON customer_name
```

Names may not be unique.

---

## Reduce Data Before Joining

Example:

Instead of:

```
10 million rows

JOIN

10 million rows
```

Filter first:

```
100,000 rows

JOIN

50,000 rows
```

---

# Avoiding Duplicate Rows

A common performance and accuracy problem is accidental duplication.

Example:

```
Customers

customer_id

1


Orders

customer_id

1
1
1
```

A join creates:

```
Customer 1
Order 1

Customer 1
Order 2

Customer 1
Order 3
```

Always understand table relationships.

---

# Indexing

Indexes improve data lookup speed.

Example:

Without index:

```
Scan every row
```

With index:

```
Direct lookup
```

Common indexed columns:

- Primary keys
- Foreign keys
- Frequently filtered columns

Example:

```sql
CREATE INDEX idx_customer

ON customers(customer_id);
```

---

# Columnar Storage

Modern analytics databases use columnar storage.

Examples:

- DuckDB
- BigQuery
- Snowflake
- Redshift

Instead of:

```
Row storage

id | name | email | revenue
```

Column storage:

```
id column

name column

email column

revenue column
```

Benefits:

- Faster analytics queries
- Better compression
- Efficient aggregations

---

# Partitioning

Partitioning divides large tables into smaller sections.

Example:

Sales table:

```
sales_2024

sales_2025

sales_2026
```

A query:

```sql
WHERE year = 2026
```

only scans relevant data.

---

# Materialization Strategies

Analytics engineering tools like dbt support different materializations.

## View

Example:

```sql
CREATE VIEW
```

Advantages:

- Always fresh
- No storage cost

Disadvantages:

- Query runs every time

---

## Table

Example:

```sql
CREATE TABLE
```

Advantages:

- Faster reporting
- Stored results

Disadvantages:

- Requires rebuilding

---

## Incremental Models

Only process new or changed data.

Example:

```
Existing data

+

New records
```

Benefits:

- Faster pipelines
- Lower compute cost

---

# CTE Performance Considerations

CTEs improve readability.

However, complex CTE chains can sometimes increase processing.

Example:

```
CTE 1

↓

CTE 2

↓

CTE 3

↓

Final Query
```

For very large datasets:

Consider:

- Materializing intermediate results
- Creating tables
- Incremental models

---

# Avoiding Expensive Functions

Functions applied to columns may reduce optimization.

Example:

Less efficient:

```sql
WHERE LOWER(email)='test@email.com'
```

Better:

Store standardized values:

```sql
email_lower
```

Then:

```sql
WHERE email_lower='test@email.com'
```

---

# Aggregation Optimization

Aggregations can be expensive.

Example:

```sql
SELECT

customer_id,

COUNT(*)

FROM transactions

GROUP BY customer_id;
```

Optimization:

- Reduce rows first
- Use appropriate storage
- Avoid repeated calculations

---

# Query Caching

Many BI systems cache query results.

Examples:

- Power BI cache
- Database cache
- Warehouse cache

Good modeling improves cache effectiveness.

---

# Performance In dbt Projects

dbt performance depends on:

- Model design
- Materialization
- Dependencies
- SQL efficiency

Example:

Poor structure:

```
Raw Table

↓

Huge SQL Model

↓

Dashboard
```

Better:

```
Staging

↓

Intermediate

↓

Mart

↓

Dashboard
```

---

# Performance Improvements Applied In SupportOps Intelligence

The project uses several performance practices:

## Explicit Columns

Models select required fields instead of:

```sql
SELECT *
```

---

## Layered Transformations

Structure:

```
stg_ticket

↓

int_ticket_metrics

↓

fact_ticket

↓

support_dashboard
```

---

## Parquet Export

Power BI consumes optimized files:

```
DuckDB

↓

Parquet

↓

Power BI
```

Parquet provides:

- Compression
- Efficient reads
- Columnar storage

---

# Performance Checklist

Before deploying SQL:

## Query Design

- Are unnecessary columns removed?
- Are filters applied early?
- Are joins required?

---

## Data Modeling

- Are relationships correct?
- Are tables appropriately structured?
- Are metrics reusable?

---

## Production Readiness

- Has execution time been tested?
- Are failures monitored?
- Is documentation available?

---

# Skills To Master

## SQL Optimization

Learn:

- Query plans
- Indexing
- Partitioning
- Join optimization
- Aggregation strategies


## Database Systems

Learn:

- Storage engines
- Execution engines
- Transactions
- Query optimizers


## Cloud Warehouses

Learn:

- Snowflake optimization
- BigQuery partitioning
- Redshift distribution


---

# Recommended Resources

## Books

### SQL Performance Explained

Author:
Markus Winand

Focus:

- Query optimization
- Indexes
- Execution plans


### Database Internals

Author:
Alex Petrov

Focus:

- Storage engines
- Database architecture


---

## Documentation

DuckDB Performance:

https://duckdb.org/docs/guides/performance/overview


PostgreSQL EXPLAIN:

https://www.postgresql.org/docs/current/using-explain.html


---

## Courses

### Use The Index, Luke

https://use-the-index-luke.com/


### Data Engineering Zoomcamp

DataTalksClub:

https://github.com/DataTalksClub/data-engineering-zoomcamp


---

# Summary

SQL performance optimization is essential for building scalable analytics systems.

A professional analytics engineer must understand:

- Efficient query writing
- Database execution
- Storage optimization
- Model design
- Pipeline performance

Performance is not only about speed; it is about building reliable, cost-effective analytical systems.