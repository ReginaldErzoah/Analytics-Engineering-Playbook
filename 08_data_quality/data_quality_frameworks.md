# Data Quality Frameworks

## Overview

Data quality is the process of ensuring that data is:

- Accurate
- Complete
- Consistent
- Reliable
- Timely
- Valid
- Usable for decision-making

In analytics engineering, data quality is critical because business decisions depend on trusted data.

A dashboard can look visually impressive, but if the underlying data is incorrect, the insights are unreliable.

The SupportOps Intelligence Analytics project applied data quality principles through:

- Data profiling
- Data cleaning
- dbt tests
- Schema validation
- Data modeling practices

---

# Why Data Quality Matters

Modern organizations rely heavily on analytics.

Examples:

- Customer support reporting
- Revenue forecasting
- Operational dashboards
- Performance monitoring

Poor-quality data can lead to:

- Incorrect KPIs
- Wrong business decisions
- Loss of stakeholder trust
- Failed reporting processes

Example:

A support dashboard shows:

```

SLA Success Rate: 95%

```

However, if:

- Resolution timestamps are missing
- SLA calculations are incorrect
- Duplicate tickets exist

The KPI becomes misleading.

---

# Dimensions of Data Quality

Data quality is usually measured using several dimensions.

---

# 1. Accuracy

## Definition

Accuracy measures whether data correctly represents reality.

Example:

Customer location:

Correct:

```

Accra

```

Incorrect:

```

Accraaa

```

---

## Accuracy Checks

Examples:

- Validate values against source systems
- Compare calculated fields
- Check business rules

---

# 2. Completeness

## Definition

Completeness measures whether required data exists.

Example:

Ticket record:

```

Ticket ID

Customer ID

Agent ID

Resolution Time

```

If resolution time is missing:

The record is incomplete.

---

## Completeness Checks

Examples:

- Null checks
- Required field validation
- Missing record detection

---

# 3. Consistency

## Definition

Consistency ensures that data follows the same rules across systems.

Example:

Customer country:

System A:

```

Ghana

```

System B:

```

GH

```

These represent the same value but are inconsistent.

---

## Consistency Checks

Examples:

- Standardized naming conventions
- Matching reference values
- Data type validation

---

# 4. Validity

## Definition

Validity checks whether data follows expected formats and rules.

Example:

Email:

Valid:

```

[customer@gmail.com](mailto:customer@gmail.com)

```

Invalid:

```

customer@gmail

```

---

## Validity Checks

Examples:

- Data type checks
- Range validation
- Pattern matching

---

# 5. Uniqueness

## Definition

Uniqueness ensures records are not duplicated.

Example:

A ticket ID should appear once.

Correct:

```

TKT-100001

```

Incorrect:

```

TKT-100001
TKT-100001

```

---

## Uniqueness Checks

Examples:

- Primary key tests
- Duplicate detection

---

# 6. Timeliness

## Definition

Timeliness measures whether data is available when needed.

Example:

A daily support dashboard should contain yesterday's tickets.

Not:

```

Last updated 6 months ago

```

---

# Data Quality Framework

A professional data quality framework contains:

```

Data Profiling

```
   ↓
```

Data Validation

```
   ↓
```

Testing

```
   ↓
```

Monitoring

```
   ↓
```

Issue Resolution

```

---

# Step 1: Data Profiling

## Definition

Data profiling analyzes datasets before transformation.

Objectives:

- Understand structure
- Identify problems
- Discover patterns

---

# Profiling Activities

Examples:

## Column Analysis

Check:

- Data types
- Unique values
- Missing values

---

## Distribution Analysis

Example:

Ticket priority:

```

Low       5000

Medium    10000

High      4000

Critical  1000

```

---

## Outlier Detection

Example:

Resolution time:

```

Most tickets:

1-48 hours

One ticket:

5000 hours

```

---

# Data Profiling in SupportOps Intelligence

The project used:

```

notebooks/

01_data_profiling.ipynb

```

Tools:

- Pandas
- Seaborn
- Matplotlib

Activities:

- Missing value analysis
- Data distribution checks
- Column inspection

---

# Step 2: Data Cleaning

Data cleaning improves raw data quality.

Activities included:

- Removing duplicates
- Fixing data types
- Standardizing values
- Handling missing data

Output:

```

data/cleaned/

customer_support_tickets_clean.csv

```

---

# Step 3: Data Testing

Testing ensures data remains reliable after transformation.

Types:

- Schema tests
- Data validation tests
- Business rule tests

---

# Data Quality Testing in dbt

dbt provides automated tests.

Example:

```

not_null

unique

relationships

accepted_values

````

---

# Not Null Tests

Ensure important columns contain values.

Example:

```yaml
tests:

  - not_null
````

Applied to:

```
ticket_id

customer_key

agent_key
```

---

# Unique Tests

Ensure identifiers are unique.

Example:

```yaml
tests:

  - unique
```

Applied to:

```
ticket_id
```

---

# Relationship Tests

Validate foreign keys.

Example:

```
fact_ticket.customer_key

↓

dim_customer.customer_key
```

Ensures:

Every ticket has a valid customer.

---

# Accepted Values Tests

Ensure columns contain valid categories.

Example:

Priority:

```
Low

Medium

High

Critical
```

Invalid:

```
Urgent123
```

---

# Data Quality in SupportOps Intelligence

The project implemented:

```
16 dbt tests
```

Including:

* Not null tests
* Unique tests
* Relationship tests

---

# Data Quality Monitoring

Testing happens during development.

Monitoring happens continuously.

---

# Monitoring Questions

Examples:

## Freshness

Is new data arriving?

---

## Volume

Did the number of records suddenly change?

Example:

Normally:

```
20,000 tickets/day
```

Today:

```
100 tickets
```

Possible problem.

---

## Distribution

Did values change unexpectedly?

Example:

SLA success rate:

Normal:

```
85%
```

Suddenly:

```
10%
```

---

# Data Quality Metrics

Common metrics:

## Completeness Score

Formula:

```
Complete Records / Total Records
```

---

## Duplicate Rate

Formula:

```
Duplicate Records / Total Records
```

---

## Error Rate

Formula:

```
Invalid Records / Total Records
```

---

# Data Quality Pipeline

Professional architecture:

```
             Raw Data

                 |

                 ↓

          Data Profiling

                 |

                 ↓

          Data Cleaning

                 |

                 ↓

             dbt Tests

                 |

                 ↓

        Quality Monitoring

                 |

                 ↓

          BI Reporting
```

---

# Data Quality Best Practices

## 1. Test Early

Do not wait until reporting.

---

## 2. Automate Checks

Manual checks do not scale.

Use:

* dbt tests
* Python validation
* CI/CD pipelines

---

## 3. Document Expectations

Example:

A ticket must have:

```
ticket_id

customer_id

created_date
```

---

## 4. Monitor Production Data

Quality issues can appear after deployment.

---

# Tools Used in Data Quality

| Tool               | Purpose                     |
| ------------------ | --------------------------- |
| Pandas             | Data profiling and cleaning |
| dbt Tests          | Transformation validation   |
| Great Expectations | Data validation framework   |
| Soda               | Data monitoring             |
| Monte Carlo        | Data observability          |

---

# Skills Required

## SQL

Learn:

* Validation queries
* Data profiling queries
* Duplicate detection

---

## Python

Learn:

* Pandas validation
* Automated checks
* Data profiling scripts

---

## dbt

Learn:

* Schema tests
* Custom tests
* Documentation

---

## Data Engineering

Learn:

* Pipeline monitoring
* Data observability
* Data reliability

---

# Resources

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Recommended for:

* Data reliability
* Pipeline design
* Data systems

### Designing Data-Intensive Applications

Author:

Martin Kleppmann

Recommended for:

* Reliable systems
* Distributed data architecture

---

## Courses

Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

Great Expectations Documentation:

[https://docs.greatexpectations.io/](https://docs.greatexpectations.io/)

dbt Testing Documentation:

[https://docs.getdbt.com/docs/build/data-tests](https://docs.getdbt.com/docs/build/data-tests)

---

# Summary

Data quality is a foundation of trustworthy analytics.

A strong analytics engineering workflow ensures:

```
Clean Data

+

Tested Transformations

+

Reliable Models

+

Trusted Dashboards
```

The SupportOps Intelligence Analytics project applied data quality practices through:

* Python profiling
* Data cleaning
* dbt testing
* Schema validation
* Analytical modeling

These practices ensure that business users can trust the final Power BI insights.
