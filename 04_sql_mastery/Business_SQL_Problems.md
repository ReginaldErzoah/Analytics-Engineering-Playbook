# Business SQL Problems

## Overview

Analytics Engineers and Data Analysts are rarely evaluated only on SQL syntax.

Companies test whether you can translate business questions into reliable SQL solutions.

Business SQL problems usually involve:

- Understanding business requirements
- Identifying required tables
- Joining datasets correctly
- Creating meaningful metrics
- Handling missing or incorrect data
- Communicating insights

This document covers common analytical SQL scenarios.

---

# 1. Customer Support Analytics Problems

## Problem 1: Total Number of Tickets

### Business Question

How many customer support tickets were created?

### SQL

```sql
SELECT

COUNT(*) AS total_tickets

FROM tickets;
```

### Analytics Use Case

Used as a top-level KPI:

- Ticket volume
- Support workload
- Demand monitoring

---

# Problem 2: Ticket Volume by Month

### Business Question

How many tickets were created each month?

### SQL

```sql
SELECT

DATE_TRUNC(
    'month',
    created_at
) AS ticket_month,

COUNT(*) AS total_tickets

FROM tickets

GROUP BY ticket_month

ORDER BY ticket_month;
```

### Insight

Helps identify:

- Seasonal support demand
- Growth trends
- Peak periods

---

# Problem 3: Tickets by Priority

### Business Question

How many tickets exist for each priority level?

### SQL

```sql
SELECT

ticket_priority,

COUNT(*) AS ticket_count

FROM tickets

GROUP BY ticket_priority

ORDER BY ticket_count DESC;
```

### Business Impact

Helps teams understand:

- Urgency distribution
- Resource allocation
- Escalation risks

---

# Problem 4: Average Resolution Time

### Business Question

What is the average time taken to resolve tickets?

### SQL

```sql
SELECT

AVG(
    resolution_hours
)

AS avg_resolution_hours

FROM tickets;
```

### KPI

Customer Support Efficiency Metric.

---

# Problem 5: First Response Performance

### Business Question

What is the average first response time?

### SQL

```sql
SELECT

AVG(
    first_response_hours
)

AS avg_first_response_hours

FROM tickets;
```

### Business Impact

Measures:

- Customer experience
- SLA performance
- Support responsiveness

---

# Problem 6: Customer Satisfaction Score

### Business Question

What is the average customer satisfaction rating?

### SQL

```sql
SELECT

AVG(
    satisfaction_rating
)

AS average_rating

FROM tickets;
```

---

# Problem 7: Tickets With Low Satisfaction

### Business Question

Identify customers with poor experiences.

### SQL

```sql
SELECT

customer_id,

ticket_id,

satisfaction_rating

FROM tickets

WHERE satisfaction_rating <= 2;
```

### Use Case

Supports:

- Root cause analysis
- Customer recovery actions

---

# Problem 8: Resolution Time by Priority

### Business Question

Do high-priority tickets take longer to resolve?

### SQL

```sql
SELECT

ticket_priority,

AVG(resolution_hours)

AS avg_resolution

FROM tickets

GROUP BY ticket_priority;
```

---

# 2. Customer Analytics Problems

---

# Problem 9: Number of Customers

### Business Question

How many unique customers exist?

### SQL

```sql
SELECT

COUNT(
    DISTINCT customer_id
)

AS customers

FROM customers;
```

---

# Problem 10: Top Customers by Revenue

### Business Question

Who are the highest-value customers?

### SQL

```sql
SELECT

customer_id,

SUM(order_amount)

AS revenue

FROM orders

GROUP BY customer_id

ORDER BY revenue DESC

LIMIT 10;
```

---

# Problem 11: Customer Purchase Frequency

### Business Question

How often does each customer purchase?

### SQL

```sql
SELECT

customer_id,

COUNT(order_id)

AS total_orders

FROM orders

GROUP BY customer_id

ORDER BY total_orders DESC;
```

---

# Problem 12: Customer Segmentation

### Business Question

Classify customers based on spending.

Example:

```sql
SELECT

customer_id,

SUM(amount) AS revenue,

CASE

WHEN SUM(amount) >= 10000
THEN 'High Value'

WHEN SUM(amount) >= 5000
THEN 'Medium Value'

ELSE 'Low Value'

END AS customer_segment

FROM orders

GROUP BY customer_id;
```

---

# 3. Product Analytics Problems

---

# Problem 13: Best Selling Products

### Business Question

Which products generate the most sales?

### SQL

```sql
SELECT

product_id,

SUM(quantity)

AS units_sold

FROM orders

GROUP BY product_id

ORDER BY units_sold DESC

LIMIT 10;
```

