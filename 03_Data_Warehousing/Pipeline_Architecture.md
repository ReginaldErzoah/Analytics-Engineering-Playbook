# Data Pipeline Architecture

## Overview

A data pipeline architecture defines how data moves from source systems to analytical destinations.

A well-designed pipeline ensures that data is:

- Reliable
- Scalable
- Secure
- Maintainable
- Available for analytics

Modern analytics platforms typically follow an ELT architecture.

---

# Basic Data Pipeline Architecture

A simple pipeline:

```
Data Sources

      ↓

Data Ingestion

      ↓

Raw Storage

      ↓

Transformation Layer

      ↓

Analytics Models

      ↓

BI & Applications
```

---

# Pipeline Architecture Components

## 1. Data Sources

The origin of data.

Examples:

### Operational Databases

- PostgreSQL
- MySQL
- SQL Server

Example:

```
customers

orders

transactions
```

---

### SaaS Applications

Examples:

- Salesforce
- Zendesk
- HubSpot

---

### Files

Examples:

- CSV
- Excel
- JSON
- Parquet

---

### APIs

Examples:

- Payment APIs
- Weather APIs
- Customer support APIs

---

# 2. Data Ingestion Layer

The ingestion layer moves data from sources into storage.

Responsibilities:

- Extract data
- Schedule transfers
- Handle authentication
- Manage failures

---

# Types of Data Ingestion

## Batch Ingestion

Data moves at scheduled intervals.

Example:

```
Every night:

Load yesterday's transactions
```

Common for:

- Reporting
- Finance
- Historical analysis

---

## Real-Time Ingestion

Data moves continuously.

Example:

```
Customer action

↓

Event captured immediately
```

Common for:

- Fraud detection
- Monitoring
- Recommendations

---

# Popular Data Ingestion Tools

|Tool|Purpose|
|-|-|
|Airbyte|Open-source data integration|
|Fivetran|Managed connectors|
|Stitch|Cloud data pipelines|
|Kafka|Real-time event streaming|
|AWS Glue|Cloud ETL service|

---

# 3. Data Storage Layer

After ingestion, data is stored.

Common storage systems:

## Data Warehouse

Designed for analytics.

Examples:

- Snowflake
- BigQuery
- Amazon Redshift

Used for:

- BI reporting
- Analytics queries
- Aggregations

---

## Data Lake

Stores raw data in different formats.

Examples:

- Amazon S3
- Azure Data Lake
- Google Cloud Storage

Stores:

- Files
- Logs
- Images
- Raw datasets

---

## Lakehouse

Combines warehouse and lake capabilities.

Examples:

- Databricks
- Delta Lake

---

# 4. Transformation Layer

Transforms raw data into analytical datasets.

Common tasks:

- Cleaning
- Joining tables
- Creating metrics
- Applying business rules

Tools:

- dbt
- SQL
- Spark
- DuckDB

---

# Transformation Architecture

Example:

```
Raw Tables

      ↓

Staging Models

      ↓

Intermediate Models

      ↓

Business Models
```

---

# 5. Analytics Layer

The analytics layer provides data to users.

Examples:

- Power BI
- Tableau
- Looker

Common outputs:

- Dashboards
- Reports
- KPIs
- Forecasts

---

# Modern Analytics Engineering Architecture

Example:

```
                Data Sources

                      |

                      ↓

              Airbyte / Fivetran

                      |

                      ↓

              Cloud Data Warehouse

             (Snowflake / BigQuery)

                      |

                      ↓

                     dbt

                      |

        ----------------------------

        |                          |

   Dimension Tables          Fact Tables

        |                          |

        ----------------------------

                      |

                      ↓

              Power BI / Looker
```

---

# Medallion Architecture

A popular data architecture pattern.

It organizes data into three layers:

```
Bronze

↓

Silver

↓

Gold
```

---

# Bronze Layer

Raw data.

Characteristics:

- Original format
- Minimal transformation
- Historical storage

Example:

```
raw_customer_tickets
```

---

# Silver Layer

Cleaned and standardized data.

Transformations:

- Remove duplicates
- Standardize values
- Fix data types

Example:

```
stg_customer_tickets
```

---

# Gold Layer

Business-ready data.

Used by:

- Analysts
- Dashboards
- Executives

Example:

```
fact_ticket_metrics
```

---

# Data Pipeline Example: Customer Support Analytics

Architecture:

```
Zendesk / Support System

          ↓

Raw Tickets Data

          ↓

DuckDB Warehouse

          ↓

dbt Staging Models

          ↓

dbt Intermediate Models

          ↓

Fact Ticket Metrics

          ↓

Power BI Dashboard
```

---

# Pipeline Dependencies

Data pipelines usually have dependencies.

Example:

```
customers

    ↓

orders

    ↓

customer_sales_metrics
```

The pipeline must execute in the correct order.

---

# Pipeline Orchestration

Orchestration manages:

- Scheduling
- Dependencies
- Retries
- Monitoring

Examples:

|Tool|Description|
|-|-|
|Apache Airflow|Workflow scheduling|
|Dagster|Modern data orchestration|
|Prefect|Python workflows|
|dbt Cloud|dbt job scheduling|

---

# Pipeline Monitoring

Important metrics:

## Pipeline Status

Example:

```
Success

Failed

Running
```

---

## Data Freshness

Example:

```
Last updated:

10 minutes ago
```

---

## Runtime Monitoring

Example:

```
Expected:

20 minutes

Actual:

2 hours
```

---

## Data Quality Monitoring

Checks:

- Missing values
- Duplicate records
- Invalid values
- Schema changes

---

# Pipeline Design Principles

## 1. Reliability

Pipelines should:

- Handle failures
- Retry automatically
- Produce consistent results

---

## 2. Scalability

Should support:

- More users
- More data
- More sources

---

## 3. Maintainability

Use:

- Modular components
- Documentation
- Version control

---

## 4. Observability

Teams should understand:

- What failed
- Why it failed
- How to fix it

---

# Common Pipeline Failures

## Missing Data

Cause:

- Source system failure

Solution:

- Freshness monitoring
- Alerts

---

## Schema Changes

Cause:

Source changes:

```
customer_name

↓

full_name
```

Solution:

- Schema validation
- Documentation

---

## Performance Issues

Causes:

- Large joins
- Poor SQL
- Too much data processing

Solutions:

- Query optimization
- Incremental models

---

# Interview Questions

## What are the main components of a data pipeline?

Source systems, ingestion, storage, transformation, analytics, and monitoring.

---

## What is the difference between a data lake and warehouse?

A lake stores raw data; a warehouse stores structured data optimized for analytics.

---

## What is the purpose of orchestration?

To manage scheduling, dependencies, execution, and failures.

---

## Explain a modern analytics architecture.

Data sources feed a warehouse through ingestion tools. dbt transforms warehouse data into analytical models consumed by BI tools.

---

# Key Takeaway

A data pipeline is the backbone of analytics engineering.

A mature architecture provides:

✅ Reliable data movement  
✅ Scalable processing  
✅ Tested transformations  
✅ Clear lineage  
✅ Trusted analytics