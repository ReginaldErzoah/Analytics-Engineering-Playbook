# dbt Testing

## Overview

Data quality is one of the most important responsibilities of an analytics engineer.

A dashboard is only as reliable as the data behind it.

dbt provides a testing framework that allows analytics engineers to automatically validate:

- Data completeness
- Data uniqueness
- Data relationships
- Business rules
- Transformation accuracy

In the SupportOps Intelligence Analytics project, dbt testing was used to ensure that the final analytical model was reliable before connecting it to Power BI.

---

# Why Data Testing Matters

Without automated testing:

```

Raw Data

```
 ↓
```

Transformation

```
 ↓
```

Dashboard

```
 ↓
```

Wrong Business Decisions

```

Problems that may go unnoticed:

- Duplicate records
- Missing IDs
- Broken relationships
- Invalid values
- Incorrect calculations

---

# Data Testing Philosophy

A professional analytics engineering workflow should follow:

```

Build

↓

Test

↓

Document

↓

Deploy

````

Testing should happen every time models change.

---

# Types of dbt Tests

dbt tests are divided into two major categories:

## 1. Generic Tests

Predefined reusable tests provided by dbt.

Examples:

- unique
- not_null
- relationships
- accepted_values

---

## 2. Singular Tests

Custom SQL tests written for specific business rules.

Example:

```sql
SELECT *

FROM fact_ticket

WHERE resolution_time_hours < 0
````

If this query returns rows:

The test fails.

---

# Generic dbt Tests

## 1. not_null Test

Purpose:

Ensures a column does not contain missing values.

Example:

```yaml
columns:

  - name: ticket_id

    tests:

      - not_null
```

Validation:

Before:

```
ticket_id

TKT-100001

NULL

TKT-100003
```

Result:

```
FAIL
```

---

## 2. unique Test

Purpose:

Ensures values are not duplicated.

Example:

```yaml
columns:

  - name: ticket_id

    tests:

      - unique
```

Validation:

Before:

```
ticket_id

TKT-100001

TKT-100001
```

Result:

```
FAIL
```

---

## 3. Relationships Test

Purpose:

Checks foreign key integrity.

Example:

```yaml
columns:

  - name: customer_key

    tests:

      - relationships:

          to: ref('dim_customer')

          field: customer_key
```

Checks:

```
fact_ticket.customer_key

exists in

dim_customer.customer_key
```

---

## 4. Accepted Values Test

Purpose:

Ensures columns contain only allowed values.

Example:

```yaml
columns:

  - name: priority_level

    tests:

      - accepted_values:

          values:

            - High

            - Medium

            - Low
```

---

# Schema Tests In SupportOps Intelligence

The project used:

```
dbt/models/marts/schema.yml
```

and:

```
dbt/models/staging/schema.yml
```

---

# Fact Table Tests

## fact_ticket.ticket_id

Tests:

```yaml
- not_null

- unique
```

Reason:

Every ticket requires a unique identifier.

---

## Fact Table Relationships

Tested:

```
fact_ticket

      |

      ↓

dim_customer
```

Example:

```yaml
relationships:

  to: ref('dim_customer')

  field: customer_key
```

---

Tested relationships:

```
fact_ticket.customer_key

        ↓

dim_customer.customer_key


fact_ticket.agent_key

        ↓

dim_agent.agent_key


fact_ticket.priority_key

        ↓

dim_priority.priority_key


fact_ticket.channel_key

        ↓

dim_channel.channel_key
```

---

# Final Test Results

Command:

```bash
python -m dbt.cli.main test
```

Result:

```
PASS=16

WARN=0

ERROR=0
```

All data quality checks passed.

---

# Testing Workflow

A typical dbt testing workflow:

```
Create Model

      ↓

Add Tests

      ↓

Run dbt run

      ↓

Run dbt test

      ↓

Review Results

      ↓

Deploy
```

---

# Running Tests

## Test Everything

Command:

```bash
dbt test
```

Runs all tests.

---

## Test Specific Model

Example:

```bash
dbt test --select fact_ticket
```

