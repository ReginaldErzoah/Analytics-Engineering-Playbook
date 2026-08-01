# Data Marts

## Overview

A data mart is a smaller, focused subset of a data warehouse designed to support a specific business function, department, or analytical purpose.

While a data warehouse serves the entire organization, a data mart provides targeted datasets that are easier for specific teams to understand and use.

Examples:

```
Sales Data Mart

Finance Data Mart

Marketing Data Mart

Customer Support Data Mart

Operations Data Mart
```

---

# Data Warehouse vs Data Mart

|Category|Data Warehouse|Data Mart|
|-|-|-|
|Scope|Enterprise-wide|Department-specific|
|Users|Entire organization|Specific teams|
|Size|Large|Smaller|
|Purpose|Central data repository|Focused analytics|
|Complexity|Higher|Lower|
|Examples|Company-wide analytics|Sales reporting|

---

# Why Organizations Use Data Marts

## 1. Simplified Analytics

A complete warehouse may contain hundreds of tables.

A data mart provides only the required data.

Example:

Customer Support team may only need:

```
Tickets

Customers

Products

Agents

Resolution Metrics
```

They do not need:

```
Payroll

Inventory

Finance Transactions
```

---

## 2. Faster Query Performance

Because data marts contain focused datasets, queries are often faster.

Example:

Instead of querying:

```
Enterprise Warehouse

10 billion rows
```

an analyst queries:

```
Customer Support Mart

50 million rows
```

---

## 3. Better User Experience

Business users can work with datasets that match their domain.

Example:

Finance users understand:

```
Revenue

Costs

Margins
```

Customer support users understand:

```
Tickets

Resolution Time

Customer Satisfaction
```

---

# Types of Data Marts

There are three common approaches.

---

# 1. Dependent Data Mart

A dependent data mart is created from an existing enterprise data warehouse.

Architecture:

```
Source Systems

      ↓

Enterprise Data Warehouse

      ↓

Department Data Mart

      ↓

BI Reports
```

Advantages:

- Consistent data definitions
- Centralized governance
- Better data quality

Disadvantages:

- Requires warehouse investment

---

# 2. Independent Data Mart

An independent data mart is built directly from source systems.

Architecture:

```
Source Systems

      ↓

Department Data Mart

      ↓

Reports
```

Advantages:

- Faster implementation
- Useful for small teams

Disadvantages:

- Risk of inconsistent metrics
- Duplicate logic

---

# 3. Hybrid Data Mart

Combines warehouse and direct source data.

Example:

```
Warehouse Data

+

External Data

        ↓

Hybrid Data Mart
```

Used when organizations need flexibility.

---

# Data Mart Architecture

Modern analytics architecture:

```
                  Source Systems

                        |

                        ↓

                 Data Warehouse

                        |

                        ↓

              Business Data Marts

                        |

        ┌───────────────┼───────────────┐

        ↓               ↓               ↓

    Sales Mart     Finance Mart    Support Mart

        |               |               |

        ↓               ↓               ↓

      BI             BI              BI

```

---

# Examples of Data Marts

## Sales Data Mart

Used by sales teams.

Contains:

```
fact_sales

dim_customer

dim_product

dim_salesperson
```

Metrics:

- Revenue
- Sales growth
- Conversion rate
- Customer acquisition

---

# Finance Data Mart

Used by finance teams.

Contains:

```
fact_transactions

dim_account

dim_department

dim_date
```

Metrics:

- Expenses
- Budget variance
- Profit margins

---

# Customer Support Data Mart

Used by customer experience teams.

Contains:

```
fact_ticket_metrics

dim_customers

dim_products
```

Metrics:

- Ticket volume
- Resolution time
- First response time
- Customer satisfaction score
- Support channel performance

---

# Data Marts in dbt

In analytics engineering, data marts are usually represented by the final business models.

Example dbt project:

```
models/

    staging/

        stg_customer_support_tickets.sql


    intermediate/

        int_ticket_performance.sql


    marts/

        fact_ticket_metrics.sql

        dim_customers.sql

        dim_products.sql
```

The `marts` layer represents the business-ready datasets.

---

# Characteristics of Good Data Marts

## Business-Focused

Models should answer specific business questions.

Example:

Bad:

```
all_database_tables
```

Good:

```
customer_support_performance
```

---

## Well Documented

Each table should explain:

- Purpose
- Owner
- Columns
- Metrics

Example:

```
fact_ticket_metrics

Purpose:
Tracks customer support ticket performance.
```

---

## Tested

Important tests:

```
unique keys

not null fields

valid values

relationships
```

Example:

```yaml
tests:

- unique

- not_null
```

---

## Consistent Metrics

Business calculations should be standardized.

Example:

Resolution Hours should have one definition.

Not:

```
Team A:
Closed time - Created time

Team B:
Solved time - First response time
```

---

# Data Mart Design Process

## Step 1: Identify Business Need

Example:

Customer Support wants to improve response times.

---

## Step 2: Define Metrics

Required metrics:

```
Average resolution time

Average first response time

Ticket volume

Customer satisfaction
```

---

## Step 3: Identify Required Data

Sources:

```
Support tickets

Customer information

Product data
```

---

## Step 4: Build Models

Example:

```
stg_customer_support_tickets

        ↓

int_ticket_performance

        ↓

fact_ticket_metrics
```

---

## Step 5: Connect BI Tools

Example:

```
fact_ticket_metrics

        ↓

Power BI Dashboard
```

---

# Data Mart Best Practices

## Avoid Creating Too Many Data Marts

Too many marts create:

- Duplicate logic
- Maintenance issues
- Conflicting metrics

---

## Reuse Existing Models

Prefer:

```
Existing warehouse models

        ↓

Multiple marts
```

instead of:

```
Every team builds separate pipelines
```

---

## Keep Business Logic Close to Data

Define metrics in transformation layers.

Example:

dbt model:

```
resolution_hours
```

rather than rebuilding calculations in every dashboard.

---

# Interview Questions

## What is a data mart?

A focused subset of a data warehouse designed for a specific business function.

---

## Why use data marts?

To simplify analytics, improve performance, and provide business-focused datasets.

---

## What is the difference between a warehouse and a mart?

A warehouse stores enterprise-wide data, while a mart serves a specific department.

---

## Are dbt marts the same as database marts?

Conceptually yes. In modern analytics engineering, the final dbt models often represent business data marts.

---

# Key Takeaway

Data marts bridge the gap between enterprise data platforms and business users.

They transform complex warehouse structures into simple, trusted datasets that allow teams to answer important business questions quickly.

A strong analytics engineer understands how to design marts that are:

✅ Business-focused  
✅ Reusable  
✅ Tested  
✅ Documented  
✅ Trusted by stakeholders