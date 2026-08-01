# dbt Models and Layers

## Overview

dbt models are the core building blocks of an analytics engineering project.

A model is a SQL file that transforms data into a reusable dataset.

In dbt:

```

SQL File

```
 ↓
```

dbt Compilation

```
 ↓
```

Database Object

```

The database object can be:

- View
- Table
- Incremental table

Models allow analytics engineers to transform raw operational data into clean, reliable, business-ready datasets.

In the SupportOps Intelligence Analytics project, dbt models transformed customer support ticket data into a dimensional model used by Power BI.

---

# What Is a dbt Model?

A dbt model is a SQL file stored inside the `models` directory.

Example:

```

models/

└── staging/

```
└── stg_ticket.sql
```

````

SQL:

```sql
SELECT *

FROM raw_tickets
````

When dbt executes:

```bash
dbt run
```

It creates a database object:

```
stg_ticket
```

---

# Why Models Matter

Without dbt:

```
Raw Table

↓

Large SQL Script

↓

Dashboard
```

Problems:

* Difficult to maintain
* Hard to debug
* No lineage
* No testing
* Logic duplication

With dbt:

```
Raw Data

↓

Staging Model

↓

Intermediate Models

↓

Mart Models

↓

Dashboard
```

Benefits:

* Modular transformations
* Reusable logic
* Clear dependencies
* Better collaboration

---

# The Three-Layer dbt Architecture

Professional dbt projects usually follow:

```
                 Raw Data

                    |

                    ↓

              Staging Layer

                    |

                    ↓

           Intermediate Layer

                    |

                    ↓

              Mart Layer

                    |

                    ↓

              BI / Analytics
```

Each layer has a specific responsibility.

---

# Layer 1: Staging Models

## Purpose

The staging layer prepares raw data for further transformation.

It represents the first transformation step.

Location:

```
models/staging/
```

---

# Responsibilities of Staging Models

Staging models handle:

## Renaming Columns

Example:

Raw:

```
cust_email
```

becomes:

```
customer_email
```

---

## Data Type Conversion

Example:

Convert:

```
VARCHAR date
```

into:

```
DATE
```

---

## Basic Cleaning

Examples:

* Remove unnecessary columns
* Standardize text formats
* Handle simple NULL values

---

## Source Normalization

Different source systems may use different naming conventions.

Example:

CRM:

```
customer_email
```

ERP:

```
email_address
```

Staging creates consistency.

---

# Staging Model Example

File:

```
models/staging/stg_ticket.sql
```

Example:

```sql
SELECT

    ticket_id,

    customer_name,

    customer_email,

    issue_category,

    priority_level,

    submission_date,

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

stg_ticket
```

---

# Layer 2: Intermediate Models

## Purpose

Intermediate models contain business logic and reusable calculations.

Location:

```
models/intermediate/
```

They sit between:

```
Staging

    ↓

Intermediate

    ↓

Marts
```

---

# Responsibilities of Intermediate Models

Intermediate models handle:

* Complex calculations
* Business rules
* Data enrichment
* Metric preparation

---

# Example: SLA Calculation

Business rule:

```
If resolution time <= SLA target:

Ticket met SLA

Otherwise:

Ticket missed SLA
```

SQL:

```sql
CASE

WHEN resolution_time_hours <= sla_target_hours

THEN 'Within SLA'

ELSE 'Outside SLA'

END AS sla_performance
```

---

# SupportOps Intermediate Model

File:

```
models/intermediate/int_ticket_metrics.sql
```

Created:

```
sla_performance

resolution_performance

ticket_complexity

sla_risk_status

resolution_variance_hours
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

# Layer 3: Mart Models

## Purpose

Mart models create analytics-ready datasets.

Location:

```
models/marts/
```

These are consumed by:

* Power BI
* Tableau
* Looker
* Analysts
* Reporting systems

---

# Types of Mart Models

Mart models usually contain:

## Dimension Tables

Store descriptive information.

Examples:

```
dim_customer

dim_agent

dim_category
```

---

## Fact Tables

Store measurable events.

Examples:

```
fact_ticket

fact_sales

fact_orders
```

---

## Reporting Tables

Pre-aggregated business datasets.

Examples:

```
support_dashboard

