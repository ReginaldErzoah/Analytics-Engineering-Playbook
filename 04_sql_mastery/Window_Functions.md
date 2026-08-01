# SQL Window Functions

## Overview

Window functions are one of the most important advanced SQL concepts for Analytics Engineers and Data Analysts.

They allow calculations across related rows while keeping the original row-level detail.

Unlike aggregate functions:

- `GROUP BY` collapses rows into summaries.
- Window functions preserve individual records.

Window functions are heavily used for:

- Ranking customers
- Calculating running totals
- Comparing periods
- Identifying latest records
- Customer support performance analysis
- Data deduplication
- Trend analysis

---

# 1. Window Function Structure

Basic syntax:

```sql
FUNCTION_NAME()

OVER(
    PARTITION BY column
    ORDER BY column
)
```

Example:

```sql
SELECT

customer_id,

order_date,

amount,

SUM(amount)

OVER(
    PARTITION BY customer_id
)

AS customer_total_sales

FROM orders;
```

---

# 2. Difference Between GROUP BY and Window Functions

## GROUP BY

Example:

```sql
SELECT

customer_id,

SUM(amount)

FROM orders

GROUP BY customer_id;
```

Result:

| customer_id | total_sales |
|-|-|
| 1 | 500 |
| 2 | 700 |

The original orders disappear.

---

## Window Function

```sql
SELECT

customer_id,

order_id,

amount,

SUM(amount)

OVER(
    PARTITION BY customer_id
)

FROM orders;
```

Result:

| customer_id | order_id | amount | customer_total |
|-|-|-|-|
|1|101|200|500|
|1|102|300|500|
|2|103|700|700|

Rows are preserved.

---

# 3. PARTITION BY

PARTITION BY divides data into groups.

Example:

Calculate each customer's total orders:

```sql
SELECT

customer_id,

order_id,

COUNT(*)

OVER(
    PARTITION BY customer_id
)

AS customer_orders

FROM orders;
```

Each customer is analyzed separately.

---

# 4. ORDER BY Inside Window Functions

ORDER BY determines calculation sequence.

Example:

Running sales total:

```sql
SELECT

order_date,

amount,

SUM(amount)

OVER(
    ORDER BY order_date
)

AS cumulative_sales

FROM orders;
```

Output:

| date | amount | cumulative |
|-|-|-|
|Jan 1|100|100|
|Jan 2|200|300|
|Jan 3|150|450|

---

# 5. Ranking Functions

Ranking functions assign positions.

Common functions:

| Function | Description |
|-|-|
|ROW_NUMBER()|Unique sequential ranking|
|RANK()|Ranking with gaps|
|DENSE_RANK()|Ranking without gaps|
|NTILE()|Divides records into groups|

---

# 6. ROW_NUMBER()

Assigns a unique number to every row.

Example:

```sql
SELECT

customer_id,

order_date,

ROW_NUMBER()

OVER(
    ORDER BY order_date
)

AS row_number

FROM orders;
```

Result:

|order_date|row_number|
|-|-|
|Jan 1|1|
|Jan 2|2|
|Jan 3|3|

---

# 7. ROW_NUMBER for Deduplication

Very common analytics engineering pattern.

Example:

Remove duplicate customers.

```sql
WITH duplicates AS (

SELECT

*,

ROW_NUMBER()

OVER(
    PARTITION BY customer_email
    ORDER BY created_at DESC
)

AS row_num

FROM customers

)

SELECT *

FROM duplicates

WHERE row_num = 1;
```

Logic:

```
Duplicate records

        ↓

Partition by customer

        ↓

Keep latest record

        ↓

Remove duplicates
```

Used heavily in:

- dbt models
- Customer master tables
- Event pipelines

---

# 8. RANK()

Ranks records but leaves gaps.

Example:

```sql
SELECT

customer_id,

revenue,

RANK()

OVER(
ORDER BY revenue DESC
)

AS ranking

FROM customer_sales;
```

Example:

|Customer|Revenue|Rank|
|-|-|-|
|A|1000|1|
|B|900|2|
|C|900|2|
|D|500|4|

---

# 9. DENSE_RANK()

Similar to RANK but removes gaps.

Example:

|Customer|Revenue|Rank|
|-|-|-|
|A|1000|1|
|B|900|2|
|C|900|2|
|D|500|3|

---

# 10. NTILE()

Divides rows into equal groups.

Example:

Segment customers into quartiles:

