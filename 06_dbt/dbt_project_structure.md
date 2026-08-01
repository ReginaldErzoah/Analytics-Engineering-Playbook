# dbt Project Structure

## Overview

A dbt project is a structured environment where SQL transformations, tests, documentation, and configuration files are organized following analytics engineering best practices.

A well-designed dbt project should be:

- Easy to understand
- Easy to maintain
- Easy to collaborate on
- Easy to test
- Easy to scale

In the SupportOps Intelligence Analytics project, dbt was organized using a layered architecture:

```

dbt/

├── models/

│   ├── staging/

│   ├── intermediate/

│   └── marts/

│

├── tests/

├── seeds/

├── snapshots/

├── macros/

├── dbt_project.yml

└── profiles.yml

````

---

# Core dbt Project Components

## 1. dbt_project.yml

The `dbt_project.yml` file is the main configuration file for every dbt project.

It defines:

- Project name
- Model paths
- Materializations
- Variables
- Documentation settings

Example:

```yaml
name: supportops_intelligence

version: "1.0"

config-version: 2


model-paths:

  - models


models:

  supportops_intelligence:

    staging:

      +materialized: view


    marts:

      +materialized: table
````

---

# 2. Models Directory

Location:

```
models/
```

Models contain SQL transformation logic.

A model is simply a SQL file that creates a database object.

Example:

```
models/

└── staging/

    └── stg_ticket.sql
```

SQL:

```sql
SELECT *

FROM raw_tickets
```

When dbt runs:

```
stg_ticket.sql

        ↓

database view/table
```

---

# Model Layer Architecture

Professional dbt projects usually follow three layers:

```
Source Data

     ↓

Staging Layer

     ↓

Intermediate Layer

     ↓

Mart Layer

     ↓

BI Tools
```

Each layer has a specific responsibility.

---

# 1. Staging Layer

Location:

```
models/staging/
```

Purpose:

Prepare raw data for transformation.

Responsibilities:

* Rename columns
* Standardize data types
* Remove unnecessary fields
* Apply basic cleaning

The staging layer should stay close to the source.

---

Example:

Raw table:

```
customer_support_tickets
```

becomes:

```
stg_ticket
```

Example:

```sql
SELECT

    ticket_id,

    customer_email,

    priority_level,

    resolution_time_hours


FROM raw_tickets
```

---

# Staging Naming Convention

Recommended:

```
stg_<source_name>
```

Examples:

```
stg_customer

stg_orders

stg_transactions
```

Benefits:

* Easy identification
* Clear lineage
* Consistent naming

---

# 2. Intermediate Layer

Location:

```
models/intermediate/
```

Purpose:

Apply business logic between staging and marts.

Responsibilities:

* Complex calculations
* Business rules
* Reusable transformations

---

Example:

File:

```
int_ticket_metrics.sql
```

Logic:

```sql
CASE

WHEN resolution_time_hours <= sla_target_hours

THEN 'Within SLA'

ELSE 'Outside SLA'

END AS sla_performance
```

Creates:

```
sla_performance
```

---

# Intermediate Naming Convention

Recommended:

```
int_<business_logic>
```

Examples:

```
int_customer_metrics

int_sales_summary

int_ticket_metrics
```

---

# 3. Mart Layer

Location:

```
models/marts/
```

Purpose:

Create analytics-ready datasets.

These models are consumed by:

* Power BI
* Tableau
* Looker
* Analysts
* Data scientists

---

Mart models usually contain:

* Fact tables
* Dimension tables
* Business dashboards

---

# SupportOps Mart Structure

```
models/marts/

├── dim_agent.sql

├── dim_category.sql

├── dim_channel.sql

├── dim_customer.sql

├── dim_priority.sql

├── fact_ticket.sql

└── support_dashboard.sql
```

---

# Fact Tables

Fact tables contain measurable business events.

Example:

```
fact_ticket
```

Contains:

* Ticket ID
* Customer key
* Agent key
* Category key
* Resolution metrics

Example:

```
fact_ticket

ticket_id

customer_key

agent_key

resolution_time_hours

satisfaction_score
```

---

# Dimension Tables

Dimension tables provide descriptive information.

Examples:

```
dim_customer

dim_agent

dim_category

dim_priority
```

They answer:

* Who?
* What?
* Where?
* When?

---

# 3. Tests Directory

