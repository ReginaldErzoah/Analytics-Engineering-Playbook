# Data Modeling

## Overview

Data modeling is the foundation of analytics engineering.

It defines how data is structured, organized, and transformed so that organizations can reliably answer business questions.

A strong data model enables:

- Consistent metrics
- Faster analytics
- Reliable dashboards
- Easier collaboration
- Scalable data systems

The goal of analytics engineering is not only to transform data but to create trusted analytical assets that business teams can confidently use.

---

# Data Modeling Workflow

A typical analytics engineering workflow follows:

```
Raw Data Sources

        ↓

Staging Models

        ↓

Intermediate Models

        ↓

Fact & Dimension Models

        ↓

BI Dashboards & Analytics
```

Example:

Customer Support Analytics:

```
customer_support_tickets.csv

        ↓

stg_customer_support_tickets

        ↓

int_ticket_performance

        ↓

fact_ticket_metrics
dim_customers
dim_products

        ↓

Power BI Dashboard
```

---

# Core Concepts Covered

## 1. Dimensional Modeling

File:

```
Dimensional_Modeling.md
```

Topics:

- Analytical modeling principles
- Facts and dimensions
- Business process modeling
- Star schema design

---

## 2. Star Schema

File:

```
Star_Schema.md
```

Topics:

- Fact tables
- Dimension tables
- Relationships
- BI-friendly structures

Example:

```
             dim_customer

                    |

                    |

dim_product ---- fact_sales ---- dim_date

```

---

## 3. Fact Tables

File:

```
Fact_Tables.md
```

Topics:

- Measures
- Transactions
- Events
- Grain definition

Examples:

```
fact_sales

fact_orders

fact_support_tickets
```

---

## 4. Dimension Tables

File:

```
Dimension_Tables.md
```

Topics:

- Descriptive attributes
- Business entities
- Surrogate keys

Examples:

```
dim_customer

dim_product

dim_employee
```

---

## 5. Slowly Changing Dimensions

File:

```
SCD_Types.md
```

Topics:

- Handling changing attributes
- Historical tracking
- SCD Type 1
- SCD Type 2
- dbt snapshots

---

## 6. Data Modeling Best Practices

File:

```
Data_Modeling_Best_Practices.md
```

Topics:

- Defining grain
- Naming conventions
- Data quality
- Documentation
- Performance optimization

---

# Important Data Modeling Principles

## Define Grain First

Before creating a fact table:

Ask:

> What does one row represent?

Example:

```
fact_ticket_metrics

One row = one customer support ticket
```

---

## Separate Facts and Dimensions

Facts:

Contain measurable events.

Examples:

```
sales_amount

resolution_time

quantity
```

Dimensions:

Contain descriptive information.

Examples:

```
customer_name

product_category

location
```

---

## Build Trusted Metrics Once

Avoid calculating the same metric in multiple places.

Bad:

```
SQL calculation

+

Power BI calculation

+

Excel calculation
```

Better:

```
Centralized metric definition
```

---

# Common Analytics Engineering Model Layers

## Staging Layer

Purpose:

Clean source data.

Examples:

```
stg_customers

stg_orders
```

---

## Intermediate Layer

Purpose:

Reusable transformations.

Examples:

```
int_customer_orders

int_sales_metrics
```

---

## Mart Layer

Purpose:

Business-facing models.

Examples:

```
fact_sales

dim_customer
```

---

# Tools Used in Data Modeling

|Technology|Purpose|
|-|-|
|dbt|Transformation and modeling framework|
|SQL|Data transformation language|
|DuckDB|Analytical database engine|
|Snowflake|Cloud data warehouse|
|BigQuery|Cloud data warehouse|
|Power BI|Business intelligence reporting|
|Looker|Analytics and semantic modeling|

---

# Real Project Application

## Customer Support Analytics Platform

This repository includes a practical implementation of dimensional modeling.

Architecture:

```
Raw CSV

    ↓

dbt staging model

    ↓

Intermediate performance model

    ↓

Dimensional marts


dim_customers

dim_products

fact_ticket_metrics

    ↓

Power BI Dashboard
```

---

## Implemented Concepts

The project demonstrates:

✅ Star schema design

✅ Fact and dimension separation

✅ Surrogate keys

✅ Business metric modeling

✅ Data quality testing

✅ BI-ready datasets

---

# Interview Preparation Topics

Common analytics engineering interview areas:

## Modeling

- Explain star schema
- Difference between fact and dimension tables
- Define grain
- Design a model from business requirements

---

## Data Warehousing

- OLTP vs OLAP
- Warehouse architecture
- Data marts
- Lakehouse concepts

---

## dbt

- Model layers
- Testing
- Documentation
- Incremental models
- Snapshots

---

# Recommended Learning Order

Follow this sequence:

```
1. Dimensional Modeling

        ↓

2. Fact Tables

        ↓

3. Dimension Tables

        ↓

4. Star Schema

        ↓

5. Slowly Changing Dimensions

        ↓

6. Data Modeling Best Practices
```

---

# Key Takeaway

Data modeling is the bridge between raw data and business intelligence.

A strong analytics engineer understands how to design models that are:

- Accurate
- Scalable
- Maintainable
- Easy to analyze

Good analytics starts with good modeling.