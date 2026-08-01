# KPI Design

## Overview

Key Performance Indicators (KPIs) are measurable values used to evaluate business performance and determine whether objectives are being achieved.

In analytics engineering, KPI design connects:

- Business goals
- Data models
- Analytical calculations
- BI dashboards

A technically correct dashboard is not useful if the KPIs do not answer important business questions.

The SupportOps Intelligence Analytics project focused on designing customer support KPIs that measure:

- Ticket volume
- Resolution efficiency
- SLA performance
- Customer satisfaction
- Agent productivity

---

# What Is a KPI?

## Definition

A KPI is a measurable value that shows progress toward a specific business objective.

Example:

Business goal:

```

Improve customer support efficiency

```

KPI:

```

Average Resolution Time

```

Measurement:

```

Average hours required to resolve tickets

```

---

# KPI vs Metric

Although often used interchangeably, they are different.

---

# Metric

A metric is any measurable value.

Examples:

```

Number of tickets

Number of customers

Average response time

```

---

# KPI

A KPI is a metric connected to a business objective.

Example:

Metric:

```

Tickets resolved

```

KPI:

```

SLA Compliance Rate

```

because it measures service performance.

---

# KPI Design Framework

A strong KPI should answer:

```

What are we measuring?

Why does it matter?

How is it calculated?

Who uses it?

What action does it support?

```

---

# KPI Design Process

Professional workflow:

```

Understand Business Goal

```
    ↓
```

Define Question

```
    ↓
```

Select KPI

```
    ↓
```

Create Formula

```
    ↓
```

Validate Data

```
    ↓
```

Visualize

```
    ↓
```

Monitor Performance

```

---

# SMART KPI Principles

Good KPIs should be:

## Specific

Clearly defined.

Bad:

```

Improve support

```

Good:

```

Reduce average resolution time

```

---

## Measurable

Must have a calculation.

Example:

```

Average Resolution Time =
Total Resolution Hours / Resolved Tickets

```

---

## Achievable

Targets should be realistic.

---

## Relevant

Must support business goals.

---

## Time-Based

Should include a period.

Example:

```

Monthly SLA Compliance Rate

```

---

# SupportOps Intelligence KPI Framework

The project focused on customer support operations.

Business questions:

- How many tickets are created?
- How efficiently are tickets resolved?
- Are SLAs being achieved?
- Which agents perform best?
- What issues affect customers most?

---

# KPI 1: Total Tickets

## Purpose

Measures overall support workload.

---

## Formula

```

Total Tickets =
COUNT(ticket_id)

````

---

## DAX

```DAX
Total Tickets =
COUNT(fact_ticket[ticket_id])
````

---

## Business Use

Helps understand:

* Support demand
* Workload changes
* Growth trends

---

# KPI 2: Average Resolution Time

## Purpose

Measures how quickly support issues are resolved.

---

## Formula

```
Average Resolution Time =

Total Resolution Hours

/

Resolved Tickets
```

---

## DAX

```DAX
Average Resolution Time =
AVERAGE(
fact_ticket[resolution_time_hours]
)
```

---

## Business Use

Helps identify:

* Slow processes
* Difficult categories
* Agent efficiency issues

---

# KPI 3: SLA Compliance Rate

## Purpose

Measures whether tickets are resolved within expected service levels.

---

## Formula

```
SLA Compliance Rate =

Tickets Meeting SLA

/

Total Resolved Tickets

× 100
```

---

## DAX Example

```DAX
SLA Compliance Rate =

DIVIDE(

COUNTROWS(

FILTER(

fact_ticket,

fact_ticket[sla_status] = "Met"

)

),

COUNTROWS(fact_ticket)

)
```

---

## Business Use

Measures:

* Service reliability
* Customer experience
* Operational performance

---

# KPI 4: Customer Satisfaction Score

## Purpose

Measures customer experience after support interactions.

---

## Formula

```
Average Satisfaction =

Total Satisfaction Scores

/

Number of Responses
```

---

## DAX

```DAX
Average Satisfaction =
AVERAGE(
fact_ticket[satisfaction_score]
)
```

---

## Business Use

Helps identify:

* Customer experience trends
* Service problems
* Improvement areas

---

# KPI 5: Tickets Per Agent

## Purpose

Measures agent workload distribution.

---

## Formula

```
Tickets Per Agent =

Total Tickets

/

Number of Agents
```

---

## Business Use

Helps with:

* Workforce planning
* Agent balancing
* Capacity decisions

