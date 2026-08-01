# Pipeline Monitoring

## Overview

Pipeline monitoring is the process of observing, tracking, and maintaining data pipelines to ensure they run reliably and produce accurate data.

A production data pipeline must not only move data — it must deliver:

- Correct data
- On time
- Consistently
- Without failures

A pipeline without monitoring is like a machine running without sensors.

---

# Why Pipeline Monitoring Matters

Data pipelines can fail silently.

Example:

```

Pipeline Runs Successfully

↓

Dashboard Updates

↓

Business Makes Decisions

↓

Data Was Incorrect

```

The pipeline completed, but the output was wrong.

Monitoring helps detect:

- Failures
- Delays
- Data quality issues
- Performance problems

---

# Pipeline Monitoring Goals

A good monitoring system answers:

## 1. Did The Pipeline Run?

Example:

```

Status:

SUCCESS

```

or:

```

FAILED

```

---

## 2. Did It Run On Time?

Example:

Expected:

```

Daily refresh at 01:00

```

Actual:

```

Completed at 06:30

```

Problem:

Dashboard data is delayed.

---

## 3. Was The Data Correct?

Example:

Expected:

```

100,000 records

```

Received:

```

50 records

```

Possible issue:

- Source failure
- Missing data
- Incorrect extraction

---

# Types Of Pipeline Monitoring

Main categories:

1. Operational Monitoring
2. Data Quality Monitoring
3. Performance Monitoring
4. Freshness Monitoring
5. Infrastructure Monitoring

---

# 1. Operational Monitoring

Operational monitoring checks whether pipeline processes are running correctly.

Monitor:

- Job status
- Task completion
- Errors
- Retries

---

Example:

Pipeline:

```

Extract

↓

Transform

↓

Load

```

Status:

```

Extract   ✓

Transform ✓

Load      ✗

```

---

# Common Operational Metrics

## Pipeline Success Rate

Measures how often pipelines complete successfully.

Formula:

```

Successful Runs / Total Runs

```

Example:

```

95 successful runs

/

100 total runs

=

95%

```

---

## Failure Count

Tracks failed executions.

Example:

```

Pipeline failures this week:

3

```

---

## Retry Count

Tracks how often jobs need retries.

High retries may indicate:

- Infrastructure issues
- Slow systems
- Unstable sources

---

# 2. Data Quality Monitoring

A pipeline can succeed technically but produce bad data.

Example:

```

Pipeline Status:

SUCCESS

Data:

INVALID

```

---

Data quality monitoring checks:

- Completeness
- Accuracy
- Consistency
- Validity
- Uniqueness

---

# Completeness Checks

Ensures required data exists.

Example:

Bad:

```

customer_id

NULL

````

---

Validation:

```sql
SELECT *

FROM customers

WHERE customer_id IS NULL;
````

Expected:

```
0 rows
```

---

# Accuracy Checks

Checks whether data values are correct.

Example:

Revenue should not be negative.

```sql
SELECT *

FROM sales

WHERE revenue < 0;
```

---

# Consistency Checks

Ensures related datasets agree.

Example:

Orders table:

```
1000 orders
```

Payments table:

```
950 payments
```

Possible issue.

---

# Uniqueness Checks

Checks duplicate records.

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

# Validity Checks

Ensures values follow rules.

Example:

Status values:

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

# 3. Data Freshness Monitoring

Freshness measures how recently data was updated.

Example:

Expected:

```
Updated:

01:00 AM
```

Actual:

```
Updated:

10:00 AM
```

---

Freshness problems affect:

* Dashboards
* Reports
* Business decisions

---

Example metric:

```
Data Age = Current Time - Last Update Time
```

---

# Freshness Example

Sales dashboard:

Expected:

```
Yesterday's sales available by 6 AM
```

Problem:

```
No data after 3 days
```

Monitoring should alert the team.

---

# 4. Performance Monitoring

Performance monitoring tracks how efficiently pipelines run.

Measure:

* Execution time
* Resource usage
* Processing volume

---

# Execution Time

Example:

Normal:

```
Pipeline runtime:

20 minutes
```

Problem:

```
Pipeline runtime:

3 hours
```

---

Possible causes:

* Query slowdown
* Increased data volume
* Infrastructure issues

---

# Data Volume Monitoring

Track:

* Number of records processed
* File sizes
* Table growth

---

Example:

Normal:

```
Daily records:

500,000
```

Problem:

```
Daily records:

5,000
```

---

# Resource Monitoring

Monitor:

* CPU
* Memory
* Storage
* Network

---

# 5. Infrastructure Monitoring

Tracks the systems supporting pipelines.

Examples:

* Servers
* Databases
* Cloud services

---

Monitor:

## Database Health

Check:

* Connections
* Storage
* Query performance

---

## Cloud Storage

Check:

* Availability
* File access
* Permissions

---

# Pipeline Monitoring Architecture

Typical setup:

