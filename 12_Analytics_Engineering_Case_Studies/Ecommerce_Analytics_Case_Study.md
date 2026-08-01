# E-commerce Analytics Case Study

## Overview

This case study demonstrates how an analytics engineer solves a real-world e-commerce analytics problem.

The goal is to transform raw business data into reliable analytics models that help stakeholders understand:

- Revenue performance
- Customer behavior
- Product performance
- Sales trends

---

# Business Context

## Company

**Velocity Bikes Ltd.**

Velocity Bikes is an online bicycle retailer that sells products across multiple regions.

The company collects data from:

- Customer registrations
- Product catalog
- Orders
- Transactions

Leadership wants better visibility into sales performance and customer behavior.

---

# Business Problem

The company currently stores operational data but struggles to answer important questions.

Examples:

- Which products generate the most revenue?
- Who are the most valuable customers?
- Which regions perform best?
- Are sales increasing or declining?
- What products should receive more marketing investment?

---

# Business Objectives

The analytics solution should help:

## Sales Team

Understand:

- Revenue trends
- Best-selling products
- Customer demand

---

## Marketing Team

Understand:

- Customer segments
- Buying behavior
- High-value customers

---

## Management

Monitor:

- Growth
- Profitability
- Business performance

---

# Available Data Sources

The company provides three main tables.

---

# Customers Table

Stores customer information.

Example:

|Column|Description|
|-|-|
|customer_id|Unique customer identifier|
|first_name|Customer first name|
|last_name|Customer last name|
|email|Customer email|
|city|Customer city|
|state|Customer location|

---

# Products Table

Stores product information.

Example:

|Column|Description|
|-|-|
|product_id|Unique product identifier|
|product_name|Product name|
|category|Product category|
|price|Selling price|

---

# Orders Table

Stores transaction information.

Example:

|Column|Description|
|-|-|
|order_id|Order identifier|
|customer_id|Customer who purchased|
|product_id|Purchased product|
|quantity|Number purchased|
|order_date|Transaction date|

---

# Data Challenges

Before analysis, several issues exist.

---

# 1. Duplicate Records

Problem:

```
Same customer appears multiple times
```

Solution:

Create uniqueness checks.

---

# 2. Missing Values

Examples:

```
Missing customer email

Missing product category
```

Solution:

Apply data validation rules.

---

# 3. Inconsistent Formats

Example:

```
Accra

ACCRA

accra
```

Solution:

Standardize values.

---

# Analytics Engineering Approach

The solution follows an ELT architecture.

```
Operational Database

        ↓

Raw Warehouse Tables

        ↓

Staging Models

        ↓

Analytics Models

        ↓

Dashboards
```

---

# Data Modeling

A dimensional model is created.

The main business process:

```
Customer Purchases Product
```

---

# Star Schema Design

Structure:

```
              Customers

                  |

Products ---- Sales Fact ---- Date

```

---

# Fact Table

## fact_sales

Stores measurable transactions.

Columns:

```
sale_id

customer_id

product_id

date_id

quantity

revenue
```

---

# Dimension Tables

## dim_customer

Stores customer information.

```
customer_id

customer_name

location
```

---

## dim_product

Stores product information.

```
product_id

product_name

category

price
```

---

## dim_date

Stores calendar information.

```
date_id

day

month

year

quarter
```

---

# dbt Transformation Layers

A typical dbt structure:

```
sources

↓

staging

↓

intermediate

↓

marts
```

---

# Staging Layer

Purpose:

Clean raw data.

Example:

Raw:

```
customer_name = " john smith "
```

Transformation:

```
customer_name = "John Smith"
```

---

# Intermediate Layer

Purpose:

Create reusable logic.

Examples:

- Customer orders
- Product sales calculations

---

# Mart Layer

Purpose:

Create business-ready tables.

Examples:

```
sales_summary

customer_metrics

product_performance
```

---

# Key Business Metrics

---

# 1. Total Revenue

Formula:

```
Revenue =
SUM(quantity × price)
```

Business Question:

"How much money did we generate?"

---

# 2. Total Orders

Formula:

```
COUNT(order_id)
```

Business Question:

"How many purchases occurred?"

---

# 3. Average Order Value

Formula:

```
AOV =
Total Revenue / Total Orders
```

Business Question:

"How much does a customer spend per order?"

---

# 4. Customer Lifetime Value

Formula:

```
CLV =
Total Customer Revenue Over Time
```

Business Question:

"Who are our most valuable customers?"

---

# 5. Product Revenue Contribution

Formula:

```
Product Revenue /

Total Revenue
```

Business Question:

"Which products drive business growth?"

---

# SQL Examples

## Total Revenue

```sql
SELECT

SUM(quantity * price) AS total_revenue

FROM orders;
```

---

## Top Products

```sql
SELECT

product_id,

SUM(quantity * price) AS revenue

FROM orders

GROUP BY product_id

ORDER BY revenue DESC

LIMIT 5;
```

---

## Top Customers

```sql
SELECT

customer_id,

SUM(quantity * price) AS spending

FROM orders

GROUP BY customer_id

ORDER BY spending DESC;
```

---

# Data Quality Tests

Examples:

## Unique Orders

Test:

```
order_id should be unique
```

---

## Valid Customers

Test:

```
Every order must have a customer
```

---

## Positive Revenue

Test:

```
Revenue cannot be negative
```

---

# Dashboard Requirements

A business dashboard should include:

---

# Sales Overview

Metrics:

- Total Revenue
- Total Orders
- Average Order Value
- Growth Rate

---

# Product Performance

Visuals:

- Top products
- Revenue by category
- Sales trends

---

# Customer Analytics

Visuals:

- Top customers
- Customer distribution
- Purchase frequency

---

# Business Insights Example

After analysis:

## Finding 1

A small number of products generate most revenue.

Recommendation:

Focus marketing investment on high-performing products.

---

## Finding 2

High-value customers contribute significant revenue.

Recommendation:

Create customer loyalty programs.

---

## Finding 3

Some regions have lower sales.

Recommendation:

Investigate regional demand and marketing opportunities.

---

# Analytics Engineering Deliverables

Final outputs:

```
Clean Data Models

+

Tested Metrics

+

Documentation

+

BI Dashboard

+

Business Insights
```

---

# Tools Used

## Data Transformation

- SQL
- dbt

---

## Storage

- PostgreSQL
- Snowflake
- BigQuery

---

## Visualization

- Power BI
- Tableau
- Looker

---

## Development

- Git
- GitHub

---

# Interview Discussion Points

## How would you design this analytics system?

Answer:

"I would implement an ELT architecture, load raw operational data into a warehouse, create staging models, build dimensional marts, add data quality tests, and expose metrics through dashboards."

---

## Why use a star schema?

Answer:

"Star schemas simplify analytical queries, improve performance, and make business metrics easier to understand."

---

## How do you ensure data quality?

Answer:

"I implement automated tests for uniqueness, completeness, relationships, and business rules."

---

# Key Takeaway

This case study demonstrates the complete analytics engineering lifecycle:

```
Business Problem

↓

Data Modeling

↓

SQL Transformations

↓

Quality Testing

↓

Metrics

↓

Business Insights
```

A strong analytics engineer does not only write queries — they build trusted data products that help organizations make better decisions.