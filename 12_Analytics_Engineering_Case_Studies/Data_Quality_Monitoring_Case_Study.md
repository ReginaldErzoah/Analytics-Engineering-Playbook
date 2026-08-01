# Data Quality Monitoring Case Study

## Overview

This case study demonstrates how analytics engineers design data quality monitoring systems to ensure that business data remains accurate, reliable, and trustworthy.

The goal is to build a data quality framework that detects:

- Missing data
- Incorrect values
- Schema changes
- Pipeline failures
- Data freshness issues

---

# Business Context

## Company

**FinTrust Bank**

FinTrust Bank operates a large financial analytics platform that supports:

- Customer analytics
- Risk reporting
- Financial dashboards
- Regulatory reporting

The organization processes millions of records daily from multiple systems.

---

# Business Problem

The company has experienced data reliability issues.

Examples:

- Dashboards showing incorrect numbers
- Missing transactions
- Delayed reports
- Unexpected schema changes
- Broken data pipelines

Business teams no longer fully trust analytical reports.

---

# Business Objectives

The analytics engineering team needs to:

## Improve Data Reliability

Ensure that:

- Data is accurate
- Pipelines run successfully
- Reports are trustworthy

---

## Detect Issues Early

Identify problems before they affect:

- Dashboards
- Business decisions
- Customers

---

## Establish Data Governance

Create:

- Quality standards
- Ownership rules
- Monitoring processes

---

# Data Sources

The bank receives data from multiple systems.

---

# Transaction System

Stores financial transactions.

Example:

|Column|Description|
|-|-|
|transaction_id|Transaction identifier|
|customer_id|Customer identifier|
|amount|Transaction amount|
|timestamp|Transaction time|

---

# Customer System

Stores customer information.

Example:

|Column|Description|
|-|-|
|customer_id|Customer identifier|
|name|Customer name|
|account_type|Account category|

---

# Reporting Database

Stores analytical tables.

Example:

|Column|Description|
|-|-|
|report_date|Reporting period|
|metric_name|Business metric|
|metric_value|Calculated value|

---

# Data Quality Problems

Several problems affect trust.

---

# 1. Missing Data

Problem:

Required fields contain NULL values.

Example:

```
customer_id = NULL
```

Impact:

Transactions cannot be linked.

---

# 2. Duplicate Records

Problem:

Same transaction appears multiple times.

Example:

```
Transaction ID 5001

Transaction ID 5001
```

Impact:

Revenue becomes overstated.

---

# 3. Invalid Values

Problem:

Data violates business rules.

Example:

```
Transaction Amount = -10000
```

---

# 4. Data Freshness Issues

Problem:

Data arrives late.

Example:

Expected:

```
Daily refresh at 6 AM
```

Actual:

```
Refresh at 2 PM
```

---

# 5. Schema Changes

Problem:

Source systems modify structures.

Example:

Before:

```
customer_name
```

After:

```
full_customer_name
```

Impact:

Pipelines may fail.

---

# Analytics Engineering Architecture

The monitoring system follows:

```
Source Systems

        ↓

Raw Data Layer

        ↓

Data Quality Checks

        ↓

Transformation Models

        ↓

Analytics Tables

        ↓

Monitoring Dashboard
```

---

# Data Quality Framework

The framework includes:

```
Expectation Definition

        ↓

Automated Testing

        ↓

Issue Detection

        ↓

Alerting

        ↓

Resolution
```

---

# Data Quality Dimensions

---

# Accuracy

Question:

"Is the data correct?"

Example:

```
Account Balance matches source system
```

---

# Completeness

Question:

"Is required information available?"

Example:

```
Every transaction has a customer ID
```

---

# Consistency

Question:

"Does data agree across systems?"

Example:

```
Warehouse Revenue

=

Finance Revenue
```

---

# Validity

Question:

"Does data follow expected rules?"

Example:

```
Age >= 0
```

---

# Timeliness

Question:

"Is data available when needed?"

Example:

```
Dashboard refreshed hourly
```

---

# Uniqueness

Question:

"Are duplicate records present?"

Example:

```
Transaction ID must be unique
```

