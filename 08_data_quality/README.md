# Data Quality

## Overview

Data quality is the process of ensuring that data is accurate, complete, consistent, reliable, and suitable for business decision-making.

In analytics engineering, data quality is critical because dashboards and business decisions depend on trusted data.

A data quality framework ensures:

- Data is correct
- Data is available when needed
- Data follows expected rules
- Data changes are monitored

---

# Why Data Quality Matters

Poor-quality data leads to:

- Incorrect business decisions
- Untrusted dashboards
- Revenue reporting errors
- Operational inefficiencies

Example:

A customer support dashboard shows:

```
Average Resolution Time:

2 hours
```

But the actual value is:

```
12 hours
```

because timestamps were incorrectly calculated.

This creates misleading insights.

---

# Data Quality Dimensions

Data quality is usually measured using several dimensions.

---

# 1. Accuracy

## Definition

Accuracy measures whether data correctly represents the real-world value.

Example:

Actual customer location:

```
Accra
```

Stored value:

```
Accra
```

High accuracy.

---

Incorrect:

```
Accra

stored as:

Kumasi
```

Low accuracy.

---

# 2. Completeness

## Definition

Completeness measures whether required data exists.

Example:

Customer table:

|customer_id|email|
|-|-|
|101|john@email.com|
|102|null|

The second record is incomplete.

---

Common checks:

```sql
WHERE email IS NULL
```

---

# 3. Consistency

## Definition

Consistency ensures the same data has the same meaning across systems.

Example:

System A:

```
Ghana
```

System B:

```
GH
```

The values represent the same thing but are inconsistent.

---

# 4. Validity

## Definition

Validity checks whether data follows expected rules.

Example:

Age:

Valid:

```
25
```

Invalid:

```
-5
```

---

Example validation:

```sql
WHERE age < 0
```

---

# 5. Uniqueness

## Definition

Ensures records are not duplicated.

Example:

Customer IDs:

```
101

101

102
```

Problem:

Duplicate customer.

---

SQL check:

```sql
COUNT(customer_id)

vs

COUNT(DISTINCT customer_id)
```

---

# 6. Timeliness

## Definition

Measures whether data is available when expected.

Example:

Dashboard refresh:

Expected:

```
Every morning 6 AM
```

Actual:

```
Every afternoon 3 PM
```

The data is not timely.

---

# Data Quality Framework

A typical process:

```
Define Quality Rules

        ↓

Collect Data

        ↓

Run Validation Checks

        ↓

Identify Issues

        ↓

Fix Problems

        ↓

Monitor Continuously
```

---

# Data Quality Checks

Common checks include:

## Null Checks

Ensure required fields exist.

Example:

```sql
customer_id IS NOT NULL
```

---

## Uniqueness Checks

Ensure keys are unique.

Example:

```sql
customer_id UNIQUE
```

---

## Relationship Checks

Ensure relationships exist.

Example:

Every order must have a valid customer.

```
orders.customer_id

must exist in:

customers.customer_id
```

---

## Accepted Values

Ensure values come from approved lists.

Example:

Allowed statuses:

```
open

closed

pending
```

Invalid:

```
unknown
```

---

## Range Checks

Ensure values fall within expected ranges.

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

# Data Quality in Analytics Engineering

Analytics engineers implement quality through:

- dbt tests
- SQL validation
- Monitoring systems
- Documentation

---

# dbt Data Quality Workflow

Example:

```
Raw Data

    ↓

dbt Models

    ↓

dbt Tests

    ↓

Certified Analytics Tables

    ↓

BI Dashboard
```

---

# Common dbt Tests

## not_null

Checks required fields.

Example:

```yaml
- name: ticket_id

  tests:

    - not_null
```

---

## unique

Checks duplicate records.

Example:

```yaml
tests:

- unique
```

---

## relationships

Checks foreign keys.

Example:

```yaml
tests:

- relationships:

    to: ref('customers')

    field: customer_id
```

---

## accepted_values

Checks allowed categories.

Example:

```yaml
tests:

- accepted_values:

    values:

    - open

    - closed
```

---

# Data Quality Monitoring

Production systems monitor:

## Freshness

Question:

```
Is the data arriving on time?
```

---

## Volume

Question:

```
Did today's records drop unexpectedly?
```

Example:

Normal:

```
100,000 transactions/day
```

Today:

```
500 transactions
```

Possible issue.

---

## Schema Changes

Question:

```
Did the structure change?
```

Example:

Column removed:

```
customer_email
```

---

## Distribution Changes

Question:

```
Did values change unexpectedly?
```

Example:

Customer satisfaction:

Normal:

```
Average score: 4.5
```

Today:

```
Average score: 1.2
```

---

# Data Quality Tools

|Tool|Purpose|
|-|-|
|dbt Tests|Transformation testing|
|Great Expectations|Data validation framework|
|Soda|Data quality monitoring|
|Monte Carlo|Data observability|
|Deequ|Data quality checks|

---

# Data Quality Lifecycle

```
Prevent

↓

Detect

↓

Investigate

↓

Resolve

↓

Improve
```

---

# Example: Customer Support Analytics

Potential quality issues:

## Missing Ticket IDs

Problem:

```
ticket_id = NULL
```

Impact:

Cannot track support requests.

---

## Invalid Resolution Time

Problem:

```
resolution_time = -5 hours
```

Impact:

Incorrect KPI reporting.

---

## Duplicate Tickets

Problem:

Same ticket appears twice.

Impact:

Inflated support volume.

---

# Data Quality Best Practices

## 1. Test Early

Catch problems before dashboards.

---

## 2. Automate Checks

Avoid manual validation.

---

## 3. Monitor Continuously

Quality is not a one-time activity.

---

## 4. Document Rules

Everyone should understand:

- What is valid?
- What is invalid?
- Why?

---

## 5. Assign Ownership

Every important dataset should have an owner.

---

# Interview Questions

## What is data quality?

Data quality ensures data is accurate, complete, consistent, valid, unique, and timely.

---

## Why is data quality important?

Because unreliable data leads to incorrect business decisions.

---

## How do analytics engineers maintain data quality?

Through testing, validation, documentation, and monitoring.

---

## What are common dbt tests?

- not_null
- unique
- relationships
- accepted_values

---

# Key Takeaway

Data quality is the foundation of trusted analytics.

A strong analytics engineer does not only build transformations.

They build systems that ensure:

```
Reliable Data

↓

Trusted Metrics

↓

Better Decisions
```