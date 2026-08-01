# dbt Tests

## Overview

Data quality is one of the most important responsibilities of an analytics engineer.

A dashboard is only as reliable as the data behind it.

dbt tests help ensure that transformed data meets expected quality standards before it reaches analysts and business users.

A simple idea:

```
Bad Data

    ↓

dbt Tests

    ↓

Reliable Analytics
```

---

# Why Data Testing Matters

Without testing, organizations may experience:

- Incorrect dashboards
- Wrong business decisions
- Broken pipelines
- Loss of trust in analytics

Example:

A customer support dashboard shows:

```
Average Resolution Time:
2 hours
```

But due to bad timestamps:

Actual:

```
8 hours
```

Testing helps detect these issues early.

---

# Types of dbt Tests

dbt tests are divided into:

1. Generic tests
2. Singular tests
3. Custom tests

---

# 1. Generic Tests

Generic tests are reusable tests provided by dbt.

The four built-in tests are:

- unique
- not_null
- accepted_values
- relationships

---

# Unique Test

Checks that values are not duplicated.

Example:

```yaml
columns:

- name: ticket_id

  tests:

  - unique
```

Valid:

```
ticket_id

1
2
3
4
```

Invalid:

```
ticket_id

1
2
2
3
```

---

# Not Null Test

Checks that required fields contain values.

Example:

```yaml
columns:

- name: customer_email

  tests:

  - not_null
```

Invalid:

```
customer_email

john@email.com

NULL

mary@email.com
```

---

# Accepted Values Test

Ensures columns contain only allowed values.

Example:

```yaml
columns:

- name: ticket_priority

  tests:

  - accepted_values:

      values:

      - High

      - Medium

      - Low
```

Valid:

```
High

Medium

Low
```

Invalid:

```
Urgent
```

---

# Relationships Test

Checks relationships between tables.

Example:

Fact table:

```
fact_orders.customer_id
```

should exist in:

```
dim_customers.customer_id
```

Example:

```yaml
columns:

- name: customer_id

  tests:

  - relationships:

      to: ref('dim_customers')

      field: customer_id
```

---

# 2. Singular Tests

Singular tests are custom SQL queries that return failing records.

Example:

File:

```
tests/assert_positive_resolution_hours.sql
```

SQL:

```sql
select *

from fact_ticket_metrics

where resolution_hours < 0
```

Expected:

```
0 rows returned
```

If rows appear:

```
Test Failed
```

---

# Why Use Singular Tests?

Useful for business rules.

Examples:

## Revenue Cannot Be Negative

```sql
where revenue < 0
```

---

## Resolution Time Cannot Be Negative

```sql
where resolution_hours < 0
```

---

## Closed Tickets Must Have Resolution Dates

```sql
where ticket_status='Closed'

and resolution_time is null
```

---

# 3. Custom Tests

Organizations often create reusable custom tests.

Examples:

```
macros/

tests/
```

Used for:

- Complex business rules
- Company-specific standards

---

# Schema Tests

Schema tests are usually defined in:

```
schema.yml
```

Example:

```yaml
models:

- name: dim_customers

  columns:

  - name: customer_id

    tests:

    - unique

    - not_null
```

---

# Running Tests

Run all tests:

```bash
dbt test
```

Example output:

```
PASS unique_dim_customers_customer_id

PASS not_null_dim_customers_customer_email
```

---

# Running Tests for Specific Models

Example:

```bash
dbt test --select dim_customers
```

---

# Running Specific Tests

Example:

```bash
dbt test --select test_name
```

---

# Data Quality Testing Framework

A strong analytics engineering workflow:

```
Source Data

      ↓

Freshness Tests

      ↓

Staging Models

      ↓

Schema Tests

      ↓

Intermediate Models

      ↓

Business Rule Tests

      ↓

Marts

      ↓

BI Dashboard
```

---

# Testing Different Data Layers

## Source Layer

Check:

- Data availability
- Freshness
- Required columns

Example:

```
ticket_id exists
```

---

## Staging Layer

Check:

- Data types
- Standardization
- Missing values

Example:

```
customer_email is not null
```

---

## Intermediate Layer

Check:

- Business calculations

Example:

```
resolution_hours >= 0
```

---

## Mart Layer

Check:

- Dashboard reliability

Example:

```
fact_sales has valid customer IDs
```

---

# Example: Customer Support Analytics Tests

Model:

```
fact_ticket_metrics
```

Tests:

```yaml
columns:

- name: ticket_id

  tests:

  - unique

  - not_null


- name: ticket_priority

  tests:

  - accepted_values:

      values:

      - Low

      - Medium

      - High
```

Custom test:

```
assert_positive_resolution_hours.sql
```

Checks:

```
resolution_hours >= 0
```

---

# Advanced dbt Testing Concepts

## Test Severity

Tests can be warnings or errors.

Example:

```yaml
severity: warn
```

Warning:

```
Pipeline continues
```

Error:

```
Pipeline fails
```

---

# Store Test Failures

dbt can store failed records.

Example:

```yaml
store_failures: true
```

Useful for:

- Debugging
- Monitoring trends

---

# Data Quality Metrics

Organizations track:

## Completeness

Are required fields populated?

Example:

```
customer_email missing rate
```

---

## Accuracy

Is the data correct?

Example:

```
negative revenue
```

---

## Consistency

Does data agree across systems?

Example:

```
customer count mismatch
```

---

## Timeliness

Is data available when expected?

Example:

```
daily pipeline delayed
```

---

# Testing Best Practices

## 1. Test Critical Columns

Always test:

- Primary keys
- Foreign keys
- Important metrics

---

## 2. Test Business Logic

Technical correctness is not enough.

Example:

A ticket resolution time should not be negative.

---

## 3. Run Tests Automatically

Use:

- CI/CD pipelines
- Scheduled jobs

---

## 4. Fix Root Causes

Do not simply remove failing tests.

Investigate:

```
Why did data become invalid?
```

---

# Common Interview Questions

## Why are dbt tests important?

They ensure data reliability and prevent incorrect analytics.

---

## What are the four built-in dbt tests?

```
unique

not_null

accepted_values

relationships
```

---

## What is a singular test?

A custom SQL query that identifies invalid records.

---

## Where are dbt tests usually defined?

Usually in:

```
schema.yml

or

tests/
```

---

## Should every column have a test?

No.

Focus on important fields:

- Keys
- Metrics
- Business-critical attributes

---

# Key Takeaway

Testing transforms analytics from a collection of SQL queries into a reliable data product.

A professional analytics engineering workflow includes:

✅ Automated testing  
✅ Business rule validation  
✅ Data quality monitoring  
✅ Continuous improvement  

Trusted data creates trusted decisions.