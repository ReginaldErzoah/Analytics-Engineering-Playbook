# Slowly Changing Dimensions (SCD)

## Overview

Slowly Changing Dimensions (SCD) are a data warehouse design technique used to manage changes in dimension data over time.

Dimension tables contain descriptive information about business entities.

Examples:

- Customers
- Employees
- Products
- Locations

These attributes can change.

Examples:

A customer moves:

```

Location: Accra

↓

Location: Kumasi

```

An employee changes teams:

```

Team: Customer Support

↓

Team: Technical Support

```

The challenge is:

Should the warehouse overwrite the old value, or should it preserve history?

Slowly Changing Dimensions provide strategies for handling these changes.

---

# Why Slowly Changing Dimensions Matter

Operational systems usually store only the current state.

Example:

Customer table:

```

Customer ID

Name

Location

```

Current data:

```

001

Ama Mensah

Kumasi

```

The previous location may be lost.

However, analytics often requires historical questions:

- Where was the customer located when the ticket was created?
- Which department was an employee assigned to at that time?
- What product category existed during a previous sale?

SCD solves this problem.

---

# Dimension Changes Example

Consider:

```

dim_customer

```

Initial data:

| Customer Key | Customer ID | Name | Location |
|---|---|---|---|
| 101 | C001 | Ama | Accra |

Customer moves:

```

Accra

↓

Kumasi

```

How should the warehouse handle this?

There are several approaches.

---

# Types of Slowly Changing Dimensions

The most common types are:

1. Type 0
2. Type 1
3. Type 2
4. Type 3

The most widely used in modern data warehouses are:

- Type 1
- Type 2

---

# Type 0: Fixed Dimension

## Definition

Type 0 does not allow changes.

The original value is preserved permanently.

Example:

Customer birth date:

Before:

```

Date of Birth:

1995-05-10

```

After:

```

1995-05-10

```

No update occurs.

---

# When To Use Type 0

Use when:

- Data should never change
- Historical accuracy is critical

Examples:

- Birth date
- Original registration date
- Product creation date

---

# Type 1: Overwrite Dimension

## Definition

Type 1 replaces old values with new values.

No history is preserved.

Example:

Before:

| Customer | Location |
|-|-|
| Ama | Accra |

After update:

| Customer | Location |
|-|-|
| Ama | Kumasi |

The old value disappears.

---

# Type 1 Example

Original:

```

customer_key

101

location

Accra

````

Update:

```sql
UPDATE dim_customer

SET location = 'Kumasi'

WHERE customer_key = 101;
````

Final state:

```
customer_key

101


location

Kumasi
```

---

# Advantages of Type 1

Benefits:

* Simple implementation
* Requires less storage
* Easy reporting

---

# Disadvantages of Type 1

Problems:

* Historical information is lost
* Cannot analyze previous states

Example:

Question:

"Where was the customer when they opened a ticket?"

Cannot be answered.

---

# When To Use Type 1

Use when:

* Historical changes do not matter
* Data corrections are required

Examples:

Correcting:

* Spelling mistakes
* Incorrect labels
* Formatting issues

---

# Type 2: Historical Tracking

## Definition

Type 2 preserves history by creating a new dimension record.

Instead of updating the existing row:

A new row is inserted.

---

# Type 2 Example

Initial:

| Customer Key | Customer ID | Location | Start Date | End Date | Current |
| ------------ | ----------- | -------- | ---------- | -------- | ------- |
| 101          | C001        | Accra    | 2025-01-01 | NULL     | Yes     |

Customer moves.

New record:

| Customer Key | Customer ID | Location | Start Date | End Date   | Current |
| ------------ | ----------- | -------- | ---------- | ---------- | ------- |
| 101          | C001        | Accra    | 2025-01-01 | 2026-02-01 | No      |
| 102          | C001        | Kumasi   | 2026-02-01 | NULL       | Yes     |

History is preserved.

---

# Type 2 Components

A Type 2 dimension usually contains:

## Surrogate Key

Example:

```
customer_key
```

Each historical version receives a unique key.

---

## Effective Start Date

When the record became active.

Example:

```
valid_from
```

---

## Effective End Date

When the record stopped being active.

Example:

```
valid_to
```

---

## Current Indicator

Shows active record.

Example:

```
is_current
```

---

# Type 2 Query Example

Find the customer record active on a specific date:

```sql
SELECT *

FROM dim_customer

WHERE customer_id = 'C001'

AND '2025-06-01'
BETWEEN valid_from AND valid_to;
```

---

# Advantages of Type 2

Benefits:

* Preserves complete history
* Supports accurate reporting
* Enables time-based analysis

---

# Disadvantages of Type 2

Problems:

* More complex
* Requires more storage
* Requires careful joins

---

# When To Use Type 2

Use when history matters.

Examples:

Customer:

