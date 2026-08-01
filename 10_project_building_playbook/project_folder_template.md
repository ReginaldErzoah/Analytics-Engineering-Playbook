# Project Folder Template

## Overview

A professional analytics engineering project should have a clear and predictable structure.

A well-organized project makes it easier to:

- Understand the workflow
- Collaborate with other engineers
- Maintain code
- Debug problems
- Document decisions
- Scale the project later

The folder structure should reflect the different layers of an analytics system.

---

# Recommended Analytics Engineering Project Structure

```

analytics-project/

│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── raw/
│   ├── cleaned/
│   └── external/
│
├── database/
│
├── notebooks/
│
├── python/
│
├── sql/
│
├── dbt/
│
├── dashboards/
│
├── exports/
│
├── docs/
│
├── tests/
│
├── logs/
│
└── scripts/

```

---

# Root Files

## README.md

The README is the entry point of the project.

It should explain:

- Project purpose
- Business problem
- Technology stack
- Setup instructions
- How to reproduce results

Example:

```

Customer Support Analytics Platform

Built using:

Python
DuckDB
dbt
Power BI

```

---

# requirements.txt

Contains Python dependencies.

Example:

```

pandas
numpy
duckdb
dbt-duckdb
matplotlib
seaborn

````

Purpose:

Allow another user to recreate the environment.

Installation:

```bash
pip install -r requirements.txt
````

---

# .gitignore

Controls which files Git should ignore.

Common ignored files:

```
.env

__pycache__/

*.log

target/

.venv/

.ipynb_checkpoints/
```

---

# Data Folder

The data folder stores project datasets.

Structure:

```
data/

├── raw/

├── cleaned/

└── external/
```

---

# data/raw/

Contains original data.

Rules:

* Never modify raw files
* Keep original structure
* Maintain source integrity

Example:

```
customer_support_tickets.csv
```

---

# data/cleaned/

Contains processed datasets.

Examples:

```
customer_support_tickets_clean.csv
```

Cleaning operations:

* Remove duplicates
* Fix data types
* Standardize values

---

# data/external/

Contains external reference data.

Examples:

```
country_codes.csv

calendar_dates.csv
```

---

# Database Folder

Stores local analytical databases.

Example:

```
database/

supportops.duckdb
```

Purpose:

* Store transformed data
* Provide analytical queries
* Connect BI tools

---

# Notebook Folder

Used for exploration and analysis.

Structure:

```
notebooks/

01_data_profiling.ipynb

02_data_cleaning.ipynb

03_analysis.ipynb
```

---

# Notebook Best Practices

Good notebooks:

* Have clear titles
* Explain decisions
* Avoid unnecessary code
* Produce reproducible results

Avoid:

* Random experiments
* Large production pipelines
* Hidden assumptions

---

# Python Folder

Contains reusable Python scripts.

Example:

```
python/

load_data.py

clean_data.py

export_data.py
```

---

# Python Script Responsibilities

Each script should have one purpose.

Example:

Bad:

```
main.py
```

containing:

* Cleaning
* Database loading
* Exporting
* Visualization

---

Better:

```
clean_data.py

load_to_database.py

export_to_parquet.py
```

---

# SQL Folder

Contains standalone SQL queries.

Example:

```
sql/

analysis_queries.sql

validation_queries.sql
```

Use cases:

* Exploration
* Testing
* Business analysis

---

# dbt Folder

Contains the transformation layer.

Structure:

```
dbt/

├── models/

├── tests/

├── seeds/

├── snapshots/

├── dbt_project.yml

└── logs/
```

---

# dbt/models/

Contains SQL transformation models.

Typical structure:

```
models/

├── staging/

├── intermediate/

└── marts/
```

---

# Staging Layer

Purpose:

Clean raw data.

Example:

```
raw_ticket

      ↓

stg_ticket
```

Operations:

* Rename columns
* Cast data types
* Basic cleaning

