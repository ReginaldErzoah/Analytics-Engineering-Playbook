# Cloud Data Warehouses

## Overview

A data warehouse is a centralized system designed to store, organize, and analyze large amounts of structured data.

Cloud data warehouses provide scalable analytics infrastructure without requiring organizations to manage physical servers.

Popular cloud data warehouses include:

- Google BigQuery
- Amazon Redshift
- Snowflake
- Azure Synapse Analytics

---

# Why Data Warehouses Matter

Operational databases are designed for daily transactions.

Examples:

- Customer purchases
- Payments
- User registrations

Analytics requires different capabilities:

- Complex queries
- Large-scale aggregations
- Historical analysis

A data warehouse separates these workloads.

---

# Operational Database vs Data Warehouse

|Operational Database|Data Warehouse|
|-|-|
|Designed for transactions|Designed for analytics|
|Frequent updates|Mostly read operations|
|Normalized data|Optimized analytical models|
|Current data|Historical data|
|Application-focused|Business-focused|

---

# Example

Operational database:

```
Customer places order

        ↓

Orders table updated
```

---

Data warehouse:

```
Daily sales data loaded

        ↓

Revenue trends analyzed

        ↓

Business decisions made
```

---

# Data Warehouse Architecture

A typical architecture:

```
Data Sources

      ↓

Data Ingestion

      ↓

Data Storage

      ↓

Data Warehouse

      ↓

Data Transformation

      ↓

BI / Analytics
```

---

# Components of a Data Warehouse

A modern warehouse contains:

```
Sources

+

Storage

+

Compute

+

Transformation

+

Semantic Layer

+

Analytics Tools
```

---

# 1. Data Sources

Sources provide raw data.

Examples:

- Applications
- Databases
- APIs
- Files
- Logs

Example:

```
CRM System

ERP System

Website Events
```

---

# 2. Data Ingestion

Moves data into the warehouse.

Methods:

## Batch Processing

Loads data periodically.

Example:

```
Every Night

↓

Load Sales Data
```

---

## Streaming

Loads data continuously.

Example:

```
Website Event

↓

Real-Time Processing
```

---

# 3. Storage Layer

Stores analytical data.

Examples:

- Tables
- Partitions
- Files

Modern warehouses use:

- Columnar storage
- Compression
- Distributed storage

---

# 4. Compute Layer

Processes queries.

Examples:

```
SELECT

GROUP BY

JOIN

WINDOW FUNCTIONS
```

---

# 5. Transformation Layer

Transforms raw data into useful models.

Common tools:

- SQL
- dbt
- Spark
- Python

---

# Data Warehouse Modeling

Common modeling approaches:

```
Star Schema

Snowflake Schema

Data Vault
```

---

# Star Schema

## Overview

A star schema contains:

```
Fact Tables

+

Dimension Tables
```

Structure:

```
          Customer

              |

Product --- Sales Fact --- Date

              |

          Location
```

---

# Fact Tables

Contain measurable business events.

Examples:

- Sales
- Transactions
- Orders

Example:

```
sales_fact

order_id

customer_id

product_id

amount
```

---

# Dimension Tables

Contain descriptive information.

Examples:

- Customers
- Products
- Employees
- Locations

Example:

```
customer_dimension

customer_id

name

country
```

---

# Snowflake Schema

A normalized version of star schema.

Example:

```
Sales Fact

    |

Customer Dimension

    |

Location Dimension
```

---

# Data Warehouse vs Data Lake

|Data Warehouse|Data Lake|
|-|-|
|Structured data|Raw data|
|Schema before storage|Schema later|
|Analytics optimized|Flexible storage|
|SQL focused|Multiple processing methods|

---

# Modern Lakehouse Architecture

A lakehouse combines:

```
Data Lake

+

Data Warehouse
```

Benefits:

- Low-cost storage
- Analytics performance
- Machine learning support

---

# Cloud Data Warehouse Examples

## Google BigQuery

Features:

- Serverless
- SQL analytics
- Massive scale

---

## Amazon Redshift

Features:

- Columnar storage
- MPP architecture
- AWS integration

---

## Snowflake

Features:

- Multi-cloud
- Separation of storage and compute
- Data sharing

---

## Azure Synapse Analytics

Features:

- Enterprise analytics
- SQL pools
- Spark integration

---

# Storage and Compute Separation

Modern warehouses separate:

```
Storage

+

Compute
```

Example:

Store:

```
100 TB Data
```

Use compute only when querying.

Benefits:

- Independent scaling
- Cost control
- Better flexibility

---

# Partitioning

Partitioning divides large tables into smaller sections.

Example:

Sales table:

```
sales_2024

sales_2025

sales_2026
```

---

Benefits:

- Faster queries
- Reduced processing cost

---

# Clustering

Clustering organizes related data together.

Example:

Cluster by:

```
customer_id
```

Benefits:

- Improved query performance
- Efficient filtering

---

# Data Warehouse Security

Important concepts:

## Access Control

Controls:

- Who can access data
- What they can do

---

## Encryption

Protects:

- Stored data
- Data movement

---

## Auditing

Tracks:

- User activity
- Data access

---

# Data Warehouse Performance Optimization

## Select Only Required Columns

Avoid:

```sql
SELECT *

FROM sales;
```

Better:

```sql
SELECT

customer_id,

amount

FROM sales;
```

---

## Filter Early

Better:

```sql
WHERE date >= '2026-01-01'
```

---

## Optimize Joins

Avoid unnecessary joins.

---

## Use Appropriate Data Types

Example:

```
INTEGER

DATE

BOOLEAN
```

instead of:

```
STRING
```

---

# Data Warehouses In Analytics Engineering

Analytics engineers commonly:

- Design models
- Build transformations
- Create metrics
- Optimize queries
- Maintain documentation

Typical workflow:

```
Raw Tables

      ↓

Staging Models

      ↓

Intermediate Models

      ↓

Mart Models

      ↓

Dashboards
```

---

# Interview Questions

## What is a data warehouse?

A data warehouse is a centralized analytical system designed to store and analyze structured business data.

---

## Difference between database and warehouse?

Databases support transactions, while warehouses support analytical queries.

---

## What is a fact table?

A fact table stores measurable business events such as sales or transactions.

---

## What is a dimension table?

A dimension table stores descriptive attributes used for analysis.

---

## What is the difference between a data lake and warehouse?

A data lake stores raw data, while a warehouse stores structured data optimized for analytics.

---

# Key Takeaway

Cloud data warehouses are the foundation of modern analytics platforms.

They provide:

```
Scalable Storage

+

Fast Analytics

+

Reliable Data Models

+

Business Insights
```

Understanding warehouses is essential for analytics engineers building production-grade data systems.