Location:

```
tests/
```

Contains custom SQL tests.

Example:

```
tests/

└── duplicate_ticket_check.sql
```

A test returns failing records.

Example:

```sql
SELECT

ticket_id,

COUNT(*)

FROM fact_ticket

GROUP BY ticket_id

HAVING COUNT(*) > 1
```

If rows are returned:

The test fails.

---

# 4. Seeds Directory

Location:

```
seeds/
```

Seeds store small CSV files managed by dbt.

Used for:

* Lookup tables
* Reference data
* Static mappings

Example:

```
seeds/

priority_mapping.csv
```

CSV:

```
priority,score

High,3

Medium,2

Low,1
```

Run:

```bash
dbt seed
```

---

# 5. Snapshots Directory

Location:

```
snapshots/
```

Snapshots track historical changes.

Used for:

Slowly Changing Dimensions.

Example:

Customer changes:

Before:

```
Customer A

Location: Accra
```

After:

```
Customer A

Location: Kumasi
```

Snapshot preserves history.

---

# 6. Macros Directory

Location:

```
macros/
```

Macros are reusable SQL functions.

Example:

Instead of repeating:

```sql
UPPER(column_name)
```

Create:

```sql
{% macro clean_text(column) %}

UPPER({{ column }})

{% endmacro %}
```

Use:

```sql
{{ clean_text(customer_name) }}
```

---

# 7. Profiles Configuration

Location:

Usually:

```
~/.dbt/profiles.yml
```

Defines database connection settings.

Example:

```yaml
supportops_intelligence:

  target: dev


  outputs:

    dev:

      type: duckdb

      path: database/supportops.duckdb
```

---

# SupportOps Intelligence dbt Structure

Final structure:

```
dbt/

├── models/

│
├── staging/

│      ├── stg_ticket.sql

│      └── schema.yml

│
├── intermediate/

│      └── int_ticket_metrics.sql

│
└── marts/

       ├── dim_agent.sql

       ├── dim_category.sql

       ├── dim_channel.sql

       ├── dim_customer.sql

       ├── dim_priority.sql

       ├── fact_ticket.sql

       ├── support_dashboard.sql

       └── schema.yml


├── tests/

├── seeds/

├── snapshots/

└── dbt_project.yml
```

---

# Recommended dbt Folder Structure For Future Projects

For larger projects:

```
models/

├── staging/

│
│   ├── crm/

│   ├── finance/

│   └── marketing/


├── intermediate/

│
│   ├── customer/

│   └── revenue/


└── marts/

    ├── core/

    ├── reporting/

    └── analytics/
```

---

# Best Practices

## Keep Layers Separate

Avoid:

```
raw data → dashboard table
```

Prefer:

```
raw

↓

staging

↓

intermediate

↓

marts

↓

dashboard
```

---

## Avoid Business Logic In BI Tools

Bad:

Power BI contains:

* SLA calculations
* Revenue rules
* Customer classification

Better:

dbt creates:

* Metrics
* Dimensions
* Business rules

Power BI only visualizes.

---

## Name Models Clearly

Good:

```
fact_sales

dim_customer

int_customer_metrics
```

Bad:

```
final_table

new_data

table2
```

---

# Skills Required

## SQL

Master:

* CTEs
* Window functions
* Aggregations
* Joins
* Query optimization

---

## Data Modeling

Master:

* Star schema
* Facts
* Dimensions
* Surrogate keys
* Slowly changing dimensions

---

## Software Engineering

Master:

* Git workflows
* Code organization
* Testing
* Documentation

---

## Analytics Engineering

Master:

* dbt workflows
* Data contracts
* Data lineage
* CI/CD pipelines

---

# Resources

## Documentation

dbt Documentation:

[https://docs.getdbt.com/](https://docs.getdbt.com/)

---

## Courses

dbt Fundamentals:

[https://learn.getdbt.com/](https://learn.getdbt.com/)

Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

---

### The Analytics Engineering Guide

Author:

Carmen Huidobro

---

# Summary

A strong dbt project structure creates a reliable analytics engineering workflow.

The layered approach used in SupportOps Intelligence Analytics:

```
Staging

↓

Intermediate

↓

Marts

↓

BI Dashboard
```

made the project:

* Maintainable
* Testable
* Documented
* Scalable

This structure represents the foundation of professional analytics engineering projects.