Only tests:

```
fact_ticket
```

---

## Test Before Building

Command:

```bash
dbt build
```

Runs:

* Models
* Tests
* Snapshots
* Seeds

in dependency order.

---

# Custom SQL Tests

Sometimes generic tests are not enough.

Example:

Business requirement:

> Resolution time should never be negative.

Create:

```
tests/no_negative_resolution.sql
```

SQL:

```sql
SELECT *

FROM {{ ref('fact_ticket') }}

WHERE resolution_time_hours < 0
```

Expected:

```
0 rows
```

---

# Testing Business Logic

Technical tests are not enough.

Analytics engineers must test business rules.

Examples:

## SLA Performance

Rule:

```
resolution_time_hours <= sla_target_hours
```

Expected:

```
Within SLA
```

---

## Customer Satisfaction

Rule:

```
satisfaction_score BETWEEN 1 AND 5
```

Expected:

```
Valid rating
```

---

## Ticket Categories

Rule:

```
Every ticket has a category
```

Expected:

```
No NULL categories
```

---

# Testing Layers

Different layers require different tests.

---

# Staging Tests

Focus:

* Source completeness
* Data types
* Required columns

Examples:

```
not_null

accepted_values
```

---

# Intermediate Tests

Focus:

* Business calculations
* Logic correctness

Examples:

```
SLA calculations

Resolution categories
```

---

# Mart Tests

Focus:

* Analytics reliability

Examples:

```
Primary keys

Foreign keys

Metric accuracy
```

---

# Data Quality Framework

A professional testing framework covers:

## Completeness

Question:

"Do we have all required data?"

Tests:

* not_null
* row counts

---

## Uniqueness

Question:

"Are records duplicated?"

Tests:

* unique

---

## Validity

Question:

"Are values correct?"

Tests:

* accepted_values
* custom tests

---

## Consistency

Question:

"Do tables agree with each other?"

Tests:

* relationships

---

# Common Testing Mistakes

## 1. Testing Too Little

Bad:

Only testing one table.

Better:

Test every critical model.

---

## 2. Testing Everything Without Purpose

Bad:

Adding unnecessary tests everywhere.

Better:

Test business-critical columns.

---

## 3. Ignoring Business Logic

Technical tests do not guarantee correct metrics.

Example:

A SLA formula can run successfully but still be wrong.

---

# Advanced dbt Testing Topics To Learn

## Unit Testing

Testing SQL logic before deployment.

Learn:

* dbt unit tests
* Test-driven development

---

## Data Contracts

Define expectations between:

* Producers
* Consumers
* Data teams

---

## CI/CD Testing

Automatically run:

```
Git Push

↓

Build

↓

Test

↓

Deploy
```

---

## Freshness Testing

Check whether data is updated on schedule.

Example:

```
Last update:

2 hours ago

Expected:

Daily
```

---

# Skills Required

## SQL

Master:

* Query validation
* Data profiling
* Business logic checks

---

## dbt

Master:

* Schema tests
* Custom tests
* dbt build
* CI workflows

---

## Data Quality

Master:

* Data validation frameworks
* Data observability
* Data contracts

---

## Software Engineering

Master:

* Automated testing
* Git workflows
* CI/CD pipelines

---

# Resources

## Official Documentation

dbt Testing:

[https://docs.getdbt.com/docs/build/data-tests](https://docs.getdbt.com/docs/build/data-tests)

---

## Courses

dbt Fundamentals:

[https://learn.getdbt.com/](https://learn.getdbt.com/)

---

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Topics:

* Data quality
* Reliability
* Production systems

---

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Topics:

* Dimensional modeling
* Data warehouse design

---

# Summary

dbt testing ensures that analytics systems produce trustworthy information.

The SupportOps Intelligence Analytics project implemented:

* Primary key validation
* Foreign key validation
* Null checks
* Uniqueness checks
* Relationship testing

The final result:

```
16 Tests Passed

0 Errors

0 Warnings
```

Automated testing transformed the project from a simple SQL pipeline into a professional analytics engineering workflow.
