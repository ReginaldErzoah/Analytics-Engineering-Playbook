# Customer Analytics Case Study

## Overview

This case study demonstrates how analytics engineers use data modeling, SQL transformations, and business metrics to understand customer behavior.

The goal is to build a customer analytics system that helps a business answer:

- Who are our customers?
- Which customers generate the most value?
- How often do customers purchase?
- Which customers are likely to leave?
- How can we improve retention?

---

# Business Context

## Company

**Nova Retail Group**

Nova Retail Group is a growing retail company operating through online and physical channels.

The company collects customer information from:

- Website purchases
- Mobile applications
- Customer accounts
- Marketing campaigns

---

# Business Problem

The company has customer data but lacks visibility into customer behavior.

Leadership wants answers to questions such as:

- Who are our highest-value customers?
- Which customers purchase frequently?
- How many customers are returning?
- Which customers have become inactive?
- What customer segments exist?

---

# Business Objectives

The analytics solution should support:

## Marketing Team

Understand:

- Customer segments
- Campaign targeting
- Customer engagement

---

## Sales Team

Understand:

- Purchase patterns
- High-value customers
- Revenue opportunities

---

## Management

Monitor:

- Retention
- Customer growth
- Customer profitability

---

# Available Data Sources

The company has several datasets.

---

# Customers Table

Contains customer profiles.

Example:

|Column|Description|
|-|-|
|customer_id|Unique customer identifier|
|name|Customer name|
|email|Customer email|
|gender|Customer demographic|
|location|Customer location|
|registration_date|Account creation date|

---

# Orders Table

Contains purchase transactions.

Example:

|Column|Description|
|-|-|
|order_id|Order identifier|
|customer_id|Customer identifier|
|order_date|Purchase date|
|amount|Order value|

---

# Products Table

Contains purchased products.

Example:

|Column|Description|
|-|-|
|product_id|Product identifier|
|category|Product category|
|price|Product price|

---

# Marketing Table

Contains campaign information.

Example:

|Column|Description|
|-|-|
|campaign_id|Campaign identifier|
|customer_id|Target customer|
|channel|Marketing channel|
|conversion|Purchase outcome|

---

# Data Challenges

Customer data often contains quality issues.

---

# Duplicate Customers

Problem:

```
John Smith

john.smith@email.com

Same customer
```

Solution:

Apply identity resolution rules.

---

# Missing Customer Information

Examples:

```
Missing location

Missing email
```

Solution:

Create validation checks.

---

# Inconsistent Customer Data

Example:

```
USA

United States

US
```

Solution:

Standardize values.

---

# Analytics Engineering Architecture

The solution follows an ELT approach.

```
Customer Systems

        ↓

Raw Warehouse Tables

        ↓

Staging Models

        ↓

Customer Analytics Models

        ↓

Dashboards
```

---

# Data Modeling

The customer analytics model contains:

```
Customer Dimension

        |

Customer Activity Fact

        |

Transaction Fact
```

---

# Customer Dimension

## dim_customer

Stores customer attributes.

Example:

```
customer_id

name

location

registration_date

customer_segment
```

---

# Transaction Fact

## fact_transactions

Stores purchase activity.

Example:

```
transaction_id

customer_id

date_id

amount

product_id
```

---

# Customer Activity Fact

Stores customer behavior.

Example:

```
customer_id

number_of_orders

last_purchase_date

total_spend
```

---

# Customer Metrics

Analytics engineers create metrics that describe customer behavior.

---

# 1. Total Customers

Formula:

```
COUNT(customer_id)
```

Business Question:

"How many customers do we have?"

---

# 2. Active Customers

Definition:

Customers who purchased within a defined period.

Example:

```
Purchased within last 90 days
```

---

# 3. Customer Retention Rate

Formula:

```
Customers who returned /

Customers from previous period
```

---

# 4. Purchase Frequency

Formula:

```
Number of Orders /

Number of Customers
```

---

# 5. Average Customer Spend

Formula:

