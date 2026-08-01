# Microsoft Azure

## Overview

Microsoft Azure is a cloud computing platform that provides services for building, deploying, and managing applications and data solutions.

Azure is widely used by organizations for:

- Data storage
- Data engineering
- Analytics
- Machine learning
- Enterprise applications

For analytics engineers, Azure provides tools for building scalable data platforms and analytics workflows.

---

# Why Azure Matters For Analytics Engineering

Azure enables organizations to build:

- Data lakes
- Data warehouses
- ETL pipelines
- Analytics platforms
- Machine learning systems

A typical Azure analytics architecture:

```
Data Sources

      ↓

Azure Data Lake Storage

      ↓

Azure Data Factory

      ↓

Azure Synapse Analytics

      ↓

BI Dashboard
```

---

# Azure Core Data Services

Important Azure services for analytics:

```
Azure Data Lake Storage

Azure Data Factory

Azure Synapse Analytics

Azure Databricks

Azure SQL Database

Azure Functions

Power BI
```

---

# 1. Azure Data Lake Storage (ADLS)

## Overview

Azure Data Lake Storage is a scalable cloud storage service designed for storing large volumes of data.

It supports:

- Structured data
- Semi-structured data
- Unstructured data

Examples:

- CSV files
- JSON files
- Logs
- Images
- Machine learning datasets

---

# ADLS Architecture

Example:

```
Data Sources

      ↓

ADLS Raw Layer

      ↓

Processing Layer

      ↓

Analytics Layer
```

---

# Data Lake Layers

A common structure:

```
adls://company-data/

│

├── raw/

│   └── transactions.csv

│

├── cleaned/

│   └── transactions.parquet

│

└── curated/

    └── customer_metrics.parquet
```

---

# ADLS Advantages

Benefits:

- Highly scalable
- Secure
- Low-cost storage
- Integrates with Azure analytics services

---

# 2. Azure Data Factory (ADF)

## Overview

Azure Data Factory is a cloud-based data integration service.

It is used to:

- Extract data
- Move data
- Transform data
- Orchestrate pipelines

---

# Data Factory Pipeline

Example:

```
Source Database

        ↓

Azure Data Factory

        ↓

Transformation

        ↓

Data Warehouse
```

---

# ADF Components

## Pipeline

A workflow containing multiple activities.

Example:

```
Extract Data

↓

Transform Data

↓

Load Data
```

---

## Dataset

Represents the data being processed.

Examples:

```
CSV File

SQL Table

API Response
```

---

## Linked Service

Defines connections to external systems.

Examples:

```
Database Connection

Storage Connection
```

---

# ADF Activities

Common activities:

## Copy Activity

Moves data between systems.

Example:

```
SQL Database

↓

Data Lake
```

---

## Data Flow

Performs visual transformations.

Example:

```
Clean Data

Remove Duplicates

Join Tables
```

---

# 3. Azure Synapse Analytics

## Overview

Azure Synapse Analytics is Microsoft's cloud analytics platform.

It combines:

- Data warehousing
- Big data analytics
- Data integration

---

# Synapse Architecture

Example:

```
Data Lake

      ↓

Synapse Workspace

      ↓

SQL Analytics

      ↓

BI Tools
```

---

# Synapse Components

## Dedicated SQL Pool

A traditional data warehouse engine.

Used for:

- Enterprise reporting
- Large SQL workloads

---

## Serverless SQL Pool

Query data without managing infrastructure.

Example:

```
Query Files In Data Lake

↓

Return Results
```

---

## Spark Pools

Used for large-scale processing.

Supports:

- Python
- Scala
- Spark SQL

---

# Synapse Use Cases

Examples:

- Customer analytics
- Financial reporting
- Enterprise dashboards
- Large-scale data processing

---

# 4. Azure Databricks

## Overview

Azure Databricks is a cloud analytics platform based on Apache Spark.

Used for:

- Data engineering
- Machine learning
- Big data processing

---

# Databricks Workflow

Example:

```
Raw Data

      ↓

Spark Processing

      ↓

Clean Data

      ↓

Analytics Tables
```

---

# Databricks Supports

Languages:

- Python
- SQL
- Scala
- R

---

# Databricks Use Cases

Examples:

- Data transformations
- Feature engineering
- Machine learning pipelines
- Large-scale analytics

---

# 5. Azure SQL Database

## Overview

Azure SQL Database is a managed relational database service.

Supports:

- SQL Server workloads
- Transactional applications
- Operational databases

---

Analytics workflow:

```
Application Database

        ↓

ETL Pipeline

        ↓

Analytics Warehouse
```

---

# 6. Azure Functions

## Overview

Azure Functions is a serverless compute service.

It runs code in response to events.

---

Example:

```
New File Uploaded

        ↓

Azure Function Triggered

        ↓

Process File
```

---

Analytics use cases:

- Data validation
- Automation
- Event processing

---

# 7. Power BI Integration

Power BI is Microsoft's business intelligence platform.

Azure integrates with Power BI for:

- Dashboards
- Reports
- Data visualization

---

Example:

```
Azure Synapse

        ↓

Power BI

        ↓

Business Dashboard
```

---

# Azure Analytics Architecture Example

Complete enterprise workflow:

```
Operational Database

        ↓

Azure Data Factory

        ↓

Azure Data Lake Storage

        ↓

Azure Databricks

        ↓

Azure Synapse Analytics

        ↓

Power BI
```

---

# Azure Security Concepts

## Azure Active Directory (Entra ID)

Manages:

- Users
- Groups
- Permissions

---

## Role-Based Access Control (RBAC)

Controls resource access.

Example:

```
Data Analyst

↓

Read Access
```

---

## Encryption

Azure supports:

- Encryption at rest
- Encryption in transit

---

# Azure Cost Optimization

Important practices:

## Optimize Storage

Use appropriate storage tiers.

Example:

```
Hot Storage

↓

Frequently Used Data
```

```
Archive Storage

↓

Rarely Used Data
```

---

## Optimize Compute

Avoid unnecessary:

- Virtual machines
- Spark clusters
- SQL resources

---

## Monitor Usage

Track:

- Performance
- Costs
- Resource utilization

---

# Azure For Analytics Engineers

Important skills:

## Data Storage

Know:

```
Azure Data Lake Storage
```

---

## Data Pipelines

Know:

```
Azure Data Factory
```

---

## Data Warehouse

Know:

```
Azure Synapse
```

---

## Processing

Know:

```
Azure Databricks
```

---

## Visualization

Know:

```
Power BI
```

---

# Azure vs AWS

|Azure|AWS|
|-|-|
|Azure Data Lake Storage|Amazon S3|
|Synapse Analytics|Redshift|
|Data Factory|AWS Glue|
|Databricks|EMR/Databricks|
|Power BI integration|Multiple BI integrations|

---

# Interview Questions

## What is Azure Data Factory?

Azure Data Factory is a cloud service used for data integration and building ETL pipelines.

---

## What is Azure Synapse Analytics?

Synapse is a cloud analytics platform combining data warehousing and big data processing.

---

## Difference between Data Lake and Data Warehouse?

A data lake stores raw data, while a warehouse stores structured data optimized for analytics.

---

## What is Azure Databricks?

Azure Databricks is a Spark-based analytics platform used for large-scale data processing and machine learning.

---

# Key Takeaway

Azure provides a complete ecosystem for modern analytics platforms.

The core workflow:

```
Ingest Data

      ↓

Store Data

      ↓

Process Data

      ↓

Transform Data

      ↓

Analyze Data
```

Azure skills are highly valuable for analytics engineers working in enterprise environments.