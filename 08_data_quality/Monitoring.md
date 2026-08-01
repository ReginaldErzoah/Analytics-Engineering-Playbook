# Data Quality Monitoring

## Overview

Data quality monitoring is the continuous process of observing, measuring, and alerting teams about the health of data assets.

While data testing checks whether data meets predefined rules, monitoring continuously tracks data behavior over time.

A strong monitoring system helps teams answer:

- Is the data available?
- Is the data accurate?
- Has anything changed unexpectedly?
- Are pipelines working correctly?

---

# Why Data Quality Monitoring Matters

Modern organizations depend on data for:

- Business intelligence
- Customer operations
- Forecasting
- Decision-making
- Automation

A broken data pipeline can create:

```
Incorrect Data

↓

Incorrect Metrics

↓

Incorrect Decisions
```

---

# Data Quality Monitoring Lifecycle

A typical workflow:

```
Collect Data Metrics

        ↓

Analyze Data Behavior

        ↓

Detect Anomalies

        ↓

Send Alerts

        ↓

Investigate Root Cause

        ↓

Resolve Issue
```

---

# Key Data Monitoring Areas

## 1. Data Freshness Monitoring

## Definition

Measures whether data is updated within the expected timeframe.

The question:

> "Is the data arriving on time?"

---

## Example

Expected:

```
Customer tickets updated daily at 6 AM
```

Actual:

```
Last update:

3 days ago
```

Possible issues:

- Pipeline failure
- Source system outage
- Authentication failure

---

## Freshness Metrics

Examples:

```
Last successful update time

Data delay duration

Pipeline execution time
```

---

# 2. Data Volume Monitoring

## Definition

Tracks the number of records arriving over time.

The question:

> "Did the amount of data change unexpectedly?"

---

## Example

Normal daily ticket volume:

```
50,000 tickets
```

Today:

```
500 tickets
```

Possible problems:

- Missing data
- Failed ingestion
- Source changes

---

## Volume Checks

Example:

```sql
SELECT COUNT(*)

FROM tickets;
```

Compare against historical averages.

---

# 3. Schema Monitoring

## Definition

Detects changes in the structure of datasets.

The question:

> "Did the data structure change?"

---

## Examples

Unexpected changes:

Column removed:

```
customer_email
```

Column renamed:

```
created_at

↓

creation_date
```

Data type changed:

```
amount:

integer

↓

string
```

---

# 4. Distribution Monitoring

## Definition

Tracks changes in the statistical properties of data.

The question:

> "Did the behavior of the data change?"

---

## Example

Customer satisfaction scores:

Normal:

```
Average:

4.5 / 5
```

Today:

```
Average:

1.8 / 5
```

Possible causes:

- Survey issue
- Customer experience problem
- Data pipeline error

---

# 5. Duplicate Monitoring

## Definition

Detects unexpected duplicate records.

Example:

Expected:

```
1 ticket = 1 record
```

Actual:

```
1 ticket = 5 records
```

---

SQL Example:

```sql
SELECT

ticket_id,

COUNT(*)

FROM tickets

GROUP BY ticket_id

HAVING COUNT(*) > 1;
```

---

# 6. Null Monitoring

## Definition

Tracks missing values over time.

Example:

Customer email completeness:

Yesterday:

```
98%
```

Today:

```
60%
```

Potential issue:

Source system stopped sending emails.

---

# Data Observability

## Overview

Data observability is a broader approach that combines:

- Monitoring
- Testing
- Lineage
- Incident management

It helps teams understand:

```
What happened?

Why did it happen?

Who is affected?
```

---

# The Five Pillars of Data Observability

## 1. Freshness

Can users access recent data?

---

## 2. Distribution

Are values behaving normally?

---

## 3. Volume

Are record counts expected?

---

## 4. Schema

Has the structure changed?

---

## 5. Lineage

Where did the data come from?

---

# Data Monitoring Tools

|Tool|Purpose|
|-|-|
|Monte Carlo|Data observability platform|
|Soda|Data quality monitoring|
|Great Expectations|Validation framework|
|Datadog|Infrastructure and data monitoring|
|dbt Cloud|Transformation monitoring|
|Apache Airflow|Workflow monitoring|

---

# Monitoring in dbt

dbt provides monitoring capabilities through:

- dbt tests
- Documentation
- Logs
- Job status
- Source freshness

---

Example:

```yaml
sources:

  - name: support

    tables:

      - name: tickets

        freshness:

          warn_after:

            count: 12

            period: hour
```

---

# Monitoring Pipeline Failures

Common failures:

## Failed SQL Transformation

Cause:

- Syntax error
- Schema change

---

## Missing Source Data

Cause:

- API failure
- File not delivered

---

## Slow Pipeline

Cause:

- Growing data volume
- Poor query performance

---

## Incorrect Metrics

Cause:

- Business logic changes
- Transformation errors

---

# Alerting Strategy

Good alerts should:

- Identify the problem
- Explain impact
- Notify the correct team

---

Example:

Poor alert:

```
Pipeline failed
```

Better alert:

```
Customer Support Pipeline Failed

Impact:

Power BI dashboard data is outdated

Failure:

dbt model fact_ticket_metrics

Action:

Review warehouse connection
```

---

# Example: Customer Support Analytics Monitoring

Dashboard depends on:

```
Support System

        ↓

Raw Tickets

        ↓

dbt Models

        ↓

Power BI Dashboard
```

Monitoring checks:

---

## Freshness

Question:

```
Did today's tickets arrive?
```

---

## Volume

Question:

```
Are ticket numbers normal?
```

---

## Quality

Question:

```
Are resolution times valid?
```

---

## Schema

Question:

```
Did the support platform change fields?
```

---

# Data Quality Incident Process

When an issue occurs:

```
Detect Problem

      ↓

Identify Impact

      ↓

Find Root Cause

      ↓

Fix Pipeline

      ↓

Validate Recovery

      ↓

Document Incident
```

---

# Best Practices

## 1. Monitor Critical Data Assets

Prioritize:

- Revenue tables
- Customer tables
- KPI datasets

---

## 2. Combine Testing and Monitoring

Testing:

```
Does data meet rules?
```

Monitoring:

```
Is data behaving normally?
```

Both are required.

---

## 3. Maintain Data Lineage

Understand:

```
Source

↓

Transformation

↓

Dashboard
```

---

## 4. Establish Data Ownership

Every important dataset should have:

- Owner
- Documentation
- Support process

---

## 5. Track Historical Trends

Monitoring improves when comparing:

- Today vs yesterday
- This month vs last month
- Current vs expected behavior

---

# Interview Questions

## What is data quality monitoring?

Continuous observation of data health to detect issues and maintain reliability.

---

## Difference between testing and monitoring?

Testing checks predefined rules; monitoring tracks ongoing behavior and anomalies.

---

## What metrics would you monitor for a dashboard?

I would monitor:

- Data freshness
- Record volume
- Schema changes
- Missing values
- Duplicate records
- Pipeline failures

---

## What is data observability?

A practice of understanding the health, reliability, and behavior of data systems through monitoring, lineage, and incident detection.

---

# Key Takeaway

Data quality monitoring ensures analytics systems remain reliable after deployment.

A mature analytics platform continuously:

```
Tests Data

↓

Monitors Data

↓

Detects Issues

↓

Alerts Teams

↓

Improves Reliability
```