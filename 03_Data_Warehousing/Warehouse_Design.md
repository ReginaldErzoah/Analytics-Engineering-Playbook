# Data Warehouse Design

## Overview

Data warehouse design is the process of structuring analytical data so that it is:

- Easy to understand
- Efficient to query
- Reliable for reporting
- Scalable as data grows

A well-designed warehouse allows analysts, analytics engineers, and business teams to answer questions quickly and consistently.

---

# Goals of Data Warehouse Design

A good warehouse design should provide:

## 1. Performance

Analytical queries should execute efficiently.

Example:

A dashboard showing:

```
Monthly revenue trends
```

should load quickly even with millions of records.

---

## 2. Data Consistency

Different teams should use the same definitions.

Example:

Without standardization:

Team A:

```
Active Customer = Purchased within 30 days
```

Team B:

```
Active Customer = Logged in within 90 days
```

A warehouse creates trusted business definitions.

---

## 3. Scalability

The design should support:

- More users
- More data
- More business processes

---

## 4. Maintainability

Models should be:

- Documented
- Tested
- Modular
- Easy to update

---

# Data Warehouse Layers

Modern warehouses commonly separate data into layers.

---

# 1. Raw Layer (Bronze)

The raw layer contains data exactly as received from source systems.

Example:

```
customer_support_tickets.csv
```

Characteristics:

- Minimal transformation
- Original source data preserved
- Used for auditing and reprocessing

Example:

```
raw_customer_support_tickets
```

---

# 2. Staging Layer (Silver)

The staging layer cleans and standardizes raw data.

Common transformations:

- Rename columns
- Fix data types
- Remove duplicates
- Standardize values

Example:

Raw:

```
Customer Email
```

Staging:

```
customer_email
```

dbt example:

```
stg_customer_support_tickets
```

---

# 3. Intermediate Layer

The intermediate layer contains business logic transformations.

Purpose:

- Complex calculations
- Reusable logic
- Data preparation

Example:

```
int_ticket_performance
```

Calculations:

```
resolution_hours

response_time_quality_flag

cleaned timestamps
```

---

# 4. Mart Layer (Gold)

The mart layer contains business-ready models.

Used by:

- Analysts
- BI tools
- Executives

Examples:

```
fact_ticket_metrics

dim_customers

dim_products
```

---

# Modern Warehouse Architecture

```
                 Sources

                    |

                    ↓

              Raw Layer

                    |

                    ↓

            Staging Models

                    |

                    ↓

          Intermediate Models

                    |

                    ↓

            Data Marts

                    |

                    ↓

             BI Dashboards

```

---

# Data Modeling Approaches

There are several ways to design analytical databases.

---

# 1. Star Schema

The most common BI design.

Structure:

```
              dim_customer

                    |

                    |

dim_product ---- fact_sales ---- dim_date

```

Characteristics:

- Simple structure
- Fast queries
- Easy for BI users

Used by:

- Power BI
- Tableau
- Looker

---

# 2. Snowflake Schema

A more normalized dimensional model.

Example:

```
dim_customer

      |

dim_location

      |

dim_country
```

Advantages:

- Reduces duplication

Disadvantages:

- More joins
- More complexity

---

# 3. Data Vault

A modeling approach designed for enterprise scalability.

Main components:

## Hubs

Business keys.

Example:

```
Customer Hub
```

---

## Links

Relationships.

Example:

```
Customer purchases Product
```

---

## Satellites

Historical attributes.

Example:

```
Customer Address History
```

Used in:

- Large enterprises
- Complex data environments

---

# Fact and Dimension Design

A warehouse usually separates:

## Facts

Contain measurements.

Examples:

```
sales_amount

quantity

resolution_hours
```

---

## Dimensions

Contain context.

Examples:

```
customer_name

product_category

date
```

---

# Choosing the Right Grain

One of the most important warehouse design decisions is defining the grain.

## Grain Definition

The grain describes what one row represents.

Example:

Fact sales grain:

```
One row per product sale transaction
```

Customer support grain:

```
One row per support ticket
```

---

# Why Grain Matters

Incorrect grain creates:

- Duplicate data
- Incorrect metrics
- Reporting errors

Example:

Bad design:

```
One row = customer + ticket + product + month
```

Good design:

```
One row = one support ticket
```

---

# Warehouse Naming Conventions

Recommended:

## Tables

Use clear prefixes:

```
stg_

int_

fact_

dim_
```

Examples:

```
stg_orders

int_customer_metrics

fact_sales

dim_product
```

---

## Columns

Use descriptive names:

Good:

```
customer_email
```

Avoid:

```
cust_em
```

---

# Data Warehouse Design Best Practices

## 1. Define Business Requirements First

Before creating models, understand:

- Business questions
- KPIs
- Users
- Reporting needs

---

## 2. Start With the Grain

Always define:

```
What does one row represent?
```

---

## 3. Build Modular Models

Avoid one massive SQL query.

Better:

```
staging

↓

intermediate

↓

marts
```

---

## 4. Add Data Quality Checks

Examples:

```
unique keys

not null fields

accepted values

relationships
```

---

## 5. Document Everything

Documentation should explain:

- Table purpose
- Column meanings
- Business definitions
- Dependencies

---

# Analytics Engineering Example

Customer Support Analytics architecture:

```
Raw CSV

customer_support_tickets.csv

        ↓

stg_customer_support_tickets

        ↓

int_ticket_performance

        ↓

        ┌───────────────┐
        │               │
        ↓               ↓

dim_customers     dim_products

        \          /

         \        /

       fact_ticket_metrics

                ↓

           Power BI Dashboard

```

---

# Interview Questions

## What is the most important step when designing a warehouse?

Defining the business process and grain.

---

## Why use layers in a warehouse?

To separate responsibilities and make transformations easier to maintain.

---

## What is a data mart?

A business-focused subset of a warehouse.

Example:

Customer Support Data Mart.

---

## Why create fact and dimension tables?

To organize measurements separately from descriptive information.

---

# Key Takeaway

A well-designed data warehouse turns raw operational data into reliable analytical assets.

The core principles are:

✅ Define business processes  
✅ Choose the correct grain  
✅ Separate facts and dimensions  
✅ Build modular transformation layers  
✅ Test and document everything  

Good warehouse design is the foundation of effective analytics engineering.