```sql
SELECT

customer_id,

revenue,

NTILE(4)

OVER(
ORDER BY revenue DESC
)

AS customer_group

FROM customers;
```

Use cases:

- Customer segmentation
- Performance groups
- Percentile analysis

---

# 11. LAG()

LAG accesses previous rows.

Syntax:

```sql
LAG(column)

OVER(
ORDER BY column
)
```

Example:

Compare monthly sales:

```sql
SELECT

month,

sales,

LAG(sales)

OVER(
ORDER BY month
)

AS previous_month_sales

FROM monthly_sales;
```

Result:

|Month|Sales|Previous|
|-|-|-|
|Jan|1000|NULL|
|Feb|1200|1000|
|Mar|1500|1200|

---

# 12. LEAD()

LEAD accesses future rows.

Example:

```sql
SELECT

order_date,

LEAD(order_date)

OVER(
ORDER BY order_date
)

AS next_order_date

FROM orders;
```

Useful for:

- Customer journeys
- Next event analysis
- Time between activities

---

# 13. FIRST_VALUE()

Returns the first value in a window.

Example:

Customer first purchase:

```sql
SELECT

customer_id,

order_date,

FIRST_VALUE(order_date)

OVER(
PARTITION BY customer_id
ORDER BY order_date
)

AS first_purchase

FROM orders;
```

---

# 14. LAST_VALUE()

Returns the last value.

Example:

Customer latest purchase:

```sql
SELECT

customer_id,

order_date,

LAST_VALUE(order_date)

OVER(
PARTITION BY customer_id
ORDER BY order_date
)

AS latest_purchase

FROM orders;
```

---

# 15. Window Frames

Window frames control which rows are included.

Example:

```sql
SUM(amount)

OVER(

ORDER BY order_date

ROWS BETWEEN
UNBOUNDED PRECEDING
AND CURRENT ROW

)
```

Meaning:

```
Start from first row

        ↓

Continue until current row
```

Used for:

- Running totals
- Moving averages
- Trend analysis

---

# 16. Moving Average

Example:

3-day moving average:

```sql
SELECT

date,

sales,

AVG(sales)

OVER(

ORDER BY date

ROWS BETWEEN
2 PRECEDING
AND CURRENT ROW

)

AS moving_average

FROM daily_sales;
```

---

# 17. Customer Support Analytics Examples

## Rank Agents by Resolution Time

```sql
SELECT

agent_id,

AVG(resolution_hours)

AS avg_resolution,

RANK()

OVER(
ORDER BY AVG(resolution_hours)
)

FROM tickets

GROUP BY agent_id;
```

---

## Find Latest Customer Ticket

```sql
WITH ranked AS (

SELECT

*,

ROW_NUMBER()

OVER(

PARTITION BY customer_id

ORDER BY created_at DESC

)

AS rn

FROM tickets

)

SELECT *

FROM ranked

WHERE rn = 1;
```

---

## Calculate SLA Performance

Example:

Rank tickets by response speed:

```sql
SELECT

ticket_id,

first_response_hours,

RANK()

OVER(

ORDER BY first_response_hours ASC

)

AS response_rank

FROM tickets;
```

---

# 18. Window Functions in dbt

Analytics Engineers frequently use window functions inside models.

Example:

```sql
WITH ticket_history AS (

SELECT

ticket_id,

customer_id,

created_at,

ROW_NUMBER()

OVER(

PARTITION BY customer_id

ORDER BY created_at DESC

)

AS latest_ticket

FROM tickets

)

SELECT *

FROM ticket_history

WHERE latest_ticket = 1;
```

---

# Common Interview Questions

## Q1. Difference between RANK and ROW_NUMBER?

ROW_NUMBER:

- Every row gets a unique number.

RANK:

- Equal values receive the same rank.
- Creates gaps.

---

## Q2. Why use window functions instead of GROUP BY?

Because window functions allow calculations while maintaining row-level information.

---

## Q3. What is PARTITION BY?

It divides rows into independent groups for window calculations.

---

## Q4. Difference between LAG and LEAD?

LAG:

Looks backward.

LEAD:

Looks forward.

---

# Analytics Engineering Applications

Window functions are essential for:

✅ Customer segmentation  
✅ SLA monitoring  
✅ Ranking performance  
✅ Historical comparisons  
✅ Duplicate removal  
✅ Incremental models  
✅ Trend analysis  
✅ Cohort analysis  

---

# Key Takeaway

A strong Analytics Engineer should master window functions because they bridge the gap between raw data and business insights.

They are one of the most common SQL skills tested in analytics interviews.