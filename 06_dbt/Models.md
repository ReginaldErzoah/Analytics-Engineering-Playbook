# dbt Models

## Overview

Models are the core building blocks of dbt projects.

A dbt model is a SQL file that defines how raw data is transformed into analytical datasets.

In simple terms:

```
SQL file

        ↓

dbt execution

        ↓

Database table or view

        ↓

Analytics / BI dashboard
```

Models represent the transformation logic of an analytics engineering workflow.

---

# Why Models Matter

Without models, analysts often write complex SQL directly inside dashboards.

This creates problems:

- Duplicate logic
- Inconsistent metrics
- Difficult maintenance
- Poor documentation
- Limited testing

dbt models move business logic into a centralized transformation layer.

---

# Example Transformation Flow

Customer Support Analytics:

```
Raw Data

customer_support_tickets

          ↓

Staging Model

stg_customer_support_tickets

          ↓

Intermediate Model

int_ticket_performance

          ↓

Mart Models

fact_ticket_metrics

dim_customers

dim_products

          ↓

Power BI Dashboard
```

---

# Types of dbt Models

Most analytics engineering projects use three main layers:

1. Staging models
2. Intermediate models
3. Mart models

---

# 1. Staging Models

Staging models are the first transformation layer.

Purpose:

- Clean raw data
- Rename columns
- Standardize formats
- Cast data types
- Remove obvious quality issues

They usually have the prefix:

```
stg_
```

Example:

```
stg_customer_support_tickets.sql
```

---

## Example Staging Model

Raw table:

```
raw_customer_support_tickets
```

Columns:

```
Customer Email

Ticket Status

Purchase Date
```

Transformation:

```sql
select

    lower(customer_email) as customer_email,

    ticket_status,

    cast(purchase_date as date) as purchase_date

from raw_customer_support_tickets
```

Result:

```
customer_email

ticket_status

purchase_date
```

---

# Staging Model Best Practices

## Keep Logic Simple

Avoid complex calculations.

Good:

```
Rename columns

Clean values

Fix data types
```

Avoid:

```
Business KPIs

Aggregations

Complex joins
```

---

## One Source Per Staging Model

Recommended:

```
stg_customers

stg_orders

stg_products
```

Instead of:

```
stg_everything.sql
```

---

# 2. Intermediate Models

Intermediate models contain reusable business logic.

They sit between staging and marts.

Naming convention:

```
int_
```

Example:

```
int_ticket_performance.sql
```

---

# Purpose of Intermediate Models

Used for:

- Complex transformations
- Multiple joins
- Reusable calculations
- Preparing data for marts

---

# Example

Staging:

```
stg_customer_support_tickets
```

Contains:

```
ticket_id

customer_email

first_response_time

resolution_time
```

Intermediate model calculates:

```
resolution_hours

response_time_quality_flag

clean timestamps
```

---

Example:

```sql
select

ticket_id,

date_diff(
'hour',
first_response_time,
resolution_time
) as resolution_hours

from {{ ref(
'stg_customer_support_tickets'
) }}
```

---

# 3. Mart Models

Mart models are final business-ready datasets.

They are consumed by:

- Analysts
- BI tools
- Executives

Common prefixes:

```
fact_

dim_
```

---

# Dimension Models

Dimensions describe entities.

Examples:

```
dim_customers

dim_products

dim_dates
```

Contains:

```
Customer attributes

Product information

Categories
```

---

# Fact Models

Facts contain measurable events.

Examples:

```
fact_sales

fact_orders

fact_ticket_metrics
```

Contains:

```
Revenue

Quantity

Resolution Hours

Satisfaction Score
```

---

# Model Dependencies

dbt uses:

```
ref()
```

to understand relationships.

Example:

```sql
from {{ ref(
'stg_customers'
) }}
```

Creates:

```
stg_customers

        ↓

dim_customers
```

---

# Model Materializations

Models can be created as:

## Table

Stored physically.

Example:

```sql
{{ config(
materialized='table'
) }}
```

Use for:

- Large analytical tables
- Reporting models

---

## View

Virtual table.

Example:

```sql
{{ config(
materialized='view'
) }}
```

Use for:

- Simple transformations
- Frequently changing data

---

## Incremental

Processes only new records.

Example:

```sql
{{ config(
materialized='incremental'
) }}
```

Use for:

- Large datasets
- Event data

---

## Ephemeral

Temporary SQL logic.

Use for:

- Small reusable transformations

---

# Model Configuration

Models can be configured inside SQL:

Example:

```sql
{{ config(
    materialized='table',
    schema='analytics'
) }}
```

---

Or in:

```
dbt_project.yml
```

Example:

```yaml
models:

 customer_project:

    staging:

      materialized: view

    marts:

      materialized: table
```

---

# Model Documentation

Models should include documentation.

Example:

```yaml
models:

- name: fact_ticket_metrics

  description:

    "Customer support ticket performance metrics"
```

---

# Model Testing

Models should contain tests.

Example:

```yaml
columns:

- name: ticket_id

  tests:

  - unique

  - not_null
```

---

# Model Naming Convention

Recommended:

|Layer|Prefix|Example|
|-|-|-|
|Staging|stg_|stg_orders|
|Intermediate|int_|int_customer_metrics|
|Dimension|dim_|dim_customers|
|Fact|fact_|fact_sales|

---

# Model Organization Example

```
models/

├── staging/

│
├── stg_customers.sql

│
├── stg_orders.sql


├── intermediate/

│
├── int_customer_orders.sql


└── marts/

    ├── dim_customers.sql

    ├── dim_products.sql

    └── fact_orders.sql
```

---

# Common Model Mistakes

## 1. Creating Huge SQL Files

Bad:

```
one_model.sql

5000 lines
```

Better:

```
multiple modular models
```

---

## 2. Putting Business Logic in BI Tools

Bad:

```
Power BI calculates everything
```

Better:

```
dbt calculates metrics

BI visualizes results
```

---

## 3. Missing Tests

Untested models create unreliable analytics.

---

## 4. Poor Naming

Bad:

```
table1.sql
```

Good:

```
fact_ticket_metrics.sql
```

---

# Example: Customer Support Analytics Project

Model structure:

```
models/

├── staging/

│   └── stg_customer_support_tickets.sql


├── intermediate/

│   └── int_ticket_performance.sql


└── marts/

    ├── dim_customers.sql

    ├── dim_products.sql

    └── fact_ticket_metrics.sql
```

---

# Interview Questions

## What is a dbt model?

A SQL file containing transformation logic that dbt materializes into a database object.

---

## What is the difference between staging and mart models?

Staging cleans source data, while marts create business-ready analytical datasets.

---

## Why use intermediate models?

To separate complex reusable logic from final reporting models.

---

## Why should metrics be created in dbt instead of dashboards?

To ensure consistent definitions across all reporting tools.

---

# Key Takeaway

dbt models are the foundation of analytics engineering.

A strong model design creates:

✅ Clean data layers  
✅ Reusable transformations  
✅ Trusted metrics  
✅ Better dashboards  
✅ Easier collaboration  

Good analytics engineers do not just write SQL — they design maintainable data products.