# dbt Project Implementation: SupportOps Intelligence Analytics

## Overview

This document explains how dbt was implemented in the SupportOps Intelligence Analytics project.

The goal of the implementation was to transform cleaned customer support ticket data into an analytics-ready data model that could support business intelligence reporting in Power BI.

The project followed a professional analytics engineering workflow:

```

Raw Data

```
↓
```

Data Cleaning (Python)

```
↓
```

DuckDB Data Warehouse

```
↓
```

dbt Transformations

```
↓
```

Dimensional Data Model

```
↓
```

Parquet Exports

```
↓
```

Power BI Dashboard

```

---

# Project Objective

The objective was to build a complete analytics engineering pipeline for customer support operations.

The final solution needed to answer business questions such as:

- How many support tickets are received?
- How efficiently are tickets resolved?
- Are SLA targets being achieved?
- Which agents perform best?
- Which categories generate the most issues?
- How satisfied are customers?

---

# Technology Stack

| Area | Technology | Purpose |
|---|---|---|
| Programming | Python | Data cleaning, automation, scripting |
| Database | DuckDB | Local analytical database |
| Transformation | dbt | SQL-based data modeling |
| Data Processing | Pandas | Data preparation and analysis |
| Visualization | Power BI | Business intelligence dashboard |
| Storage Format | Parquet | Analytical data exchange |
| Version Control | Git/GitHub | Project management |
| Environment | Virtual Environment | Dependency isolation |

---

# Initial Data Source

The project started with:

```

data/raw/

customer_support_tickets.csv

```

The raw dataset contained:

- Ticket information
- Customer details
- Agent assignments
- Support channels
- Priority levels
- Resolution information
- Customer satisfaction scores

---

# Step 1: Data Profiling

Before transformation, the dataset was analyzed.

Tools used:

- Jupyter Notebook
- Pandas
- Seaborn
- Matplotlib

Objectives:

- Understand dataset structure
- Identify missing values
- Detect duplicates
- Check data types
- Analyze distributions

Notebook:

```

notebooks/

01_data_profiling.ipynb

```

---

# Step 2: Data Cleaning

The raw dataset was cleaned using Python.

Notebook:

```

notebooks/

02_data_cleaning.ipynb

```

Cleaning activities included:

- Handling missing values
- Standardizing column names
- Correcting data types
- Removing duplicates
- Preparing analytical fields

Output:

```

data/cleaned/

customer_support_tickets_clean.csv

```

---

# Step 3: Loading Data Into DuckDB

The cleaned dataset was loaded into DuckDB.

Python script:

```

python/

load_to_duckdb.py

```

Purpose:

- Create analytical database
- Store cleaned data
- Provide SQL execution environment

Database:

```

database/

supportops.duckdb

```

---

# Step 4: Creating dbt Project

The dbt project was initialized.

Project structure:

```

dbt/

├── dbt_project.yml

├── models/

├── tests/

├── snapshots/

└── seeds/

```

---

# Step 5: Creating the Source Definition

The raw table was registered as a dbt source.

File:

```

models/staging/sources.yml

````

Purpose:

- Define upstream data
- Create lineage
- Improve documentation

Example:

```yaml
sources:

  - name: supportops

    tables:

      - name: customer_support_tickets
````

---

# Step 6: Creating the Staging Layer

The first dbt model cleaned the source table.

Model:

```
models/staging/

stg_ticket.sql
```

Responsibilities:

* Rename columns
* Standardize fields
* Prepare data for transformation

Flow:

```
raw table

    ↓

stg_ticket
```

Materialization:

```
view
```

---

# Step 7: Creating the Intermediate Layer

Business calculations were created in:

```
models/intermediate/

int_ticket_metrics.sql
```

This model generated:

* Resolution metrics
* SLA calculations
* Ticket performance indicators

Example logic:

```sql
CASE

WHEN resolution_time_hours <= sla_target_hours

THEN 'Within SLA'

ELSE 'Outside SLA'

END
```

Flow:

```
stg_ticket

    ↓

int_ticket_metrics
```

---

# Step 8: Creating Mart Models

The final analytical layer was created inside:

```
models/marts/
```

The project used a dimensional model.

---

# Dimension Models

## dim_customer

Stores customer attributes.

Contains:

* Customer key
* Customer information
* Customer location

---

