# AWS (Amazon Web Services)

## Overview

Amazon Web Services (AWS) is the world's largest cloud computing platform.

AWS provides hundreds of cloud services that allow organizations to:

- Store data
- Process information
- Build applications
- Run analytics workloads
- Deploy machine learning systems

For analytics engineers, AWS provides the infrastructure required to build modern data platforms.

---

# Why AWS Matters For Analytics Engineering

Modern analytics systems commonly use AWS for:

- Data storage
- Data ingestion
- Data transformation
- Data warehousing
- Workflow automation

A typical AWS analytics architecture:

```
Data Sources

      ↓

Amazon S3

      ↓

AWS Glue / Processing

      ↓

Amazon Redshift

      ↓

BI Dashboard
```

---

# AWS Core Data Services

The most important AWS services for analytics are:

```
Amazon S3

Amazon Redshift

AWS Glue

Amazon Athena

Amazon EMR

AWS Lambda

Amazon RDS
```

---

# 1. Amazon S3

## Overview

Amazon Simple Storage Service (S3) is an object storage service.

It stores:

- Files
- Datasets
- Logs
- Images
- Backups

---

# S3 Concepts

## Bucket

A bucket is a container for storing objects.

Example:

```
company-data-bucket
```

---

## Object

An object is a file stored inside a bucket.

Example:

```
sales_2026.csv
```

---

# S3 Data Lake Architecture

A common structure:

```
s3://company-data/

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

# S3 Storage Layers

## Raw Layer

Contains original data.

Example:

```
API Responses

CSV Files

Logs
```

---

## Processed Layer

Contains cleaned data.

Example:

```
Validated Tables

Standardized Formats
```

---

## Analytics Layer

Contains data ready for analysis.

Example:

```
Customer Metrics

Sales Reports
```

---

# S3 Advantages

Benefits:

- Highly scalable
- Durable storage
- Cost effective
- Integrates with analytics tools

---

# 2. Amazon Redshift

## Overview

Amazon Redshift is a cloud data warehouse.

It is designed for:

- Analytical queries
- Business intelligence
- Large-scale SQL workloads

---

# Redshift Architecture

Example:

```
Data Sources

      ↓

ETL Pipeline

      ↓

Redshift Warehouse

      ↓

BI Tools
```

---

# Redshift Features

## Columnar Storage

Stores data by columns.

Benefits:

- Faster analytics queries
- Better compression

---

## Massively Parallel Processing (MPP)

Multiple nodes process queries simultaneously.

Example:

```
Large Query

      ↓

Split Work

      ↓

Multiple Nodes Execute
```

---

# Redshift Use Cases

Common uses:

- Sales analytics
- Customer analytics
- Financial reporting
- Business dashboards

---

# SQL In Redshift

Example:

```sql
SELECT

customer_id,

SUM(order_value) AS revenue

FROM orders

GROUP BY customer_id;
```

---

# 3. AWS Glue

## Overview

AWS Glue is a serverless data integration service.

It is used for:

- ETL jobs
- Data discovery
- Data transformation

---

# Glue Workflow

Example:

```
Data Source

      ↓

AWS Glue Job

      ↓

Clean Data

      ↓

Data Warehouse
```

---

# Glue Components

## Glue Crawler

Automatically discovers data structure.

Example:

```
CSV File

↓

Detect Columns

↓

Create Schema
```

---

## Glue Data Catalog

Stores metadata about datasets.

Example:

```
Table Name

Columns

Data Types

Location
```

---

## Glue Jobs

Execute transformation logic.

Can use:

- Python
- Apache Spark

---

# 4. Amazon Athena

## Overview

Amazon Athena is a serverless SQL query service.

It allows users to query data directly from S3.

---

Example:

```
CSV Files In S3

        ↓

Athena SQL Query

        ↓

Results
```

---

Example query:

```sql
SELECT *

FROM sales

WHERE year = 2026;
```

---

# Athena Advantages

Benefits:

- No infrastructure management
- Pay per query
- SQL-based

---

# 5. Amazon EMR

## Overview

Amazon Elastic MapReduce (EMR) is a managed big data platform.

Used for:

- Apache Spark
- Hadoop
- Large-scale processing

---

Example:

```
Large Dataset

      ↓

EMR Cluster

      ↓

Spark Processing

      ↓

Output Data
```

---

# EMR Use Cases

Examples:

- Machine learning pipelines
- Log analysis
- Large-scale transformations

---

# 6. AWS Lambda

## Overview

AWS Lambda is a serverless compute service.

It runs code without managing servers.

---

Analytics examples:

- Trigger data processing
- Validate files
- Process events

---

Example:

```
New File Uploaded To S3

        ↓

Lambda Trigger

        ↓

Process File
```

---

# 7. Amazon RDS

## Overview

Amazon Relational Database Service provides managed databases.

Supported databases:

- PostgreSQL
- MySQL
- MariaDB
- SQL Server

---

Analytics workflow:

```
Operational Database

        ↓

ETL Pipeline

        ↓

Analytics Warehouse
```

---

# AWS Analytics Architecture Example

A complete analytics platform:

```
CRM System

      ↓

Amazon RDS

      ↓

AWS Glue

      ↓

Amazon S3

      ↓

Amazon Redshift

      ↓

Power BI Dashboard
```

---

# AWS Security Concepts

## IAM

Identity and Access Management controls permissions.

Example:

```
Analyst

↓

Read Only Access
```

---

## Roles

Allow services to access resources securely.

Example:

```
Glue Job

↓

Access S3 Bucket
```

---

## Encryption

AWS supports:

- Data encryption at rest
- Data encryption in transit

---

# AWS Cost Optimization

Important practices:

## Use Appropriate Storage Classes

Example:

Frequently accessed data:

```
S3 Standard
```

Rarely accessed data:

```
S3 Glacier
```

---

## Optimize Queries

Poor:

```sql
SELECT *
FROM huge_table;
```

Better:

```sql
SELECT

needed_columns

FROM huge_table;
```

---

## Monitor Resources

Track:

- Storage usage
- Query costs
- Compute usage

---

# AWS For Analytics Engineers

Important skills:

## Storage

Know:

```
S3
```

---

## Warehousing

Know:

```
Redshift
```

---

## ETL

Know:

```
Glue
```

---

## SQL Analytics

Know:

```
Athena
```

---

## Security

Know:

```
IAM
```

---

# AWS vs Traditional Data Infrastructure

|Traditional|AWS|
|-|-|
|Physical servers|Cloud resources|
|Manual scaling|Automatic scaling|
|High upfront cost|Usage-based pricing|
|Manual maintenance|Managed services|

---

# Interview Questions

## What is Amazon S3?

Amazon S3 is a scalable object storage service used for storing files, datasets, backups, and data lake storage.

---

## What is Redshift?

Redshift is AWS's cloud data warehouse optimized for analytical SQL workloads.

---

## Difference between Athena and Redshift?

Athena queries data directly from S3, while Redshift stores structured analytical data in a warehouse.

---

## What is AWS Glue?

AWS Glue is a serverless ETL and data integration service.

---

# Key Takeaway

AWS provides the building blocks for modern analytics platforms.

A typical analytics engineer workflow:

```
Store Data

      ↓

Process Data

      ↓

Transform Data

      ↓

Warehouse Data

      ↓

Analyze Data
```

AWS knowledge enables engineers to build scalable, reliable, and production-ready data systems.