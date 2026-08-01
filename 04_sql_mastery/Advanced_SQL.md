# Advanced SQL

## Overview

Advanced SQL focuses on techniques used by Analytics Engineers and Data Analysts to transform, analyze, and optimize data.

These concepts are essential when working with:

- Data warehouses
- dbt models
- Business intelligence dashboards
- Analytics pipelines
- Large-scale datasets

Key topics:

- Common Table Expressions (CTEs)
- Subqueries
- Views
- Temporary Tables
- Window Functions
- Conditional Aggregation
- Date Operations
- Query Optimization
- Analytical Transformations

---

# 1. Common Table Expressions (CTEs)

## What is a CTE?

A Common Table Expression (CTE) creates a temporary named result set that can be referenced within a SQL query.

Syntax:

```sql
WITH cte_name AS (

    SELECT
        columns

    FROM table

)

SELECT *
FROM cte_name;
```

---

## Example

Without CTE:

```sql
SELECT
    customer_id,
    SUM(amount) AS total_sales

FROM orders

GROUP BY customer_id;
```

With CTE:

```sql
WITH customer_sales AS (

    SELECT
        customer_id,
        SUM(amount) AS total_sales

    FROM orders

    GROUP BY customer_id

)

SELECT *
FROM customer_sales;
```

Advantages:

- Improves readability
- Breaks complex logic into steps
- Easier debugging
- Common pattern in dbt models

---

# 2. Multiple CTEs

Complex analytics pipelines often contain multiple transformation steps.

Example:

```sql
WITH orders_clean AS (

    SELECT
        *
    FROM raw_orders
    WHERE amount IS NOT NULL

),

customer_summary AS (

    SELECT
        customer_id,
        SUM(amount) AS revenue

    FROM orders_clean

    GROUP BY customer_id

)

SELECT *

FROM customer_summary;
```

Analytics engineering pattern:

```
Raw Data

   ↓

Cleaning CTE

   ↓

Business Logic CTE

   ↓

Final Model
```

---

# 3. Subqueries

A subquery is a query inside another query.

Example:

Find customers whose spending is above average.

```sql
SELECT
    customer_id,
    amount

FROM orders

WHERE amount >

(
    SELECT AVG(amount)
    FROM orders
);
```

---

# 4. Correlated Subqueries

A correlated subquery references the outer query.

Example:

Find each customer's highest order.

```sql
SELECT
    customer_id,
    order_id,
    amount

FROM orders o

WHERE amount =
(
    SELECT MAX(amount)

    FROM orders

    WHERE customer_id = o.customer_id
);
```

---

# 5. Views

## What is a View?

A view is a saved SQL query that behaves like a table.

Creating a view:

```sql
CREATE VIEW customer_orders AS

SELECT
    customer_id,
    COUNT(*) AS total_orders

FROM orders

GROUP BY customer_id;
```

Querying a view:

```sql
SELECT *
FROM customer_orders;
```

---

## Views in Analytics Engineering

Views are commonly used for:

- Reusable transformations
- Simplifying reporting logic
- Creating semantic layers

In dbt:

```sql
{{ config(
    materialized='view'
) }}
```

---

# 6. Temporary Tables

Temporary tables store intermediate results during a session.

Example:

```sql
CREATE TEMP TABLE high_value_customers AS

SELECT *

FROM customers

WHERE lifetime_value > 10000;
```

Useful for:

- Exploratory analysis
- Debugging transformations
- Complex workflows

---

# 7. Conditional Aggregation

Conditional aggregation combines CASE statements with aggregate functions.

Example:

Count tickets by status:

```sql
SELECT

COUNT(
    CASE
        WHEN ticket_status = 'Closed'
        THEN ticket_id
    END
) AS closed_tickets,

COUNT(
    CASE
        WHEN ticket_status = 'Open'
        THEN ticket_id
    END
) AS open_tickets

FROM tickets;
```

---

## Business Example

Customer support KPI:

```sql
SELECT

AVG(
    CASE
        WHEN ticket_priority = 'High'
        THEN resolution_hours
    END
) AS avg_high_priority_resolution

FROM tickets;
```

---

# 8. Pivoting Data

Pivoting converts rows into columns.

Example:

Original:

| Month | Status | Tickets |
|-|-|-|
| Jan | Open | 50 |
| Jan | Closed | 100 |

After pivot:

| Month | Open | Closed |
|-|-|-|
| Jan | 50 | 100 |

Example:

```sql
SELECT

month,

SUM(
    CASE WHEN status='Open'
    THEN tickets
    END
) AS open_tickets,

SUM(
    CASE WHEN status='Closed'
    THEN tickets
    END
) AS closed_tickets

FROM support

GROUP BY month;
```

---

# 9. Date Functions

