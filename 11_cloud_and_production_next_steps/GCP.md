# Google Cloud Platform (GCP)

## Overview

Google Cloud Platform (GCP) is Google's cloud computing platform that provides infrastructure and services for building scalable applications, data platforms, and machine learning systems.

GCP is especially popular for:

- Data analytics
- Artificial intelligence
- Machine learning
- Large-scale data processing

For analytics engineers, GCP provides powerful tools for building modern data platforms.

---

# Why GCP Matters For Analytics Engineering

GCP enables organizations to build:

- Data lakes
- Cloud warehouses
- Data pipelines
- Machine learning platforms
- Real-time analytics systems

A typical GCP analytics architecture:

```
Data Sources

      ↓

Google Cloud Storage

      ↓

Dataflow / Dataproc

      ↓

BigQuery

      ↓

Looker / BI Dashboard
```

---

# GCP Core Data Services

Important GCP services for analytics:

```
Google Cloud Storage

BigQuery

Dataflow

Dataproc

Cloud Composer

Cloud Functions

Cloud SQL

Looker
```

---

# 1. Google Cloud Storage (GCS)

## Overview

Google Cloud Storage is an object storage service used to store large amounts of data.

It supports:

- Structured data
- Semi-structured data
- Unstructured data

Examples:

- CSV files
- JSON files
- Images
- Logs
- Backups

---

# GCS Concepts

## Bucket

A bucket stores objects.

Example:

```
company-analytics-data
```

---

## Object

A file stored inside a bucket.

Example:

```
customer_transactions.csv
```

---

# Data Lake Architecture With GCS

Example:

```
gs://company-data/

│

├── raw/

│   └── sales.csv

│

├── processed/

│   └── clean_sales.parquet

│

└── analytics/

    └── customer_metrics.parquet
```

---

# GCS Storage Classes

GCP provides different storage options.

## Standard Storage

For frequently accessed data.

Example:

```
Active analytics datasets
```

---

## Nearline Storage

For data accessed occasionally.

---

## Coldline Storage

For rarely accessed data.

---

## Archive Storage

For long-term retention.

---

# GCS Advantages

Benefits:

- Highly scalable
- Secure
- Durable
- Integrates with BigQuery and Dataflow

---

# 2. BigQuery

## Overview

BigQuery is Google's fully managed cloud data warehouse.

It is designed for:

- Large-scale SQL analytics
- Business intelligence
- Data exploration

---

# Why BigQuery Is Important

BigQuery allows organizations to analyze:

```
Terabytes

+

Petabytes

+

Large Enterprise Datasets
```

without managing servers.

---

# BigQuery Architecture

Example:

```
Data Sources

      ↓

BigQuery Tables

      ↓

SQL Queries

      ↓

Analytics Results
```

---

# BigQuery Features

## Serverless Architecture

Users do not manage infrastructure.

Example:

```
Upload Data

↓

Run SQL

↓

Receive Results
```

---

## Columnar Storage

Stores data by columns.

Benefits:

- Faster analytical queries
- Better compression

---

## Massive Parallel Processing

Big queries are distributed across many machines.

Example:

```
Large Query

      ↓

Multiple Workers

      ↓

Combined Results
```

---

# BigQuery SQL Example

```sql
SELECT

customer_id,

SUM(amount) AS total_sales

FROM orders

GROUP BY customer_id;
```

---

# BigQuery Use Cases

Examples:

- Sales analytics
- Marketing analytics
- Financial reporting
- Customer intelligence
- Machine learning features

---

# 3. Dataflow

## Overview

Google Cloud Dataflow is a managed data processing service.

It supports:

- Batch processing
- Stream processing

Built on:

```
Apache Beam
```

---

# Dataflow Workflow

Example:

```
Raw Data

      ↓

Dataflow Pipeline

      ↓

Cleaned Data

      ↓

BigQuery
```

---

# Dataflow Use Cases

Examples:

- ETL pipelines
- Real-time analytics
- Event processing

---

# Batch Processing Example

Process:

```
Daily Sales Files

↓

Transform Data

↓

Load Warehouse
```

---

# Streaming Example

Process:

```
Website Events

↓

Dataflow

↓

Real-Time Dashboard
```

---

# 4. Dataproc

## Overview

Google Cloud Dataproc is a managed Apache Spark and Hadoop service.