---

# KPI 6: Critical Ticket Volume

## Purpose

Tracks urgent customer issues.

---

## Formula

```
Critical Tickets =

COUNT(Tickets WHERE Priority = Critical)
```

---

## Business Use

Helps identify:

* Major incidents
* Customer risks
* Operational pressure

---

# KPI Selection for Dashboards

A dashboard should not display every available metric.

Too many KPIs create confusion.

---

# Executive Dashboard KPIs

Recommended:

```
Total Tickets

Average Resolution Time

SLA Compliance Rate

Customer Satisfaction Score
```

---

# Operational Dashboard KPIs

Recommended:

```
Tickets Per Agent

Average Resolution Time

Tickets By Priority

Tickets By Category
```

---

# Customer Dashboard KPIs

Recommended:

```
Tickets By Customer

Top Issue Categories

Channel Distribution

Customer Satisfaction
```

---

# KPI Hierarchy

Professional analytics uses layers.

---

# Strategic KPIs

Used by leadership.

Examples:

```
Customer Satisfaction

SLA Compliance

Overall Support Performance
```

---

# Operational KPIs

Used by managers.

Examples:

```
Agent Performance

Resolution Time

Ticket Backlog
```

---

# Diagnostic Metrics

Used for investigation.

Examples:

```
Tickets By Category

Tickets By Channel

Priority Distribution
```

---

# KPI Calculation Best Practices

## 1. Define Formulas Clearly

Avoid ambiguity.

Bad:

```
Resolution Time
```

Good:

```
Average hours from ticket creation to ticket closure
```

---

## 2. Document Business Meaning

A KPI should include:

* Definition
* Formula
* Data source
* Owner

---

## 3. Validate Against Business Expectations

Example:

If SLA compliance suddenly changes:

Investigate:

* Data changes
* Logic changes
* Business changes

---

## 4. Avoid Vanity Metrics

Vanity metrics look impressive but do not drive decisions.

Example:

```
Total Customers Registered
```

may not matter if:

```
Customer Satisfaction is declining
```

---

# KPI Documentation Template

Example:

```
KPI Name:

SLA Compliance Rate


Definition:

Percentage of tickets resolved within SLA target.


Formula:

Met SLA Tickets / Total Resolved Tickets


Source:

fact_ticket


Owner:

Customer Support Team


Frequency:

Daily
```

---

# KPI Implementation in Power BI

A professional workflow:

```
Data Model

      ↓

DAX Measures

      ↓

KPI Cards

      ↓

Charts

      ↓

Business Interpretation
```

---

# KPI Implementation in SupportOps

Measures were created from:

```
fact_ticket
```

using:

* COUNT
* AVERAGE
* FILTER
* DIVIDE

---

# Common KPI Mistakes

## 1. Measuring Without Purpose

Bad:

```
Number of Columns in Database
```

No business meaning.

---

## 2. Incorrect Granularity

Example:

Comparing:

```
Daily Tickets

with

Yearly Satisfaction
```

creates misleading insights.

---

## 3. Ignoring Data Quality

A KPI is only as reliable as its source data.

---

## 4. Too Many KPIs

A dashboard should focus attention.

---

# KPI Skills Required

## Business Analysis

Learn:

* Requirement gathering
* Business questions
* Performance measurement

---

## SQL

Learn:

* Aggregations
* Filtering
* Window functions

---

## Power BI

Learn:

* DAX
* KPI cards
* Dashboard design

---

## Analytics Engineering

Learn:

* Metric layers
* Data modeling
* Documentation

---

# Resources

## Books

### Measure What Matters

Author:

John Doerr

Recommended for:

* Goal setting
* Performance measurement

### Storytelling with Data

Author:

Cole Nussbaumer Knaflic

Recommended for:

* Communicating insights

---

## Courses

Microsoft Power BI Learning:

[https://learn.microsoft.com/power-bi/](https://learn.microsoft.com/power-bi/)

SQLBI:

[https://www.sqlbi.com/](https://www.sqlbi.com/)

---

# Summary

KPI design connects technical analytics work with business outcomes.

A successful KPI framework requires:

```
Business Understanding

+

Reliable Data

+

Clear Definitions

+

Correct Calculations

+

Effective Communication
```

The SupportOps Intelligence Analytics project demonstrated KPI design by transforming support data into measurable insights around:

* Workload
* Efficiency
* SLA performance
* Customer satisfaction
* Agent productivity