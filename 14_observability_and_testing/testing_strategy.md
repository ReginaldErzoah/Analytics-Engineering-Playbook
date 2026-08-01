# Testing Strategy

## Overview

Testing is the process of verifying that a data system produces accurate, reliable, and expected results.

In analytics engineering, testing ensures that:

- Data is correct
- Transformations work properly
- Pipelines remain reliable
- Business metrics are trustworthy

A professional analytics project should treat data testing as seriously as software testing.

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

Incorrect Decisions

```

---

With testing:

```

Raw Data

↓

Validation

↓

Transformation

↓

Quality Checks

↓

Trusted Analytics

````

---

# Types Of Data Testing

A complete testing strategy includes:

1. Unit Testing
2. Data Quality Testing
3. Integration Testing
4. Regression Testing
5. Pipeline Testing
6. Performance Testing

---

# 1. Unit Testing

Unit testing verifies that a small component works correctly.

In analytics engineering:

A unit can be:

- SQL model
- Python function
- Transformation logic

---

Example Python function:

```python
def calculate_revenue(price, quantity):

    return price * quantity
````

Test:

```python
assert calculate_revenue(100, 2) == 200
```

---

# SQL Unit Testing

Example:

Model:

```sql
SELECT

customer_id,

SUM(amount) AS revenue

FROM orders

GROUP BY customer_id
```

Expected:

```
Customer 1 = 500
```

Test verifies:

```
Actual result = Expected result
```

---

# 2. Data Quality Testing

Data quality tests verify that data meets expectations.

Common dimensions:

* Completeness
* Accuracy
* Consistency
* Validity
* Uniqueness

---

# Completeness Testing

Checks missing values.

Example:

Customer IDs should exist.

SQL:

```sql
SELECT *

FROM customers

WHERE customer_id IS NULL;
```

Expected:

```
0 rows
```

---

# Uniqueness Testing

Checks duplicates.

Example:

Customer IDs should be unique.

```sql
SELECT

customer_id,

COUNT(*)

FROM customers

GROUP BY customer_id

HAVING COUNT(*) > 1;
```

---

# Validity Testing

Checks allowed values.

Example:

Ticket status:

Allowed:

```
Open

Closed

Pending
```

Invalid:

```
Unknown
```

---

SQL:

```sql
SELECT *

FROM tickets

WHERE status NOT IN

(
'Open',
'Closed',
'Pending'
);
```

---

# Accuracy Testing

Checks business correctness.

Example:

Revenue should not be negative.

```sql
SELECT *

FROM sales

WHERE revenue < 0;
```

---

# Consistency Testing

Checks relationships between tables.

Example:

Every order should have a valid customer.

```sql
SELECT *

FROM orders o

LEFT JOIN customers c

ON o.customer_id = c.customer_id

WHERE c.customer_id IS NULL;
```

---

# 3. Integration Testing

Integration testing verifies that multiple components work together.

Example:

```
Python Pipeline

+

DuckDB

+

dbt

+

Power BI
```

---

Test:

```
Extract Data

↓

Load Data

↓

Transform Data

↓

Generate Output
```

Expected:

Complete workflow succeeds.

---

# 4. Regression Testing

Regression testing ensures that new changes do not break existing functionality.

Example:

Before:

```
Revenue KPI = $100,000
```

Developer changes SQL.

After:

```
Revenue KPI = $20,000
```

Regression testing detects unexpected changes.

---

# Regression Testing Example

Store expected results:

```
fact_sales

Rows:

100000
```

After changes:

```
Rows:

95000
```

Investigate difference.

---

# 5. Pipeline Testing

Pipeline testing validates complete data workflows.

Example:

```
Source Files

↓

Extraction

↓

Transformation

↓

Storage

↓

Analytics Tables
```

Checks:

* Pipeline completes
* Data arrives
* Tables are created

---

# 6. Performance Testing

Performance testing measures efficiency.

Check:

* Query runtime
* Memory usage
* Processing speed

---

Example:

Before optimization:

```
Query time:

10 minutes
```

After:

```
Query time:

30 seconds
```

---

# Testing Pyramid For Analytics Engineering

A useful testing hierarchy:

```
          End-to-End Tests

              ↑

      Integration Tests

              ↑

       Data Quality Tests

              ↑

        Unit Tests
```

---

# dbt Testing Strategy

dbt provides built-in testing.

Common tests:

* unique
* not_null
* relationships
* accepted_values

---

# dbt Schema Tests

Example:

```yaml
models:

