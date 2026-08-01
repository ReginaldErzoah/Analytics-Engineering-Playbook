# dbt (Data Build Tool)

## Overview

dbt (Data Build Tool) is an analytics engineering framework used to transform data inside a data warehouse using SQL.

It allows analysts and analytics engineers to:

- Transform raw data into trusted datasets
- Build modular SQL pipelines
- Test data quality
- Document data assets
- Track data lineage
- Deploy analytics workflows reliably

dbt focuses on the **T (Transformation)** part of the modern ELT workflow.

---

# Where dbt Fits in the Data Stack

Traditional ETL:

```
Extract

 ↓

Transform

 ↓

Load

 ↓

Data Warehouse
```

Modern ELT:

```
Extract

 ↓

Load

 ↓

Transform with dbt

 ↓

Analytics Layer
```

---

# Analytics Engineering Workflow

A typical workflow:

```
Data Sources

(Database, APIs, CSV, Applications)

          ↓

Data Warehouse / Lakehouse

          ↓

dbt Transformations

          ↓

Analytical Models

          ↓

BI Tools

(Power BI, Tableau, Looker)
```

---

# dbt Core Concepts

## Models

SQL files that transform data.

Example:

```
models/marts/fact_sales.sql
```

Creates:

```
fact_sales table
```

---

## Sources

Represent raw data entering the warehouse.

Example:

```
CRM Database

        ↓

source.crm.customers
```

---

## Tests

Validate data quality.

Examples:

```
unique

not_null

relationships

accepted_values
```

---

## Documentation

Explains:

- Models
- Columns
- Sources
- Lineage

---

## Macros

Reusable SQL logic using Jinja.

Example:

```
clean_email()
```

---

## Seeds

Small CSV reference datasets managed inside dbt.

Example:

```
country_codes.csv
```

---

# dbt Project Structure

Typical structure:

```
analytics_project/

├── models/

│   ├── staging/

│   ├── intermediate/

│   └── marts/


├── tests/


├── macros/


├── seeds/


├── snapshots/


├── dbt_project.yml


└── packages.yml
```

---

# dbt Model Layers

## Staging Layer

Purpose:

Clean raw data.

Example:

```
stg_customers
```

---

## Intermediate Layer

Purpose:

Reusable transformations.

Example:

```
int_customer_metrics
```

---

## Mart Layer

Purpose:

Business-ready datasets.

Examples:

```
dim_customers

fact_orders
```

---

# Important dbt Commands

## Install Packages

```bash
dbt deps
```

---

## Run Models

```bash
dbt run
```

---

## Test Data

```bash
dbt test
```

---

## Build Everything

```bash
dbt build
```

Runs:

```
Models

Tests

Seeds

Snapshots
```

---

## Generate Documentation

```bash
dbt docs generate
```

---

## View Documentation

```bash
dbt docs serve
```

---

## Check Source Freshness

```bash
dbt source freshness
```

---

# Common dbt Workflow

```
Create Model

      ↓

Write SQL

      ↓

Add Documentation

      ↓

Add Tests

      ↓

Run dbt build

      ↓

Commit to Git

      ↓

Pull Request

      ↓

Deploy
```

---

# dbt with Version Control

Professional teams use Git.

Example:

```
Developer Branch

        ↓

Pull Request

        ↓

CI Tests

        ↓

Main Branch

        ↓

Production Deployment
```

---

# dbt in the Modern Data Stack

Common architecture:

```
Airflow / Fivetran / Airbyte

            ↓

Warehouse

(BigQuery, Snowflake, Redshift, DuckDB)

            ↓

dbt

            ↓

BI Layer

(Looker, Tableau, Power BI)
```

---

# Technologies Commonly Used with dbt

| Area | Technologies |
|-|-|
| Data Warehouses | Snowflake, BigQuery, Redshift, Databricks |
| Local Analytics | DuckDB |
| Transformation | dbt Core, dbt Cloud |
| Version Control | Git, GitHub |
| Orchestration | Airflow, Dagster, Prefect |
| BI Tools | Power BI, Tableau, Looker |
| Data Quality | dbt Tests, Great Expectations |

---

# Example: Customer Support Analytics Project

Architecture:

```
Customer Support CSV

          ↓

DuckDB

          ↓

dbt Sources

          ↓

Staging Models

stg_customer_support_tickets

          ↓

Intermediate Models

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

# Skills Required for dbt Analytics Engineers

A strong dbt analytics engineer understands:

## SQL

- Joins
- Window functions
- Aggregations
- Query optimization

---

## Data Modeling

- Star schema
- Fact tables
- Dimension tables
- Slowly changing dimensions

---

## Data Warehousing

- OLTP vs OLAP
- Warehouses
- Data marts
- Lakehouse concepts

---

## Data Quality

- Testing
- Validation
- Monitoring
- Governance

---

## Software Engineering

- Git
- Code reviews
- CI/CD
- Documentation

---

# dbt Interview Topics

Important areas:

- What is dbt?
- Difference between ETL and ELT
- Models and materializations
- ref() vs source()
- dbt tests
- Incremental models
- Macros
- Documentation
- Deployment workflows
- Data modeling strategies

---

# Learning Path

Recommended order:

1. SQL Fundamentals

2. Data Modeling

3. Data Warehousing

4. dbt Core Concepts

5. Testing and Documentation

6. Deployment and CI/CD

7. Analytics Engineering Projects

---

# Key Takeaway

dbt transforms SQL analysts into analytics engineers.

It provides the structure needed to build:

✅ Reliable data pipelines  
✅ Tested analytical models  
✅ Documented datasets  
✅ Scalable reporting systems  

A modern analytics engineer combines:

**SQL + Data Modeling + Software Engineering + Business Understanding**