# Data Warehouses vs Data Lakes

## Overview

Modern data platforms need systems to store, organize, and process large amounts of data.

Two major storage approaches are:

- Data warehouses
- Data lakes

Both store data, but they are designed for different purposes.

A modern analytics platform often combines both approaches using a lakehouse architecture.

---

# Data Warehouse

A data warehouse is a centralized system designed for storing structured data optimized for analytics.

It contains:

- Cleaned data
- Modeled data
- Business-ready datasets

Typical users:

- Analysts
- Analytics engineers
- Business intelligence teams

---

# Data Warehouse Architecture

Typical flow:

```

Data Sources

↓

Data Ingestion

↓

Data Warehouse

↓

Data Models

↓

Dashboards

```

---

# Example Data Warehouse Tables

Customer analytics warehouse:

```

customers

orders

products

sales_metrics

````

---

# Characteristics Of Data Warehouses

## Structured Data

Data follows predefined schemas.

Example:

Customers table:

| customer_id | name | email |
|---|---|---|
| 1 | John | john@email.com |

---

## Schema Before Data

The structure is defined before loading.

Example:

```sql
CREATE TABLE customers (

customer_id INTEGER,

name VARCHAR,

email VARCHAR

);
````

---

## Optimized For Analytics

Designed for:

* Aggregations
* Reporting
* Business intelligence

Example:

```sql
SELECT

region,

SUM(revenue)

FROM sales

GROUP BY region;
```

---

# Popular Data Warehouse Technologies

## Cloud Warehouses

Examples:

* Snowflake
* Google BigQuery
* Amazon Redshift
* Azure Synapse

---

## Open Source / Local

Examples:

* DuckDB
* PostgreSQL

---

# Advantages Of Data Warehouses

## High Query Performance

Optimized for analytical workloads.

---

## Trusted Data

Contains:

* Clean data
* Tested models
* Business definitions

---

## Security

Supports:

* Access control
* Permissions
* Governance

---

# Disadvantages Of Data Warehouses

## Expensive At Large Scale

Cloud warehouses charge for:

* Storage
* Compute

---

## Less Flexible

Usually designed around structured data.

---

# Data Lake

A data lake is a storage system that keeps large amounts of raw data in its original format.

It can store:

* Structured data
* Semi-structured data
* Unstructured data

---

# Data Lake Architecture

Typical flow:

```
Data Sources

↓

Data Lake

↓

Processing

↓

Analytics Systems
```

---

# Data Lake Examples

Stored files:

```
data/

├── customers.csv

├── transactions.json

├── images/

└── logs/
```

---

# Characteristics Of Data Lakes

## Stores Raw Data

Example:

Original API response:

```json
{
 "customer_id":101,
 "purchase_amount":200
}
```

stored without modification.

---

## Schema On Read

The structure is applied when data is used.

Example:

Raw file:

```
transactions.json
```

Later interpreted as:

```
transaction_id

customer_id

amount
```

---

## Supports Many Data Types

Examples:

Structured:

```
CSV
```

Semi-structured:

```
JSON
XML
```

Unstructured:

```
Images
Videos
Logs
```

---

# Popular Data Lake Technologies

Storage:

* Amazon S3
* Azure Data Lake Storage
* Google Cloud Storage

Processing:

* Apache Spark
* Databricks

---

# Advantages Of Data Lakes

## Low-Cost Storage

Can store huge amounts of data cheaply.

---

## Flexibility

Store data before knowing future use cases.

---

## Supports Advanced Analytics

Useful for:

* Machine learning
* Data science
* AI workloads

---

# Disadvantages Of Data Lakes

## Data Quality Problems

Without governance:

```
Data Lake

↓

Data Swamp
```

---

## Difficult Discovery

Users may not know:

* What data exists
* Which data is reliable

---

## Requires Strong Governance

Needs:

* Metadata management
* Documentation
* Quality checks

---

# Data Warehouse vs Data Lake Comparison

| Feature      | Data Warehouse     | Data Lake             |
| ------------ | ------------------ | --------------------- |
| Purpose      | Analytics          | Storage + Analytics   |
| Data Type    | Structured         | All types             |
| Schema       | Schema before data | Schema after data     |
| Users        | Analysts           | Engineers, scientists |
| Cost         | Higher             | Lower                 |
| Query Speed  | Very fast          | Depends on processing |
| Data Quality | Usually high       | Requires governance   |
| Main Use     | BI reporting       | Big data, ML          |

