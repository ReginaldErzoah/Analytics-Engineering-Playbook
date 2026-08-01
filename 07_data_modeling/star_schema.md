# Star Schema

## Overview

A star schema is a dimensional data modeling design used in analytical databases and business intelligence systems.

It organizes data into:

- One central fact table
- Multiple surrounding dimension tables

The structure creates a simple, intuitive, and efficient model for analytics.

The SupportOps Intelligence Analytics project used a star schema to prepare customer support data for Power BI reporting.

---

# Star Schema Structure

A typical star schema looks like:

```

```
                Dimension

                    |

                    |
```

Dimension -------- Fact Table -------- Dimension

```
                    |

                    |

               Dimension
```

```

The fact table is the center of the model.

Dimension tables provide context.

---

# Why Use a Star Schema?

Traditional operational databases are designed for transactions.

Examples:

- Creating customer records
- Updating tickets
- Processing orders

Analytics systems have different goals:

- Summarizing data
- Finding trends
- Measuring performance
- Supporting decisions

Star schemas are designed specifically for these analytical needs.

---

# Components of a Star Schema

A star schema contains two major components:

1. Fact Tables
2. Dimension Tables

---

# Fact Tables

## Definition

A fact table stores measurable business events.

It represents activities that happen in a business process.

Examples:

- Sales transactions
- Website visits
- Support tickets
- Orders

---

# Fact Table Characteristics

Fact tables contain:

## 1. Foreign Keys

Links to dimensions.

Example:

```

customer_key

agent_key

category_key

priority_key

```

---

## 2. Measures

Numeric values used for analysis.

Examples:

```

resolution_time_hours

ticket_count

satisfaction_score

```

---

## 3. Event Identifier

A unique identifier for the business event.

Example:

```

ticket_id

```

---

# Dimension Tables

## Definition

Dimension tables contain descriptive information about business entities.

They provide context for facts.

Examples:

- Customers
- Employees
- Products
- Locations
- Dates

---

# Dimension Characteristics

Dimensions contain:

- Names
- Categories
- Descriptions
- Attributes

Example:

```

Customer Dimension

customer_key

customer_name

email

location

```

---

# SupportOps Intelligence Star Schema

The project implemented:

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

# Fact Table

## fact_ticket

The central analytical table.

Purpose:

Store every customer support ticket event.

Structure:

```

fact_ticket

ticket_id

customer_key

agent_key

category_key

channel_key

priority_key

resolution_time_hours

satisfaction_score

sla_status

```

---

# Dimension Tables

## dim_customer

Purpose:

Store customer information.

Contains:

```

customer_key

customer_id

customer_name

email

location

```

Used to answer:

- Which customers create the most tickets?
- Which regions generate issues?

---

## dim_agent

Purpose:

Store support agent information.

Contains:

```

agent_key

agent_id

agent_name

```

Used to answer:

- Which agents handle the most tickets?
- Which agents achieve the best SLA rates?

---

## dim_category

Purpose:

Store ticket issue categories.

Contains:

```

category_key

category_name

```

Used to answer:

- Which problems occur most frequently?
- Which categories require attention?

---

## dim_channel

Purpose:

Store customer communication channels.

Contains:

```

channel_key

channel_name

```

Examples:

```

Email

Phone

Chat

```

Used to answer:

- Which channels receive the most requests?

---

## dim_priority

Purpose:

Store ticket priority information.

Contains:

```

priority_key

priority_level

```

Examples:

```

Low

Medium

High

Critical

```

Used to answer:

- How many critical tickets exist?
- Are high priority tickets resolved quickly?

---

# Grain Definition

Every fact table requires a clearly defined grain.

The grain of:

```

fact_ticket

```

is:

```

One row represents one customer support ticket.

```

Example:

```

Ticket ID

TKT-100001

=

One business event

```

---

# Star Schema Relationships

Relationships follow:

```

Dimension (One)

```
    |

    |
```

Fact Table (Many)

```

Example:

```

dim_customer

```
   1

   |

   *
```

fact_ticket

```

Meaning:

