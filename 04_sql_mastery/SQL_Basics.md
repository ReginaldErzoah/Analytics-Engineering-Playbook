# SQL Basics

## Overview

SQL (Structured Query Language) is a programming language used to communicate with relational databases.

In analytics engineering, SQL is primarily used for:

- Extracting data from databases
- Cleaning and transforming raw data
- Creating analytical datasets
- Building metrics and KPIs
- Validating data quality
- Supporting business intelligence reporting

SQL is the primary language used by Analytics Engineers because most analytical transformations happen inside databases and data warehouses.

---

# 1. Relational Database Concepts

A relational database stores data in structured tables.

A table consists of:

- Rows → Individual records
- Columns → Attributes describing each record
- Primary Key → Unique identifier for each record
- Foreign Key → Relationship between tables

Example:

## Customers Table

| customer_id | customer_name | country |
|---|---|---|
| 1 | John Smith | Ghana |
| 2 | Sarah Lee | Kenya |

## Orders Table

| order_id | customer_id | amount |
|---|---|---|
| 101 | 1 | 250 |
| 102 | 2 | 500 |

Relationship:

```
Customers
    |
    |
customer_id
    |
    |
Orders
```

---

# 2. SQL Statement Categories

SQL commands are grouped into different categories.

## DDL (Data Definition Language)

Used to define database structures.

Examples:

- CREATE
- ALTER
- DROP
- TRUNCATE

Example:

```sql
CREATE TABLE customers (
    customer_id INT,
    customer_name VARCHAR(100)
);
```

---

## DML (Data Manipulation Language)

Used to modify data inside tables.

Examples:

- INSERT
- UPDATE
- DELETE

Example:

```sql
UPDATE customers
SET country = 'Ghana'
WHERE customer_id = 1;
```

---

## DQL (Data Query Language)

Used to retrieve data.

Main command:

- SELECT

Example:

```sql
SELECT *
FROM customers;
```

---

# 3. SELECT Statement

The SELECT statement retrieves data from tables.

Basic syntax:

```sql
SELECT column_name
FROM table_name;
```

Example:

```sql
SELECT customer_name
FROM customers;
```

Result:

| customer_name |
|---|
| John Smith |
| Sarah Lee |

---

# Selecting Multiple Columns

```sql
SELECT
    customer_id,
    customer_name,
    country
FROM customers;
```

---

# Selecting All Columns

Use `*` to select every column.

```sql
SELECT *
FROM customers;
```

Best practice:

Avoid using `*` in production analytics models because:

- It can return unnecessary columns
- It makes models harder to maintain
- Schema changes can break downstream logic

Prefer:

```sql
SELECT
    customer_id,
    customer_name,
    country
FROM customers;
```

---

# 4. SQL Aliases

Aliases rename columns or tables.

## Column Alias

```sql
SELECT
    customer_name AS name
FROM customers;
```

Result:

| name |
|---|
| John Smith |

---

## Table Alias

Useful when joining tables.

Example:

```sql
SELECT
    c.customer_name
FROM customers AS c;
```

---

# 5. Filtering Data with WHERE

The WHERE clause filters records.

Syntax:

```sql
SELECT *
FROM table_name
WHERE condition;
```

Example:

```sql
SELECT *
FROM customers
WHERE country = 'Ghana';
```

---

# Comparison Operators

| Operator | Meaning |
|---|---|
| = | Equal |
| != | Not equal |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal |
| <= | Less than or equal |

Example:

```sql
SELECT *
FROM products
WHERE price > 500;
```

---

# Logical Operators

## AND

Both conditions must be true.

```sql
SELECT *
FROM customers
WHERE country = 'Ghana'
AND age >= 25;
```

---

## OR

Either condition can be true.

```sql
SELECT *
FROM customers
WHERE country = 'Ghana'
OR country = 'Nigeria';
```

---

## NOT

Negates a condition.

```sql
SELECT *
FROM customers
WHERE NOT country = 'Ghana';
```

---

# 6. Filtering with BETWEEN

Used for ranges.

Example:

```sql
SELECT *
FROM orders
WHERE amount BETWEEN 100 AND 500;
```

Equivalent:

```sql
amount >= 100
AND amount <= 500
```

---

# 7. Filtering with IN

Used when checking multiple values.

Example:

```sql
SELECT *
FROM customers
WHERE country IN ('Ghana','Nigeria','Kenya');
```

Equivalent:

```sql
country = 'Ghana'
OR country = 'Nigeria'
OR country = 'Kenya'
```

---

# 8. Pattern Matching with LIKE

Used for text searches.

## Starts With

```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'John%';
```

Matches:

- John Smith
- Johnny Lee