* Location changes
* Customer segment changes

Employee:

* Department changes
* Role changes

Products:

* Category changes

---

# Type 3: Previous Value Tracking

## Definition

Type 3 stores limited history by adding extra columns.

Example:

| Customer | Current Location | Previous Location |
| -------- | ---------------- | ----------------- |
| Ama      | Kumasi           | Accra             |

---

# Type 3 Example

Before:

```
location

Accra
```

After:

```
current_location

Kumasi


previous_location

Accra
```

---

# Advantages of Type 3

Benefits:

* Simple querying
* Stores recent history

---

# Disadvantages of Type 3

Problems:

* Limited history
* Requires additional columns

---

# Type 3 Use Cases

Useful when only the previous value matters.

Example:

* Previous account manager
* Previous region

---

# Type 4: History Table

## Definition

Type 4 separates current and historical records.

Example:

Current table:

```
dim_customer
```

History table:

```
dim_customer_history
```

---

# Type 4 Structure

Current:

```
dim_customer

Customer ID

Current Location
```

History:

```
dim_customer_history

Customer ID

Old Location

Date Changed
```

---

# Type 4 Advantages

Benefits:

* Clear separation
* Good performance for current data

---

# Type 4 Disadvantages

Problems:

* More tables
* More complex architecture

---

# Slowly Changing Dimensions in dbt

dbt supports SCD workflows through:

* Snapshots
* Incremental models

---

# dbt Snapshots

Snapshots track changes in source data over time.

Example:

Source:

```
customers
```

Snapshot:

```
customer_history
```

---

# Snapshot Example

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

FROM customers

{% endsnapshot %}
```

---

# Snapshot Strategies

## Timestamp Strategy

Uses update timestamps.

Example:

```
updated_at
```

When timestamp changes:

A new version is created.

---

## Check Strategy

Compares column values.

Example:

```sql
check_cols=[
'name',
'location'
]
```

When values change:

A new record is created.

---

# Slowly Changing Dimensions in SupportOps Intelligence

The current project used:

```
dim_customer

dim_agent

dim_category

dim_channel

dim_priority
```

These dimensions were mostly static.

Therefore, Type 1 behavior was sufficient.

---

# Future SCD Improvements

For a production support system, Type 2 could be applied to:

## dim_customer

Track:

* Location changes
* Customer segments

---

## dim_agent

Track:

* Team changes
* Role changes
* Department changes

---

## dim_priority

Track:

* SLA policy changes

---

# Example Production Scenario

A support agent changes department.

Before:

```
Agent:

John

Department:

Customer Support
```

After:

```
Department:

Technical Support
```

Without SCD:

Historical reports may incorrectly show:

```
All tickets handled by Technical Support
```

With Type 2:

Historical accuracy is maintained.

---

# Choosing the Right SCD Type

| Type   | History Preserved | Complexity | Common Use              |
| ------ | ----------------- | ---------- | ----------------------- |
| Type 0 | Yes               | Low        | Fixed attributes        |
| Type 1 | No                | Low        | Corrections             |
| Type 2 | Yes               | Medium     | Analytics history       |
| Type 3 | Limited           | Medium     | Previous value          |
| Type 4 | Yes               | High       | Separate history tables |

---

# Best Practices

## 1. Understand Business Requirements

Ask:

Does historical accuracy matter?

---

## 2. Use Surrogate Keys

Never rely only on source identifiers.

---

## 3. Document Changes

Maintain:

* Effective dates
* Change reasons
* Ownership

---

## 4. Test Historical Logic

Validate:

* Duplicate records
* Missing dates
* Incorrect versions

---

# Skills Required

## Data Warehousing

Learn:

* Dimensional modeling
* Historical tracking
* Warehouse design

---

## dbt

Learn:

* Snapshots
* Incremental models
* Testing

---

## SQL

Learn:

* MERGE statements
* Window functions
* Date logic

---

## Analytics Engineering

Learn:

* Business requirements
* Data governance
* Data reliability

---

# Resources

## Books

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Recommended for:

* Dimensional modeling
* Slowly changing dimensions
* Enterprise analytics

### Designing Data-Intensive Applications

Author:

Martin Kleppmann

Recommended for:

* Data architecture
* Reliable systems

---

## Documentation

dbt Snapshots:

[https://docs.getdbt.com/docs/build/snapshots](https://docs.getdbt.com/docs/build/snapshots)

Kimball Group:

[https://www.kimballgroup.com/](https://www.kimballgroup.com/)

---

# Summary

Slowly Changing Dimensions provide a structured way to manage changing business information over time.

The major approaches are:

```
Type 1

Overwrite Changes


Type 2

Preserve History


Type 3

Store Previous Values


Type 4

Separate History Tables
```

For professional analytics engineering projects, Type 2 is the most important pattern to master because it enables accurate historical reporting and trusted business analytics.

