# Data Quality Engineering

## Overview

Data quality engineering is the practice of ensuring that data is accurate, complete, consistent, reliable, and usable throughout the data lifecycle.

High-quality data is essential for:

- Analytics
- Business intelligence
- Machine learning
- Decision-making

Poor data quality leads to:

- Incorrect reports
- Bad business decisions
- Failed machine learning models

---

# Why Data Quality Matters

Organizations make decisions based on data.

Example:

```
Sales Data

      ↓

Revenue Report

      ↓

Business Decision
```

If the sales data is incorrect:

```
Incorrect Data

      ↓

Incorrect Report

      ↓

Poor Decision
```

---

# Dimensions of Data Quality

Data quality is measured using several dimensions.

---

# 1. Accuracy

## Definition

Accuracy measures whether data correctly represents reality.

Example:

Correct:

```
Customer Age = 35
```

Incorrect:

```
Customer Age = 350
```

---

# 2. Completeness

## Definition

Completeness measures whether required data exists.

Example:

Complete:

```
Customer ID

Name

Email
```

Incomplete:

```
Customer ID

Name

Missing Email
```

---

# 3. Consistency

## Definition

Consistency means data has the same meaning across systems.

Example:

System A:

```
Country = Ghana
```

System B:

```
Country = GH
```

These should follow a standard format.

---

# 4. Validity

## Definition

Validity checks whether data follows expected rules.

Example:

Valid:

```
Email = user@example.com
```

Invalid:

```
Email = hello@
```

---

# 5. Uniqueness

## Definition

Ensures records are not duplicated.

Example:

Bad:

```
Customer ID 101

Customer ID 101
```

---

# 6. Timeliness

## Definition

Measures whether data is available when needed.

Example:

Real-time dashboard:

```
Data delay = 24 hours
```

Problem:

Data is not timely.

---

# Data Quality Problems

Common issues:

- Missing values
- Duplicate records
- Incorrect formats
- Invalid values
- Data delays
- Schema changes

---

# Missing Data

## Problem

Important fields contain no values.

Example:

```
customer_email = NULL
```

---

## Solutions

Methods:

- Data validation
- Default values
- Business rules

---

# Duplicate Data

## Problem

The same record appears multiple times.

Example:

```
Order ID 500

Order ID 500
```

---

## Solutions

Use:

- Unique keys
- Deduplication logic
- Data matching

---

# Incorrect Data Types

Example:

Wrong:

```
Age = "25 years"
```

Correct:

```
Age = 25
```

---

# Data Validation

Data validation checks whether incoming data meets expectations.

Examples:

## Schema Validation

Checks:

- Columns
- Data types
- Structure

---

Example:

Expected:

```
customer_id INTEGER
```

Received:

```
customer_id TEXT
```

---

## Range Validation

Checks acceptable values.

Example:

```
Age between 0 and 120
```

---

## Format Validation

Checks formatting rules.

Example:

Email:

```
name@example.com
```

---

# Data Quality Checks

Common checks include:

## Null Checks

Example:

```sql
SELECT *

FROM customers

WHERE email IS NULL;
```

---

## Duplicate Checks

Example:

```sql
SELECT

customer_id,

COUNT(*)

FROM customers

GROUP BY customer_id

HAVING COUNT(*) > 1;
```

---

## Relationship Checks

Ensures relationships exist.

Example:

Every order should have a customer.

---

# Data Quality Framework

A mature data quality process:

```
Define Rules

      ↓

Run Tests

      ↓

Detect Issues

      ↓

Fix Problems

      ↓

Monitor Continuously
```

---

# Data Quality Tools

Popular tools:

## Great Expectations

Open-source data validation framework.

Used for:

- Data testing
- Documentation
- Profiling

---

## dbt Tests

Used in analytics engineering.

Examples:

- Not null tests
- Unique tests
- Relationship tests

---

## Soda

Data quality monitoring platform.

---

## Monte Carlo

Data observability platform.

---

# Data Quality In Data Pipelines

A reliable pipeline includes quality checks.

Example:

```
Extract Data

      ↓

Validate Data

      ↓

Transform Data

      ↓

Load Warehouse

      ↓

Run Tests
```

---

# Data Observability

## Overview

Data observability monitors the health of data systems.

It answers:

- Is data available?
- Is data correct?
- Did something change?

---

# Data Observability Pillars

## Freshness

Is data updated on time?

---

## Volume

Is the amount of data expected?

Example:

Normal:

```
1 million rows
```

Problem:

```
10 rows
```

---

## Schema

Did the structure change?

---

## Distribution

Did values change unexpectedly?

Example:

Average sales suddenly drops by 90%.

---

# Data Quality And Machine Learning

Poor data quality affects ML models.

Example:

Bad:

```
Incorrect customer labels

↓

Poor predictions
```

Good:

```
Clean training data

↓

Better model performance
```

---

# Data Quality Pipeline Example

Customer analytics:

```
CRM Database

      ↓

Extract Customers

      ↓

Validate Records

      ↓

Remove Duplicates

      ↓

Standardize Fields

      ↓

Load Warehouse

      ↓

Dashboard
```

---

# Data Quality Best Practices

## 1. Test Early

Catch problems before they spread.

---

## 2. Automate Checks

Avoid manual inspection.

---

## 3. Monitor Continuously

Quality can degrade over time.

---

## 4. Document Expectations

Define:

- Valid formats
- Required fields
- Business rules

---

## 5. Assign Ownership

Every important dataset should have an owner.

---

# Data Contracts

## Overview

A data contract defines expectations between data producers and consumers.

It specifies:

- Schema
- Data types
- Quality rules
- Ownership

---

Example:

A sales table contract:

```
order_id

INTEGER

Required

Unique
```

---

# Data Quality Engineering Workflow

```
Understand Data

      ↓

Define Rules

      ↓

Build Tests

      ↓

Monitor Results

      ↓

Improve Quality
```

---

# Interview Questions

## What is data quality?

Data quality measures how accurate, complete, consistent, and reliable data is.

---

## Why is data quality important?

Poor-quality data leads to incorrect analysis and unreliable decisions.

---

## What are common data quality dimensions?

Examples:

- Accuracy
- Completeness
- Consistency
- Validity
- Uniqueness
- Timeliness

---

## What is data observability?

Data observability is the ability to monitor and understand the health of data systems.

---

## What are dbt tests?

dbt tests validate analytics models by checking rules such as uniqueness, relationships, and missing values.

---

# Key Takeaway

Data quality engineering ensures that data can be trusted.

It provides:

```
Reliable Data

+

Accurate Analytics

+

Better Decisions

+

Stable Data Systems
```

High-quality data is the foundation of every successful analytics and machine learning platform.