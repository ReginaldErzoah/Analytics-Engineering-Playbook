# Dimensional Modeling

## Overview

Dimensional modeling is a data modeling approach designed specifically for analytics and business intelligence systems.

It organizes data into structures that make reporting:

- Faster
- Easier to understand
- More reliable
- More efficient for analytical queries

Dimensional modeling is widely used in:

- Data warehouses
- Analytics platforms
- Business intelligence systems
- Reporting environments

In the SupportOps Intelligence Analytics project, dimensional modeling was used to transform customer support operational data into a star schema optimized for Power BI reporting.

---

# Why Dimensional Modeling Exists

Operational systems are designed for running businesses.

Examples:

- Customer management systems
- Ticketing systems
- ERP systems
- CRM platforms

These systems prioritize:

- Data entry
- Transaction processing
- Operational workflows

They are not optimized for analytics.

---

# Operational Database Structure

A transactional database may look like:

```

Customer Table

*

Ticket Table

*

Agent Table

*

Category Table

*

Channel Table

```

Each table may contain repeated information.

Example:

```

Ticket Table

Ticket ID
Customer Name
Customer Email
Agent Name
Category
Priority
Resolution Time

```

Problems:

- Duplicate information
- Difficult reporting
- Slow analytical queries

---

# Analytical Database Structure

Dimensional modeling restructures data into:

```

```
             Dimensions

                 |

                 ↓

              Fact Table

                 |

                 ↓

             BI Reports
```

```

This creates an analytics-friendly structure.

---

# Core Concepts of Dimensional Modeling

The two major components are:

1. Fact Tables
2. Dimension Tables

---

# Fact Tables

## Definition

A fact table stores measurable business events.

It answers:

"What happened?"

Examples:

- Sales transactions
- Customer purchases
- Support tickets
- Website visits

---

# Fact Table Characteristics

Fact tables usually contain:

## Measures

Numeric values that can be aggregated.

Examples:

```

revenue

quantity

resolution_hours

satisfaction_score

```

---

## Foreign Keys

References to dimensions.

Examples:

```

customer_key

agent_key

category_key

priority_key

```

---

## Business Event Identifier

Example:

```

ticket_id

order_id

transaction_id

```

---

# SupportOps Fact Table Example

The project created:

```

fact_ticket

```

Purpose:

Store every support ticket event.

Structure:

```

fact_ticket

|

├── ticket_id

├── customer_key

├── agent_key

├── category_key

├── channel_key

├── priority_key

├── resolution_time_hours

├── satisfaction_score

└── SLA status

```

---

# Dimension Tables

## Definition

Dimension tables store descriptive information about business entities.

They answer:

"Who?"

"What?"

"Where?"

"When?"

---

# Examples of Dimensions

## Customer Dimension

```

dim_customer

```

Contains:

```

customer_key

customer_name

email

location

```

---

## Agent Dimension

```

dim_agent

```

Contains:

```

agent_key

agent_name

team

```

---

## Category Dimension

```

dim_category

```

Contains:

```

category_key

category_name

```

---

## Channel Dimension

```

dim_channel

```

Contains:

```

channel_key

channel_name

```

---

## Priority Dimension

```

dim_priority

```

Contains:

```

priority_key

priority_level

```

---

# Star Schema

The most common dimensional modeling design is the star schema.

Structure:

```

```
                dim_customer


                     |

                     |
```

dim_agent -------- fact_ticket -------- dim_category

```
                     |


                     |


              dim_priority


                     |


                     |


               dim_channel
```

```

---

# Why Is It Called Star Schema?

Because the structure visually resembles a star:

```

```
      Dimension

          |
```

Dimension -- Fact -- Dimension

```
          |

      Dimension
```

````

The fact table sits in the center.

Dimensions surround it.

---

# Benefits of Star Schema

## 1. Simple Queries

Example:

Question:

"How many tickets came from email?"

Query:

```sql
SELECT

channel_name,

COUNT(*)

FROM fact_ticket

JOIN dim_channel

GROUP BY channel_name;
````

---

## 2. Better BI Performance

Power BI performs well with:

* Fact tables
* Dimensions
* Clear relationships

---

## 3. Easier Business Understanding

Business users understand:

```
Customer

Agent

Category

Ticket
```

better than normalized database structures.

---

# Grain in Dimensional Modeling

## Definition

Grain defines exactly what one row represents.

It is the most important design decision.

---

# Example

Fact table:

```
fact_ticket
```

Grain:

```
One row represents one support ticket.
```

