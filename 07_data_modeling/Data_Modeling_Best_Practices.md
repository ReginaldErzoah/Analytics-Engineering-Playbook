# Data Modeling Best Practices

## Overview

Data modeling is the process of designing the structure, relationships, and organization of data so that it can be stored, transformed, analyzed, and reported efficiently.

For analytics engineering, good data modeling creates a reliable bridge between:

```
Raw Data

      ↓

Cleaned Data

      ↓

Business Logic

      ↓

Analytics & Reporting
```

A well-designed data model ensures:

- Accurate metrics
- Consistent reporting
- Faster queries
- Easier maintenance
- Better collaboration between technical and business teams

---

# Principles of Good Data Modeling

## 1. Understand the Business Process First

Before creating tables, understand:

- What business event is being measured?
- What questions should analytics answer?
- What decisions will users make?

Example:

Customer Support Analytics:

Business process:

```
Customer submits support ticket
```

Questions:

- How many tickets are created?
- How long does resolution take?
- Which products create the most issues?
- How satisfied are customers?

---

# 2. Define the Grain Clearly

Grain defines exactly what one row represents.

This is one of the most important concepts in dimensional modeling.

Example:

Fact table:

```
fact_ticket_metrics
```

Grain:

```
One row represents one customer support ticket
```

Meaning:

```
ticket_id = 1001
```

represents one unique support interaction.

---

## Why Grain Matters

Incorrect grain causes:

- Duplicate records
- Incorrect calculations
- Inflated metrics

Example:

Wrong:

```
One row = customer + ticket
```

when customers can have multiple tickets.

Correct:

```
One row = one ticket
```

---

# 3. Separate Facts and Dimensions

A common analytics modeling pattern:

```
              Dimensions


                 ↓


            Fact Table


                 ↑


              Dimensions

```

Facts:

Store measurements.

Examples:

```
sales_amount

resolution_hours

quantity

cost

```

Dimensions:

Store descriptions.

Examples:

```
customer_name

product_category

region

date

```

---

# 4. Use Surrogate Keys

A surrogate key is a generated identifier used inside analytical models.

Example:

Natural key:

```
customer_email
```

Surrogate key:

```
customer_id = abc123
```

Generated using:

```sql
dbt_utils.generate_surrogate_key()
```

Example:

```sql
{{ dbt_utils.generate_surrogate_key([
'customer_email',
'customer_name'
]) }}
```

---

## Benefits of Surrogate Keys

They provide:

- Stable relationships
- Better joins
- Protection from changing business identifiers
- Support for SCD Type 2

---

# 5. Keep Raw Data Untouched

Never directly modify source data.

Recommended architecture:

```
Source

   ↓

Staging

   ↓

Intermediate

   ↓

Marts

```

Example dbt structure:

```
models/

├── staging/

├── intermediate/

└── marts/

```

---

# 6. Use Layered Modeling

Analytics engineering commonly follows a layered approach.

---

# Staging Layer

Purpose:

Clean and standardize source data.

Tasks:

- Rename columns
- Cast data types
- Remove obvious errors
- Standardize formats

Example:

```
stg_customer_support_tickets
```

---

# Intermediate Layer

Purpose:

Apply reusable business logic.

Tasks:

- Complex transformations
- Joins
- Calculations

Example:

```
int_ticket_performance
```

---

# Mart Layer

Purpose:

Create business-ready models.

Examples:

```
fact_ticket_metrics

dim_customers

dim_products
```

Used directly by:

- Analysts
- BI tools
- Stakeholders

---

# 7. Avoid Duplicate Business Logic

Bad approach:

Revenue calculation repeated in:

```
Power BI

SQL queries

Python scripts

Excel reports
```

Problems:

- Different numbers
- Lack of trust

Better:

Create one trusted metric:

```
fact_sales.total_revenue
```

and reuse everywhere.

---

# 8. Build Data Quality Checks

Reliable models require testing.