sales_summary

executive_metrics
```

---

# Dimension Models

Dimensions answer:

* Who?
* What?
* Where?
* When?

Example:

## dim_customer

Contains:

```
customer_key

customer_name

customer_email

customer_location
```

---

## dim_agent

Contains:

```
agent_key

assigned_agent

team
```

---

# Fact Models

Facts represent business events.

Example:

```
fact_ticket
```

Contains:

```
ticket_id

customer_key

agent_key

category_key

priority_key

resolution_time_hours

satisfaction_score
```

---

# Why Separate Facts and Dimensions?

Poor design:

```
One Large Table

Customer Information

+

Ticket Information

+

Agent Information

+

Metrics
```

Problems:

* Duplicate data
* Larger storage
* Difficult maintenance

Better:

```
dim_customer

       |

       |

fact_ticket

       |

       |

dim_agent
```

Benefits:

* Better performance
* Cleaner reporting
* Easier analysis

---

# dbt Model Dependencies

dbt automatically creates dependencies using:

```sql
{{ ref('model_name') }}
```

Example:

```sql
SELECT *

FROM {{ ref('stg_ticket') }}
```

dbt understands:

```
stg_ticket

      ↓

int_ticket_metrics

      ↓

fact_ticket
```

---

# Materialization Strategy

Different layers use different materializations.

## Staging

Usually:

```
view
```

Reason:

* Lightweight
* Always reflects source

Example:

```yaml
staging:

  +materialized: view
```

---

## Intermediate

Usually:

```
view
```

or:

```
table
```

Depends on complexity.

---

## Marts

Usually:

```
table
```

Reason:

* Faster BI queries
* Stores calculations

Example:

```yaml
marts:

  +materialized: table
```

---

# Complete SupportOps Model Flow

```
customer_support_tickets_clean.csv

              |

              ↓

        raw_tickets

              |

              ↓

        stg_ticket

              |

              ↓

    int_ticket_metrics

              |

              ↓

 --------------------------------

 |              |                |

dim_customer  dim_agent     dim_priority

              |

              ↓

          fact_ticket

              |

              ↓

    support_dashboard

              |

              ↓

          Power BI
```

---

# Best Practices For dbt Models

## 1. One Model, One Purpose

Avoid:

```
massive_transformation.sql
```

Better:

```
stg_customer.sql

int_customer_metrics.sql

dim_customer.sql
```

---

## 2. Avoid Duplicate Logic

Bad:

Same SLA calculation in:

* SQL
* Python
* Power BI

Better:

Create once in dbt.

---

## 3. Use Clear Naming

Recommended:

```
stg_

int_

dim_

fact_
```

---

## 4. Document Models

Every important model should explain:

* Purpose
* Owner
* Business meaning
* Important columns

---

## 5. Test Critical Models

Test:

* Primary keys
* Foreign keys
* Required fields
* Business rules

---

# Skills Required To Master dbt Models

## SQL

Learn:

* Advanced joins
* Window functions
* CTE design
* Query optimization

---

## Data Modeling

Learn:

* Star schema
* Dimensional modeling
* Fact table design
* Surrogate keys

---

## Business Analysis

Learn:

* Translating requirements into metrics
* KPI definitions
* Business logic documentation

---

## Software Engineering

Learn:

* Modular design
* Testing
* Version control
* Code review

---

# Resources

## Documentation

dbt Models:

[https://docs.getdbt.com/docs/build/models](https://docs.getdbt.com/docs/build/models)

---

## Books

### Designing Data-Intensive Applications

Author:

Martin Kleppmann

Focus:

* Data systems
* Reliability
* Scalability

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Modern data architecture
* Analytics engineering

---

## Courses

dbt Fundamentals:

[https://learn.getdbt.com/](https://learn.getdbt.com/)

Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

# Summary

dbt models transform raw data into business-ready datasets.

The layered approach:

```
Staging

↓

Intermediate

↓

Marts
```

creates analytics systems that are:

* Maintainable
* Tested
* Documented
* Scalable

The SupportOps Intelligence Analytics project applied this architecture to create a professional analytics engineering workflow from raw ticket data to Power BI reporting.