- name: customers

  columns:

  - name: customer_id

    tests:

    - unique

    - not_null
```

---

Meaning:

Customer IDs must:

* Exist
* Not repeat

---

# Relationships Testing

Ensures foreign keys exist.

Example:

Orders:

```
customer_id
```

must exist in:

```
customers.customer_id
```

---

Example:

```yaml
tests:

- relationships:

    to: ref('customers')

    field: customer_id
```

---

# Accepted Values Testing

Example:

Status column.

```yaml
tests:

- accepted_values:

    values:

    - Open

    - Closed

    - Pending
```

---

# Custom Data Tests

Sometimes built-in tests are not enough.

Example:

Business rule:

```
Resolution time cannot exceed 365 days
```

SQL test:

```sql
SELECT *

FROM tickets

WHERE resolution_days > 365;
```

Expected:

```
0 rows
```

---

# Python Testing With Pytest

Pytest is a popular testing framework.

Install:

```bash
pip install pytest
```

Run:

```bash
pytest
```

---

Example:

File:

```
test_metrics.py
```

Code:

```python
def test_revenue():

    assert revenue == 1000
```

---

# Testing Analytics Projects

Example:

SupportOps Intelligence Analytics.

---

## Source Testing

Check:

```
CSV files exist

Columns exist

Data types correct
```

---

## Transformation Testing

Check:

```
Response time calculation

SLA logic

Revenue calculations
```

---

## Model Testing

Check:

```
fact_ticket_performance

Unique IDs

No missing values
```

---

## Dashboard Testing

Check:

```
Numbers match database

Filters work

KPIs calculate correctly
```

---

# Testing Workflow In GitHub

A professional workflow:

```
Developer Changes Code

↓

Commit

↓

GitHub Actions Runs Tests

↓

Tests Pass

↓

Merge
```

---

# CI Testing Example

GitHub Actions:

```yaml
steps:

- install dependencies

- run pytest

- run dbt test

- validate project
```

---

# Data Contracts

A data contract defines expectations between producers and consumers.

Example:

Source team promises:

```
customer_id

Always exists

Integer type

Never removed
```

---

Benefits:

* Prevent breaking changes
* Improve reliability

---

# Testing Best Practices

## 1. Test Early

Do not wait until production.

---

## 2. Automate Tests

Manual testing does not scale.

Use:

* dbt tests
* pytest
* CI/CD

---

## 3. Test Business Logic

Technical correctness is not enough.

Example:

A query may run successfully but calculate the wrong KPI.

---

## 4. Keep Tests Simple

Tests should be:

* Easy to understand
* Easy to maintain

---

# Common Testing Mistakes

## No Tests

Problem:

Errors reach users.

---

## Testing Only Code

Problem:

Data problems are missed.

---

## Too Many Low-Value Tests

Problem:

Maintenance becomes difficult.

---

## No Business Validation

Problem:

Metrics may be technically correct but business incorrect.

---

# Testing Tools

## SQL Testing

* dbt tests
* Great Expectations
* Soda

---

## Python Testing

* pytest
* unittest

---

## Pipeline Testing

* Airflow testing
* Dagster testing

---

## Monitoring

* DataDog
* Grafana
* Monte Carlo

---

# Skills To Master

## SQL Testing

Learn:

* Validation queries
* Data checks
* Constraints

---

## dbt Testing

Learn:

* Schema tests
* Custom tests
* Documentation

---

## Python Testing

Learn:

* pytest
* Mocking
* Test design

---

## Software Engineering Practices

Learn:

* Test-driven development
* Continuous integration

---

# Resources

## Books

### Effective Data Testing

Author:

Elliot K. P.

Focus:

* Data quality practices

### Fundamentals of Software Testing

Focus:

* Testing principles

### The Pragmatic Programmer

Authors:

David Thomas and Andrew Hunt

Focus:

* Professional engineering practices

---

## Documentation

dbt Testing:

[https://docs.getdbt.com/docs/build/data-tests](https://docs.getdbt.com/docs/build/data-tests)

pytest:

[https://docs.pytest.org/](https://docs.pytest.org/)

Great Expectations:

[https://docs.greatexpectations.io/](https://docs.greatexpectations.io/)

---

# Summary

Testing creates trust in analytics systems.

A mature testing strategy:

```
Validate Inputs

↓

Test Transformations

↓

Check Data Quality

↓

Verify Pipelines

↓

Monitor Production

↓

Improve Continuously


For analytics engineers, testing is not optional. It is the foundation of reliable, production-quality data products.

