# Slowly Changing Dimensions (SCD Types)

## Overview

Slowly Changing Dimensions (SCDs) are techniques used in dimensional modeling to manage changes in dimension table data over time.

Dimensions contain descriptive information about business entities:

Examples:

- Customers
- Products
- Employees
- Locations

These attributes can change.

Example:

A customer changes their address:

Before:

```
Customer ID: 101

Name: John Smith

City: Accra
```

After:

```
Customer ID: 101

Name: John Smith

City: Kumasi
```

The analytics team must decide:

- Should we overwrite the old value?
- Should we preserve history?
- How much history is needed?

SCDs define how these changes are handled.

---

# Why Slowly Changing Dimensions Matter

Without SCD strategies, historical analysis becomes inaccurate.

Example:

A customer lived in Accra in 2024.

In 2025 they moved to Kumasi.

Question:

> Which region should receive credit for the customer's 2024 purchases?

Without history:

```
All historical sales appear under Kumasi
```

With SCD:

```
2024 purchases → Accra

2025 purchases → Kumasi
```

---

# Common SCD Types

The most common types are:

|Type|Purpose|
|-|-|
|SCD Type 0|No changes allowed|
|SCD Type 1|Overwrite old values|
|SCD Type 2|Maintain full history|
|SCD Type 3|Store limited history|
|SCD Type 4|Separate history table|
|SCD Type 6|Hybrid approach|

The most commonly used in analytics engineering:

- Type 1
- Type 2

---

# SCD Type 0 — Fixed Dimension

## Definition

SCD Type 0 assumes dimension attributes never change.

Existing values are preserved permanently.

Example:

Customer date of birth:

```
customer_id | birth_date
------------|------------
101         | 1995-05-10
```

If incorrect:

```
No update occurs.
```

---

## Use Cases

Suitable for:

- Birth dates
- Original registration date
- Historical identifiers

---

## Advantages

- Simple
- No additional processing
- Preserves original information

---

## Disadvantages

- Cannot correct mistakes
- Not suitable for changing attributes

---

# SCD Type 1 — Overwrite

## Definition

SCD Type 1 replaces old values with new values.

No historical tracking is maintained.

Example:

Before:

```
customer_id | customer_name | city

101         | John Smith    | Accra
```

Customer moves.

After:

```
customer_id | customer_name | city

101         | John Smith    | Kumasi
```

The previous value is lost.

---

# Type 1 Example SQL

```sql
UPDATE dim_customer

SET city = 'Kumasi'

WHERE customer_id = 101;
```

---

# When to Use SCD Type 1

Use when:

- Historical changes do not matter
- Corrections are required
- Data quality fixes are needed

Examples:

Correcting:

- Typographical errors
- Wrong email addresses
- Incorrect categories

---

# Advantages

✅ Simple implementation

✅ Requires less storage

✅ Faster queries

---

# Disadvantages

❌ Historical changes are lost

❌ Cannot answer historical questions

---

# SCD Type 2 — Full History Tracking

## Definition

SCD Type 2 preserves every version of a dimension record.

Instead of updating a row:

A new row is created.

Example:

Customer moves from Accra to Kumasi.

Original:

|customer_id|name|city|start_date|end_date|current_flag|
|-|-|-|-|-|-|
|101|John Smith|Accra|2024-01-01|2025-03-01|False|

New record:

|customer_id|name|city|start_date|end_date|current_flag|
|-|-|-|-|-|-|
|101|John Smith|Kumasi|2025-03-01|NULL|True|

---

# SCD Type 2 Components

A Type 2 dimension usually contains:

## Surrogate Key

A unique identifier for each version.

Example:

```
customer_key
```

---

## Natural Key

The business identifier.

Example:

```
customer_id
```

---

## Effective Dates

Track when the record was valid.

Example:

```
valid_from

valid_to
```

---

## Current Flag

Indicates active record.

Example:

```
is_current
```

---

# Type 2 Example Structure

```
dim_customer

customer_key

customer_id

customer_name

city

valid_from

valid_to

is_current

```

---

# Querying SCD Type 2

Current customers:

```sql
SELECT *

FROM dim_customer

WHERE is_current = TRUE;
```

---

Historical customer location:

```sql
SELECT *

FROM dim_customer

WHERE customer_id = 101;
```

---

# Implementing SCD Type 2 in dbt

dbt supports SCD Type 2 through snapshots.

Example:

```sql
{% snapshot customer_snapshot %}

{{
config(
target_schema='snapshots',
unique_key='customer_id',
strategy='timestamp',
updated_at='updated_at'
)
}}

SELECT *

FROM {{ source(
'crm',
'customers'
) }}

{% endsnapshot %}
```

---

# dbt Snapshot Strategies

## Timestamp Strategy

Tracks changes using a timestamp column.

Example:

```
updated_at
```

dbt checks:

```
Has updated_at changed?
```

---

## Check Strategy

Compares selected columns.

Example:

```sql
check_cols=[
'name',
'city',
'email'
]
```

If values change:

A new version is created.

---

# SCD Type 3 — Limited History

## Definition

Stores current and previous values in the same row.

Example:

|customer_id|current_city|previous_city|
|-|-|-|
|101|Kumasi|Accra|

---

## Advantages

- Simple reporting
- Stores recent history

---

## Disadvantages

- Limited history
- Cannot track multiple changes

---

# SCD Type 4 — History Table

## Definition

Current dimension and historical changes are stored separately.

Example:

Current table:

```
dim_customer
```

History table:

```
customer_history
```

---

# SCD Type 6 — Hybrid Method

Combination of:

- Type 1
- Type 2
- Type 3

Stores:

- Current values
- Previous values
- Complete history

Used in complex enterprise systems.

---

# SCD Type Comparison

|Type|History|Storage|Common Usage|
|-|-|-|-|
|Type 0|None|Low|Fixed attributes|
|Type 1|None|Low|Corrections|
|Type 2|Full|High|Enterprise analytics|
|Type 3|Limited|Medium|Previous value tracking|
|Type 4|Full|High|Separate history systems|
|Type 6|Full|High|Complex environments|

---

# SCDs in Analytics Engineering

Analytics engineers use SCDs when building:

- Customer analytics
- Sales reporting
- Product analytics
- HR analytics
- Finance reporting

Example:

Customer Support Analytics:

Dimension:

```
dim_customer
```

Possible changing attributes:

```
customer_segment

customer_region

customer_status
```

If historical analysis matters:

Use:

```
SCD Type 2
```

---

# SCDs and Data Warehouses

SCDs are common in:

- Snowflake
- BigQuery
- Redshift
- Databricks
- DuckDB analytical workflows

They allow organizations to answer:

" What did we know at that point in time?"

---

# Interview Questions

## What is a slowly changing dimension?

A method for managing changes in dimension attributes over time.

---

## Difference between SCD Type 1 and Type 2?

Type 1 overwrites old values.

Type 2 creates new records to preserve history.

---

## When would you use SCD Type 2?

When historical reporting accuracy is important.

Example:

Customer location changes affecting regional sales reporting.

---

## How does dbt implement SCD Type 2?

Through snapshots that track changes in source records over time.

---

# Key Takeaway

SCDs are essential for building reliable analytical systems.

A strong analytics engineer understands:

- When to overwrite data
- When to preserve history
- How to model changing business entities
- How dbt snapshots automate historical tracking

SCD Type 2 is the most important concept for analytics engineering interviews.