Common tests:

## Uniqueness

Example:

```
customer_id must be unique
```

---

## Not Null

Example:

```
ticket_id cannot be empty
```

---

## Accepted Values

Example:

```
priority:

High

Medium

Low

Critical
```

---

## Relationships

Example:

Every ticket must have a valid customer.

```
fact_ticket.customer_id

↓

dim_customer.customer_id
```

---

# 9. Document Your Models

Documentation improves:

- Team collaboration
- Maintenance
- Data discovery

Example dbt documentation:

```yaml
columns:

  - name: resolution_hours

    description: "Total hours required to resolve a customer ticket"
```

---

# 10. Design for Analytics Users

A good model should be understandable.

Avoid:

```
customer_master_table_final_v2
```

Prefer:

```
dim_customer
```

Use:

- Clear names
- Consistent naming conventions
- Business-friendly definitions

---

# 11. Optimize Query Performance

Techniques:

## Reduce unnecessary joins

Avoid:

```
10-table joins
```

when:

```
fact + dimension
```

is enough.

---

## Filter Early

Instead of:

```sql
SELECT *

FROM huge_table

WHERE year = 2026
```

Apply filters during transformations.

---

## Use Appropriate Materializations

dbt options:

## View

Good for:

- Lightweight transformations

---

## Table

Good for:

- Frequently queried models

---

## Incremental

Good for:

- Large datasets

---

Example:

```sql
{{ config(
materialized='incremental'
) }}
```

---

# 12. Naming Conventions

Recommended:

## Staging

Prefix:

```
stg_
```

Example:

```
stg_orders
```

---

## Intermediate

Prefix:

```
int_
```

Example:

```
int_customer_orders
```

---

## Dimensions

Prefix:

```
dim_
```

Example:

```
dim_customer
```

---

## Facts

Prefix:

```
fact_
```

Example:

```
fact_sales
```

---

# 13. Handle Changing Data Correctly

Use SCD strategies.

Example:

Customer address changes.

Options:

Type 1:

```
Overwrite
```

Type 2:

```
Preserve history
```

---

# 14. Think About Data Lineage

Every metric should have a clear origin.

Example:

Dashboard KPI:

```
Average Resolution Time
```

Lineage:

```
Power BI Measure

↓

fact_ticket_metrics

↓

int_ticket_performance

↓

stg_customer_support_tickets

↓

raw CSV
```

---

# 15. Validate Models Before Deployment

Before publishing:

Check:

## Data Tests

```
dbt test
```

---

## Documentation

```
dbt docs generate
```

---

## Build

```
dbt build
```

---

# Common Data Modeling Mistakes

## 1. No Defined Grain

Leads to incorrect metrics.

---

## 2. Mixing Facts and Dimensions

Example:

Wrong:

```
fact_sales

customer_name

product_description

```

---

## 3. Building Reports Directly From Raw Data

Creates:

- Duplicate logic
- Poor governance
- Inconsistent reporting

---

## 4. Overengineering

Not every dataset needs complex architecture.

Choose the simplest design that solves the business problem.

---

# Data Modeling Interview Questions

## Why is grain important?

Because it defines the meaning of each row and prevents incorrect calculations.

---

## Why separate facts and dimensions?

To improve performance, usability, and maintainability.

---

## Why use star schemas?

Because they simplify analytical queries and BI reporting.

---

## Why use surrogate keys?

To create stable relationships independent of changing business values.

---

## What makes a good analytics model?

A good model is:

- Accurate
- Documented
- Tested
- Scalable
- Easy for analysts to use

---

# Key Takeaway

Great analytics engineering starts with great data modeling.

A strong data model:

✅ Creates trusted metrics  
✅ Makes BI reporting easier  
✅ Improves query performance  
✅ Supports scalable pipelines  
✅ Bridges technical systems and business decisions  

Data modeling is the foundation that allows analytics teams to turn raw data into reliable insights.