---

# Data Lakehouse

A lakehouse combines the strengths of warehouses and lakes.

Goal:

```
Data Lake Flexibility

+

Warehouse Performance
```

---

# Lakehouse Architecture

Example:

```
Raw Data

↓

Data Lake

↓

Lakehouse Tables

↓

Analytics Models

↓

Dashboards
```

---

# Lakehouse Technologies

Examples:

* Databricks Lakehouse
* Apache Iceberg
* Apache Hudi
* Delta Lake

---

# Modern Data Platform Architecture

A complete modern architecture:

```
Applications

↓

Data Lake

↓

Warehouse / Lakehouse

↓

dbt Transformation Layer

↓

Data Marts

↓

BI Tools
```

---

# Where Does dbt Fit?

dbt usually works in the transformation layer.

Example:

```
Warehouse

↓

dbt Models

↓

Analytics Tables

↓

Dashboard
```

dbt does not replace storage.

It transforms data stored in:

* Warehouses
* Lakehouses
* Databases

---

# Where Does DuckDB Fit?

DuckDB is an analytical database optimized for local analytics.

Example:

```
CSV Files

↓

DuckDB

↓

dbt-duckdb

↓

Analytics Models
```

---

DuckDB can act as:

* Local warehouse
* Development environment
* Analytical engine

---

# SupportOps Intelligence Analytics Architecture

Current project:

```
Support Data

↓

CSV Files

↓

Python Cleaning

↓

DuckDB

↓

dbt

↓

Analytics Tables

↓

Power BI
```

---

# Enterprise Version

Production architecture:

```
Support Platform

↓

Data Pipeline

↓

Cloud Storage

↓

Data Warehouse

↓

dbt

↓

Analytics Models

↓

Power BI
```

---

# Choosing Between Warehouse And Lake

## Choose Data Warehouse When:

You need:

* Business reporting
* Dashboards
* KPI tracking
* Structured analytics

Examples:

* Sales analytics
* Finance reporting
* Customer analytics

---

## Choose Data Lake When:

You need:

* Large-scale storage
* Machine learning
* Raw data preservation

Examples:

* IoT data
* Logs
* Images
* Events

---

# Analytics Engineer Perspective

Analytics engineers mainly work with:

* Data warehouses
* Lakehouses
* Analytical databases

Important skills:

## SQL

For:

* Transformations
* Models
* Metrics

---

## dbt

For:

* Warehouse transformations
* Testing
* Documentation

---

## Data Modeling

For:

* Facts
* Dimensions
* Marts

---

# Common Mistakes

## Storing Everything In A Warehouse

Problem:

* Expensive storage
* Poor scalability

---

## Putting Everything In A Data Lake

Problem:

* Creates messy environments

---

## No Governance

Problem:

Users cannot trust data.

---

# Data Governance Practices

A mature platform includes:

## Metadata

Information about data.

Example:

```
Table:

fact_sales

Owner:

Analytics Team

Refresh:

Daily
```

---

## Data Catalog

Tools:

* DataHub
* Amundsen
* Collibra

---

## Quality Rules

Examples:

* No missing IDs
* Valid values
* Freshness checks

---

# Learning Path

## Beginner

Understand:

* Databases
* Tables
* Schemas

---

## Intermediate

Learn:

* Warehouses
* Data lakes
* Data modeling

---

## Advanced

Learn:

* Lakehouse architectures
* Distributed systems
* Cloud platforms

---

# Resources

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Data platforms
* Warehouses
* Lakes

### Designing Data-Intensive Applications

Author:

Martin Kleppmann

Focus:

* Large-scale data systems

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Focus:

* Dimensional modeling

---

## Documentation

DuckDB:

[https://duckdb.org/docs/](https://duckdb.org/docs/)

Snowflake:

[https://docs.snowflake.com/](https://docs.snowflake.com/)

BigQuery:

[https://cloud.google.com/bigquery/docs](https://cloud.google.com/bigquery/docs)

Databricks:

[https://docs.databricks.com/](https://docs.databricks.com/)

---

# Summary

Data warehouses and data lakes solve different problems.

A simple comparison:

```
Data Warehouse:

Clean + Structured + Analytics


Data Lake:

Raw + Flexible + Large Scale
```

Modern analytics engineering increasingly combines both approaches through lakehouse architectures.

A strong analytics engineer understands where data should live, how it should be transformed, and how users can reliably consume it.