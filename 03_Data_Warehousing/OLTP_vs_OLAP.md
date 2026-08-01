# OLTP vs OLAP

## Overview

Understanding the difference between OLTP and OLAP systems is fundamental in analytics engineering.

Organizations typically operate two different types of data systems:

- Systems that run daily business operations
- Systems that analyze business performance

These serve different purposes and require different designs.

---

# OLTP (Online Transaction Processing)

## Definition

OLTP systems are designed to manage day-to-day business transactions.

They support applications where users continuously create, update, and retrieve operational data.

Examples:

- Banking transactions
- Online purchases
- Customer registrations
- Ride bookings
- Food delivery orders

---

# Characteristics of OLTP Systems

## 1. Transaction-Focused

OLTP systems handle individual business events.

Example:

A customer places an order:

```
INSERT new order record

UPDATE inventory

PROCESS payment
```

---

## 2. High Number of Small Queries

OLTP systems receive many simple queries.

Examples:

```sql
SELECT customer_name
FROM customers
WHERE customer_id = 101;
```

```sql
UPDATE orders
SET status = 'completed'
WHERE order_id = 5001;
```

---

## 3. Current Data

OLTP systems usually contain the latest operational state.

Example:

Customer profile:

```
Current address:
Accra
```

The previous address may be overwritten.

---

## 4. Highly Normalized

OLTP databases reduce duplication through normalization.

Example:

Instead of storing:

```
Order

Customer Name

Customer Email

Product Name
```

repeatedly:

Tables are separated:

```
Customers

Orders

Products
```

---

# Examples of OLTP Systems

|System|Example|
|-|-|
|E-commerce|Order management system|
|Banking|Core banking database|
|CRM|Customer management platform|
|ERP|Enterprise resource planning|
|Applications|User databases|

Common technologies:

```
PostgreSQL

MySQL

SQL Server

Oracle Database
```

---

# OLAP (Online Analytical Processing)

## Definition

OLAP systems are designed for analyzing large volumes of historical data.

They support reporting, dashboards, and business intelligence.

Examples:

- Sales analysis
- Customer behavior analysis
- Financial reporting
- Performance monitoring

---

# Characteristics of OLAP Systems

## 1. Analysis-Focused

OLAP answers business questions.

Examples:

```
What were total sales last year?

Which products performed best?

Why did customer satisfaction decline?
```

---

## 2. Complex Queries

OLAP queries often involve:

- Aggregations
- Joins
- Filtering
- Time comparisons

Example:

```sql
SELECT
    product_category,
    SUM(revenue)

FROM sales

GROUP BY product_category;
```

---

## 3. Historical Data

OLAP systems preserve history.

Example:

Customer locations:

```
2024:
Accra

2025:
Kumasi

2026:
London
```

---

## 4. Denormalized Design

OLAP systems commonly use dimensional models.

Example:

```
fact_sales

dim_customer

dim_product

dim_date
```

This improves analytical performance.

---

# Examples of OLAP Systems

|Technology|Type|
|-|-|
|Snowflake|Cloud Data Warehouse|
|BigQuery|Cloud Data Warehouse|
|Amazon Redshift|Cloud Data Warehouse|
|DuckDB|Analytical Database|
|Databricks|Lakehouse Platform|

---

# OLTP vs OLAP Comparison

|Category|OLTP|OLAP|
|-|-|-|
|Purpose|Run business operations|Analyze business performance|
|Users|Applications and operational users|Analysts and decision makers|
|Data|Current data|Historical data|
|Queries|Simple and frequent|Complex and analytical|
|Updates|Constant inserts and updates|Batch/incremental loads|
|Design|Normalized|Dimensional/denormalized|
|Optimization|Fast transactions|Fast analysis|
|Examples|Banking system|Sales dashboard|

---

# Data Flow Between OLTP and OLAP

A typical architecture:

```
              OLTP Systems

        CRM
        ERP
        Applications

              |

              ↓

        Data Pipeline

              |

              ↓

          Data Warehouse

              |

              ↓

             OLAP

              |

              ↓

       BI Dashboards
```

---

# Example: Customer Support Analytics

## OLTP System

Customer support application stores:

```
ticket_id

customer_email

issue_description

ticket_status

created_timestamp
```

Purpose:

Support agents resolve tickets.

---

## OLAP System

Analytics warehouse stores:

```
fact_ticket_metrics

dim_customers

dim_products
```

Purpose:

Analyze:

- Average resolution time
- Ticket volume trends
- Customer satisfaction
- Agent performance

---

# Why Separate OLTP and OLAP?

## Performance

Heavy analytical queries can slow transactional applications.

Example:

Running:

```sql
SELECT *
FROM millions_of_orders
```

on a production database can affect customers.

---

## Data Consistency

A warehouse provides:

- Cleaned data
- Standardized metrics
- Validated reporting

---

## Historical Analysis

Operational systems often overwrite data.

Warehouses preserve history.

---

# Analytics Engineering Perspective

Analytics engineers usually work closer to OLAP systems.

Responsibilities include:

```
Raw warehouse data

        ↓

SQL transformations

        ↓

dbt models

        ↓

Business-ready datasets
```

---

# Interview Questions

## What is OLTP?

A system optimized for managing daily transactions.

---

## What is OLAP?

A system optimized for analytical queries and reporting.

---

## Why should analytics not run on OLTP databases?

Because analytical workloads can impact transactional performance.

---

## Why are OLAP systems often denormalized?

Because analytical queries benefit from fewer joins and simpler structures.

---

## Can the same database support OLTP and OLAP?

Technically yes, but separating workloads is usually better for scalability and performance.

---

# Key Takeaway

OLTP systems help businesses operate.

OLAP systems help businesses understand performance.

Analytics engineers transform OLTP-generated data into OLAP-ready models that support reporting, dashboards, and strategic decisions.