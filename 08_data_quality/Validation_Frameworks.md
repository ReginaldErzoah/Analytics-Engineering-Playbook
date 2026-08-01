# Data Validation Frameworks

## Overview

Data validation frameworks are tools and processes used to automatically verify that data meets predefined quality requirements.

They help organizations detect:

- Incorrect values
- Missing data
- Duplicate records
- Schema changes
- Business rule violations

before data reaches analysts, dashboards, or decision-makers.

---

# Why Data Validation Frameworks Matter

Modern organizations process large amounts of data from many sources.

Manual validation becomes impossible.

Example:

```
Customer Support System

        ↓

Millions of Tickets

        ↓

Analytics Warehouse

        ↓

Executive Dashboard
```

A small data issue can affect:

- KPIs
- Reports
- Forecasts
- Business decisions

Validation frameworks automate these checks.

---

# Data Validation Process

A typical validation workflow:

```
Data Arrival

      ↓

Define Expectations

      ↓

Run Validation Checks

      ↓

Generate Results

      ↓

Alert Teams

      ↓

Fix Issues
```

---

# Types of Validation

## 1. Schema Validation

Checks whether the structure of data is correct.

Validates:

- Column names
- Data types
- Required fields

Example:

Expected:

```
customer_id

email

created_date
```

Problem:

```
customer_id removed
```

---

# 2. Data Type Validation

Ensures columns contain correct data types.

Example:

Expected:

```
age → integer
```

Invalid:

```
age → text
```

---

# 3. Completeness Validation

Checks missing values.

Example:

Required:

```
ticket_id
```

Invalid:

```
ticket_id = NULL
```

---

# 4. Uniqueness Validation

Checks duplicate records.

Example:

```
order_id

1001

1001
```

Problem:

Duplicate transaction.

---

# 5. Range Validation

Ensures values are within acceptable limits.

Example:

Customer satisfaction:

Valid:

```
1 - 5
```

Invalid:

```
10
```

---

# 6. Referential Validation

Checks relationships between datasets.

Example:

Every order must have a valid customer.

```
orders.customer_id

must exist in:

customers.customer_id
```

---

# 7. Business Rule Validation

Checks organization-specific logic.

Example:

Support analytics:

Rule:

```
Resolution time cannot be negative
```

Validation:

```sql
resolution_hours >= 0
```

---

# Popular Data Validation Frameworks

## 1. dbt Tests

### Overview

dbt provides built-in validation capabilities directly inside transformation workflows.

Common in modern analytics engineering.

---

## Features

Supports:

- Schema testing
- Data tests
- Documentation
- CI/CD integration

---

## Example

```yaml
columns:

  - name: customer_id

    tests:

      - unique

      - not_null
```

---

## Best Used For

- Analytics warehouses
- SQL transformations
- ELT workflows

---

# 2. Great Expectations

## Overview

Great Expectations (GX) is an open-source data quality framework.

It allows teams to define expectations about data.

Example:

Expectation:

```
Customer age must be between 18 and 100
```

---

## Features

- Data profiling
- Automated validation
- Documentation reports
- Validation checkpoints

---

## Example Expectation

```python
expect_column_values_to_not_be_null(
    "customer_id"
)
```

---

## Best Used For

- Data pipelines
- Data engineering workflows
- Complex validation requirements

---

# 3. Soda

## Overview

Soda is a data quality monitoring platform.

It uses SQL-based checks called SodaCL.

---

## Features

- Automated monitoring
- Anomaly detection
- Alerts
- Data observability

---

Example:

```yaml
checks:

  - missing_count(email) = 0
```

---

## Best Used For

- Production monitoring
- Data platforms
- Continuous quality checks

---

# 4. Monte Carlo

## Overview

Monte Carlo is a data observability platform.

It focuses on monitoring data reliability.

---

## Features

- Pipeline monitoring
- Incident detection
- Data lineage
- Anomaly detection

---

## Best Used For

Large organizations with complex data platforms.

---

# 5. Amazon Deequ

## Overview

Deequ is a data quality library built on Apache Spark.

Used for large-scale data validation.

---

## Features

- Constraint verification
- Data profiling
- Metrics computation

---

# Validation Framework Comparison

|Framework|Main Purpose|Best For|
|-|-|-|
|dbt Tests|Transformation testing|Analytics engineering|
|Great Expectations|Data validation rules|Data pipelines|
|Soda|Quality monitoring|Production systems|
|Monte Carlo|Data observability|Large data platforms|
|Deequ|Large-scale validation|Spark environments|

---

# Data Validation in Analytics Engineering

A modern workflow:

```
Source Data

      ↓

Ingestion

      ↓

Raw Tables

      ↓

dbt Transformations

      ↓

dbt Tests

      ↓

Certified Data Models

      ↓

BI Dashboard
```

---

# Validation Framework Example

Customer Support Analytics:

Dataset:

```
fact_ticket_metrics
```

Validation rules:

---

## Completeness

Rule:

```
ticket_id cannot be empty
```

Test:

```
not_null
```

---

## Uniqueness

Rule:

```
One row per ticket
```

Test:

```
unique(ticket_id)
```

---

## Validity

Rule:

```
Priority must be:

Low

Medium

High
```

Test:

```
accepted_values
```

---

## Integrity

Rule:

```
Every ticket belongs to a customer
```

Test:

```
relationships
```

---

# Validation vs Testing vs Monitoring

These concepts are related but different.

|Concept|Purpose|
|-|-|
|Validation|Check whether data meets expectations|
|Testing|Automated verification of rules|
|Monitoring|Continuous observation of data health|

---

# Data Validation Best Practices

## 1. Validate Early

Detect issues close to the source.

---

## 2. Automate Checks

Avoid manual spreadsheet reviews.

---

## 3. Prioritize Critical Data

Focus on:

- Revenue data
- Customer data
- KPI tables

---

## 4. Create Clear Expectations

Document:

- What is valid?
- What is invalid?
- What happens when checks fail?

---

## 5. Integrate With CI/CD

Run validation automatically before deployment.

---

# Interview Questions

## What is a data validation framework?

A tool or process that automatically checks whether data meets predefined quality requirements.

---

## Difference between dbt tests and Great Expectations?

dbt tests are mainly used inside SQL transformation workflows, while Great Expectations provides broader data validation capabilities across pipelines.

---

## What validations would you perform on a customer table?

I would check:

- Primary key uniqueness
- Required fields
- Valid email formats
- Data types
- Duplicate records
- Relationship integrity

---

## Why automate data validation?

Because manual checks do not scale and can allow errors into production systems.

---

# Key Takeaway

Data validation frameworks protect analytics systems by ensuring that only trusted data reaches users.

A mature data platform combines:

```
Validation Rules

+

Automated Tests

+

Monitoring

+

Documentation

=

Reliable Analytics
```