## dim_agent

Stores support agent information.

Contains:

* Agent key
* Agent details

---

## dim_category

Stores issue categories.

Contains:

* Category key
* Category names

---

## dim_channel

Stores support channels.

Examples:

* Email
* Chat
* Phone

---

## dim_priority

Stores ticket priority levels.

Examples:

* Low
* Medium
* High
* Critical

---

# Fact Model

## fact_ticket

The central transaction table.

Contains:

* Ticket ID
* Customer key
* Agent key
* Category key
* Channel key
* Priority key
* Resolution metrics
* Satisfaction metrics

Relationship:

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

---

# Reporting Model

The final reporting table:

```
models/marts/

support_dashboard.sql
```

Purpose:

Provide aggregated KPIs for executive reporting.

Metrics:

* Total tickets
* Average resolution hours
* Average satisfaction
* SLA success rate
* Total customers
* Total agents

---

# Step 9: Adding Data Tests

Tests were added using YAML.

Files:

```
models/marts/schema.yml

models/staging/schema.yml
```

Tests included:

## Not Null Tests

Ensured important fields exist.

Examples:

```
ticket_id

customer_key

agent_key
```

---

## Unique Tests

Ensured primary identifiers are unique.

Examples:

```
ticket_id

customer_key

agent_key
```

---

## Relationship Tests

Validated foreign keys.

Examples:

```
fact_ticket.customer_key

↓

dim_customer.customer_key
```

---

# Step 10: Running dbt Pipeline

## Build Models

Command:

```bash
python -m dbt.cli.main run --full-refresh
```

Result:

```
PASS=9

ERROR=0
```

Models created:

* 7 tables
* 2 views

---

# Step 11: Running Tests

Command:

```bash
python -m dbt.cli.main test
```

Result:

```
PASS=16

WARN=0

ERROR=0
```

All quality checks passed.

---

# Step 12: Exporting Analytical Tables

Python script:

```
python/

export_to_parquet.py
```

Exports:

```
exports/

├── agents.parquet

├── channels.parquet

├── customers.parquet

├── fact_ticket.parquet

├── priorities.parquet

└── category.parquet
```

Purpose:

Provide Power BI with optimized analytical files.

---

# Step 13: Power BI Implementation

Power BI connected to:

* Fact table
* Dimension tables

Model:

```
Star Schema
```

Dashboard pages:

## Summary Page

Metrics:

* Total tickets
* SLA success rate
* Resolution time
* Satisfaction

---

## Agent Performance

Analyzed:

* Ticket volume
* Resolution efficiency
* Satisfaction

---

## Customer Tickets

Analyzed:

* Ticket categories
* Channels
* Priorities
* Customer trends

---

# Final Project Architecture

```
                  CSV DATA

                     |

                     ↓

              Python Cleaning

                     |

                     ↓

                 DuckDB

                     |

                     ↓

                   dbt

                     |

        ----------------------------

        |             |            |

   Dimensions      Fact Table   Dashboard

        |             |

        ----------------

              |

              ↓

           Power BI
```

---

# Lessons Learned

## Analytics Engineering

Learned:

* Building layered data transformations
* Creating reusable models
* Designing analytics-ready datasets

---

## dbt

Learned:

* Model organization
* Testing
* Documentation
* Dependency management

---

## SQL

Applied:

* CTEs
* Joins
* Aggregations
* Business logic transformations

---

## Data Modeling

Applied:

* Star schema
* Fact tables
* Dimension tables
* Surrogate keys

---

# Skills Demonstrated

This project demonstrates:

* Analytics engineering
* SQL transformation
* dbt development
* DuckDB analytics workflows
* Data modeling
* Data quality testing
* Documentation
* Power BI development
* Python automation
* Git project management

---

# Future Improvements

Possible production upgrades:

* Add Airflow orchestration
* Deploy DuckDB workflow to cloud warehouse
* Add CI/CD pipeline
* Add dbt Cloud deployment
* Add automated data monitoring
* Add incremental models
* Add stakeholder-facing documentation

---

# Summary

The SupportOps Intelligence Analytics project demonstrates a complete analytics engineering workflow.

Starting from raw customer support data, the project used Python, DuckDB, dbt, and Power BI to create a tested, documented, and business-ready analytics solution.

The implementation reflects modern analytics engineering practices used in professional data teams.