Analytics requires extensive date manipulation.

## Extract Date Components

```sql
SELECT

EXTRACT(YEAR FROM order_date) AS year,

EXTRACT(MONTH FROM order_date) AS month

FROM orders;
```

---

## Date Difference

Example:

Calculate resolution time:

```sql
SELECT

DATEDIFF(
    'hour',
    created_time,
    resolved_time
)

FROM tickets;
```

---

## Date Truncation

Group data by month:

```sql
SELECT

DATE_TRUNC(
    'month',
    order_date
)

FROM orders;
```

Common reporting periods:

- Daily
- Weekly
- Monthly
- Quarterly
- Yearly

---

# 10. NULL Handling

## COALESCE

Returns the first non-null value.

Example:

```sql
SELECT

COALESCE(phone,'Unknown')

FROM customers;
```

---

## NULLIF

Returns NULL when values match.

Example:

Avoid division by zero:

```sql
SELECT

sales /
NULLIF(customers,0)

FROM metrics;
```

---

# 11. Deduplication Techniques

Duplicate records are common in analytics pipelines.

Example:

Remove duplicates:

```sql
SELECT DISTINCT *

FROM customers;
```

---

## Using ROW_NUMBER()

More robust approach:

```sql
WITH duplicates AS (

SELECT

*,

ROW_NUMBER() OVER(
    PARTITION BY customer_id
    ORDER BY created_at DESC
) AS row_num

FROM customers

)

SELECT *

FROM duplicates

WHERE row_num = 1;
```

Use cases:

- Removing duplicate events
- Selecting latest customer record
- Data cleaning pipelines

---

# 12. MERGE Statements

MERGE combines INSERT and UPDATE operations.

Used in:

- Data warehouses
- Incremental pipelines
- Slowly Changing Dimensions

Example:

```sql
MERGE INTO customers target

USING new_customers source

ON target.customer_id = source.customer_id

WHEN MATCHED THEN

UPDATE SET
target.email = source.email

WHEN NOT MATCHED THEN

INSERT VALUES (...);
```

---

# 13. Analytical SQL Patterns

## Top N Records

Example:

Top 5 customers by revenue:

```sql
SELECT

customer_id,

SUM(amount) AS revenue

FROM orders

GROUP BY customer_id

ORDER BY revenue DESC

LIMIT 5;
```

---

## Running Totals

```sql
SELECT

order_date,

SUM(amount)

OVER(
ORDER BY order_date
)

AS cumulative_sales

FROM orders;
```

---

## Month-over-Month Growth

Example:

```sql
SELECT

month,

sales,

sales -
LAG(sales)
OVER(
ORDER BY month
)

AS growth

FROM monthly_sales;
```

---

# 14. SQL in dbt Analytics Engineering

A typical dbt model:

```sql
WITH source AS (

    SELECT *

    FROM {{ source(
        'raw',
        'tickets'
    ) }}

),

cleaned AS (

    SELECT

        ticket_id,

        LOWER(email) AS email

    FROM source

)

SELECT *

FROM cleaned;
```

SQL is used for:

- Cleaning
- Joining
- Transforming
- Aggregating
- Creating business metrics

---

# Advanced SQL Interview Questions

## Q1. Difference between CTE and Subquery?

CTEs:

- Improve readability
- Can be reused multiple times
- Better for complex transformations

Subqueries:

- Embedded directly inside another query
- Useful for simple calculations

---

## Q2. Difference between WHERE and HAVING?

WHERE:

Filters rows before aggregation.

HAVING:

Filters aggregated results.

---

## Q3. How do you remove duplicates?

Common methods:

- DISTINCT
- GROUP BY
- ROW_NUMBER()

---

## Q4. What is a window function?

A function that performs calculations across related rows without collapsing them.

Examples:

- Ranking
- Running totals
- Moving averages
- Previous/next comparisons

---

# Analytics Engineer Best Practices

## Prefer CTEs Over Huge Queries

Bad:

```sql
500 lines of nested SQL
```

Good:

```text
source_data

↓

clean_data

↓

business_logic

↓

final_model
```

---

## Make SQL Modular

Analytics engineering follows:

```
Raw Layer

↓

Staging Layer

↓

Intermediate Layer

↓

Mart Layer

↓

Dashboard
```

---

## Optimize Early

Consider:

- Selecting only required columns
- Filtering early
- Avoiding unnecessary joins
- Using appropriate indexes/partitions

---

# Key Takeaway

Advanced SQL is the foundation of analytics engineering.

A strong Analytics Engineer should be comfortable with:

✅ Complex transformations  
✅ Business logic implementation  
✅ Data cleaning  
✅ KPI calculations  
✅ Query optimization  
✅ Building reliable analytical datasets