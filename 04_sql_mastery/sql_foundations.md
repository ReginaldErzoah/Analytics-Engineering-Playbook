# SQL Foundations for Analytics Engineering

## Overview

SQL (Structured Query Language) is the foundation of analytics engineering.

Almost every analytics system relies on SQL for:

- Extracting data from databases
- Cleaning and transforming datasets
- Creating analytical models
- Building metrics
- Supporting business intelligence dashboards

An analytics engineer must be able to translate business questions into efficient SQL queries.

---

# Why SQL Matters in Analytics Engineering

Modern analytics workflows usually follow:

```
Operational Systems
        |
        ↓
Raw Data Storage
        |
        ↓
SQL Transformations
        |
        ↓
Analytics Models
        |
        ↓
BI Dashboards
```

SQL is used heavily in the transformation layer.

Example:

```
Customer Support Data

        ↓

SQL Cleaning

        ↓

Fact and Dimension Tables

        ↓

Power BI Dashboard
```

---

# Relational Database Concepts

SQL works with relational databases.

A relational database stores information in tables.

Example:

## Customers Table

| customer_id | customer_name | email |
|---|---|---|
| 1 | John Smith | john@email.com |
| 2 | Mary Jones | mary@email.com |

---

## Orders Table

| order_id | customer_id | amount |
|---|---|---|
| 101 | 1 | 250 |
| 102 | 2 | 500 |

The relationship:

```
Customers
    |
    |
customer_id
    |
    |
Orders
```

allows data to be combined.

---

# SQL Query Structure

A basic SQL query:

```sql
SELECT
    column_name

FROM
    table_name

WHERE
    condition;
```

Example:

```sql
SELECT
    customer_name

FROM
    customers;
```

Result:

```
John Smith
Mary Jones
```

---

# SELECT Statement

SELECT retrieves columns.

Example:

```sql
SELECT
    ticket_id,
    priority_level,
    resolution_time_hours

FROM
    tickets;
```

---

Selecting all columns:

```sql
SELECT *

FROM tickets;
```

Avoid using `*` in production models because:

- It may include unnecessary columns
- Schema changes can break models
- Performance may decrease

---

# Filtering Data With WHERE

WHERE filters rows.

Example:

```sql
SELECT *

FROM tickets

WHERE priority_level = 'High';
```

---

Multiple conditions:

```sql
SELECT *

FROM tickets

WHERE priority_level = 'High'

AND sla_met = TRUE;
```

---

Using OR:

```sql
SELECT *

FROM tickets

WHERE channel = 'Email'

OR channel = 'Chat';
```

---

# Comparison Operators

Common operators:

| Operator | Meaning |
|---|---|
| = | Equal |
| != | Not equal |
| > | Greater than |
| < | Less than |
| >= | Greater or equal |
| <= | Less or equal |

Example:

```sql
SELECT *

FROM tickets

WHERE resolution_time_hours > 48;
```

---

# Sorting Data With ORDER BY

ORDER BY sorts results.

Ascending:

```sql
SELECT *

FROM tickets

ORDER BY resolution_time_hours;
```

Descending:

```sql
SELECT *

FROM tickets

ORDER BY resolution_time_hours DESC;
```

---

# Limiting Results

LIMIT restricts output rows.

Example:

```sql
SELECT *

FROM tickets

LIMIT 10;
```

Useful for:

- Testing queries
- Exploring datasets
- Debugging

---

# SQL Aggregate Functions

Aggregations summarize data.

Common functions:

| Function | Purpose |
|---|---|
| COUNT | Count rows |
| SUM | Total values |
| AVG | Average |
| MIN | Minimum |
| MAX | Maximum |

---

## COUNT

Example:

```sql
SELECT
    COUNT(*) AS total_tickets

FROM tickets;
```

---

## AVG

Example:

```sql
SELECT
    AVG(resolution_time_hours)

FROM tickets;
```

---

## SUM

Example:

```sql
SELECT
    SUM(revenue)

FROM sales;
```

---

# GROUP BY

GROUP BY creates summaries by categories.

Example:

Count tickets by priority:

```sql
SELECT

priority_level,

COUNT(*) AS tickets

FROM tickets

GROUP BY priority_level;
```

Result:

| priority | tickets |
|-|-|
| High | 300 |
| Medium | 500 |
| Low | 200 |

---

# HAVING

HAVING filters aggregated results.

WHERE filters rows.

HAVING filters groups.

Example:

```sql
SELECT

agent_id,

COUNT(*) AS tickets

FROM tickets

GROUP BY agent_id

HAVING COUNT(*) > 100;
```

---

# CASE Statements

CASE creates conditional logic.

