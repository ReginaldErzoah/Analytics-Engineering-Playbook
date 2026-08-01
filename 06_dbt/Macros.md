# dbt Macros

## Overview

Macros are reusable pieces of SQL logic written using **Jinja templating**.

They allow analytics engineers to avoid repeating SQL code and create reusable transformation functions.

A simple explanation:

```
Repeated SQL Logic

        ↓

Create Macro

        ↓

Reuse Anywhere
```

---

# Why Use Macros?

Without macros, teams often repeat the same logic across multiple models.

Example:

Model 1:

```sql
lower(trim(customer_email))
```

Model 2:

```sql
lower(trim(customer_email))
```

Model 3:

```sql
lower(trim(customer_email))
```

Problems:

- Duplicate code
- Difficult maintenance
- Higher chance of inconsistent logic

---

With a macro:

```
clean_email()
```

Used everywhere:

```sql
{{ clean_email('customer_email') }}
```

---

# What Are Macros Built With?

dbt macros use:

- Jinja
- SQL
- Python-like templating syntax

Example:

```sql
{% macro macro_name() %}

SQL logic

{% endmacro %}
```

---

# Macro Structure

Basic macro:

```sql
{% macro greeting(name) %}

Hello {{ name }}

{% endmacro %}
```

Components:

## macro keyword

Defines a macro.

```jinja
{% macro %}
```

---

## Macro name

The reusable function name.

Example:

```
clean_email
```

---

## Arguments

Inputs passed into the macro.

Example:

```
column_name
```

---

## Macro body

The SQL logic.

---

# Creating a Macro

Macros are stored inside:

```
macros/
```

Example:

```
macros/

    clean_columns.sql
```

---

Example:

```sql
{% macro clean_string(column_name) %}

trim(lower({{ column_name }}))

{% endmacro %}
```

---

# Using a Macro

Model:

```sql
select

{{ clean_string(
'customer_email'
) }} as customer_email

from customers
```

Generated SQL:

```sql
select

trim(lower(customer_email))

as customer_email

from customers
```

---

# Common Macro Use Cases

---

# 1. Standardizing Data Cleaning

Example:

Cleaning emails:

```sql
{% macro clean_email(column) %}

lower(trim({{ column }}))

{% endmacro %}
```

Usage:

```sql
{{ clean_email('email') }}
```

Result:

```
john@email.com
```

---

# 2. Date Logic

Example:

Creating month start dates.

Macro:

```sql
{% macro month_start(column) %}

date_trunc(
'month',
{{ column }}
)

{% endmacro %}
```

Usage:

```sql
{{ month_start('order_date') }}
```

---

# 3. Reusable Business Rules

Example:

Customer classification:

```sql
{% macro customer_segment(amount) %}

case

when {{ amount }} > 1000

then 'High Value'

else 'Standard'

end

{% endmacro %}
```

Usage:

```sql
{{ customer_segment(
'total_spend'
) }}
```

---

# 4. Generating Surrogate Keys

A common analytics engineering task.

Instead of:

```sql
md5(
customer_id || email
)
```

Use:

```sql
{{ dbt_utils.generate_surrogate_key(
[
'customer_id',
'email'
]
) }}
```

---

# dbt-utils Macros

The dbt ecosystem provides reusable macros through packages.

Popular package:

```
dbt_utils
```

Install:

```
packages.yml
```

Example:

```yaml
packages:

- package: dbt-labs/dbt_utils

  version: 1.1.1
```

Install:

```bash
dbt deps
```

---

# Common dbt_utils Macros

## generate_surrogate_key()

Creates unique identifiers.

Example:

```sql
{{ dbt_utils.generate_surrogate_key(
[
'customer_email',
'customer_name'
]
) }}
```

Used for:

```
customer_id

product_id

order_key
```

---

## date_spine()

Creates date calendars.

Example:

```
2024-01-01

2024-01-02

2024-01-03
```

Useful for:

- Time analysis
- Missing dates
- Reporting periods

---

## star()

Selects columns automatically.

Example:

Instead of:

```sql
select

id,

name,

email,

country

from customers
```

Use:

```sql
{{ dbt_utils.star(
ref('customers')
) }}
```

---

# Macro vs Model

Common interview question.

## Model

A SQL transformation that creates a dataset.

Example:

```
fact_sales.sql
```

Produces:

```
fact_sales table
```

---

## Macro

Reusable SQL logic.

Example:

```
clean_email()
```

Produces:

```
SQL expression
```

---

# Macro vs Function

They are similar concepts.

Programming:

```
function()
```

dbt:

```
macro()
```

Both:

- Accept inputs
- Execute logic
- Return output

---

# Macro Best Practices

## 1. Keep Macros Simple

Good:

```
Reusable transformations
```

Avoid:

```
Complex business pipelines
```

---

## 2. Give Clear Names

Good:

```
clean_email

generate_month_start
```

Bad:

```
helper1

function_test
```

---

## 3. Document Macros

Explain:

- Purpose
- Inputs
- Output

Example:

```sql
-- Cleans text fields by trimming spaces
-- and converting values to lowercase
```

---

## 4. Avoid Overusing Macros

Not every SQL expression needs a macro.

Bad:

Creating:

```
add_one()
```

for:

```sql
column + 1
```

---

# Example: Customer Support Analytics Project

A macro could standardize ticket categories.

Macro:

```
macros/normalize_priority.sql
```

Logic:

```sql
case

when priority in ('High','HIGH')

then 'High'

when priority in ('Low','LOW')

then 'Low'

end
```

Used in:

```
stg_customer_support_tickets
```

Result:

```
High

Medium

Low
```

---

# Debugging Macros

Useful commands:

Compile SQL:

```bash
dbt compile
```

View generated SQL:

```
target/compiled/
```

---

# Interview Questions

## What is a dbt macro?

A reusable block of SQL logic created using Jinja.

---

## Why use macros?

To reduce duplicated SQL and standardize transformations.

---

## Where are macros stored?

Inside:

```
macros/
```

---

## What package provides common dbt macros?

```
dbt_utils
```

---

## What is the difference between a macro and a model?

A model creates a dataset; a macro creates reusable SQL logic.

---

# Key Takeaway

Macros make dbt projects cleaner, more maintainable, and easier to scale.

They help analytics engineers create reusable building blocks instead of repeating SQL everywhere.

A strong analytics engineer knows when to:

✅ Create a macro  
✅ Reuse existing packages  
✅ Keep transformations modular  
✅ Avoid unnecessary complexity