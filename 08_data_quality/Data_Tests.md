# Data Quality Tests

## Overview

Data quality tests are automated checks used to verify that data meets expected standards before it is used for analytics and decision-making.

In analytics engineering, tests help ensure:

- Reliable dashboards
- Accurate KPIs
- Consistent reporting
- Early detection of data issues

A strong data pipeline does not only transform data.

It validates that the output can be trusted.

---

# Why Data Testing Matters

Without testing:

```
Raw Data

↓

Transformation

↓

Dashboard

↓

Business Decision
```

A data issue can silently flow through the entire system.

---

With testing:

```
Raw Data

↓

Transformation

↓

Quality Checks

↓

Trusted Dataset

↓

Dashboard
```

Problems are detected before reaching users.

---

# Types of Data Quality Tests

Common categories:

```
Schema Tests

Completeness Tests

Uniqueness Tests

Validity Tests

Relationship Tests

Freshness Tests

Business Rule Tests
```

---

# 1. Schema Tests

## Purpose

Ensure the structure of data matches expectations.

Checks:

- Required columns exist
- Correct data types
- Expected tables exist

---

## Example

Expected:

```
customers

customer_id

email

created_date
```

Problem:

```
email column removed
```

The test should fail.

---

# 2. Not Null Tests

## Purpose

Ensure important fields contain values.

Example:

Every customer must have an ID.

---

dbt example:

```yaml
columns:

  - name: customer_id

    tests:

      - not_null
```

---

Failure example:

|customer_id|name|
|-|-|
|null|John|

---

# 3. Unique Tests

## Purpose

Ensure values that should be unique are not duplicated.

Common examples:

- Customer IDs
- Order IDs
- Ticket IDs

---

dbt example:

```yaml
columns:

  - name: customer_id

    tests:

      - unique
```

---

Failure example:

```
customer_id

101

101
```

---

# 4. Relationship Tests

## Purpose

Ensure relationships between tables are valid.

Example:

Every order should belong to an existing customer.

---

Tables:

```
customers

customer_id
```

and:

```
orders

customer_id
```

---

dbt example:

```yaml
columns:

  - name: customer_id

    tests:

      - relationships:

          to: ref('customers')

          field: customer_id
```

---

# 5. Accepted Values Tests

## Purpose

Ensure categorical fields contain approved values.

Example:

Ticket status:

Allowed:

```
open

pending

closed
```

Invalid:

```
completed
```

---

dbt example:

```yaml
tests:

- accepted_values:

    values:

      - open

      - closed

      - pending
```

---

# 6. Freshness Tests

## Purpose

Check whether data arrives on schedule.

Example:

Customer support data should update daily.

Expected:

```
Last update:

Today 6 AM
```

Problem:

```
Last update:

7 days ago
```

---

Freshness checks monitor:

- Source delays
- Pipeline failures
- Missing updates

---

# 7. Volume Tests

## Purpose

Detect unexpected changes in record counts.

Example:

Normal:

```
100,000 tickets/day
```

Today:

```
2,000 tickets
```

Possible issue:

- Pipeline failure
- Missing source data

---

# 8. Business Rule Tests

## Purpose

Validate business-specific logic.

Examples:

---

## Customer Satisfaction

Rule:

```
Score must be between 1 and 5
```

SQL:

```sql
select *

from surveys

where satisfaction_score < 1

or satisfaction_score > 5
```

---

## Resolution Time

Rule:

```
Resolution time cannot be negative
```

SQL:

```sql
select *

from tickets

where resolution_hours < 0
```

---

# dbt Testing Framework

dbt provides built-in testing capabilities.

A dbt test:

- Runs SQL validation
- Returns failures
- Blocks deployment if configured

---

# Generic dbt Tests

Built-in tests:

## not_null

Checks missing values.

---

## unique

Checks duplicates.

---

## relationships

Checks foreign keys.

---

## accepted_values

Checks allowed categories.

---

# Singular dbt Tests

Custom SQL tests.

Example:

File:

```
tests/no_negative_revenue.sql
```

SQL:

```sql
select *

from sales

where revenue < 0
```

If rows are returned:

```
Test Failed
```

If no rows:

```
Test Passed
```

---

# Data Testing Workflow

```
Create Model

      ↓

Define Quality Rules

      ↓

Write Tests

      ↓

Run dbt test

      ↓

Review Failures

      ↓

Fix Issues
```

---

# Testing in CI/CD

Professional teams run tests automatically.

Workflow:

```
Developer Changes Code

        ↓

Git Pull Request

        ↓

CI Pipeline Runs

        ↓

dbt Build

        ↓

dbt Test

        ↓

Merge If Successful
```

---

# Example: Customer Support Analytics Tests

Dataset:

```
fact_ticket_metrics
```

Tests:

---

## Ticket ID

Requirement:

```
Every ticket must have an ID
```

Test:

```
not_null
```

---

## Ticket Uniqueness

Requirement:

```
One row per ticket
```

Test:

```
unique
```

---

## Status Values

Requirement:

```
Only valid statuses allowed
```

Test:

```
accepted_values
```

---

## Customer Relationship

Requirement:

```
Every ticket belongs to a customer
```

Test:

```
relationships
```

---

# Data Test Levels

## Development Testing

Performed by developers.

Purpose:

- Validate new models
- Catch errors early

---

## Production Testing

Performed automatically.

Purpose:

- Protect business reporting
- Ensure reliability

---

# Best Practices

## 1. Test Important Columns

Focus on:

- Primary keys
- Foreign keys
- Metrics

---

## 2. Test Business Logic

Technical correctness is not enough.

Validate:

```
Does this metric make business sense?
```

---

## 3. Automate Testing

Avoid manual checks.

---

## 4. Document Failures

Record:

- What failed
- Why it failed
- How it was fixed

---

## 5. Test Before Reporting

Never send unvalidated data to executives.

---

# Interview Questions

## What are data quality tests?

Automated checks that validate whether data meets defined requirements.

---

## What are common dbt tests?

- not_null
- unique
- relationships
- accepted_values

---

## What is the difference between testing and monitoring?

Testing validates data rules; monitoring observes ongoing system health.

---

## Why are data tests important?

They prevent incorrect data from reaching analytics users.

---

# Key Takeaway

Data tests create confidence in analytical systems.

A production-ready analytics workflow is:

```
Transform Data

↓

Test Data

↓

Document Data

↓

Publish Data

↓

Trust Insights
```

Reliable analytics starts with reliable validation.