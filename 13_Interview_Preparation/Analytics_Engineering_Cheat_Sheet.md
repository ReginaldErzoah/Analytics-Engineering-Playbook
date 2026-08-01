# Analytics Engineering Interview Cheat Sheet

## Overview

This cheat sheet provides a quick revision guide for analytics engineering interviews.

Use it before interviews to review:

- SQL concepts
- Data modeling
- dbt
- Warehousing
- Analytics concepts
- System design
- Key terminology

---

# 1. SQL Cheat Sheet

---

# Basic Query Structure

```sql
SELECT

columns

FROM table

WHERE condition

GROUP BY columns

HAVING condition

ORDER BY column

LIMIT number;
```

---

# Filtering

```sql
SELECT *

FROM customers

WHERE country = 'Ghana';
```

---

# Aggregations

## COUNT

Counts records.

```sql
COUNT(*)
```

---

## SUM

Adds values.

```sql
SUM(revenue)
```

---

## AVG

Calculates average.

```sql
AVG(price)
```

---

## MIN / MAX

Find extremes.

```sql
MIN(price)

MAX(price)
```

---

# GROUP BY

Used for aggregating by categories.

Example:

```sql
SELECT

country,

COUNT(*)

FROM customers

GROUP BY country;
```

---

# HAVING

Filters aggregated results.

Example:

```sql
HAVING COUNT(*) > 100
```

---

# JOIN Types

---

## INNER JOIN

Returns matching records.

```
A ∩ B
```

---

## LEFT JOIN

Returns all records from left table.

```
A + matches from B
```

---

## RIGHT JOIN

Returns all records from right table.

---

## FULL JOIN

Returns everything from both tables.

---

# Window Functions

Used for calculations across rows.

---

## ROW_NUMBER

Creates unique ranking.

```sql
ROW_NUMBER()
OVER()
```

---

## RANK

Ranks values.

```sql
RANK()
OVER(
ORDER BY revenue DESC
)
```

---

## LAG

Access previous row.

```sql
LAG(revenue)
OVER(
ORDER BY month
)
```

---

## LEAD

Access next row.

---

# Common SQL Interview Patterns

---

## Top N Records

```sql
ORDER BY metric DESC

LIMIT N
```

---

## Running Total

```sql
SUM(value)
OVER(
ORDER BY date
)
```

---

## Duplicate Detection

```sql
GROUP BY column

HAVING COUNT(*) > 1
```

---

## Missing Records

Use:

```sql
LEFT JOIN

WHERE missing_column IS NULL
```

---

# 2. Data Modeling Cheat Sheet

---

# Fact Table

Stores:

- Events
- Measurements
- Transactions

Examples:

```
Sales

Payments

Orders
```

---

# Dimension Table

Stores descriptions.

Examples:

```
Customer

Product

Date

Location
```

---

# Grain

Definition:

"The level of detail represented by one row."

Example:

```
One row = one product sold in one transaction
```

---

# Star Schema

Structure:

```
        Customer

            |

Product -- Sales -- Date

```

Characteristics:

- Simple
- Fast
- BI friendly

---

# Snowflake Schema

Characteristics:

- Normalized dimensions
- More joins
- More complexity

---

# SCD Types

---

## Type 0

No changes.

---

## Type 1

Overwrite old value.

---

## Type 2

Maintain full history.

Most common.

---

## Type 3

Maintain limited history.

---

# 3. dbt Cheat Sheet

---

# dbt Purpose

dbt transforms warehouse data using SQL.

It supports:

- Modeling
- Testing
- Documentation
- Deployment

---

# dbt Layers

```
Sources

↓

Staging

↓

Intermediate

↓

Marts
```

---

# ref()

References another dbt model.

Example:

```sql
{{ ref('customers') }}
```

---

# source()

References raw data.

Example:

```sql
{{ source('raw','orders') }}
```

---

# Materializations

---

## View

Creates database view.

---

## Table

Creates physical table.

---

## Incremental

Processes only new data.

---

## Ephemeral

Temporary SQL logic.

---

# dbt Tests

Built-in tests:

```
unique

not_null

relationships

accepted_values
```

---

# dbt Documentation

Provides:

- Model descriptions
- Column definitions
- Lineage graphs

---

# 4. Data Warehouse Cheat Sheet

---

# OLTP

Operational systems.

Examples:

- Applications
- Transactions

Characteristics:

- Fast writes
- Normalized tables

---

# OLAP

Analytical systems.

Examples:

- Dashboards
- Reporting

Characteristics:

- Large queries
- Historical analysis

---

# ETL

```
Extract

↓

Transform

↓

Load
```

---

# ELT

```
Extract

↓

Load

↓

Transform
```

Modern analytics engineering uses ELT.

---

# Warehouse Architecture

```
Sources

↓

Ingestion

↓

Raw Layer

↓

Transformations

↓

Warehouse

↓

BI
```

---

# Partitioning

Splits large tables.

Example:

```
partition by date
```

Benefits:

- Faster queries
- Lower cost

---

# Clustering

Organizes related data.

Example:

```
cluster by customer_id
```

---

# 5. Analytics Cheat Sheet

---

# Common Business Metrics

---

## Revenue

```
Sales Amount
```

---

## Average Order Value

```
Revenue / Orders
```

---

## Customer Lifetime Value

```
Average Purchase × Frequency × Lifetime
```

---

## Retention Rate

```
Returning Customers / Total Customers
```

---

## Churn Rate

```
Lost Customers / Starting Customers
```

---

# Analytics Process

```
Business Question

↓

Data Collection

↓

Transformation

↓

Analysis

↓

Insight

↓

Decision
```

---

# 6. System Design Cheat Sheet

---

# General Architecture

```
Applications

↓

Data Pipeline

↓

Warehouse

↓

Transformation

↓

Analytics Models

↓

Dashboard
```

---

# Important Design Considerations

## Scalability

Can the system handle growth?

---

## Reliability

Can users trust the data?

---

## Maintainability

Can engineers update it easily?

---

## Performance

Can queries run efficiently?

---

# Data Quality Checklist

Check:

```
✓ Completeness

✓ Accuracy

✓ Consistency

✓ Validity

✓ Timeliness

✓ Uniqueness
```

---

# 7. Interview Answer Frameworks

---

# Technical Question

Use:

```
Definition

↓

Example

↓

Why It Matters
```

---

Example:

"What is a fact table?"

Answer:

"Fact tables store measurable business events. For example, a sales fact table stores transactions, revenue, and quantities. They are important because they enable analytical reporting."

---

# Design Question

Use:

```
Requirements

↓

Architecture

↓

Data Model

↓

Quality

↓

Trade-offs
```

---

# Case Study Question

Use:

```
Clarify

↓

Analyze

↓

Find Drivers

↓

Recommend Action

↓

Measure Impact
```

---

# Behavioral Question

Use:

```
STAR

Situation

Task

Action

Result
```

---

# 8. Final Interview Review Checklist

Before your interview:

```
✓ SQL

✓ Joins

✓ Window Functions

✓ Data Modeling

✓ Fact Tables

✓ Dimensions

✓ Star Schema

✓ dbt

✓ ELT

✓ Warehouses

✓ Case Studies

✓ Communication
```

---

# Final Reminder

Analytics engineering interviews are not only about writing SQL.

They test your ability to:

```
Understand Business Problems

↓

Design Reliable Data Systems

↓

Create Trusted Metrics

↓

Enable Better Decisions
```

The strongest candidates combine:

```
Technical Skills

+

Analytical Thinking

+

Engineering Discipline

+

Communication
```