# DuckDB Fundamentals for Analytics Engineering

## Overview

DuckDB is an embedded analytical database designed for fast analytical workloads.

It is often described as:

> "SQLite for analytics"

because it provides a lightweight database experience while delivering high-performance analytical processing.

DuckDB is especially useful for:

- Local analytics development
- Data exploration
- Data engineering workflows
- ETL/ELT pipelines
- Analytics engineering projects

---

# Why DuckDB Matters

Traditional databases are usually designed for transactional workloads.

Examples:

- Banking transactions
- User authentication
- Application databases

These systems prioritize:

- Many small reads/writes
- Fast individual transactions

---

DuckDB is designed for analytical workloads.

Examples:

- Reporting
- Aggregations
- Data transformation
- Business intelligence

It prioritizes:

- Large scans
- Aggregations
- Column processing
- Analytical queries

---

# DuckDB Architecture

DuckDB runs inside the application.

Traditional architecture:

```
Application

      |

Database Server

      |

Storage
```

DuckDB:

```
Application

      |

DuckDB Engine

      |

Database File
```

No separate database server is required.

---

# Key Features of DuckDB

## Embedded Database

DuckDB runs locally.

Example:

```python
import duckdb

connection = duckdb.connect(
    "analytics.duckdb"
)
```

No:

- Server setup
- Database administration
- Configuration overhead

---

## Columnar Execution

DuckDB is optimized for analytical queries.

Example:

A sales table:

```
customer_id

product_id

amount

date
```

If a query only needs:

```
amount
```

DuckDB can process only that column.

Benefits:

- Faster queries
- Less memory usage

---

## Vectorized Execution

DuckDB processes data in batches.

Instead of:

```
Row 1
Row 2
Row 3
```

It processes:

```
Batch of rows
```

Benefits:

- CPU efficiency
- Faster analytical workloads

---

# Installing DuckDB

Python installation:

```bash
pip install duckdb
```

CLI installation:

```bash
duckdb
```

---

# Connecting To DuckDB

Python:

```python
import duckdb

con = duckdb.connect(
    "database.duckdb"
)
```

---

Temporary database:

```python
con = duckdb.connect()
```

This creates an in-memory database.

---

# Creating Tables

Example:

```sql
CREATE TABLE customers (

customer_id INTEGER,

name VARCHAR

);
```

---

Insert data:

```sql
INSERT INTO customers VALUES

(1,'John'),

(2,'Mary');
```

---

Query:

```sql
SELECT *

FROM customers;
```

---

# Reading CSV Files

DuckDB can query files directly.

Example:

```sql
SELECT *

FROM read_csv_auto(
'customers.csv'
);
```

No import step required.

---

# Reading Parquet Files

DuckDB works extremely well with Parquet.

Example:

```sql
SELECT *

FROM read_parquet(
'customers.parquet'
);
```

Benefits:

- Fast reads
- Compression
- Columnar storage

---

# DuckDB With Pandas

DuckDB integrates with Python dataframes.

Example:

```python
import pandas as pd
import duckdb


df = pd.read_csv(
"tickets.csv"
)


result = duckdb.sql(
"""
SELECT *

FROM df

"""
).df()
```

---

# DuckDB SQL Capabilities

DuckDB supports:

- SQL joins
- Window functions
- Aggregations
- CTEs
- Subqueries
- Analytical functions

Example:

```sql
SELECT

priority_level,

COUNT(*) AS tickets

FROM tickets

GROUP BY priority_level;
```

---

# DuckDB As An Analytics Database

Typical workflow:

```
CSV Files

↓

DuckDB

↓

dbt Transformations

↓

Parquet Exports

↓

Power BI
```

This architecture is lightweight but production-like.

---

# DuckDB In Analytics Engineering

DuckDB is commonly used for:

## Development

Build and test models locally.

---

## Data Transformation

Execute SQL transformations.

---

## Data Validation

Run quality checks.

---

## BI Preparation

Create reporting datasets.

---

# DuckDB vs Traditional Databases

| Feature | DuckDB | PostgreSQL |
|-|-|-|
| Server required | No | Yes |
| Analytics | Excellent | Good |
| Transactions | Limited | Excellent |
| OLAP workloads | Excellent | Moderate |
| Local development | Excellent | Moderate |

---

# DuckDB vs Pandas

| Feature | DuckDB | Pandas |
|-|-|-|
| Large datasets | Better | Limited |
| SQL support | Yes | No |
| Dataframes | Supports | Native |
| Analytics queries | Excellent | Good |
| Memory efficiency | Better | Lower |

---

# DuckDB In SupportOps Intelligence

DuckDB was used as the analytical database.

Architecture:

```
Raw CSV

customer_support_tickets.csv

        |

        ↓

DuckDB

supportops.duckdb

        |

        ↓

dbt Models

stg_ticket

int_ticket_metrics

fact_ticket

dimensions

        |

        ↓

Parquet Files

        |

        ↓

Power BI Dashboard
```

---

# Database File Structure

Project database:

```
database/

supportops.duckdb
```

This file stores:

- Raw loaded data
- dbt generated tables
- Analytical models

---

# Loading Data Into DuckDB

Python script:

```
python/load_to_duckdb.py
```

Workflow:

```
CSV

↓

Python

↓

DuckDB Table
```

---

# Exporting Data From DuckDB

Python script:

```
python/export_to_parquet.py
```

Workflow:

```
DuckDB Tables

↓

Parquet Files

↓

Power BI
```

---

# Best Practices

## Keep Database Files Organized

Example:

```
database/

analytics.duckdb
```

---

## Separate Raw and Analytics Layers

Example:

```
raw data

↓

DuckDB

↓

transformed models
```

---

## Use Parquet For BI Consumption

Advantages:

- Faster loading
- Smaller files
- Better compression

---

# Skills To Master

## DuckDB

Learn:

- SQL execution
- File querying
- Python integration
- Database management


## Analytical Databases

Learn:

- OLAP concepts
- Columnar storage
- Query engines
- Data formats


## Data Formats

Learn:

- CSV
- Parquet
- JSON
- Arrow


---

# Recommended Resources

## Documentation

DuckDB Official Documentation:

https://duckdb.org/docs/


DuckDB Python API:

https://duckdb.org/docs/api/python/overview


---

## Books

### Designing Data-Intensive Applications

Author:
Martin Kleppmann

Focus:

- Data systems
- Storage
- Distributed systems


---

## Courses

### DuckDB Tutorial

MotherDuck:

https://motherduck.com/docs/


### Data Engineering Zoomcamp

DataTalksClub:

https://github.com/DataTalksClub/data-engineering-zoomcamp


---

# Summary

DuckDB is a powerful analytical database for modern analytics workflows.

It provides:

- Fast local analytics
- SQL-based transformations
- Python integration
- Parquet support

For analytics engineering projects, DuckDB provides a practical environment for building production-style data systems without requiring expensive cloud infrastructure.