---

## Ends With

```sql
WHERE email LIKE '%gmail.com';
```

---

## Contains

```sql
WHERE product_name LIKE '%phone%';
```

---

# 9. Handling NULL Values

NULL represents missing or unknown data.

Check NULL:

```sql
SELECT *
FROM customers
WHERE email IS NULL;
```

Check non-null:

```sql
SELECT *
FROM customers
WHERE email IS NOT NULL;
```

Important:

This is incorrect:

```sql
WHERE email = NULL
```

NULL requires:

```sql
IS NULL
```

---

# 10. Sorting Data with ORDER BY

Sort results.

Ascending:

```sql
SELECT *
FROM products
ORDER BY price ASC;
```

Descending:

```sql
SELECT *
FROM products
ORDER BY price DESC;
```

---

# 11. Limiting Results

Used to restrict returned rows.

Example:

```sql
SELECT *
FROM products
LIMIT 10;
```

Common use cases:

- Previewing datasets
- Finding top records
- Debugging queries

---

# 12. Removing Duplicate Records

Use DISTINCT.

Example:

```sql
SELECT DISTINCT country
FROM customers;
```

Returns unique countries only.

---

# 13. CASE Statements

CASE allows conditional logic.

Syntax:

```sql
CASE
    WHEN condition THEN result
    ELSE result
END
```

Example:

```sql
SELECT
    customer_name,

    CASE
        WHEN age < 30 THEN 'Young'
        WHEN age >= 30 THEN 'Adult'
        ELSE 'Unknown'
    END AS customer_segment

FROM customers;
```

---

# 14. Aggregate Functions

Aggregate functions summarize data.

Common functions:

| Function | Purpose |
|---|---|
| COUNT() | Count records |
| SUM() | Add values |
| AVG() | Average |
| MIN() | Minimum |
| MAX() | Maximum |

---

## COUNT

```sql
SELECT COUNT(*) AS total_customers
FROM customers;
```

---

## SUM

```sql
SELECT SUM(amount) AS total_sales
FROM orders;
```

---

## AVG

```sql
SELECT AVG(order_value)
FROM orders;
```

---

## MIN and MAX

```sql
SELECT
    MIN(price),
    MAX(price)
FROM products;
```

---

# 15. GROUP BY

GROUP BY creates summaries by categories.

Example:

Total sales by country:

```sql
SELECT
    country,
    SUM(amount) AS total_sales

FROM orders

GROUP BY country;
```

Result:

| country | total_sales |
|-|-|
| Ghana | 5000 |
| Kenya | 7000 |

---

# 16. HAVING

HAVING filters aggregated results.

WHERE filters rows.

HAVING filters groups.

Example:

Find countries with sales above 10,000:

```sql
SELECT
    country,
    SUM(amount) AS sales

FROM orders

GROUP BY country

HAVING SUM(amount) > 10000;
```

---

# 17. SQL Execution Order

Although SQL is written in this order:

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

The database executes approximately:

```
FROM
 |
JOIN
 |
WHERE
 |
GROUP BY
 |
HAVING
 |
SELECT
 |
ORDER BY
 |
LIMIT
```

Understanding execution order helps debug SQL problems.

---

# 18. Best Practices for Analytics Engineers

## Use Explicit Columns

Avoid:

```sql
SELECT *
FROM orders;
```

Prefer:

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount

FROM orders;
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
    customer_id,
    total_orders
```

---

## Format SQL Clearly

Example:

```sql
SELECT
    customer_id,
    COUNT(*) AS total_orders

FROM orders

GROUP BY customer_id;
```

---

# SQL Basics Interview Questions

## Q1. Difference between WHERE and HAVING?

Answer:

WHERE filters individual rows before aggregation.

HAVING filters aggregated results after GROUP BY.

---

## Q2. Difference between COUNT(*) and COUNT(column)?

Answer:

COUNT(*) counts all rows.

COUNT(column) ignores NULL values.

---

## Q3. Why avoid SELECT *?

Answer:

- Performance issues
- Unnecessary data transfer
- Poor maintainability
- Schema change risks

---

## Q4. Difference between DELETE and TRUNCATE?

DELETE:

- Removes selected rows
- Supports WHERE condition

TRUNCATE:

- Removes all rows
- Faster operation

---

# Analytics Engineering Application

SQL fundamentals are used when building:

```
Raw Tables

        ↓

Staging Models

        ↓

Intermediate Models

        ↓

Fact & Dimension Tables

        ↓

BI Dashboards
```

Strong SQL skills are essential for:

- dbt development
- Data modeling
- KPI creation
- Dashboard reliability
- Data quality monitoring