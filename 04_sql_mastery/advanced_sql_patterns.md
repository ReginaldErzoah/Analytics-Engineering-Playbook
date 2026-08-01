# Advanced SQL Patterns for Analytics Engineering

## Overview

Advanced SQL skills separate basic analysts from analytics engineers.

While foundational SQL focuses on retrieving and filtering data, advanced SQL focuses on:

- Building reusable transformations
- Performing complex analysis
- Creating scalable data models
- Solving business problems efficiently

Analytics engineers use advanced SQL daily when building models in tools like:

- dbt
- DuckDB
- Snowflake
- BigQuery
- PostgreSQL

---

# Common Table Expressions (CTEs)

## What is a CTE?

A Common Table Expression (CTE) creates a temporary named result that can be referenced inside a query.

Syntax:

```sql
WITH cte_name AS (

    SELECT
        *

    FROM table_name

)

SELECT *

FROM cte_name;
```

---

# Why Use CTEs?

CTEs improve:

- Query readability
- Debugging
- Reusability
- Logical separation of transformations

Instead of:

```sql
SELECT *
FROM (
    SELECT *
    FROM tickets
)
```

Use:

```sql
WITH tickets AS (

    SELECT *

    FROM tickets

)

SELECT *

FROM tickets;
```

---

# Multiple CTEs

Analytics engineering models often contain multiple transformation steps.

Example:

```sql
WITH tickets AS (

    SELECT *

    FROM raw_tickets

),

metrics AS (

    SELECT

        ticket_id,

        resolution_time_hours,

        CASE

            WHEN resolution_time_hours <= 24
            THEN 'Fast'

            ELSE 'Slow'

        END AS category

    FROM tickets

)

SELECT *

FROM metrics;
```

This pattern was used in:

```
SupportOps Intelligence

stg_ticket
      |
      ↓
int_ticket_metrics
      |
      ↓
fact_ticket
```

---

# Window Functions

## What Are Window Functions?

Window functions perform calculations across related rows without grouping them into a single row.

Unlike GROUP BY:

GROUP BY:

```
Many rows
    |
    ↓
One summary row
```

Window function:

```
Many rows
    |
    ↓
Many rows with additional calculations
```

---

# ROW_NUMBER()

Assigns a unique sequence number.

Example:

```sql
SELECT

customer_id,

order_date,

ROW_NUMBER() OVER(
    ORDER BY order_date
) AS order_number

FROM orders;
```

---

# Ranking Functions

## RANK()

Creates rankings.

Example:

```sql
SELECT

product,

revenue,

RANK() OVER(
    ORDER BY revenue DESC
) AS ranking

FROM sales;
```

Result:

| Product | Revenue | Ranking |
|-|-|-|
| A | 5000 | 1 |
| B | 3000 | 2 |

---

## DENSE_RANK()

Similar to RANK but without gaps.

Example:

```
RANK:

1
2
2
4


DENSE_RANK:

1
2
2
3
```

---

# PARTITION BY

PARTITION BY creates groups inside window functions.

Example:

Rank agents by performance:

```sql
SELECT

agent_id,

resolution_time_hours,

RANK() OVER(

PARTITION BY agent_id

ORDER BY resolution_time_hours

)

FROM tickets;
```

---

# Running Totals

Example:

```sql
SELECT

order_date,

revenue,

SUM(revenue) OVER(

ORDER BY order_date

) AS cumulative_revenue

FROM sales;
```

Useful for:

- Growth analysis
- Financial reporting
- Trend analysis

---

# Moving Averages

Example:

```sql
SELECT

date,

AVG(sales) OVER(

ORDER BY date

ROWS BETWEEN 6 PRECEDING
AND CURRENT ROW

)

AS seven_day_average

FROM daily_sales;
```

Used for:

- Trend smoothing
- Forecasting
- Monitoring

---

# Advanced JOIN Patterns

## INNER JOIN

Returns matching records.

Example:

```sql
SELECT *

FROM customers c

INNER JOIN orders o

ON c.customer_id=o.customer_id;
```

---

## LEFT JOIN

Keeps all records from the left table.

Example:

```sql
SELECT *

FROM customers c

LEFT JOIN orders o

ON c.customer_id=o.customer_id;
```

Common in analytics engineering.

Example:

```
All Customers

+

Their Orders
```

---

## Anti Join

Find records without matches.

Example:

Customers without orders:

```sql
SELECT *

FROM customers c

WHERE NOT EXISTS (

SELECT 1

FROM orders o

WHERE c.customer_id=o.customer_id

);
```

---

# Data Deduplication

Duplicate records are common.

Example:

```sql
customer_id | updated_at
------------|-----------
1           | 2025-01-01
1           | 2025-02-01
```

Keep latest:

```sql
WITH ranked AS (

SELECT

*,

ROW_NUMBER() OVER(

PARTITION BY customer_id

ORDER BY updated_at DESC

) AS rn


FROM customers

)

SELECT *

FROM ranked

WHERE rn=1;
```

---

# Slowly Changing Dimension Logic

Analytics systems often track historical changes.

Example:

Customer changes location:

```
Before:

John
USA


After:

John
Canada
```

Instead of overwriting:

Store history.

Example:

```
customer_key

customer_id

location

valid_from

valid_to

current_flag
```

---

# Conditional Aggregation

Calculate multiple metrics in one query.

Example:

```sql
SELECT

COUNT(*) AS total_tickets,

COUNT(
CASE
WHEN sla_met = TRUE
THEN 1
END
)

AS sla_met_tickets

FROM tickets;
```

---

# Pivoting Data

Convert rows into columns.

Example:

Before:

| Priority | Count |
|-|-|
| High | 100 |
| Low | 200 |

After:

| High | Low |
|-|-|
|100|200|

---

# Date Analysis Patterns

Analytics engineers frequently analyze:

- Daily trends
- Monthly performance
- Year-over-year changes

Example:

Monthly tickets:

```sql
SELECT

DATE_TRUNC(
'month',
submission_date
)

AS month,

COUNT(*) AS tickets

FROM tickets

GROUP BY month;
```

---

# Cohort Analysis

Used to understand groups over time.

Example:

Customer signup month:

```sql
SELECT

DATE_TRUNC(
'month',
signup_date
)

AS cohort_month

FROM customers;
```

Used for:

- Retention
- Churn
- Customer behavior

---

# Query Optimization Basics

## Select Only Required Columns

Avoid:

```sql
SELECT *
```

Prefer:

```sql
SELECT

ticket_id,
priority_level

FROM tickets;
```

---

## Filter Early

Bad:

```
Join millions of rows
then filter
```

Better:

```
Filter first
then join
```

---

## Avoid Unnecessary Joins

Every join increases:

- Processing cost
- Complexity
- Potential duplication

---

## Understand Query Plans

Database engines optimize queries.

Useful commands:

DuckDB:

```sql
EXPLAIN

SELECT *

FROM tickets;
```

---

# SQL in dbt Models

dbt models are SQL files.

Example:

```
models/

staging/

    stg_ticket.sql


intermediate/

    int_ticket_metrics.sql


marts/

    fact_ticket.sql
```

Each model represents a transformation layer.

---

# Advanced SQL in SupportOps Intelligence

Used concepts:

## CTEs

Implemented:

```
stg_ticket
int_ticket_metrics
fact_ticket
```

---

## CASE Logic

Used for:

- SLA performance
- Ticket complexity
- Resolution categories

---

## Aggregations

Used for:

- Total tickets
- SLA success rate
- Average resolution time

---

## Joins

Used for:

```
fact_ticket

JOIN

dimension tables
```

---

# Skills To Master

## SQL

Learn:

- CTEs
- Window functions
- Query optimization
- Advanced joins
- Data modeling queries


## Analytics

Learn:

- Cohort analysis
- Retention analysis
- KPI calculations
- Business metrics


## Database Performance

Learn:

- Query plans
- Indexing
- Partitioning
- Storage optimization


---

# Recommended Resources

## Books

### SQL Performance Explained

Author:
Markus Winand

Focus:

- Query optimization
- Indexes
- Performance


### Advanced SQL Puzzles

Author:
Joe Celko

Focus:

- Complex SQL problem solving


---

## Documentation

DuckDB SQL:

https://duckdb.org/docs/sql/introduction


PostgreSQL Window Functions:

https://www.postgresql.org/docs/current/tutorial-window.html


---

## Courses

### Advanced SQL for Data Engineers

DataCamp

https://www.datacamp.com/


### Mode Advanced SQL Tutorial

https://mode.com/sql-tutorial/


---

# Summary

Advanced SQL allows analytics engineers to build reliable analytical systems.

Mastering:

- CTEs
- Window functions
- Complex joins
- Aggregations
- Optimization

enables the creation of scalable analytics models and production-ready data solutions.