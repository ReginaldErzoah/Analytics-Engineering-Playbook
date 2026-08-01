# Fact Tables

## Overview

A fact table is a central component of dimensional data models that stores measurable business events.

Fact tables contain:

- Business transactions
- Metrics
- Foreign keys connecting dimensions
- Event timestamps

They are designed to answer analytical questions such as:

- How many sales occurred?
- How many support tickets were created?
- What was the average resolution time?
- How much revenue was generated?

---

# Fact Table Structure

A typical fact table contains:

```
Fact Table

├── Primary Event Identifier
│
├── Foreign Keys
│
└── Measures
```

Example:

```
fact_ticket_metrics
```

|Column|Type|Purpose|
|-|-|-|
|ticket_id|Key|Unique support event|
|customer_id|Foreign Key|Links to customer dimension|
|product_id|Foreign Key|Links to product dimension|
|date_id|Foreign Key|Links to date dimension|
|resolution_hours|Measure|Support efficiency metric|
|first_response_hours|Measure|Response performance metric|
|satisfaction_rating|Measure|Customer experience metric|

---

# Fact Table Characteristics

## 1. Stores Measurements

Facts contain numerical values that can be aggregated.

Examples:

```
revenue

quantity

cost

duration

rating

distance
```

Common operations:

```sql
SUM()

AVG()

COUNT()

MIN()

MAX()
```

---

## 2. Contains Foreign Keys

Fact tables connect to dimensions.

Example:

```
fact_sales

customer_id

product_id

date_id
```

Relationships:

```
fact_sales.customer_id

        ↓

dim_customer.customer_id
```

---

## 3. Contains Many Rows

Fact tables usually become the largest tables in a warehouse.

Example:

Ride-hailing platform:

```
One day:

10 million trips

One year:

billions of trips
```

This is why:

- Partitioning
- Incremental models
- Query optimization

are important.

---

# Fact Table Design Process

## Step 1: Identify the Business Process

Example:

Customer Support

Business event:

```
Customer creates support ticket
```

---

## Step 2: Define the Grain

Question:

> What does one row represent?

Example:

```
One row = One customer support ticket
```

---

## Step 3: Identify Dimensions

Ask:

What describes the event?

Example:

Ticket:

- Customer
- Product
- Channel
- Priority
- Date

---

## Step 4: Identify Facts

Ask:

What can be measured?

Example:

Ticket:

- Resolution time
- Response time
- Satisfaction score

---

# Types of Fact Tables

There are three major types:

1. Transaction Fact Tables
2. Periodic Snapshot Fact Tables
3. Accumulating Snapshot Fact Tables

---

# 1. Transaction Fact Tables

## Definition

A transaction fact table stores individual business events.

Each row represents a single transaction.

---

## Example: Sales

```
fact_sales
```

Grain:

```
One row = One customer purchase
```

Columns:

```
sale_id

customer_id

product_id

quantity

sales_amount

transaction_date
```

---

## Example: Customer Support

```
fact_ticket_metrics
```

Grain:

```
One row = One support ticket
```

Columns:

```
ticket_id

customer_id

product_id

resolution_hours

satisfaction_rating
```

---

# 2. Periodic Snapshot Fact Tables

## Definition

A periodic snapshot captures the state of a business process at regular intervals.

Examples:

- Daily
- Weekly
- Monthly

---

## Example

Customer support daily snapshot:

```
fact_support_daily_snapshot
```

Grain:

```
One row = One support team performance day
```

Columns:

```
date_id

open_tickets

closed_tickets

average_resolution_hours

average_satisfaction
```

---

## Use Cases

Useful for:

- Trend reporting
- Capacity planning
- Forecasting

Example:

Management question:

> How did support performance change over the last quarter?

---

# 3. Accumulating Snapshot Fact Tables

## Definition

Tracks a business process from beginning to completion.

The row is updated as milestones occur.

---

## Example: Support Ticket Lifecycle

Process:

```
Ticket Created

        ↓

First Response

        ↓

Resolution

        ↓

Customer Feedback
```

Table:

```
fact_ticket_lifecycle
```

Columns:

```
ticket_id

created_time

first_response_time

resolved_time

satisfaction_rating
```

---

# Additive, Semi-Additive, and Non-Additive Facts

Not all measures can be aggregated the same way.

---

# Additive Facts

Can be summed across all dimensions.

Examples:

```
sales_amount

quantity

cost
```

Example:

```sql
SUM(sales_amount)
```

---

# Semi-Additive Facts

Can be summed across some dimensions but not others.

Example:

Account balance.

Can sum by:

```
customer
```

Cannot sum by:

```
time
```

because balances represent states.

---

# Non-Additive Facts

Cannot be summed.

Examples:

```
average_rating

percentage

ratio
```

Instead use:

```sql
AVG()

COUNT()

```

---

# Fact Table Keys

## Natural Keys

Keys from source systems.

Example:

```
ticket_id = 10001
```

Advantages:

- Easy to understand

Disadvantages:

- May change
- System dependent

---

## Surrogate Keys

Generated warehouse keys.

Example:

```
ticket_key = abc123
```

Advantages:

- Stable
- Independent from source systems
- Better for modeling

---

# Fact Table Naming Convention

Common naming:

```
fact_<business_process>
```

Examples:

```
fact_sales

fact_orders

fact_tickets

fact_payments

fact_deliveries
```

---

# Fact Tables in dbt

Example:

```
models/

marts/

    fact_ticket_metrics.sql
```

Example:

```sql
{{ config(
    materialized='table'
) }}

SELECT

ticket_id,

customer_id,

product_id,

resolution_hours,

satisfaction_rating

FROM int_ticket_performance
```

---

# Testing Fact Tables

Important tests:

## Unique Keys

Ensure one row per event.

```yaml
tests:

- unique
```

---

## Not Null

Ensure required fields exist.

```yaml
tests:

- not_null
```

---

## Relationships

Ensure foreign keys exist.

Example:

```yaml
relationships:

to: ref('dim_customers')

field: customer_id
```

---

# Common Fact Table Problems

## Problem 1: Incorrect Grain

Symptoms:

- Duplicate metrics
- Inflated counts

Solution:

Define grain before building.

---

## Problem 2: Missing Dimensions

Symptoms:

- Limited analysis capability

Example:

Cannot analyze tickets by:

- Product
- Customer segment
- Region

Solution:

Add required dimensions.

---

## Problem 3: Too Many Columns

Symptoms:

- Difficult maintenance
- Confusing models

Solution:

Keep facts focused on measurements.

---

# Interview Questions

## What is a fact table?

A table containing measurable business events and metrics.

---

## What is the difference between a fact and dimension?

Facts measure events.

Dimensions describe events.

---

## Why are fact tables usually large?

Because they store every business event over time.

---

## What is grain?

The meaning of one row in a fact table.

---

## Why is grain important?

Because incorrect grain creates incorrect business metrics.

---

# Key Takeaway

Fact tables are the foundation of analytical reporting.

A well-designed fact table:

✅ Has a clear grain  
✅ Stores measurable events  
✅ Connects to dimensions  
✅ Supports reliable KPIs  
✅ Scales with business growth  

Analytics Engineers use fact tables to transform operational events into trusted business insights.