---

# Data Quality Tests

---

# Null Tests

Purpose:

Detect missing values.

Example:

```sql
SELECT *

FROM transactions

WHERE customer_id IS NULL;
```

---

# Uniqueness Tests

Purpose:

Find duplicate records.

Example:

```sql
SELECT

transaction_id,

COUNT(*)

FROM transactions

GROUP BY transaction_id

HAVING COUNT(*) > 1;
```

---

# Relationship Tests

Purpose:

Validate table relationships.

Example:

```
Every transaction must have a customer.
```

---

# Range Tests

Purpose:

Validate acceptable values.

Example:

```
Transaction amount > 0
```

---

# Freshness Tests

Purpose:

Monitor update times.

Example:

```
Latest transaction < 24 hours old
```

---

# Schema Tests

Purpose:

Detect structural changes.

Example:

Expected:

```
customer_id

amount

timestamp
```

Actual:

```
customer

value

time
```

---

# Data Quality Tools

Common tools include:

---

# Great Expectations

Used for:

- Data validation
- Expectations
- Documentation

---

# dbt Tests

Used for analytics models.

Common tests:

- unique
- not_null
- relationships

---

# Soda

Used for:

- Data monitoring
- Alerts
- Quality checks

---

# Monte Carlo

Used for:

- Data observability
- Pipeline monitoring
- Incident detection

---

# Data Quality Dashboard

A monitoring dashboard should show:

---

# Pipeline Health

Metrics:

- Successful runs
- Failed runs
- Processing time

---

# Data Freshness

Metrics:

- Last update time
- Data delay

---

# Quality Scores

Metrics:

- Passing tests
- Failed tests
- Dataset reliability

---

# Data Incidents

Track:

- Issue type
- Severity
- Resolution status

---

# Example Data Incident

## Problem

Daily sales dashboard shows revenue 80% lower than expected.

---

## Investigation

Check:

```
Source Data

↓

Pipeline Logs

↓

Transformation Models

↓

Dashboard Table
```

---

## Root Cause

A source column changed:

Before:

```
sales_amount
```

After:

```
transaction_value
```

---

## Resolution

- Update transformation logic
- Add schema monitoring
- Document change

---

# dbt Implementation Example

Model:

```sql
SELECT

customer_id,

SUM(amount) AS revenue

FROM transactions

GROUP BY customer_id;
```

Tests:

```yaml
tests:

- unique

- not_null
```

---

# Data Quality Workflow

```
Create Rules

        ↓

Automate Tests

        ↓

Monitor Results

        ↓

Alert Teams

        ↓

Fix Issues

        ↓

Improve System
```

---

# Best Practices

## 1. Shift Left

Test data early in pipelines.

---

## 2. Automate Monitoring

Avoid manual checks.

---

## 3. Define Ownership

Every important dataset should have:

- Owner
- Documentation
- Quality rules

---

## 4. Track Data Incidents

Record:

- Cause
- Impact
- Resolution

---

## 5. Build Trust

Reliable data creates reliable decisions.

---

# Analytics Engineering Deliverables

Final outputs:

```
Data Quality Framework

+

Automated Tests

+

Monitoring Dashboard

+

Alert System

+

Documentation
```

---

# Tools Used

## Transformation

- SQL
- dbt

---

## Data Quality

- Great Expectations
- Soda
- Monte Carlo

---

## Warehouse

- Snowflake
- BigQuery
- Redshift

---

## Development

- Git
- GitHub

---

# Interview Discussion Points

## How would you improve data trust?

Answer:

"I would implement automated quality checks, monitor freshness and schema changes, create ownership rules, and establish processes for resolving data incidents."

---

## What tests would you implement?

Answer:

"I would implement uniqueness tests, null checks, relationship validation, range checks, freshness monitoring, and schema validation."

---

# Key Takeaway

Data quality monitoring ensures that analytics systems produce trusted information.

The process:

```
Data

↓

Validation

↓

Monitoring

↓

Detection

↓

Resolution

↓

Trustworthy Analytics
```

A strong analytics engineer does not only build data models — they ensure that those models remain reliable over time.