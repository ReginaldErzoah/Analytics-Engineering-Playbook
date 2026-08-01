# DuckDB Project Implementation

## Overview

This document explains how DuckDB was implemented in the SupportOps Intelligence Analytics project and provides a repeatable workflow for using DuckDB in future analytics engineering projects.

DuckDB acted as the analytical database layer between Python data processing and dbt transformations.

The complete workflow was:

```

Raw Data
|
↓
Python Data Cleaning
|
↓
Clean CSV Dataset
|
↓
DuckDB Database
|
↓
dbt Transformations
|
↓
Analytics Data Model
|
↓
Parquet Exports
|
↓
Power BI Dashboard

```

DuckDB allowed the project to have a complete modern analytics engineering architecture without requiring cloud infrastructure.

---

# Project Architecture

The SupportOps Intelligence architecture consisted of five major layers:

## 1. Data Source Layer

Location:

```

data/raw/

```

Contains:

```

customer_support_tickets.csv

```

Purpose:

- Store original source data
- Preserve raw information
- Maintain reproducibility

Raw data should never be modified directly.

---

# 2. Data Cleaning Layer

Location:

```

data/cleaned/

```

Contains:

```

customer_support_tickets_clean.csv

```

Python notebooks handled:

- Data profiling
- Missing value analysis
- Data type correction
- Duplicate checks
- Data quality improvements

Tools:

- Python
- Pandas
- NumPy
- Jupyter Notebook

---

# 3. DuckDB Database Layer

Location:

```

database/

```

Contains:

```

supportops.duckdb

````

Purpose:

- Store analytical tables
- Execute SQL transformations
- Serve as dbt database target

DuckDB replaced the need for:

- PostgreSQL server
- Cloud warehouse
- Complex infrastructure

---

# Creating the DuckDB Database

A DuckDB database can be created using Python:

```python
import duckdb


connection = duckdb.connect(
    "database/supportops.duckdb"
)


connection.close()
````

This creates:

```
database/
└── supportops.duckdb
```

---

# Loading Clean Data Into DuckDB

Example:

```python
import duckdb


connection = duckdb.connect(
    "database/supportops.duckdb"
)


connection.execute(
    """

    CREATE OR REPLACE TABLE raw_tickets AS

    SELECT *

    FROM read_csv_auto(
        'data/cleaned/customer_support_tickets_clean.csv'
    )

    """
)


connection.close()
```

This creates the initial analytical table.

---

# 4. dbt Transformation Layer

Location:

```
dbt/models/
```

The DuckDB database became the target database for dbt.

The transformation workflow:

```
raw_tickets

      ↓

stg_ticket

      ↓

int_ticket_metrics

      ↓

dimension tables

      ↓

fact table

      ↓

dashboard table
```

---

# dbt and DuckDB Configuration

The dbt profile used DuckDB:

Example:

```yaml
supportops_intelligence:

  target: dev

  outputs:

    dev:

      type: duckdb

      path: ../database/supportops.duckdb
```

This allowed dbt commands to directly create tables inside DuckDB.

---

# Creating Analytical Models

## Staging Model

File:

```
models/staging/stg_ticket.sql
```

Purpose:

* Rename columns
* Standardize data types
* Clean source fields

Example:

```sql
SELECT *

FROM raw_tickets
```

---

# Intermediate Model

File:

```
models/intermediate/int_ticket_metrics.sql
```

Purpose:

Create business logic.

Examples:

* Resolution performance
* SLA classification
* Resolution categories
* Ticket complexity

Example:

```sql
CASE

WHEN resolution_time_hours <= sla_target_hours

THEN 'Within SLA'

ELSE 'Outside SLA'

END AS sla_performance
```

---

# Mart Models

Location:

```
models/marts/
```

Created:

```
dim_customer
dim_agent
dim_category
dim_channel
dim_priority
fact_ticket
support_dashboard
```

These tables were designed for BI reporting.

---

# Star Schema Implementation

The final DuckDB model followed dimensional modeling principles.

Structure:

