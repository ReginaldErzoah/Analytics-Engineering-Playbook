# dbt Sources

## Overview

In dbt, **sources** represent raw data tables that exist outside of dbt models.

They define where the original data comes from before transformations begin.

A typical analytics engineering workflow:

```
External Data Source

        ↓

dbt Source Definition

        ↓

Staging Models

        ↓

Intermediate Models

        ↓

Mart Models

        ↓

BI Dashboards
```

---

# Why Define Sources?

Without sources, analysts reference raw tables directly:

```sql
select *

from raw_customer_support_tickets
```

This creates problems:

- No documentation
- No lineage tracking
- No freshness monitoring
- Difficult maintenance

With dbt sources:

```sql
select *

from {{ source(
'customer_support',
'tickets'
) }}
```

dbt understands the origin of the data.

---

# Source vs Model

A common interview question:

## Source

Represents data that already exists.

Examples:

```
CSV files

Application databases

ERP systems

CRM systems

APIs
```

---

## Model

Represents transformations created by dbt.

Examples:

```
stg_customers

dim_customers

fact_orders
```

---

# Source Structure

Sources are usually defined inside:

```
models/
```

Example:

```
models/

├── staging/

│
├── sources.yml

└── stg_customers.sql
```

---

# Defining a Source

Example:

```yaml
version: 2

sources:

  - name: customer_support

    description:

      "Raw customer support data"


    tables:

      - name: tickets

        description:

          "Raw customer support ticket records"
```

---

# Understanding Source Configuration

Example:

```yaml
sources:

  - name: customer_support
```

Creates a source namespace:

```
customer_support
```

---

```yaml
tables:

  - name: tickets
```

Defines the raw table:

```
tickets
```

---

Usage:

```sql
{{ source(
'customer_support',
'tickets'
) }}
```

---

# Adding Database and Schema

In a data warehouse:

```yaml
sources:

  - name: salesforce

    database: production

    schema: crm

    tables:

      - name: customers
```

dbt generates:

```
production.crm.customers
```

---

# Source Freshness

One major advantage of sources is monitoring whether data is arriving on time.

Example:

```yaml
sources:

- name: customer_support

  freshness:

    warn_after:

      count: 12

      period: hour


    error_after:

      count: 24

      period: hour
```

Meaning:

Warning:

```
No update for 12 hours
```

Error:

```
No update for 24 hours
```

---

# Running Freshness Checks

Command:

```bash
dbt source freshness
```

Example output:

```
PASS customer_support.tickets

Last loaded:

2 hours ago
```

---

# Source Tests

Sources can have tests.

Example:

```yaml
sources:

- name: customer_support

  tables:

  - name: tickets

    columns:

    - name: ticket_id

      tests:

      - unique

      - not_null
```

---

# Source Documentation

Sources appear automatically in dbt documentation.

Generate:

```bash
dbt docs generate
```

View:

```bash
dbt docs serve
```

Documentation shows:

```
Raw Source

     ↓

Staging Model

     ↓

Intermediate Model

     ↓

Mart Model
```

---

# Source Naming Best Practices

## Use Business Names

Good:

```yaml
customer_support
```

Bad:

```yaml
table1
```

---

## Keep Raw Names

Avoid renaming raw systems unnecessarily.

Example:

Good:

```
salesforce_customers
```

Bad:

```
customer_data_final
```

---

# Example: Customer Support Analytics Project

Raw CSV:

```
customer_support_tickets.csv
```

Loaded into DuckDB:

```
raw.customer_support_tickets
```

Source definition:

```yaml
sources:

- name: raw

  tables:

  - name: customer_support_tickets
```

Staging model:

```sql
select *

from {{ source(
'raw',
'customer_support_tickets'
) }}
```

Flow:

```
customer_support_tickets

          ↓

stg_customer_support_tickets

          ↓

fact_ticket_metrics

          ↓

Power BI Dashboard
```

---

# Source vs Seed

Another common interview question.

## Source

External data already stored somewhere.

Examples:

```
Database tables

APIs

Application data
```

---

## Seed

Small static datasets stored inside the dbt project.

Examples:

```
country_codes.csv

currency_codes.csv
```

---

# Source vs Ref

## source()

Used for raw data.

Example:

```sql
{{ source(
'raw',
'customers'
) }}
```

---

## ref()

Used for dbt models.

Example:

```sql
{{ ref(
'dim_customers'
) }}
```

---

# Source Lineage Example

```
CRM Database

      ↓

source.crm.customers

      ↓

stg_customers

      ↓

dim_customers

      ↓

Customer Dashboard
```

---

# Common Source Mistakes

## 1. Referencing Raw Tables Directly

Bad:

```sql
from raw.customers
```

Better:

```sql
from {{ source(
'crm',
'customers'
) }}
```

---

## 2. No Freshness Checks

Without freshness:

```
Dashboard may show outdated data
```

---

## 3. Missing Documentation

Undocumented sources create confusion.

---

# Interview Questions

## What is a dbt source?

A source represents raw data loaded into a warehouse before dbt transformations.

---

## Why use sources instead of direct table references?

Sources provide documentation, lineage, testing, and freshness monitoring.

---

## What is the difference between source() and ref()?

`source()` references raw external tables.

`ref()` references dbt-created models.

---

## How do you check if source data is updated?

Use:

```bash
dbt source freshness
```

---

# Key Takeaway

Sources create a reliable connection between raw data systems and dbt transformations.

They provide:

✅ Data lineage  
✅ Documentation  
✅ Freshness monitoring  
✅ Quality checks  
✅ Better governance  

A strong analytics engineer always knows where data originates before transforming it.