# dbt Concepts

## Overview

dbt (Data Build Tool) is built around a set of concepts that allow analytics engineers to create reliable, scalable, and maintainable data transformation workflows.

The core idea behind dbt is:

> Transform data inside the warehouse using modular SQL models while applying software engineering practices.

Key dbt concepts include:

- Projects
- Models
- Materializations
- Sources
- References
- Tests
- Documentation
- Jinja
- Macros
- Packages
- Environments
- Lineage

---

# 1. dbt Projects

A dbt project is the complete environment containing all files required to build analytics models.

A typical project:

```
analytics_project/

├── dbt_project.yml

├── models/

├── tests/

├── macros/

├── seeds/

├── snapshots/

└── packages.yml
```

---

# dbt_project.yml

This is the main configuration file.

It defines:

- Project name
- Profile connection
- Model configurations
- Paths

Example:

```yaml
name: customer_support_analytics

version: '1.0'

config-version: 2

profile: analytics_project
```

---

# 2. dbt Models

A model is a SQL file that represents a transformation.

Example:

File:

```
models/staging/stg_customers.sql
```

SQL:

```sql
select

    customer_id,
    customer_name,
    email

from customers
```

When dbt runs:

```bash
dbt run
```

It creates a database object.

---

# Model Purpose

Models transform:

```
Raw Data

    ↓

Clean Data

    ↓

Business Data
```

Example:

Raw:

```
customer_support_tickets
```

Model:

```
fact_ticket_metrics
```

Dashboard:

```
Customer Support Performance
```

---

# 3. Materializations

Materialization determines how dbt creates models in the database.

The four main types:

- Table
- View
- Incremental
- Ephemeral

---

# Table Materialization

Creates a physical table.

Example:

```sql
{{ config(
    materialized='table'
) }}
```

Result:

```
CREATE TABLE fact_sales
```

Advantages:

- Fast querying
- Good for reporting tables

Disadvantages:

- Requires rebuilding entire table

---

# View Materialization

Creates a database view.

Example:

```sql
{{ config(
    materialized='view'
) }}
```

Result:

```
CREATE VIEW customer_summary
```

Advantages:

- Always updated
- Saves storage

Disadvantages:

- Query performance may be slower

---

# Incremental Materialization

Processes only new or changed records.

Example:

```sql
{{ config(
    materialized='incremental'
) }}
```

Useful for:

- Large datasets
- Event streams
- Transaction tables

Example:

Instead of processing:

```
1 billion rows
```

Process:

```
10,000 new rows
```

---

# Ephemeral Materialization

Creates temporary SQL logic.

Example:

```sql
{{ config(
    materialized='ephemeral'
) }}
```

The model is not stored.

dbt injects the SQL into downstream models.

Useful for:

- Reusable transformations
- Small helper models

---

# 4. References (`ref()`)

The `ref()` function creates relationships between dbt models.

Example:

```sql
select *

from {{ ref('stg_customers') }}
```

Instead of:

```sql
from analytics.stg_customers
```

---

# Why Use ref()?

## Dependency Management

dbt automatically understands:

```
stg_customers

       ↓

dim_customers

       ↓

customer_dashboard
```

---

## Environment Management

dbt can automatically change:

Development:

```
dev_schema.table
```

Production:

```
prod_schema.table
```

---

# 5. Sources

Sources represent raw tables loaded into the warehouse.

Example:

```yaml
sources:

  - name: support

    tables:

      - name: tickets
```

Usage:

```sql
select *

from {{ source(
    'support',
    'tickets'
) }}
```

---

# Why Define Sources?

Sources provide:

- Documentation
- Testing
- Lineage tracking
- Freshness checks

---

# 6. Tests

Tests validate whether data meets expectations.

Example:

```yaml
columns:

- name: customer_id

  tests:

  - unique

  - not_null
```

---

# Generic Tests

Built-in tests:

## unique

Checks duplicates.

Example:

```
customer_id
```

---

## not_null

Checks missing values.

Example:

```
ticket_id
```

---

## accepted_values

Checks categories.

Example:

```
status:

open

closed

pending
```

---

## relationships

Checks foreign keys.

Example:

```
fact_sales.customer_id

exists in

dim_customer.customer_id
```

---

# 7. Documentation

dbt supports automatic documentation.

Documentation includes:

- Models
- Columns
- Tests
- Dependencies

Generate:

```bash
dbt docs generate
```

View:

```bash
dbt docs serve
```

---

# 8. Jinja

dbt uses Jinja templating to make SQL dynamic.

Example:

Normal SQL:

```sql
select *

from customers
```

Jinja SQL:

```sql
select *

from {{ ref('customers') }}
```

---

# Common Jinja Features

## Variables

```jinja
{{ variable_name }}
```

---

## Conditions

```jinja
{% if condition %}

{% endif %}
```

---

## Loops

```jinja
{% for column in columns %}

{% endfor %}
```

---

# 9. Macros

Macros are reusable blocks of SQL logic.

Example:

Instead of writing:

```sql
upper(trim(customer_name))
```

everywhere:

Create:

```
clean_customer_name()
```

---

Macro example:

```sql
{% macro clean_string(column) %}

trim(lower({{ column }}))

{% endmacro %}
```

Usage:

```sql
{{ clean_string('email') }}
```

---

# 10. Packages

Packages allow teams to reuse existing dbt functionality.

Installed through:

```
packages.yml
```

Example:

```yaml
packages:

- package: dbt-labs/dbt_utils

  version: 1.1.1
```

Install:

```bash
dbt deps
```

---

# dbt_utils Package

Popular utilities:

## Surrogate keys

```sql
{{ dbt_utils.generate_surrogate_key(
[
'customer_id'
]
) }}
```

---

## Date spine

Creates date calendars.

Useful for:

- Time series analysis
- Reporting

---

# 11. Lineage

dbt automatically builds a dependency graph.

Example:

```
source_customer_data

          ↓

stg_customers

          ↓

dim_customers

          ↓

dashboard
```

Benefits:

- Impact analysis
- Debugging
- Understanding pipelines

---

# 12. Environments

Analytics teams usually have:

## Development

Used by analysts.

Example:

```
dev_reginald_schema
```

---

## Production

Used for reporting.

Example:

```
analytics_schema
```

---

# 13. Deployment

Production workflows usually include:

```
Developer

    ↓

Git Branch

    ↓

Pull Request

    ↓

CI Tests

    ↓

Merge

    ↓

Production Deployment
```

---

# dbt Mental Model

Think of dbt as:

```
SQL + Software Engineering Practices
```

It adds:

```
Version Control

Testing

Documentation

Dependency Management

Automation
```

---

# Interview Questions

## What problem does dbt solve?

It makes SQL transformations modular, tested, documented, and maintainable.

---

## What is the difference between source() and ref()?

`source()` references raw data.

`ref()` references dbt models.

---

## What are dbt materializations?

They define how dbt creates database objects.

Examples:

- Table
- View
- Incremental
- Ephemeral

---

## Why is ref() important?

It creates dependencies and allows dbt to build models in the correct order.

---

# Key Takeaway

Mastering dbt concepts means understanding how to build analytics pipelines that are:

✅ Modular  
✅ Tested  
✅ Documented  
✅ Version-controlled  
✅ Production-ready  

dbt is the foundation of modern analytics engineering workflows.