# Apache Spark

## Overview

Apache Spark is an open-source distributed computing framework designed for processing large-scale data quickly.

Spark is widely used in:

- Data engineering
- Big data analytics
- Machine learning
- Real-time data processing

It allows organizations to process datasets that are too large for traditional single-machine systems.

---

# Why Apache Spark Matters

Traditional processing:

```
Large Dataset

        ↓

Single Computer

        ↓

Slow Processing
```

Spark approach:

```
Large Dataset

        ↓

Distributed Cluster

        ↓

Parallel Processing

        ↓

Faster Results
```

---

# Spark Core Concept

Spark uses distributed computing.

Instead of one machine processing everything:

```
Dataset

        ↓

Split Into Partitions

        ↓

Multiple Machines Process

        ↓

Combine Results
```

---

# Apache Spark Architecture

A Spark application contains:

```
Driver Program

        ↓

Cluster Manager

        ↓

Worker Nodes

        ↓

Executors
```

---

# Spark Components

## Driver Program

The driver controls the Spark application.

Responsibilities:

- Creates execution plan
- Coordinates tasks
- Collects results

---

## Cluster Manager

Allocates resources.

Examples:

- Spark Standalone
- YARN
- Kubernetes

---

## Worker Nodes

Machines that execute tasks.

---

## Executors

Processes that run tasks on worker nodes.

Responsibilities:

- Execute computations
- Store data in memory
- Return results

---

# Spark Data Processing Model

Spark processes data through:

```
Transformations

+

Actions
```

---

# Transformations

Transformations create new datasets from existing data.

They are lazy operations.

Examples:

- filter()
- map()
- select()
- join()

---

Example:

```python
filtered_data = data.filter(
    data.amount > 100
)
```

Nothing executes immediately.

---

# Actions

Actions trigger execution.

Examples:

- count()
- collect()
- save()

---

Example:

```python
filtered_data.count()
```

Spark now executes the transformation.

---

# Lazy Evaluation

Spark does not immediately execute transformations.

Example:

```
Read Data

↓

Filter Rows

↓

Aggregate

↓

Save Output
```

Spark builds a plan first.

Benefits:

- Optimization
- Faster execution
- Reduced unnecessary work

---

# Spark RDDs

## Overview

RDD stands for:

```
Resilient Distributed Dataset
```

RDDs are Spark's original distributed data structure.

Characteristics:

- Distributed
- Immutable
- Fault tolerant

---

# RDD Example

Data:

```
[1,2,3,4,5]
```

Spark distributes:

```
Worker 1

[1,2]


Worker 2

[3,4]


Worker 3

[5]
```

---

# Spark DataFrames

## Overview

DataFrames provide a higher-level interface similar to tables in databases.

They are commonly used today.

Example:

```
Customers Table

customer_id

name

country
```

---

# DataFrame Example

Python:

```python
df.select(
    "customer_id",
    "name"
)
```

---

# Spark SQL

Spark supports SQL queries.

Example:

```sql
SELECT

customer_id,

SUM(amount)

FROM sales

GROUP BY customer_id;
```

---

# Spark Libraries

Spark includes multiple libraries.

---

# Spark SQL

Used for:

- Structured data
- SQL queries
- DataFrames

---

# Spark Streaming

Used for:

- Real-time data processing
- Event streams

Examples:

- Website events
- IoT data

---

# MLlib

Spark's machine learning library.

Used for:

- Classification
- Regression
- Clustering

---

# GraphX

Used for graph processing.

Examples:

- Social networks
- Recommendation systems

---

# Spark With Python (PySpark)

## Overview

PySpark is Spark's Python API.

It allows Python developers to use Spark.

---

Example:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

df = spark.read.csv(
    "sales.csv"
)
```

---

# Spark Data Processing Example

Task:

Calculate total sales by customer.

Input:

```
orders.csv
```

Columns:

```
customer_id

amount
```

Process:

```python
sales = df.groupBy(
    "customer_id"
).sum(
    "amount"
)
```

Output:

```
customer_id | total_sales
```

---

# Spark File Formats

Spark commonly works with:

## CSV

Simple but inefficient for large data.

---

## JSON

Flexible but larger.

---

## Parquet

Preferred for analytics.

Advantages:

- Columnar storage
- Compression
- Faster queries

---

# Spark Performance Optimization

## Partitioning

Spark divides data into partitions.

Example:

```
Large Dataset

↓

Partition 1

Partition 2

Partition 3
```

---

## Caching

Stores frequently used data in memory.

Example:

```python
df.cache()
```

---

## Avoid Unnecessary Shuffles

Shuffling moves data between machines.

Can slow processing.

---

## Use Efficient Formats

Prefer:

```
Parquet
```

instead of:

```
CSV
```

---

# Spark In Cloud Platforms

Spark is commonly used with:

## AWS

Examples:

- Amazon EMR
- AWS Glue

---

## Azure

Example:

- Azure Databricks

---

## Google Cloud

Examples:

- Dataproc
- Databricks

---

# Spark And Data Engineering

Common workflows:

```
Raw Data

↓

Spark Processing

↓

Clean Dataset

↓

Data Warehouse
```

---

# Spark And Analytics Engineering

Analytics engineers may use Spark for:

- Large transformations
- Data preparation
- Feature engineering

---

# Spark And Machine Learning

Example workflow:

```
Raw Customer Data

↓

Spark Cleaning

↓

Feature Engineering

↓

ML Model Training
```

---

# Spark Advantages

## Scalability

Processes very large datasets.

---

## Speed

Uses:

- Parallel processing
- Memory computation

---

## Flexibility

Supports:

- SQL
- Python
- Scala
- Java

---

# Spark Limitations

## Requires Cluster Management

Distributed systems add complexity.

---

## Not Always Needed

Small datasets may be faster with:

- SQL
- Pandas

---

## Memory Requirements

Poor optimization can cause failures.

---

# Spark Best Practices

## 1. Understand Data Size

Do not use Spark unnecessarily.

---

## 2. Optimize Partitions

Too many or too few partitions hurt performance.

---

## 3. Use Columnar Formats

Prefer:

```
Parquet

Delta Lake
```

---

## 4. Monitor Jobs

Track:

- Execution time
- Memory usage
- Failures

---

# Interview Questions

## What is Apache Spark?

Apache Spark is a distributed computing framework used for large-scale data processing.

---

## Why is Spark fast?

Spark uses distributed processing and in-memory computation.

---

## Difference between RDD and DataFrame?

RDDs are low-level distributed collections, while DataFrames provide structured, optimized data processing.

---

## What is lazy evaluation?

Lazy evaluation means Spark delays execution until an action requires results.

---

## What is PySpark?

PySpark is the Python API for Apache Spark.

---

# Key Takeaway

Apache Spark enables scalable data processing.

It provides:

```
Distributed Computing

+

Fast Processing

+

Large-Scale Transformation

+

Machine Learning Support
```

Spark is a core technology for modern data engineering and analytics platforms.