# Validation and Testing

## Overview

Validation and testing are essential practices in analytics engineering because they ensure that data transformations produce accurate, reliable, and trustworthy outputs.

A modern analytics system is not complete when data pipelines run successfully.

A pipeline can execute without errors while still producing incorrect business results.

Examples:

- Incorrect KPI calculations
- Missing customer records
- Duplicate tickets
- Broken relationships between tables
- Invalid categories

Testing protects analytical systems from these failures.

In the SupportOps Intelligence Analytics project, validation and testing were implemented through:

- Data profiling
- Cleaning validation
- dbt schema tests
- Relationship checks
- Model validation

---

# Why Data Testing Matters

Without testing, analysts may unknowingly build reports using incorrect data.

Example:

A Power BI dashboard displays:

```

Average Resolution Time:

12 hours

```

However:

- Some tickets have missing resolution timestamps
- Duplicate tickets exist
- Invalid records are included

The actual metric may be incorrect.

Testing ensures:

```

Reliable Data

↓

Reliable Models

↓

Reliable Reports

↓

Reliable Decisions

```

---

# Types of Data Validation

Data validation ensures data meets expected rules.

Common validation categories:

1. Schema validation
2. Data type validation
3. Completeness validation
4. Uniqueness validation
5. Relationship validation
6. Business rule validation

---

# 1. Schema Validation

## Definition

Schema validation checks whether tables contain the expected structure.

It validates:

- Column names
- Required columns
- Data types

---

Example:

Expected:

```

fact_ticket

ticket_id

customer_key

agent_key

resolution_time_hours

```

Problem:

```

resolution_time

```

instead of:

```

resolution_time_hours

```

The model may fail or produce incorrect results.

---

# 2. Data Type Validation

## Definition

Ensures columns contain the correct data types.

Examples:

Correct:

```

ticket_id → String

resolution_time_hours → Numeric

created_date → Date

```

Incorrect:

```

resolution_time_hours → Text

```

---

# 3. Completeness Validation

Checks whether required fields contain values.

Example:

A support ticket requires:

```

ticket_id

customer_id

created_date

```

A record missing:

```

ticket_id

```

should fail validation.

---

# 4. Uniqueness Validation

Checks that identifiers are not duplicated.

Example:

Ticket IDs should be unique.

Correct:

```

TKT001

TKT002

TKT003

```

Incorrect:

```

TKT001

TKT001

TKT003

```

---

# 5. Relationship Validation

Ensures relationships between tables are valid.

Example:

A ticket references:

```

customer_key = 1005

```

The customer dimension must contain:

```

customer_key = 1005

```

Otherwise:

The ticket becomes an orphan record.

---

# 6. Business Rule Validation

Checks whether data follows business logic.

Examples:

A resolved ticket must have:

```

resolution_date

```

A critical ticket should have:

```

priority = Critical

```

A satisfaction score should be:

```

1 - 5

```

---

# Testing Layers in Analytics Engineering

Professional analytics systems test data at multiple stages.

```

Raw Data

↓

Staging Models

↓

Intermediate Models

↓

Marts

↓

BI Reports

```

---

# Raw Data Testing

Purpose:

Understand source quality.

Checks:

- Missing values
- Duplicate rows
- Invalid formats

Tools:

- Python
- Pandas
- Data profiling notebooks

---

# Staging Layer Testing

The staging layer prepares source data.

Testing focuses on:

- Renaming columns
- Data types
- Basic cleaning

Example:

Model:

```

stg_ticket

```

Checks:

```

ticket_id exists

dates are valid

columns renamed correctly

```

---

# Intermediate Layer Testing

Intermediate models contain business logic.

Testing focuses on:

- Calculations
- Transformations
- Joins

Example:

Model:

```

int_ticket_metrics

```

Checks:

```

resolution_time_hours

sla_status

ticket_metrics

```

are calculated correctly.

---

# Mart Layer Testing

Mart models serve reporting.

Testing focuses on:

- Business correctness
- Relationships
- KPI accuracy

Examples:

```

fact_ticket

dim_customer

dim_agent

```

---

# dbt Testing

dbt provides automated testing functionality.

Tests are written in:

```

schema.yml

````

---

# Built-in dbt Tests

dbt includes:

## Not Null

Checks missing values.

Example:

```yaml
columns:

  - name: ticket_id

    tests:

      - not_null