---

# Problem 14: Revenue by Product

```sql
SELECT

product_id,

SUM(
    quantity * price
)

AS revenue

FROM order_items

GROUP BY product_id

ORDER BY revenue DESC;
```

---

# Problem 15: Products With No Sales

### Business Question

Find products that have never been purchased.

### SQL

```sql
SELECT

p.product_id,

p.product_name

FROM products p

LEFT JOIN order_items o

ON p.product_id = o.product_id

WHERE o.product_id IS NULL;
```

---

# 4. Operational Analytics Problems

---

# Problem 16: Daily Performance Report

### Business Question

Create a daily operational summary.

### SQL

```sql
SELECT

DATE(created_at) AS date,

COUNT(*) AS tickets,

AVG(resolution_hours)

AS avg_resolution

FROM tickets

GROUP BY date

ORDER BY date;
```

---

# Problem 17: Identify Performance Issues

### Business Question

Find days where resolution time exceeded target.

```sql
SELECT

DATE(created_at) AS date,

AVG(resolution_hours)

AS avg_resolution

FROM tickets

GROUP BY date

HAVING AVG(resolution_hours) > 24;
```

---

# Problem 18: SLA Breaches

### Business Question

How many tickets violated SLA?

```sql
SELECT

COUNT(*) AS sla_breaches

FROM tickets

WHERE resolution_hours > 24;
```

---

# 5. Data Quality Problems

Analytics Engineers must ensure reporting data is reliable.

---

# Problem 19: Find Duplicate Records

```sql
SELECT

ticket_id,

COUNT(*) AS occurrences

FROM tickets

GROUP BY ticket_id

HAVING COUNT(*) > 1;
```

---

# Problem 20: Missing Values

Find missing customer emails:

```sql
SELECT *

FROM customers

WHERE email IS NULL;
```

---

# Problem 21: Invalid Values

Find negative resolution times:

```sql
SELECT *

FROM tickets

WHERE resolution_hours < 0;
```

---

# 6. Advanced Analytical Problems

---

# Problem 22: Month-over-Month Growth

### Business Question

How did ticket volume change compared to the previous month?

```sql
WITH monthly_tickets AS (

SELECT

DATE_TRUNC(
'month',
created_at
) AS month,

COUNT(*) AS tickets

FROM tickets

GROUP BY month

)

SELECT

month,

tickets,

tickets -
LAG(tickets)

OVER(
ORDER BY month
)

AS growth

FROM monthly_tickets;
```

---

# Problem 23: Rank Customers

### Business Question

Rank customers by revenue.

```sql
SELECT

customer_id,

SUM(amount) AS revenue,

RANK()

OVER(
ORDER BY SUM(amount) DESC
)

AS ranking

FROM orders

GROUP BY customer_id;
```

---

# Problem 24: Latest Customer Record

### Business Question

Get the latest information for each customer.

```sql
WITH ranked AS (

SELECT

*,

ROW_NUMBER()

OVER(

PARTITION BY customer_id

ORDER BY updated_at DESC

) AS rn

FROM customer_history

)

SELECT *

FROM ranked

WHERE rn = 1;
```

---

# SQL Interview Framework

When given a business problem:

## Step 1: Understand the Metric

Ask:

- What exactly are we measuring?
- What is the business definition?

Example:

"Customer satisfaction"

Could mean:

- Average rating
- Percentage satisfied customers
- CSAT score

---

## Step 2: Identify Required Tables

Determine:

- Fact tables
- Dimension tables
- Relationships

Example:

```
fact_tickets

      |

dim_customers

      |

dim_products
```

---

## Step 3: Validate the Data

Check:

- NULL values
- Duplicates
- Incorrect joins
- Unexpected values

---

## Step 4: Build the Query

Start simple:

1. Select required columns
2. Filter data
3. Join tables
4. Aggregate
5. Add business logic

---

# Common SQL Interview Questions

## How would you analyze declining customer satisfaction?

Approach:

1. Track CSAT over time
2. Segment by:
   - Product
   - Region
   - Ticket type
   - Priority
3. Identify affected groups
4. Investigate root causes

---

## How would you improve a slow dashboard query?

Possible solutions:

- Reduce unnecessary columns
- Optimize joins
- Create aggregated tables
- Add indexes
- Use partitioning
- Precompute metrics

---

# Key Takeaway

Strong SQL is not about writing complex queries.

It is about:

- Understanding business problems
- Building accurate metrics
- Creating reliable datasets
- Communicating actionable insights

Analytics Engineers use SQL to transform raw data into trusted business intelligence.