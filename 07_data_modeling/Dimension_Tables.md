# Dimension Tables

## Overview

Dimension tables are descriptive tables used in analytical data models to provide context around business events.

While fact tables answer:

> "How much happened?"

Dimension tables answer:

> "Who, what, where, when, and how did it happen?"

Dimension tables contain attributes that allow users to filter, group, and analyze metrics.

Examples:

- Customers
- Products
- Employees
- Locations
- Dates
- Channels

---

# Fact Tables vs Dimension Tables

|Fact Tables|Dimension Tables|
|-|-|
|Store measurements|Store descriptions|
|Usually large|Usually smaller|
|Contain metrics|Contain attributes|
|Frequently updated with events|Change less frequently|
|Used for calculations|Used for filtering and grouping|

Example:

Customer Support Analytics:

```
                 dim_customer

                       |

                       |

              fact_ticket_metrics

                       |

                       |

                 dim_product
```

---

# Purpose of Dimension Tables

Dimension tables improve analytics by providing:

## 1. Business-Friendly Data

Raw systems often contain technical fields.

Example:

Raw:

```
customer_type_code = C03
```

Dimension:

```
customer_segment = Premium Customer
```

---

## 2. Reusable Attributes

Instead of storing customer details repeatedly:

```
Ticket 1
Customer Name
Customer Email

Ticket 2
Customer Name
Customer Email

Ticket 3
Customer Name
Customer Email
```

Create:

```
dim_customer
```

and reference it.

Benefits:

- Less duplication
- Better consistency
- Easier maintenance

---

## 3. Better Dashboard Performance

BI tools perform better with properly modeled dimensions.

Example:

Power BI filtering:

```
Customer Segment

        ↓

dim_customer

        ↓

fact_sales
```

---

# Common Dimension Types

Common dimensions include:

1. Customer Dimension
2. Product Dimension
3. Date Dimension
4. Location Dimension
5. Employee Dimension
6. Channel Dimension

---

# Customer Dimension

A customer dimension stores customer-related information.

Example:

```
dim_customers
```

Structure:

|Column|Description|
|-|-|
|customer_id|Surrogate key|
|customer_name|Customer name|
|customer_email|Email address|
|customer_gender|Gender|
|customer_age|Age group|

---

## Customer Dimension Usage

Allows analysis:

- Tickets by customer
- Purchases by customer
- Customer lifetime value
- Customer segmentation

Example query:

```sql
SELECT

customer_name,

COUNT(ticket_id)

FROM fact_ticket_metrics

JOIN dim_customers

ON fact_ticket_metrics.customer_id =
dim_customers.customer_id

GROUP BY customer_name;
```

---

# Product Dimension

A product dimension stores information about products.

Example:

```
dim_products
```

Structure:

|Column|Description|
|-|-|
|product_id|Product key|
|product_name|Product name|
|category|Product category|
|brand|Product brand|

---

## Product Dimension Usage

Supports:

- Product performance
- Sales analysis
- Support issue analysis

Customer support example:

Question:

> Which products generate the highest number of support tickets?

Requires:

```
fact_ticket_metrics

+

dim_products
```

---

# Date Dimension

A date dimension is one of the most important dimensions in analytics.

Example:

```
dim_date
```

Structure:

|Column|Description|
|-|-|
|date_id|Date key|
|date|Calendar date|
|month|Month name|
|quarter|Quarter|
|year|Year|

---

# Why Use Date Dimensions?

Instead of:

```sql
YEAR(created_date)
```

everywhere,

use:

```
dim_date.year
```

Benefits:

- Consistent reporting
- Easier time intelligence
- Better BI performance

---

# Date Dimension Example

```
date_id

20260101

date

2026-01-01

month

January

quarter

Q1

year

2026
```

---

# Location Dimension

Stores geographic information.

Example:

```
dim_location
```

Columns:

```
location_id

country

region

city
```

Used for:

- Regional analysis
- Market performance
- Operations reporting

---

# Role-Playing Dimensions

A dimension can be used multiple times in a fact table.

Example:

Date dimension:

```
dim_date
```

Connected to:

```
created_date

resolved_date

closed_date
```

Same dimension, different business meaning.

---

# Degenerate Dimensions

A degenerate dimension is an identifier stored directly in a fact table.

Example:

```
fact_ticket_metrics

ticket_id

customer_id

resolution_hours
```

Why?

Because:

```
ticket_id
```

has no additional attributes.

Creating:

```
dim_ticket
```

would add unnecessary complexity.

---

# Junk Dimensions

A junk dimension combines low-cardinality attributes.

Example:

Ticket attributes:

```
priority

status

channel
```

Instead of:

```
priority_dimension

status_dimension

channel_dimension
```

combine:

```
dim_ticket_attributes
```

Useful when attributes:

- Have few possible values
- Are frequently analyzed together

---

# Slowly Changing Dimensions (SCD)

Dimensions change over time.

Example:

Customer moves:

```
Old Address

Accra

        ↓

New Address

Kumasi
```

The warehouse must decide:

- Keep history?
- Replace old value?
- Track both?

Handled using SCD strategies.

Covered in:

```
SCD_Types.md
```

---

# Surrogate Keys in Dimensions

Dimensions should usually use surrogate keys.

Example:

Source:

```
customer_email
```

Warehouse:

```
customer_id
```

Example dbt:

```sql
{{ dbt_utils.generate_surrogate_key(
[
'customer_email'
]
) }}
```

Benefits:

- Stable relationships
- Handles changing attributes
- Independent from source systems

---

# Dimension Table Design Best Practices

## 1. Keep Dimensions Descriptive

Good:

```
customer_name

customer_segment

region
```

Avoid:

```
total_sales

ticket_count
```

Those belong in facts.

---

## 2. Use Clear Naming

Recommended:

```
dim_customer

dim_product

dim_date
```

Avoid:

```
customer_table_final_new
```

---

## 3. Add Documentation

Document:

- Column meaning
- Business definitions
- Data sources

Example:

```
customer_segment:

A business-defined classification
based on customer activity.
```

---

## 4. Test Dimension Quality

Common tests:

### Unique Keys

```yaml
tests:

- unique
```

---

### Required Fields

```yaml
tests:

- not_null
```

---

### Valid Values

```yaml
tests:

- accepted_values
```

---

# Dimensions in dbt

Typical structure:

```
models/

marts/

    dim_customers.sql

    dim_products.sql

    dim_date.sql
```

Example:

```sql
SELECT

{{ dbt_utils.generate_surrogate_key(
['customer_email']
) }}

AS customer_id,

customer_email,

customer_name

FROM staging_customers
```

---

# Common Interview Questions

## What is a dimension table?

A table containing descriptive attributes used to analyze facts.

---

## Why separate dimensions from facts?

To reduce duplication and create reusable analytical structures.

---

## What is a surrogate key?

A warehouse-generated identifier used instead of source system keys.

---

## What is a slowly changing dimension?

A method for managing changes in dimension attributes over time.

---

## Why is the date dimension important?

It enables consistent time-based reporting and analysis.

---

# Key Takeaway

Dimension tables provide the context required to understand business metrics.

A strong Analytics Engineer designs dimensions that are:

✅ Consistent  
✅ Reusable  
✅ Well documented  
✅ Easy to query  
✅ Compatible with BI tools  

Dimensions make analytical data understandable for both technical teams and business users.