````

---

## Unique

Checks duplicate values.

Example:

```yaml
columns:

  - name: ticket_id

    tests:

      - unique
```

---

## Relationships

Checks foreign keys.

Example:

```yaml
columns:

  - name: customer_key

    tests:

      - relationships:

          to: ref('dim_customer')

          field: customer_key
```

---

## Accepted Values

Checks allowed categories.

Example:

```yaml
columns:

  - name: priority_level

    tests:

      - accepted_values:

          values:

          - Low

          - Medium

          - High

          - Critical
```

---

# SupportOps Testing Implementation

The project used dbt tests across analytical models.

Test coverage included:

## fact_ticket

Validated:

* Ticket ID uniqueness
* Ticket ID completeness
* Foreign key relationships

---

## dim_customer

Validated:

* Customer key uniqueness
* Customer email completeness

---

## dim_agent

Validated:

* Agent key uniqueness
* Agent assignment completeness

---

## dim_channel

Validated:

* Channel key uniqueness

---

## dim_priority

Validated:

* Priority key uniqueness

---

# Running dbt Tests

Command:

```bash
dbt test
```

Example output:

```
PASS

not_null_fact_ticket_ticket_id

PASS

unique_fact_ticket_ticket_id

PASS

relationships_fact_ticket_customer_key
```

---

# Custom Data Tests

Sometimes built-in tests are not enough.

Example:

Business rule:

```
Resolution time cannot be negative
```

Custom test:

```sql
SELECT *

FROM fact_ticket

WHERE resolution_time_hours < 0
```

If rows are returned:

The test fails.

---

# Unit Testing vs Data Testing

## Software Unit Testing

Tests code behavior.

Example:

Python function:

```python
calculate_average()
```

---

## Data Testing

Tests data correctness.

Example:

```
Are all customer keys valid?
```

---

# Data Testing Best Practices

## 1. Test Critical Fields

Always test:

* Primary keys
* Foreign keys
* Metrics
* Dates

---

## 2. Test Business Logic

Technical correctness is not enough.

A query can run successfully and still calculate the wrong KPI.

---

## 3. Automate Testing

Run tests:

* Before deployment
* During pipeline execution
* In CI/CD workflows

---

## 4. Document Expectations

Example:

```
fact_ticket

One row represents one customer support ticket.

ticket_id must be unique.

customer_key must exist in dim_customer.
```

---

# Testing Workflow

Professional workflow:

```
Write Transformation

        ↓

Create Tests

        ↓

Run dbt Test

        ↓

Fix Failures

        ↓

Deploy Model

        ↓

Monitor Production
```

---

# Testing Tools

| Tool               | Purpose                   |
| ------------------ | ------------------------- |
| dbt Tests          | Transformation validation |
| Great Expectations | Data validation framework |
| Pandas             | Data checking scripts     |
| pytest             | Python testing            |
| Soda               | Data monitoring           |
| Monte Carlo        | Data observability        |

---

# Skills Required

## SQL

Learn:

* Data validation queries
* Assertions
* Business rule checks

---

## Python

Learn:

* pytest
* Pandas validation
* Automated checks

---

## dbt

Learn:

* Schema tests
* Custom tests
* CI testing

---

## Data Engineering

Learn:

* Pipeline testing
* Monitoring
* Reliability engineering

---

# Resources

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Recommended for:

* Data reliability
* Pipeline engineering
* Production systems

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Recommended for:

* Analytical modeling
* Data correctness

---

## Documentation

dbt Testing:

[https://docs.getdbt.com/docs/build/data-tests](https://docs.getdbt.com/docs/build/data-tests)

Great Expectations:

[https://docs.greatexpectations.io/](https://docs.greatexpectations.io/)

pytest:

[https://docs.pytest.org/](https://docs.pytest.org/)

---

# Summary

Testing is what transforms analytics engineering from simple SQL development into professional data engineering.

A reliable analytics system requires:

```
Validated Sources

+

Tested Transformations

+

Trusted Data Models

+

Accurate Business Intelligence
```

The SupportOps Intelligence Analytics project applied testing through:

* Python data validation
* dbt schema tests
* Relationship checks
* Model quality assurance

These practices ensure that analytical outputs remain trustworthy as systems grow.