```
                 dim_customer

                      |

                      |

dim_agent ---- fact_ticket ---- dim_category

                      |

                      |

              dim_priority

                      |

                      |

              dim_channel
```

The fact table stored:

* Ticket transactions
* Foreign keys
* Metrics

Dimension tables stored:

* Descriptive attributes
* Business context

---

# Running DuckDB + dbt Workflow

## Step 1: Activate Environment

```bash
dbt-env\Scripts\activate
```

---

## Step 2: Run dbt Models

Full refresh:

```bash
python -m dbt.cli.main run --full-refresh
```

This:

* Drops existing models
* Recreates tables
* Rebuilds the database

---

## Step 3: Run Tests

```bash
python -m dbt.cli.main test
```

Validated:

* Not null constraints
* Unique keys
* Relationships

Example:

```
PASS=16
ERROR=0
```

---

# Exporting DuckDB Tables

After transformation, analytics tables were exported.

Script:

```
python/export_to_parquet.py
```

Exports:

```
exports/

├── fact_ticket.parquet

├── customers.parquet

├── agents.parquet

├── channels.parquet

├── priorities.parquet

└── category.parquet
```

---

# Export Process

Python executed:

```python
COPY main.fact_ticket

TO 'exports/fact_ticket.parquet'

(FORMAT PARQUET)
```

Advantages:

* Efficient storage
* BI compatibility
* Portable datasets

---

# Connecting DuckDB Outputs To Power BI

Power BI consumed:

```
exports/*.parquet
```

The BI model used:

```
fact_ticket
        |
        |
-------------------------
|       |       |       |
customer agent category priority
```

Power BI created:

* Measures
* KPIs
* Visualizations
* Business insights

---

# Lessons Learned From Implementation

## 1. Separate Transformation Responsibilities

Python should handle:

* Data extraction
* Cleaning
* Automation

SQL/dbt should handle:

* Business logic
* Modeling
* Metrics

---

## 2. Avoid Building BI Logic Inside Raw Tables

Instead:

```
Raw Data

↓

Clean Data

↓

Analytics Models

↓

BI Layer
```

---

## 3. Always Test Data Models

Without testing:

* Duplicate keys
* Missing values
* Broken relationships

can silently enter reports.

---

# Future Analytics Engineering Workflow Template

For future projects:

```
1. Create project structure

2. Profile raw data

3. Clean data using Python

4. Load data into DuckDB

5. Create dbt project

6. Build staging models

7. Create intermediate transformations

8. Build dimensional model

9. Add dbt tests

10. Generate documentation

11. Export analytical tables

12. Build Power BI dashboard

13. Version control with Git

14. Deploy pipeline
```

---

# Skills Required To Master This Workflow

## Python

Learn:

* Pandas
* File handling
* Automation scripts
* Error handling
* Virtual environments

---

## SQL

Learn:

* Joins
* CTEs
* Window functions
* Aggregations
* Query optimization

---

## DuckDB

Learn:

* Database creation
* SQL execution
* Parquet workflows
* Python integration

---

## dbt

Learn:

* Models
* Materializations
* Tests
* Documentation
* Macros

---

## Data Modeling

Learn:

* Star schema
* Facts and dimensions
* Keys
* Slowly changing dimensions

---

## Business Intelligence

Learn:

* Power BI modeling
* DAX
* KPI development
* Dashboard storytelling

---

# Resources

## DuckDB Documentation

[https://duckdb.org/docs/](https://duckdb.org/docs/)

---

## dbt Documentation

[https://docs.getdbt.com/](https://docs.getdbt.com/)

---

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Modern data platforms
* Data pipelines
* Data architecture

### The Analytics Engineering Guide

Author:

Carmen Huidobro

Focus:

* Analytics engineering practices
* Modern transformation workflows

---

# Summary

DuckDB provided the analytical database foundation for SupportOps Intelligence Analytics.

The implementation demonstrated a complete analytics engineering workflow:

* Python for preparation
* DuckDB for analytical storage
* dbt for transformation
* Dimensional modeling for reporting
* Power BI for business intelligence

This workflow represents the foundation of modern analytics engineering practices and can be adapted to many business domains.
