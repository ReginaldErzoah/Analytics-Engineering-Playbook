# dbt Fundamentals

## Overview

dbt (data build tool) is an analytics engineering framework used to transform data inside a database using SQL.

It enables analysts and analytics engineers to apply software engineering practices to data transformation workflows.

Instead of manually writing SQL scripts and creating tables, dbt provides:

- Modular SQL transformations
- Dependency management
- Testing frameworks
- Documentation generation
- Version control workflows
- Reproducible analytics pipelines

In the SupportOps Intelligence Analytics project, dbt was responsible for transforming cleaned ticket data stored in DuckDB into analytics-ready models for Power BI.

---

# What Problem Does dbt Solve?

Traditional analytics workflows often look like:

```

Raw Data
|
↓
Manual SQL Scripts
|
↓
Tables Created Manually
|
↓
Dashboard

```

Problems:

- Difficult to track changes
- No dependency management
- Hard to test
- Difficult collaboration
- Poor documentation

dbt improves this process:

```

Raw Data

↓

Staging Models

↓

Intermediate Models

↓

Mart Models

↓

BI Dashboard

```

---

# What Is Analytics Engineering?

Analytics engineering sits between:

- Data engineering
- Data analysis
- Business intelligence

An analytics engineer:

- Cleans and transforms data
- Builds analytical datasets
- Defines business metrics
- Ensures data quality
- Creates reusable data models

dbt is one of the main tools used by analytics engineers.

---

# How dbt Works

dbt follows the principle:

> "Transform data where it already lives."

Instead of moving data into Python:

```

Database
|
↓
Python Processing
|
↓
Database

```

dbt keeps transformations inside the database:

```

Database

```
↓
```

SQL Transformation

```
↓
```

Analytics Tables

```

This approach is called ELT:

```

Extract

Load

Transform

```

---

# Core dbt Concepts

## 1. Models

Models are SQL files that define transformations.

Example:

File:

```

models/staging/stg_ticket.sql

````

Contains:

```sql
SELECT *

FROM raw_tickets
````

When dbt runs, it creates a database object.

Example:

```
stg_ticket
```

Models are the foundation of dbt projects.

---

# 2. Sources

Sources define where raw data comes from.

Example:

```yaml
sources:

  - name: supportops

    tables:

      - name: raw_tickets
```

Benefits:

* Documents original data sources
* Enables source testing
* Creates lineage tracking

---

# 3. References (ref)

The `ref()` function creates dependencies between models.

Example:

```sql
SELECT *

FROM {{ ref('stg_ticket') }}
```

Instead of:

```sql
SELECT *

FROM stg_ticket
```

dbt understands:

```
stg_ticket

     ↓

int_ticket_metrics
```

and builds models in the correct order.

---

# 4. Materializations

Materializations define how dbt creates database objects.

Main types:

## View

Creates a database view.

Example:

```yaml
materialized: view
```

Characteristics:

* Does not store data physically
* Always runs the underlying query

Used for:

* Staging models
* Lightweight transformations

---

## Table

Creates a physical table.

Example:

```yaml
materialized: table
```

Characteristics:

* Faster querying
* Stores transformed data

Used for:

* Dimension tables
* Fact tables

---

## Incremental

Only processes new data.

Example:

```yaml
materialized: incremental
```

Used for:

* Large datasets
* Production pipelines

---

# dbt Project Structure

A typical dbt project:

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
```

---

# SupportOps dbt Structure

The project used:

```
dbt/

├── models/

│
├── staging/

│      └── stg_ticket.sql

│
├── intermediate/

│      └── int_ticket_metrics.sql

│
└── marts/

       ├── dim_customer.sql

       ├── dim_agent.sql

       ├── dim_category.sql

       ├── dim_channel.sql

       ├── dim_priority.sql

       ├── fact_ticket.sql

       └── support_dashboard.sql
