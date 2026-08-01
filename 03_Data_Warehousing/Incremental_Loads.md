# Incremental Loads

## Overview

Incremental loading is a data pipeline strategy where only new or changed data is processed instead of rebuilding the entire dataset every time.

It is one of the most important techniques for scaling analytics systems.

Instead of:

```
Process all historical data

Every single run
```

Incremental loading does:

```
Process only new or modified records
```

---

# Full Refresh vs Incremental Load

## Full Refresh

A full refresh rebuilds the entire dataset.

Example:

```
Day 1:

Load 1 million records


Day 2:

Delete table

Reload 1 million records
```

---

## Incremental Load

Only new records are processed.

Example:

```
Day 1:

Load 1 million records


Day 2:

Load only yesterday's 10,000 new records
```

---

# Why Incremental Loads Matter

Large datasets can contain:

- Millions of transactions
- Billions of events
- Years of history

Reprocessing everything causes:

- Slow pipelines
- Higher costs
- Increased warehouse usage
- Longer dashboard refresh times

Incremental models improve:

- Performance
- Scalability
- Reliability

---

# Incremental Loading Pattern

Typical workflow:

```
Source Data

      ↓

Check New Records

      ↓

Transform New Records

      ↓

Merge Into Existing Table

      ↓

Updated Analytics Model
```

---

# Common Incremental Strategies

## 1. Append Only

New records are added.

Example:

```
Existing Table:

100,000 rows


New Data:

5,000 rows


Result:

105,000 rows
```

Used for:

- Event logs
- Transactions
- Activity streams

---

Example:

```sql
insert into sales

select *

from new_sales
```

---

# 2. Update Existing Records

Existing records are modified.

Example:

Customer changes email:

Before:

```
john@email.com
```

After:

```
john.new@email.com
```

The old record is updated.

---

# 3. Upsert (Merge)

Combines:

- Insert new records
- Update changed records

Example:

```
If customer exists:

    Update


Else:

    Insert
```

Common in:

- Customer databases
- CRM systems

---

Example:

```sql
MERGE INTO customers target

USING updates source

ON target.customer_id = source.customer_id

WHEN MATCHED THEN

UPDATE SET email = source.email

WHEN NOT MATCHED THEN

INSERT VALUES(source.customer_id);
```

---

# 4. Slowly Changing Dimension Loads

Used when historical changes matter.

Example:

Customer address changes:

```
Old:

Accra


New:

Kumasi
```

Instead of replacing:

```
Keep both versions
```

Common with:

- SCD Type 2
- Dimension tables

---

# Identifying New Records

A pipeline needs a way to identify new data.

Common methods:

---

# 1. Timestamp-Based Loading

Use:

- created_at
- updated_at
- modified_date

Example:

```sql
where updated_at >

last_run_timestamp
```

---

# 2. Increasing ID

Use sequential identifiers.

Example:

```
order_id

1001

1002

1003
```

Load:

```
WHERE order_id > last_processed_id
```

---

# 3. Change Data Capture (CDC)

Tracks changes directly from databases.

Captures:

- Inserts
- Updates
- Deletes

Example tools:

- Debezium
- AWS DMS
- Fivetran

---

# Incremental Models in dbt

dbt supports incremental materialization.

Example:

```sql
{{ config(

materialized='incremental'

) }}
```

---

Example:

```sql
select *

from orders

{% if is_incremental() %}

where order_date >

(select max(order_date)

from {{ this }})

{% endif %}
```

---

# Understanding is_incremental()

The function checks whether dbt is running incrementally.

Example:

First run:

```
Create complete table
```

Future runs:

```
Process only new records
```

---

# dbt Incremental Workflow

```
First Execution

        ↓

Create Table

        ↓

New Data Arrives

        ↓

Run dbt build

        ↓

Only New Records Processed

        ↓

Update Table
```

---

# Incremental Loading Challenges

## 1. Late Arriving Data

Problem:

A record arrives late.

Example:

```
Order from Monday

arrives Wednesday
```

Solution:

Use lookback windows.

Example:

```sql
where order_date >= current_date - interval '7 days'
```

---

# 2. Updates to Historical Records

Problem:

Old records change.

Example:

Customer profile update.

Solution:

Use:

- Merge logic
- CDC
- SCD patterns

---

# 3. Duplicate Records

Problem:

Pipeline runs twice.

Result:

Duplicate rows.

Solutions:

- Unique keys
- Deduplication logic
- Data tests

---

# 4. Schema Changes

Problem:

Source adds a new column.

Solution:

- Schema monitoring
- Documentation
- Controlled migrations

---

# Incremental Load Best Practices

## Use Reliable Unique Keys

Example:

```
customer_id

transaction_id

ticket_id
```

---

## Track Load Metadata

Useful fields:

```
created_at

updated_at

loaded_at

batch_id
```

---

## Test Incremental Models

Important tests:

```yaml
tests:

- unique

- not_null
```

---

## Monitor Data Freshness

Check:

```
When was data last updated?
```

---

## Document Logic

Explain:

- How records are identified
- How updates are handled

---

# Example: Customer Support Analytics

Source:

```
Support Tickets System
```

Daily incoming data:

```
10,000 new tickets
```

Without incremental loading:

```
Process:

50 million tickets
```

With incremental loading:

```
Process:

10,000 new tickets
```

dbt model:

```
fact_ticket_metrics
```

Only new and updated tickets are transformed.

---

# Interview Questions

## What is an incremental load?

A process that loads only new or changed records instead of rebuilding the entire dataset.

---

## Why use incremental models?

To improve performance, reduce cost, and scale data processing.

---

## Difference between full refresh and incremental?

Full refresh rebuilds everything; incremental updates only changed data.

---

## What problems occur with incremental loading?

Common issues include:

- Late arriving data
- Duplicate records
- Schema changes
- Historical updates

---

# Key Takeaway

Incremental loading is essential for production analytics systems.

A scalable pipeline does not repeatedly process everything.

It intelligently identifies:

```
New Data

+

Changed Data

+

Required Updates
```

and processes only what is necessary.