One customer can have many tickets.

---

# Why Power BI Works Well With Star Schemas

Power BI performs best when:

- Relationships are simple
- Dimensions are separated
- Facts contain measurements

---

# Benefits in Power BI

## Better Performance

Power BI can efficiently filter:

```

Customer

↓

Tickets

↓

Metrics

````

---

## Easier DAX Measures

Example:

Total Tickets:

```DAX
Total Tickets =
COUNT(fact_ticket[ticket_id])
````

---

Average Resolution:

```DAX
Average Resolution =
AVERAGE(
fact_ticket[resolution_time_hours]
)
```

---

## Better User Experience

Users can easily navigate:

```
Customer

Agent

Category

Channel

Priority
```

---

# Star Schema vs Flat Tables

## Flat Table

Example:

```
ticket_id

customer_name

agent_name

category_name

channel_name

resolution_time
```

Problems:

* Repeated data
* Larger storage
* Difficult maintenance

---

## Star Schema

Example:

```
fact_ticket

+

dim_customer

+

dim_agent

+

dim_category
```

Advantages:

* Less duplication
* Better scalability
* Easier analysis

---

# Star Schema vs Snowflake Schema

## Star Schema

Characteristics:

* Dimensions are denormalized
* Simple structure
* Faster BI queries

Example:

```
fact_ticket

     |

dim_customer
```

---

## Snowflake Schema

Characteristics:

* Dimensions are normalized
* More tables
* More complex relationships

Example:

```
fact_ticket

      |

dim_customer

      |

dim_location
```

---

# Why We Chose Star Schema

For SupportOps Intelligence Analytics:

Star schema was chosen because:

* Dataset size was manageable
* Power BI performance was important
* Business users needed simple navigation
* Reporting requirements were straightforward

---

# Building a Star Schema Workflow

A professional workflow:

```
Understand Business Process

        ↓

Define Grain

        ↓

Identify Facts

        ↓

Identify Dimensions

        ↓

Create Relationships

        ↓

Add Tests

        ↓

Build BI Reports
```

---

# Common Star Schema Mistakes

## 1. Undefined Grain

Problem:

Rows represent different things.

Example:

```
One row = ticket + customer + agent activity
```

Result:

Incorrect metrics.

---

## 2. Too Many Attributes in Facts

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

## 3. Missing Dimensions

Example:

Adding:

```
dim_date
```

could enable:

* Monthly trends
* Weekly performance
* Seasonality analysis

---

# Future Improvements For SupportOps Model

Potential additions:

## Date Dimension

```
dim_date
```

For:

* Monthly analysis
* Weekly trends
* Year comparisons

---

## SLA Dimension

```
dim_sla
```

For:

* Different SLA rules
* Priority-based targets

---

## Agent Performance Snapshot

For:

* Historical agent performance tracking
* Workforce planning

---

# Skills Required

## Data Modeling

Learn:

* Star schemas
* Snowflake schemas
* Grain definition
* Fact and dimension design

---

## SQL

Learn:

* Joins
* Aggregations
* Window functions
* CTEs

---

## BI Development

Learn:

* Power BI relationships
* DAX
* Semantic models

---

## dbt

Learn:

* Building marts
* Model dependencies
* Testing relationships

---

# Resources

## Books

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Recommended for:

* Star schemas
* Fact tables
* Dimensions
* Enterprise warehouses

### Designing Data-Intensive Applications

Author:

Martin Kleppmann

Recommended for:

* Data systems
* Scalability
* Reliability

---

## Courses

Data Engineering Zoomcamp:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

Microsoft Power BI Learning:

[https://learn.microsoft.com/power-bi/](https://learn.microsoft.com/power-bi/)

---

# Summary

The star schema is one of the most important patterns in analytics engineering.

It separates:

```
Business Events

(Facts)

+

Business Context

(Dimensions)
```

The SupportOps Intelligence Analytics project used this approach to create a clean analytical model that powered Power BI dashboards.

The final model provided:

* Better performance
* Clear business relationships
* Reliable KPIs
* Scalable analytics architecture
