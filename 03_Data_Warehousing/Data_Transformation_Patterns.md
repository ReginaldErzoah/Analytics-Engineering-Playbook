# Data Transformation Patterns

## Overview

Data transformation is the process of converting raw data into structured, meaningful, and analytics-ready datasets.

In analytics engineering, transformations are responsible for:

- Cleaning data
- Standardizing formats
- Applying business logic
- Creating metrics
- Preparing datasets for reporting

A strong transformation layer creates reliable data products.

---

# Why Data Transformation Matters

Raw data is usually not ready for analysis.

Example:

Raw customer support data:

|ticket_id|created_at|status|agent|
|-|-|-|-|
|101|2026-01-01 09:30|closed|john|

Problems:

- Inconsistent formats
- Missing values
- Technical column names
- No business metrics

Transformation creates:

|ticket_id|resolution_hours|status_category|
|-|-|-|
|101|4.5|Resolved|

---

# Transformation Layers

Modern analytics engineering usually follows:

```
Raw Data

     ↓

Staging Layer

     ↓

Intermediate Layer

     ↓

Mart Layer
```

---

# 1. Staging Transformations

The staging layer prepares raw data.

Responsibilities:

- Rename columns
- Cast data types
- Remove unnecessary fields
- Standardize values

Example:

Raw:

```sql
customer_name
```

Transform:

```sql
customer_full_name
```

---

Example:

```sql
select

id as customer_id,

trim(name) as customer_name,

cast(created_at as date) as signup_date

from raw_customers
```

---

# 2. Intermediate Transformations

Intermediate models combine cleaned datasets.

Used for:

- Complex joins
- Reusable calculations
- Business logic preparation

Example:

```
stg_orders

+

stg_customers

↓

int_customer_orders
```

---

Example:

```sql
select

c.customer_id,

count(o.order_id) as total_orders

from customers c

left join orders o

on c.customer_id = o.customer_id

group by c.customer_id
```

---

# 3. Mart Transformations

Mart models serve business users.

Examples:

```
fact_sales

dim_customer

fact_ticket_metrics
```

They contain:

- KPIs
- Business metrics
- Reporting logic

---

# Common Transformation Patterns

---

# 1. Filtering Records

Removing unnecessary records.

Example:

Remove cancelled orders:

```sql
select *

from orders

where status != 'cancelled'
```

---

# 2. Column Renaming

Making technical fields business-friendly.

Before:

```
cust_nm
```

After:

```
customer_name
```

---

# 3. Data Type Conversion

Ensuring correct data types.

Example:

```sql
cast(order_date as date)
```

Common conversions:

- String → Date
- String → Number
- Integer → Decimal

---

# 4. Standardization

Making values consistent.

Example:

Before:

```
USA

United States

US
```

After:

```
United States
```

---

# 5. Deduplication

Removing duplicate records.

Example:

Problem:

```
customer_id

101

101

101
```

Solution:

```sql
row_number()
```

Example:

```sql
select *

from (

select *,

row_number() over(

partition by customer_id

order by created_at desc

) as rn

from customers

)

where rn = 1
```

---

# 6. Joining Data

Combining multiple datasets.

Example:

```
Customers

+

Orders

↓

Customer Sales
```

Example:

```sql
select

c.customer_name,

o.order_amount

from customers c

join orders o

on c.customer_id=o.customer_id
```

---

# 7. Aggregation

Creating summary metrics.

Examples:

- Total sales
- Average order value
- Ticket resolution time

Example:

```sql
select

customer_id,

sum(amount) as total_sales

from orders

group by customer_id
```

---

# 8. Creating Derived Metrics

Creating new business measures.

Example:

Customer lifetime value:

```
total_orders × average_order_value
```

Example:

```sql
sum(order_amount)

as customer_lifetime_value
```

---

# 9. Slowly Changing Dimensions

Managing changes in dimension data.

Example:

Customer changes address:

Old:

```
Accra
```

New:

```
Kumasi
```

Strategies:

- SCD Type 1
- SCD Type 2

---

# 10. Incremental Transformations

Instead of rebuilding everything, process only new records.

Example:

Instead of:

```
Process 1 billion rows
```

Process:

```
Yesterday's new records
```

Common in:

- Large fact tables
- Event data

---

# Common dbt Materializations

dbt supports different ways to store models.

---

## View

Creates database view.

Example:

```
stg_customers
```

Advantages:

- Always updated
- No storage duplication

---

## Table

Creates physical table.

Example:

```
fact_sales
```

Advantages:

- Faster queries
- Better performance

---

## Incremental

Updates only new data.

Example:

```
large_transactions
```

Advantages:

- Efficient
- Faster processing

---

## Ephemeral

Temporary SQL logic.

Used for:

- Reusable transformations
- Avoiding unnecessary tables

---

# Transformation Best Practices

## 1. Keep Transformations Simple

Avoid:

```
One huge SQL model
```

Prefer:

```
Multiple small models
```

---

## 2. Separate Cleaning From Business Logic

Cleaning:

```
Remove duplicates

Fix data types
```

Business logic:

```
Calculate KPIs
```

---

## 3. Avoid Hardcoded Values

Bad:

```sql
where country='Ghana'
```

Better:

Use:

- Reference tables
- Seeds
- Configurations

---

## 4. Test Transformations

Examples:

```yaml
tests:

- unique

- not_null
```

---

## 5. Document Business Logic

Explain:

- Why calculations exist
- How metrics are defined

---

# Example: Customer Support Analytics Transformation

Raw:

```
tickets.csv
```

↓

Staging:

```
stg_tickets

- Clean dates
- Standardize status
- Rename fields
```

↓

Intermediate:

```
int_ticket_resolution

- Calculate resolution duration
- Join customer information
```

↓

Mart:

```
fact_ticket_metrics

- Resolution time KPI
- Response KPI
- Satisfaction metrics
```

↓

Dashboard:

```
Power BI Customer Support Dashboard
```

---

# Interview Questions

## What are transformation layers?

They separate raw cleaning, reusable logic, and business-ready datasets.

---

## Why separate staging and marts?

To improve maintainability, testing, and reuse.

---

## What is an incremental model?

A model that processes only new or changed records instead of rebuilding everything.

---

## Why avoid business logic in dashboards?

Because calculations become inconsistent and difficult to manage.

---

# Key Takeaway

Data transformation is where raw data becomes a trusted analytical asset.

A professional transformation workflow is:

```
Clean

↓

Standardize

↓

Combine

↓

Calculate Metrics

↓

Test

↓

Document

↓

Serve Analytics
```

Strong transformations create reliable business decisions.