```
Data Pipeline

↓

Logs

↓

Metrics

↓

Monitoring System

↓

Alerts

↓

Engineering Team
```

---

# Logging

Logs record pipeline events.

Example:

```
2026-08-01

01:00

Started extraction

01:05

Loaded 50,000 rows

01:10

Completed successfully
```

---

# What To Log

Important information:

## Pipeline Name

Example:

```
customer_pipeline
```

---

## Execution Time

Example:

```
Started:

01:00

Finished:

01:20
```

---

## Record Counts

Example:

```
Input:

100,000

Output:

99,800
```

---

## Errors

Example:

```
Database connection failed
```

---

# Metrics

Metrics are numerical measurements.

Examples:

## Pipeline Runtime

```
20 minutes
```

---

## Records Processed

```
500,000 rows
```

---

## Failure Rate

```
2%
```

---

# Alerts

Alerts notify teams when problems occur.

Examples:

## Failure Alert

```
Pipeline failed:

customer_pipeline
```

---

## Freshness Alert

```
Data has not updated in 24 hours
```

---

## Quality Alert

```
Duplicate customers detected
```

---

# Alerting Tools

Examples:

* Slack
* Email
* PagerDuty
* Microsoft Teams

---

# Monitoring Tools

## Data Pipeline Monitoring

### Apache Airflow

Provides:

* DAG monitoring
* Task status
* Logs
* Scheduling

---

### Dagster

Provides:

* Pipeline observability
* Asset monitoring
* Testing

---

### Prefect

Provides:

* Workflow monitoring
* Cloud dashboard

---

# Data Quality Tools

Examples:

## Great Expectations

Used for:

* Data validation
* Quality checks

---

## dbt Tests

Example:

```yaml
tests:

- unique

- not_null

- accepted_values
```

---

## Soda

Used for:

* Data monitoring
* Data quality checks

---

# Pipeline Monitoring In Analytics Engineering

Analytics engineers monitor:

* dbt runs
* Model failures
* Test failures
* Data freshness

---

Example:

dbt workflow:

```
dbt run

↓

dbt test

↓

Generate Documentation

↓

Refresh Dashboard
```

---

If tests fail:

```
Pipeline Stops

↓

Alert Team

↓

Fix Issue
```

---

# dbt Monitoring Example

Model:

```
fact_sales
```

Tests:

```yaml
columns:

- name: order_id

  tests:

  - unique

  - not_null
```

---

Failure:

```
Duplicate order IDs found
```

Action:

Investigate source data.

---

# Production Pipeline Example

SupportOps Analytics:

```
Support System

↓

Daily Extraction

↓

Pipeline Execution

↓

Data Validation

↓

dbt Models

↓

Dashboard Refresh
```

Monitoring:

```
Check:

✓ Pipeline success

✓ Record counts

✓ Data freshness

✓ dbt tests
```

---

# Common Pipeline Problems

## 1. Source System Failure

Example:

API unavailable.

Solution:

* Retry
* Alert

---

## 2. Schema Changes

Example:

Column renamed:

Before:

```
customer_id
```

After:

```
client_id
```

Solution:

* Schema validation
* Documentation

---

## 3. Data Volume Increase

Example:

Records increase:

```
100k

↓

10 million
```

Solution:

* Optimize queries
* Scale infrastructure

---

## 4. Incorrect Logic

Example:

Revenue calculation changes.

Solution:

* Testing
* Code review

---

# Pipeline Monitoring Best Practices

## Monitor Everything Important

Track:

* Status
* Runtime
* Data quality
* Freshness

---

## Create Meaningful Alerts

Avoid alert overload.

Bad:

```
100 notifications daily
```

Good:

```
Only important failures
```

---

## Automate Checks

Use:

* dbt tests
* Monitoring tools
* CI/CD

---

## Document Failures

Maintain:

* Root causes
* Fixes
* Prevention steps

---

# Skills To Learn

## SQL

For:

* Data validation
* Quality checks

---

## Python

For:

* Logging
* Automation

---

## dbt

For:

* Testing
* Documentation
* Model monitoring

---

## Orchestration

Learn:

* Airflow
* Dagster
* Prefect

---

# Resources

## Books

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Data reliability
* Pipeline architecture

### Data Engineering Teams

Author:

George Fraser

Focus:

* Production data systems

---

## Documentation

Apache Airflow:

[https://airflow.apache.org/docs/](https://airflow.apache.org/docs/)

dbt Documentation:

[https://docs.getdbt.com/](https://docs.getdbt.com/)

Great Expectations:

[https://docs.greatexpectations.io/](https://docs.greatexpectations.io/)

---

# Summary

Pipeline monitoring ensures that data systems remain reliable.

A mature monitoring strategy checks:

```
Pipeline Execution

↓

Data Quality

↓

Freshness

↓

Performance

↓

Alerts

↓

Continuous Improvement


For analytics engineers, monitoring is essential because trustworthy analytics depends on trustworthy pipelines.