Example:

```sql
SELECT

ticket_id,

CASE

WHEN resolution_time_hours <= 24
THEN 'Fast'

WHEN resolution_time_hours <= 72
THEN 'Medium'

ELSE 'Slow'

END AS resolution_category

FROM tickets;
```

Used frequently for:

- KPI categories
- Business rules
- Segmentation

---

# NULL Handling

NULL represents missing values.

Checking NULL:

```sql
SELECT *

FROM customers

WHERE email IS NULL;
```

Checking non-null:

```sql
SELECT *

FROM customers

WHERE email IS NOT NULL;
```

---

Replacing NULL:

```sql
SELECT

COALESCE(phone,'Unknown')

FROM customers;
```

---

# DISTINCT

DISTINCT removes duplicates.

Example:

```sql
SELECT DISTINCT

ticket_channel

FROM tickets;
```

Result:

```
Email
Phone
Chat
```

---

# SQL Aliases

Aliases rename columns.

Example:

```sql
SELECT

COUNT(*) AS total_tickets

FROM tickets;
```

Benefits:

- Improves readability
- Makes reports clearer

---

# SQL Data Types

Common types:

## Numeric

Examples:

```
INTEGER
DECIMAL
FLOAT
```

---

## Text

Examples:

```
VARCHAR
TEXT
STRING
```

---

## Date and Time

Examples:

```
DATE
TIMESTAMP
```

---

# SQL Functions Used Frequently

## String Functions

Example:

```sql
LOWER(email)
```

Converts text to lowercase.

---

```sql
UPPER(name)
```

Converts text to uppercase.

---

## Date Functions

Example:

```sql
EXTRACT(YEAR FROM submission_date)
```

---

## Mathematical Functions

Example:

```sql
ROUND(avg_resolution,2)
```

---

# SQL in Analytics Engineering

SQL is used to build:

## Staging Models

Purpose:

- Clean source data
- Rename fields
- Standardize formats

Example:

```
stg_ticket.sql
```

---

## Intermediate Models

Purpose:

- Business logic
- Calculations
- Reusable transformations

Example:

```
int_ticket_metrics.sql
```

---

## Mart Models

Purpose:

- Reporting-ready tables

Examples:

```
fact_ticket.sql

dim_customer.sql

support_dashboard.sql
```

---

# SQL Workflow in SupportOps Intelligence

The project workflow:

```
Raw CSV

↓

stg_ticket

↓

int_ticket_metrics

↓

fact_ticket + dimensions

↓

support_dashboard

↓

Power BI
```

SQL handled:

- Data modeling
- KPI calculations
- SLA analysis
- Ticket metrics

---

# SQL Best Practices

## Use Explicit Columns

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

## Use Meaningful Names

Bad:

```sql
SELECT a,b,c
```

Good:

```sql
SELECT

customer_email,
resolution_time_hours
```

---

## Format Queries Clearly

Good formatting improves:

- Debugging
- Collaboration
- Maintenance

---

## Comment Complex Logic

Example:

```sql
-- Calculate SLA performance

CASE

WHEN resolution_time_hours <= sla_target_hours

THEN 'Within SLA'

END
```

---

# Skills To Master

## SQL Fundamentals

Learn:

- SELECT
- WHERE
- ORDER BY
- LIMIT
- CASE
- GROUP BY
- HAVING

---

## Intermediate SQL

Learn:

- JOINs
- CTEs
- Window functions
- Subqueries

---

## Advanced Analytics SQL

Learn:

- Ranking functions
- Cohort analysis
- Retention analysis
- Time-series analysis
- Query optimization

---

# Recommended Resources

## Books

### SQL for Data Analysis

Author:
Cathy Tanimura

Focus:

- Analytical SQL
- Business questions
- Data exploration


### Fundamentals of Database Systems

Authors:
Ramez Elmasri and Sham Navathe

Focus:

- Database theory
- Relational modeling


---

## Documentation

PostgreSQL Documentation:

https://www.postgresql.org/docs/


DuckDB SQL Documentation:

https://duckdb.org/docs/sql/introduction


---

## Courses

### SQLBolt

https://sqlbolt.com/


### Mode SQL Tutorial

https://mode.com/sql-tutorial/


### Data Analysis with SQL

DataCamp:

https://www.datacamp.com/


---

# Summary

SQL is the core language of analytics engineering.

A strong analytics engineer should be able to:

1. Query raw datasets
2. Transform messy data
3. Build analytical models
4. Create reliable metrics
5. Optimize queries
6. Translate business problems into data solutions

SQL combined with Python, dbt, DuckDB, and BI tools forms the foundation of modern analytics engineering.