Therefore:

```
100,000 rows

=

100,000 tickets
```

---

# Why Grain Matters

Incorrect grain causes:

* Duplicate metrics
* Wrong calculations
* Reporting errors

Example:

Bad design:

```
One row = ticket + customer + agent interaction
```

Better:

```
One row = one ticket event
```

---

# Measures in Fact Tables

Measures should be:

* Numeric
* Additive where possible
* Meaningful for analysis

---

# Types of Measures

## Additive Measures

Can be summed across all dimensions.

Example:

```
ticket_count
```

---

## Semi-Additive Measures

Can be summed across some dimensions.

Example:

```
account_balance
```

---

## Non-Additive Measures

Cannot be summed.

Example:

```
customer_satisfaction_score
```

Usually aggregated using:

* Average
* Median

---

# Keys in Dimensional Modeling

## Natural Keys

Original identifiers from source systems.

Examples:

```
customer_id

ticket_id
```

---

## Surrogate Keys

Artificial keys created for the warehouse.

Examples:

```
customer_key

agent_key
```

---

# Why Use Surrogate Keys?

Advantages:

* Avoid dependency on source systems
* Handle historical changes
* Improve warehouse consistency

---

# Example

Source:

```
customer_id = 12345
```

Warehouse:

```
customer_key = 987
```

The warehouse uses:

```
customer_key
```

for relationships.

---

# Slowly Changing Dimensions

Dimension data changes over time.

Example:

Customer:

```
Location = Accra
```

Later:

```
Location = Kumasi
```

How should history be handled?

This is solved using Slowly Changing Dimensions.

---

# Types of Slowly Changing Dimensions

## Type 1

Overwrite old data.

Example:

Before:

```
Location = Accra
```

After:

```
Location = Kumasi
```

Old value disappears.

Used when:

* History does not matter

---

## Type 2

Create a new record.

Example:

```
Customer Key | Location | Start Date

001           Accra      2025

002           Kumasi     2026
```

Preserves history.

Most common in warehouses.

---

## Type 3

Store previous value.

Example:

```
Current Location

Previous Location
```

Less common.

---

# Dimensional Modeling In SupportOps Intelligence

The project followed this architecture:

```
                 dim_customer

                       |

                       |

dim_agent ---- fact_ticket ---- dim_category

                       |

                       |

              dim_priority

                       |

                       |

                dim_channel
```

---

# Power BI Relationship Design

The BI model used:

```
One-to-Many Relationships
```

Example:

```
dim_customer

       1

       |

       |

       *

fact_ticket
```

Meaning:

One customer can have many tickets.

---

# Dimensional Modeling Best Practices

## 1. Define Grain First

Before creating tables:

Ask:

"What does one row represent?"

---

## 2. Keep Facts Numeric

Avoid storing descriptions inside facts.

Bad:

```
fact_ticket

customer_name

agent_name

category_name
```

Better:

```
fact_ticket

customer_key

agent_key

category_key
```

---

## 3. Keep Dimensions Descriptive

Dimensions should contain:

* Names
* Categories
* Attributes

---

## 4. Avoid Excessive Normalization

Analytics databases prioritize usability.

Do not design like operational systems.

---

# Skills Required

## SQL

Learn:

* Joins
* Aggregations
* Window functions
* Query optimization

---

## Data Warehousing

Learn:

* Star schemas
* Snowflake schemas
* Fact tables
* Dimension tables

---

## Business Intelligence

Learn:

* Reporting requirements
* KPI design
* Dashboard modeling

---

## dbt

Learn:

* Building dimensional models
* Creating marts
* Testing relationships

---

# Resources

## Books

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Focus:

* Dimensional modeling
* Fact tables
* Dimensions
* Data warehouses

### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Focus:

* Modern data platforms
* Analytics engineering

---

## Courses

Kimball Group Training:

[https://www.kimballgroup.com/](https://www.kimballgroup.com/)

Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

## Documentation

dbt Dimensional Modeling:

[https://docs.getdbt.com/](https://docs.getdbt.com/)

---

# Summary

Dimensional modeling is the foundation of analytical data systems.

The approach separates:

```
Facts

+

Dimensions
```

to create simple, scalable, and high-performing analytics solutions.

The SupportOps Intelligence Analytics project applied dimensional modeling through a star schema containing:

* fact_ticket
* dim_customer
* dim_agent
* dim_category
* dim_channel
* dim_priority

This design allowed Power BI to efficiently analyze customer support performance and operational KPIs.