Used for:

- Big data processing
- Distributed analytics
- Machine learning workloads

---

# Dataproc Workflow

Example:

```
Large Dataset

      ↓

Dataproc Cluster

      ↓

Spark Processing

      ↓

Output Data
```

---

# Dataproc Supports

Technologies:

- Apache Spark
- Hadoop
- Hive
- Presto

---

# Dataproc Use Cases

Examples:

- Large-scale transformations
- Log analysis
- Machine learning preparation

---

# 5. Cloud Composer

## Overview

Cloud Composer is Google's managed workflow orchestration service.

It is based on:

```
Apache Airflow
```

---

# Workflow Example

```
Extract Data

      ↓

Transform Data

      ↓

Load BigQuery

      ↓

Run Tests
```

---

# Composer Use Cases

Used for:

- Scheduling pipelines
- Managing dependencies
- Monitoring workflows

---

# 6. Cloud Functions

## Overview

Cloud Functions is a serverless execution service.

It runs code in response to events.

---

Example:

```
New File Uploaded

      ↓

Cloud Function Triggered

      ↓

Process File
```

---

# Analytics Use Cases

Examples:

- Data validation
- File processing
- API automation

---

# 7. Cloud SQL

## Overview

Cloud SQL is Google's managed relational database service.

Supports:

- PostgreSQL
- MySQL
- SQL Server

---

Analytics workflow:

```
Application Database

        ↓

Data Pipeline

        ↓

BigQuery

        ↓

Dashboard
```

---

# 8. Looker

## Overview

Looker is Google's business intelligence and analytics platform.

Used for:

- Dashboards
- Reports
- Data exploration

---

Example:

```
BigQuery

      ↓

Looker

      ↓

Business Insights
```

---

# GCP Analytics Architecture Example

Complete workflow:

```
Application Database

        ↓

Cloud Storage

        ↓

Dataflow

        ↓

BigQuery

        ↓

Looker Dashboard
```

---

# GCP Security Concepts

## Identity and Access Management (IAM)

Controls:

- Users
- Roles
- Permissions

Example:

```
Analyst

↓

Query Access Only
```

---

## Service Accounts

Allow applications and services to access resources securely.

Example:

```
Dataflow Job

↓

Read GCS Bucket
```

---

## Encryption

GCP provides:

- Encryption at rest
- Encryption in transit

---

# GCP Cost Optimization

Important practices:

## Optimize Queries

Avoid:

```sql
SELECT *

FROM huge_table;
```

Prefer:

```sql
SELECT

required_columns

FROM huge_table;
```

---

## Partition Tables

Partition large datasets by:

- Date
- Region
- Category

Benefits:

- Faster queries
- Lower costs

---

## Monitor Resources

Track:

- Query usage
- Storage
- Compute

---

# GCP For Analytics Engineers

Important skills:

## Storage

Know:

```
Cloud Storage
```

---

## Warehouse

Know:

```
BigQuery
```

---

## Processing

Know:

```
Dataflow

Dataproc
```

---

## Orchestration

Know:

```
Cloud Composer
```

---

## Visualization

Know:

```
Looker
```

---

# GCP vs AWS vs Azure

|Analytics Need|AWS|Azure|GCP|
|-|-|-|-|
|Object Storage|S3|ADLS|Cloud Storage|
|Warehouse|Redshift|Synapse|BigQuery|
|ETL|Glue|Data Factory|Dataflow|
|Big Data|EMR|Databricks|Dataproc|
|BI|QuickSight|Power BI|Looker|

---

# Interview Questions

## What is BigQuery?

BigQuery is Google's serverless cloud data warehouse designed for large-scale SQL analytics.

---

## What is Dataflow?

Dataflow is a managed service for batch and streaming data processing.

---

## Difference between BigQuery and Cloud Storage?

Cloud Storage stores raw files, while BigQuery stores structured analytical data optimized for querying.

---

## What is Cloud Composer?

Cloud Composer is a managed Apache Airflow service used for workflow orchestration.

---

# Key Takeaway

GCP provides a powerful ecosystem for modern analytics engineering.

The typical workflow:

```
Collect Data

      ↓

Store Data

      ↓

Process Data

      ↓

Warehouse Data

      ↓

Analyze Data
```

Mastering GCP enables engineers to build scalable, cloud-native analytics platforms.