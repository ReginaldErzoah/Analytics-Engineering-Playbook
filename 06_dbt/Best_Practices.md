# dbt Best Practices

## Overview

A successful dbt project is not only about writing SQL that works.

Professional analytics engineering requires:

- Clean project structure
- Maintainable models
- Reliable testing
- Good documentation
- Performance optimization
- Collaboration practices

The goal of dbt is to build trusted, scalable data products.

---

# 1. Follow a Layered Model Architecture

A recommended dbt structure:

```
models/

├── staging/

├── intermediate/

└── marts/
```

Each layer has a clear responsibility.

---

# Staging Layer

Purpose:

- Clean raw data
- Rename fields
- Standardize formats
- Cast data types

Example:

```
stg_customer_support_tickets
```

Should not contain:

- Complex business calculations
- Heavy aggregations

---

# Intermediate Layer

Purpose:

- Reusable transformations
- Complex joins
- Business logic preparation

Example:

```
int_ticket_performance
```

---

# Mart Layer

Purpose:

Create business-ready datasets.

Examples:

```
fact_ticket_metrics

dim_customers

dim_products
```

---

# 2. Use Clear Naming Conventions

Good names make projects easier to understand.

---

## Models

Use prefixes:

|Type|Prefix|Example|
|-|-|-|
|Staging|stg_|stg_orders|
|Intermediate|int_|int_customer_metrics|
|Dimension|dim_|dim_customers|
|Fact|fact_|fact_sales|

---

## Columns

Use business-friendly names.

Bad:

```
cust_id
```

Better:

```
customer_id
```

---

# 3. Keep Models Modular

Avoid large SQL files.

Bad:

```
customer_analysis.sql

3000 lines
```

Better:

```
stg_customers.sql

int_customer_orders.sql

dim_customers.sql
```

Benefits:

- Easier debugging
- Reusable logic
- Better testing

---

# 4. Use ref() Instead of Hardcoded Tables

Avoid:

```sql
select *

from analytics.customers
```

Use:

```sql
select *

from {{ ref(
'dim_customers'
) }}
```

Benefits:

- Creates lineage
- Handles dependencies
- Easier migrations

---

# 5. Use source() for Raw Data

Avoid:

```sql
from raw.customers
```

Use:

```sql
from {{ source(
'crm',
'customers'
) }}
```

Benefits:

- Documentation
- Freshness checks
- Data lineage

---

# 6. Test Important Data

Do not test everything blindly.

Focus on:

- Primary keys
- Foreign keys
- Important metrics
- Business rules

Example:

```yaml
columns:

- name: customer_id

  tests:

  - unique

  - not_null
```

---

# 7. Document Models and Metrics

Every important model should explain:

- Purpose
- Business meaning
- Column definitions

Example:

```yaml
description:

"Contains customer support ticket performance metrics."
```

---

# 8. Avoid Business Logic in BI Tools

Bad architecture:

```
Raw Data

 ↓

Power BI Calculations

 ↓

Dashboard
```

Problems:

- Logic duplicated
- Difficult testing
- Different dashboards show different numbers

---

Better:

```
Raw Data

 ↓

dbt Transformations

 ↓

Certified Models

 ↓

Power BI
```

---

# 9. Use Version Control

All dbt projects should use Git.

Example workflow:

```
Create branch

        ↓

Modify models

        ↓

Run tests

        ↓

Create pull request

        ↓

Review

        ↓

Merge
```

---

# 10. Write SQL That Is Easy to Read

Good:

```sql
select

customer_id,

count(*) as total_tickets

from tickets

group by customer_id
```

Avoid:

```sql
select customer_id,count(*)from tickets group by customer_id
```

---

# 11. Avoid SELECT *

Bad:

```sql
select *

from customers
```

Problems:

- Unexpected columns
- Performance issues
- Breaking changes

Better:

```sql
select

customer_id,

customer_name,

email

from customers
```

---

# 12. Use Incremental Models for Large Tables

For large datasets:

Avoid rebuilding everything.

Example:

Bad:

```
Process 500 million rows daily
```

Better:

```
Process only new records
```

Example:

```sql
{{ config(
materialized='incremental'
) }}
```

---

# 13. Optimize Model Performance

Consider:

- Reducing unnecessary joins
- Filtering early
- Selecting only required columns
- Using proper materializations

---

# 14. Maintain Documentation

Documentation should be updated when:

- Models change
- Metrics change
- Business definitions change

Outdated documentation creates distrust.

---

# 15. Use Packages Carefully

Packages provide reusable functionality.

Example:

```
dbt_utils
```

Benefits:

- Faster development
- Standardized logic

Avoid adding unnecessary packages.

---

# 16. Separate Technical and Business Logic

Technical transformations:

Example:

```
Convert timestamp

Rename column
```

Business logic:

Example:

```
Define active customer

Calculate KPI
```

Both should be clearly organized.

---

# 17. Create Reusable Metrics

Metrics should have one definition.

Example:

Customer Satisfaction Score:

```
Defined once in dbt

Used everywhere
```

Avoid:

```
Dashboard A calculates differently from Dashboard B
```

---

# 18. Monitor Data Quality

Production systems should monitor:

- Missing data
- Schema changes
- Freshness
- Failed tests

---

# 19. Review SQL Performance

Before production:

Check:

- Query runtime
- Table size
- Join efficiency

---

# 20. Build for Collaboration

A good dbt project allows another engineer to:

- Understand models
- Run the project
- Modify logic safely

---

# Example: Customer Support Analytics Project

Poor approach:

```
Raw CSV

↓

One huge SQL file

↓

Power BI Dashboard
```

Problems:

- No tests
- No documentation
- Difficult maintenance

---

Professional approach:

```
Raw Data

↓

sources.yml

↓

stg_customer_support_tickets

↓

int_ticket_performance

↓

fact_ticket_metrics

↓

dbt tests

↓

Power BI Dashboard
```

---

# Common Interview Questions

## What are dbt best practices?

They include modular models, testing, documentation, version control, and clear naming conventions.

---

## Why use staging, intermediate, and mart layers?

To separate responsibilities and create maintainable transformations.

---

## Why avoid logic in dashboards?

Because business logic becomes inconsistent and difficult to test.

---

## Why use ref()?

To create dependencies, lineage, and reliable model execution order.

---

# Key Takeaway

A professional dbt project should be:

✅ Modular  
✅ Tested  
✅ Documented  
✅ Version-controlled  
✅ Performant  
✅ Easy to understand  

Analytics engineering is not just transforming data — it is engineering reliable data products.