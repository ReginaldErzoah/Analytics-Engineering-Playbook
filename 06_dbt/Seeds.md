# dbt Seeds

## Overview

Seeds are CSV files stored inside a dbt project that dbt can load into the database as tables.

They are mainly used for **small, static reference datasets** that do not change frequently.

Example workflow:

```
CSV File

    ↓

dbt seed

    ↓

Database Table

    ↓

Used in Models
```

---

# Why Use Seeds?

Many analytics projects require small lookup tables.

Examples:

- Country codes
- Currency mappings
- Product categories
- Customer segments
- Status mappings

Instead of manually creating these tables in the database, dbt seeds allow them to be version-controlled with the project.

---

# Where Are Seeds Stored?

Seeds are stored in:

```
seeds/
```

Example:

```
analytics_project/

├── models/

├── macros/

├── tests/

└── seeds/

    └── country_codes.csv
```

---

# Example Seed File

File:

```
seeds/country_codes.csv
```

Content:

```csv
country_code,country_name

GH,Ghana

US,United States

GB,United Kingdom

DE,Germany
```

---

# Loading Seeds

Command:

```bash
dbt seed
```

dbt creates a table:

```
country_codes
```

inside the target database.

---

# Using Seeds in Models

After loading:

```sql
select *

from {{ ref(
'country_codes'
) }}
```

dbt treats seeds like models.

---

# Seed Configuration

Seeds can be configured in:

```
dbt_project.yml
```

Example:

```yaml
seeds:

  analytics_project:

    +schema: reference_data
```

Result:

```
reference_data.country_codes
```

---

# Seed Data Types

dbt automatically detects column types.

Example:

CSV:

```csv
product_id,price

101,25.50
```

dbt creates:

```
product_id → integer

price → decimal
```

---

# Changing Column Types

Sometimes automatic detection is incorrect.

Example:

A postal code:

```
00125
```

may become:

```
125
```

because dbt thinks it is numeric.

Solution:

Configure the column type:

```yaml
seeds:

 analytics_project:

  country_codes:

   +column_types:

    postal_code: varchar
```

---

# Seed Testing

Seeds can have tests like models.

Example:

schema.yml:

```yaml
version: 2

seeds:

- name: country_codes

  columns:

  - name: country_code

    tests:

    - unique

    - not_null
```

---

# Seeds vs Sources

Common interview question.

## Seed

Data managed inside the dbt project.

Example:

```
country_codes.csv
```

Used for:

```
Reference data
Lookup tables
Mappings
```

---

## Source

Data that already exists outside dbt.

Examples:

```
CRM database

ERP system

Application database
```

---

# Seeds vs Models

## Seed

Input data.

Example:

```
currency_codes.csv
```

---

## Model

Transformation logic.

Example:

```
dim_customers.sql
```

---

# When Should You Use Seeds?

Good use cases:

## Small Reference Tables

Example:

```
country_codes

currency_codes
```

---

## Business Mapping Tables

Example:

Raw:

```
priority_code

1

2

3
```

Seed:

```
priority_code,priority_name

1,Low

2,Medium

3,High
```

---

## Static Configuration Data

Example:

```
support_categories.csv
```

---

# When NOT to Use Seeds

Avoid seeds for:

## Large Datasets

Bad:

```
10 million customer records
```

Use:

```
warehouse loading tools
```

---

## Frequently Changing Data

Bad:

```
daily sales transactions
```

Use:

```
ETL/ELT pipeline
```

---

## Operational Databases

Do not use seeds as replacements for:

- CRM systems
- ERP systems
- Application databases

---

# Example: Customer Support Analytics Project

A customer support dashboard may need:

```
ticket_priority_mapping.csv
```

Seed:

```csv
priority_code,priority_level

1,Low

2,Medium

3,High
```

Loaded:

```
dbt seed
```

Used:

```
fact_ticket_metrics
```

Transformation:

```
Raw Priority Code

        ↓

Seed Mapping Table

        ↓

Business Friendly Priority
```

---

# Seed Best Practices

## 1. Keep Seeds Small

Recommended:

```
Hundreds or thousands of rows
```

Not:

```
Millions of rows
```

---

## 2. Document Seeds

Explain:

- Purpose
- Owner
- Update frequency

---

## 3. Version Control Seeds

Because seeds live inside Git:

```
Change history is tracked
```

---

## 4. Test Important Columns

Example:

Primary identifiers:

```
unique

not_null
```

---

# Common Seed Commands

Load seeds:

```bash
dbt seed
```

Load one seed:

```bash
dbt seed --select country_codes
```

Run tests:

```bash
dbt test
```

Refresh seed:

```bash
dbt seed --full-refresh
```

---

# Interview Questions

## What is a dbt seed?

A CSV file stored in a dbt project that dbt loads into the database as a table.

---

## When would you use seeds?

For small static reference datasets.

---

## What is the difference between seeds and sources?

Seeds are managed by dbt; sources are external raw data systems.

---

## Can seeds be tested?

Yes. Seeds support dbt tests and documentation.

---

## Should you store customer transaction data as seeds?

No. Seeds are not designed for large or frequently changing datasets.

---

# Key Takeaway

Seeds provide a simple way to manage small reference datasets inside an analytics engineering workflow.

They provide:

✅ Version-controlled reference data  
✅ Easy deployment  
✅ Documentation  
✅ Testing support  

Use seeds for stable lookup tables, not production-scale data pipelines.