---

# Intermediate Layer

Purpose:

Business transformations.

Example:

```
stg_ticket

      ↓

int_ticket_metrics
```

Operations:

* Calculations
* Business logic
* Combining models

---

# Mart Layer

Purpose:

Final analytical tables.

Example:

```
fact_ticket

dim_customer

dim_agent
```

Used by:

* BI tools
* Analysts
* Reporting systems

---

# dashboards Folder

Contains BI files.

Example:

```
dashboards/

SupportOps Intelligence Analytics.pbix
```

Contains:

* Reports
* Visualizations
* KPI dashboards

---

# exports Folder

Contains analytical outputs.

Examples:

```
customers.parquet

fact_ticket.parquet

agents.parquet
```

Purpose:

Share analytical datasets.

---

# docs Folder

Contains project documentation.

Example:

```
docs/

architecture.md

data_dictionary.md

business_metrics.md

screenshots/
```

---

# Documentation Files

## architecture.md

Explains:

* System design
* Data flow
* Technology choices

---

## data_dictionary.md

Documents:

* Tables
* Columns
* Definitions

Example:

```
ticket_id

Unique identifier for support ticket
```

---

## business_metrics.md

Documents:

* KPIs
* Formulas
* Business meaning

Example:

```
Average Resolution Time

Average hours required to close tickets
```

---

# tests Folder

Contains testing logic.

Examples:

```
tests/

test_data_quality.py

test_transformations.py
```

---

# logs Folder

Stores execution logs.

Examples:

```
dbt.log

pipeline.log
```

Useful for:

* Debugging
* Monitoring

---

# scripts Folder

Contains automation scripts.

Examples:

```
setup_project.sh

run_pipeline.sh

deploy.sh
```

---

# Example Complete Workflow

```
Raw Data

      ↓

data/raw/


      ↓

Python Cleaning


      ↓

data/cleaned/


      ↓

DuckDB


      ↓

dbt Models


      ↓

Analytics Tables


      ↓

exports/


      ↓

Power BI


      ↓

docs/
```

---

# SupportOps Intelligence Folder Structure

The project followed:

```
SupportOps Intelligence Analytics/

├── dashboards/

├── data/

│   ├── raw/

│   └── cleaned/

│
├── database/

├── dbt/

├── docs/

├── exports/

├── logs/

├── notebooks/

├── python/

└── README.md
```

---

# Folder Naming Best Practices

## Use Clear Names

Good:

```
data_processing.py
```

Bad:

```
script1.py
```

---

## Use Lowercase

Recommended:

```
data_pipeline/
```

Avoid:

```
DataPipeline/
```

---

## Avoid Spaces

Avoid:

```
Analytics Project/
```

Prefer:

```
analytics_project/
```

---

# Professional Improvements For Production Projects

Large projects may add:

```
analytics-project/

├── airflow/

├── docker/

├── terraform/

├── ci_cd/

├── cloud/

└── monitoring/
```

---

# Skills Required

## File System Management

Learn:

* Directory structures
* File permissions
* Path management

## Bash

Learn:

* Automation scripts
* Environment setup
* Pipeline execution

## Git

Learn:

* Version control
* Branching
* Collaboration

## Data Engineering

Learn:

* Pipelines
* Storage layers
* Orchestration

---

# Resources

## Books

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Focus:

* Analytics architecture
* Dimensional modeling

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Modern data platforms
* Data workflows

---

## Courses

DataTalksClub Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

dbt Learn:

[https://learn.getdbt.com/](https://learn.getdbt.com/)

---

# Summary

A strong folder structure is the foundation of a maintainable analytics engineering project.

A professional project should separate:

```
Data

+

Code

+

Transformations

+

Analytics

+

Documentation

+

Automation
```

The SupportOps Intelligence Analytics project structure demonstrates how analytics engineers organize real-world projects from raw data to business insights.