```
Total Revenue /

Number of Customers
```

---

# 6. Customer Lifetime Value (CLV)

Formula:

```
Average Purchase Value

×

Purchase Frequency

×

Customer Lifespan
```

---

# Customer Segmentation

Customer segmentation groups customers based on behavior.

Common segments:

```
High Value Customers

Regular Customers

New Customers

Inactive Customers
```

---

# RFM Analysis

A common customer analytics framework.

RFM stands for:

```
Recency

Frequency

Monetary Value
```

---

# Recency

Measures:

"When did the customer last purchase?"

Example:

```
Last purchase:
5 days ago
```

---

# Frequency

Measures:

"How often does the customer buy?"

Example:

```
20 purchases/year
```

---

# Monetary Value

Measures:

"How much does the customer spend?"

Example:

```
$5,000 lifetime spend
```

---

# RFM Customer Segments

Example:

## Champions

Characteristics:

- Recent buyers
- Frequent purchases
- High spending

---

## Loyal Customers

Characteristics:

- Regular purchases
- Strong relationship

---

## At Risk Customers

Characteristics:

- Previously valuable
- Long period without purchase

---

## New Customers

Characteristics:

- Recent first purchase

---

# SQL Examples

## Total Customer Spending

```sql
SELECT

customer_id,

SUM(amount) AS total_spend

FROM orders

GROUP BY customer_id;
```

---

## Purchase Frequency

```sql
SELECT

customer_id,

COUNT(order_id) AS order_count

FROM orders

GROUP BY customer_id;
```

---

## Last Purchase Date

```sql
SELECT

customer_id,

MAX(order_date) AS last_purchase

FROM orders

GROUP BY customer_id;
```

---

# Customer Analytics dbt Models

Example structure:

```
models/

staging/

    stg_customers.sql

    stg_orders.sql


intermediate/

    customer_orders.sql


marts/

    customer_metrics.sql
```

---

# Data Quality Tests

Important tests:

---

## Customer ID Uniqueness

Rule:

```
customer_id must be unique
```

---

## Valid Orders

Rule:

```
Every order must belong to a customer
```

---

## Positive Spending

Rule:

```
Customer spending cannot be negative
```

---

## Freshness Tests

Rule:

```
Customer data updated daily
```

---

# Dashboard Requirements

A customer analytics dashboard should include:

---

# Customer Overview

Metrics:

- Total customers
- New customers
- Active customers
- Retention rate

---

# Customer Segmentation

Visuals:

- Customer segments
- Revenue by segment
- Customer distribution

---

# Customer Behavior

Visuals:

- Purchase frequency
- Average order value
- Customer trends

---

# Business Insights Example

## Finding 1

High-value customers contribute most revenue.

Recommendation:

Create loyalty programs.

---

## Finding 2

Many customers purchase once and never return.

Recommendation:

Improve retention campaigns.

---

## Finding 3

Inactive customers represent lost revenue opportunities.

Recommendation:

Launch reactivation campaigns.

---

# Analytics Engineering Deliverables

The final solution includes:

```
Customer Data Models

+

Automated Tests

+

Customer Metrics Layer

+

Dashboard

+

Business Recommendations
```

---

# Tools Used

## Transformation

- SQL
- dbt

---

## Data Warehouse

- Snowflake
- BigQuery
- PostgreSQL

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

## How would you analyze customer behavior?

Answer:

"I would create customer-level analytical models using transaction history, calculate metrics such as CLV, retention, and RFM scores, then build dashboards to support customer strategy."

---

## How would you identify customers at risk?

Answer:

"I would analyze purchase recency, frequency decline, and spending changes to identify customers showing signs of inactivity."

---

# Key Takeaway

Customer analytics transforms transactional data into customer intelligence.

The analytics engineering process:

```
Raw Customer Data

↓

Data Models

↓

Customer Metrics

↓

Segmentation

↓

Business Decisions
```

A strong customer analytics system helps organizations increase retention, improve marketing, and maximize customer value.