```

---

# dbt Commands

## Check dbt Installation

```bash
dbt --version
```

---

## Initialize Project

```bash
dbt init project_name
```

Creates a new dbt project.

---

## Parse Project

```bash
dbt parse
```

Checks:

* Syntax
* Configuration
* Dependencies

---

## Run Models

```bash
dbt run
```

Builds all models.

---

## Run Specific Models

Example:

```bash
dbt run --select fact_ticket
```

Runs only:

```
fact_ticket
```

---

## Run Dependencies

Example:

```bash
dbt run --select +fact_ticket
```

Runs:

```
staging

↓

intermediate

↓

fact_ticket
```

---

## Test Data

```bash
dbt test
```

Runs:

* Schema tests
* Data tests

---

## Generate Documentation

```bash
dbt docs generate
```

Creates documentation metadata.

---

## View Documentation

```bash
dbt docs serve
```

Launches documentation website.

---

# dbt Testing

Data quality is a core dbt feature.

Common tests:

## not_null

Ensures values exist.

Example:

```yaml
tests:

  - not_null
```

---

## unique

Ensures no duplicates.

Example:

```yaml
tests:

  - unique
```

---

## relationships

Tests foreign keys.

Example:

```yaml
relationships:

  to: ref('dim_customer')

  field: customer_key
```

---

# dbt Testing In SupportOps

The project included 16 tests.

Examples:

```
not_null_fact_ticket_ticket_id

unique_fact_ticket_ticket_id

relationships_fact_ticket_customer_key

relationships_fact_ticket_agent_key
```

Final result:

```
PASS=16

ERROR=0
```

---

# dbt Documentation

dbt automatically creates documentation from:

* Models
* Sources
* Columns
* Tests

Documentation improves:

* Collaboration
* Maintenance
* Understanding

---

# dbt Lineage

dbt creates a DAG:

Directed Acyclic Graph.

Example:

```
raw_tickets

      ↓

stg_ticket

      ↓

int_ticket_metrics

      ↓

fact_ticket

      ↓

Power BI
```

Benefits:

* Understand dependencies
* Identify failures
* Track impact of changes

---

# dbt Best Practices

## 1. Keep Models Modular

Avoid one massive SQL file.

Bad:

```
everything.sql
```

Better:

```
staging

intermediate

marts
```

---

## 2. Use Meaningful Names

Good:

```
fact_ticket

dim_customer
```

Bad:

```
table1

final_data
```

---

## 3. Test Important Columns

Always test:

* Primary keys
* Foreign keys
* Critical metrics

---

## 4. Document Business Logic

Complex calculations should be explained.

Example:

SLA performance:

```
Resolution Time <= SLA Target

= Within SLA
```

---

# Skills To Master For dbt

## SQL

Required:

* CTEs
* Joins
* Window functions
* Aggregations
* Subqueries

---

## Data Modeling

Required:

* Facts
* Dimensions
* Star schema
* Slowly changing dimensions

---

## Software Engineering

Required:

* Git
* Testing
* Documentation
* Code reviews

---

## Data Quality

Required:

* Validation rules
* Monitoring
* Data contracts

---

# Resources

## Official Documentation

dbt Docs:

[https://docs.getdbt.com/](https://docs.getdbt.com/)

---

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Data architecture
* Pipelines
* Modern data platforms

### The Analytics Engineering Guide

Author:

Carmen Huidobro

Focus:

* Analytics engineering workflows
* dbt practices

---

## Courses

### dbt Fundamentals

dbt Learn:

[https://learn.getdbt.com/](https://learn.getdbt.com/)

---

### Data Engineering Zoomcamp

DataTalksClub:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

# Summary

dbt transformed SupportOps Intelligence Analytics from raw cleaned data into a professional analytics model.

Through dbt, the project implemented:

* Modular SQL transformations
* Dimensional modeling
* Automated testing
* Documentation
* Data lineage
* Reproducible analytics workflows

Mastering dbt is essential for becoming a professional analytics engineer.
