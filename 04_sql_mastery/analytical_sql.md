# Analytical SQL for Analytics Engineering

## Overview

Analytical SQL focuses on using SQL to answer business questions through data analysis.

While basic SQL retrieves information, analytical SQL transforms raw data into:

- Business insights
- KPIs
- Performance metrics
- Trends
- Comparisons
- Decision-support information

Analytics engineers use analytical SQL when building:

- Data marts
- BI datasets
- Reporting tables
- Metrics layers

---

# Analytical Thinking With SQL

A strong analytical SQL workflow starts with a business question.

Example:

Business Question:

> Which support agents are performing best?

Translate into:

```
Performance =

Resolution Speed
+
SLA Compliance
+
Customer Satisfaction
```

Then create SQL metrics:

```sql
AVG(resolution_time_hours)

AVG(satisfaction_score)

SLA success rate
```

---

# The Analytical SQL Process

```
Business Question

        ↓

Identify Required Data

        ↓

Define Metrics

        ↓

Write SQL Logic

        ↓

Validate Results

        ↓

Create Reporting Model
```

---

# KPI Development With SQL

KPIs (Key Performance Indicators) measure business performance.

Examples:

## Support KPIs

| KPI | SQL Calculation |
|-|-|
| Total Tickets | COUNT(ticket_id) |
| Average Resolution Time | AVG(hours) |
| SLA Success Rate | Successful tickets / Total tickets |
| Customer Satisfaction | AVG(score) |

---

# Counting Business Events

Example:

Total tickets:

```sql
SELECT

COUNT(ticket_id)

AS total_tickets

FROM tickets;
```

---

# Calculating Averages

Example:

Average resolution time:

```sql
SELECT

AVG(resolution_time_hours)

AS avg_resolution_time

FROM tickets;
```

---

# Percentage Calculations

Example:

SLA success rate:

```sql
SELECT

100.0 *
SUM(
CASE
WHEN sla_met = TRUE
THEN 1
ELSE 0
END
)

/

COUNT(*)

AS sla_success_rate

FROM tickets;
```

---

# Trend Analysis

Businesses often analyze changes over time.

Example:

Monthly ticket volume:

```sql
SELECT

DATE_TRUNC(
'month',
submission_date
)

AS month,

COUNT(*) AS tickets

FROM tickets

GROUP BY month

ORDER BY month;
```

---

# Year-over-Year Analysis

Compare current performance against previous periods.

Example:

```
2025 Revenue

vs

2024 Revenue
```

Common techniques:

- Window functions
- Date comparisons
- Lag functions

Example:

```sql
SELECT

month,

revenue,

LAG(revenue)

OVER(
ORDER BY month
)

AS previous_month

FROM sales;
```

---

# Ranking Analysis

Used for:

- Top customers
- Best products
- Highest-performing agents

Example:

```sql
SELECT

agent_id,

AVG(satisfaction_score)
AS avg_score,

RANK() OVER(

ORDER BY AVG(satisfaction_score) DESC

)

AS ranking

FROM tickets

GROUP BY agent_id;
```

---

# Segmentation Analysis

Segmentation divides data into groups.

Examples:

Customers by:

- Region
- Age
- Purchase behavior
- Support history

Tickets by:

- Priority
- Category
- Channel

Example:

```sql
SELECT

priority_level,

COUNT(*) AS tickets

FROM tickets

GROUP BY priority_level;
```

---

# Funnel Analysis

Funnels measure progression through stages.

Example:

Customer support funnel:

```
Ticket Submitted

        ↓

Assigned

        ↓

Resolved

        ↓

Customer Feedback
```

SQL tracks conversion between stages.

---

# Retention Analysis

Retention measures continued engagement.

Example:

Customers returning for support:

```sql
COUNT(DISTINCT customer_id)
```

over different time periods.

Used for:

- Customer loyalty
- Product quality analysis
- Churn detection

---

# Cohort Analysis

Cohorts group users by a shared starting point.

Example:

Customers grouped by signup month:

```sql
SELECT

DATE_TRUNC(
'month',
signup_date
)

AS cohort_month,

COUNT(customer_id)

FROM customers

GROUP BY cohort_month;
```

---

# Customer Analysis Patterns

Common questions:

## Who are our most valuable customers?

SQL approach:

```sql
SUM(revenue)

GROUP BY customer_id
```

---

## Which customers create the most support tickets?

SQL approach:

```sql
COUNT(ticket_id)

GROUP BY customer_id
```

---

# Operational Performance Analysis

Analytics engineers frequently analyze operations.

Examples:

## Agent Performance

Metrics:

- Tickets handled
- Resolution speed
- SLA compliance
- Satisfaction score

---

## Category Performance

Metrics:

- Ticket volume
- Resolution difficulty
- Customer impact

---

## Channel Performance

Metrics:

- Email vs Chat vs Phone
- Response times
- Customer satisfaction

---

# Data Quality Analysis With SQL

SQL can identify data problems.

Examples:

## Duplicate Records

```sql
SELECT

ticket_id,

COUNT(*)

FROM tickets

GROUP BY ticket_id

HAVING COUNT(*) > 1;
```

---

## Missing Values

```sql
SELECT *

FROM tickets

WHERE customer_email IS NULL;
```

---

## Invalid Values

Example:

Negative resolution time:

```sql
SELECT *

FROM tickets

WHERE resolution_time_hours < 0;
```

---

# Analytical SQL In SupportOps Intelligence

The project uses analytical SQL to calculate:

## SLA Performance

Logic:

```
Resolution Time <= SLA Target

=

Within SLA
```

---

## Resolution Performance

Categories:

```
0-24 hours
Fast

24-48 hours
Standard

48-72 hours
Slow

72+ hours
Delayed
```

---

## Ticket Complexity

Based on:

- Resolution effort
- Time required

---

## Dashboard Metrics

Created through SQL:

- Total tickets
- Average resolution time
- SLA success rate
- Customer counts
- Agent counts

---

# Analytical SQL Best Practices

## Define Metrics Clearly

Before writing SQL:

Document:

- Formula
- Business meaning
- Data source

---

## Avoid Metric Duplication

A KPI should have:

- One definition
- One source of truth

---

## Validate Results

Always compare:

SQL result

against:

- Source data
- Business expectations
- Previous reports

---

## Make SQL Reusable

Instead of:

```
One huge query
```

Prefer:

```
Staging model

↓

Intermediate model

↓

Mart model
```

---

# Skills To Master

## Business Analytics

Learn:

- KPI design
- Metrics definition
- Business problem translation
- Root cause analysis


## SQL

Learn:

- Window functions
- Date analysis
- Cohorts
- Funnels
- Ranking


## Data Modeling

Learn:

- Fact tables
- Dimension tables
- Semantic layers


---

# Recommended Resources

## Books

### SQL for Data Analysis

Author:
Cathy Tanimura

Focus:

- Analytical SQL
- Real-world analysis


### The Analytics Engineering Handbook

Author:
Alexey Grigorev

Focus:

- Analytics workflows
- Modern data practices


---

## Courses

### Data Analysis with SQL

Mode Analytics:

https://mode.com/sql-tutorial/


### Advanced SQL

DataCamp:

https://www.datacamp.com/


---

## Documentation

DuckDB Analytics:

https://duckdb.org/docs/guides/overview


dbt Semantic Layer:

https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-semantic-layer


---

# Summary

Analytical SQL transforms data into business intelligence.

A professional analytics engineer should be able to:

- Define meaningful KPIs
- Translate business questions into queries
- Build analytical models
- Analyze trends
- Create trusted metrics

Analytical SQL is the bridge between raw data and business decisions.