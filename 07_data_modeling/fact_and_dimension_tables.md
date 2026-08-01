# Fact and Dimension Tables

## Overview

Fact and dimension tables are the foundation of dimensional data modeling.

Together they create the structure that powers:

- Data warehouses
- Business intelligence systems
- Analytics platforms
- Reporting solutions

A well-designed analytical model separates:

- Business events
- Business descriptions

Facts store what happened.

Dimensions explain the context around what happened.

In the SupportOps Intelligence Analytics project, fact and dimension tables were used to transform customer support data into an analytics-ready star schema for Power BI.

---

# The Difference Between Facts and Dimensions

## Fact Tables

Fact tables answer:

> What happened?

They store measurable business activities.

Examples:

- A customer opened a support ticket
- A customer purchased a product
- A user visited a website

---

## Dimension Tables

Dimension tables answer:

> Who, what, where, when, and how?

They provide descriptive context.

Examples:

- Customer information
- Agent information
- Ticket category
- Support channel

---

# Simple Analogy

Think about a sales receipt.

The receipt itself is the fact.

```

Purchase

Amount: $500

Quantity: 3

Date: 2026-08-01

```

The details explaining the purchase are dimensions.

```

Customer

Product

Store

Date

```

---

# Fact Tables

## Definition

A fact table is a central table containing measurable events.

Each row represents a business occurrence.

Examples:

```

One row = one sale

One row = one support ticket

One row = one transaction

```

---

# Fact Table Components

A fact table normally contains:

## 1. Keys

Foreign keys connecting to dimensions.

Example:

```

customer_key

agent_key

category_key

```

---

## 2. Measures

Numbers that can be analyzed.

Examples:

```

sales_amount

quantity

resolution_hours

satisfaction_score

```

---

## 3. Event Identifier

Unique identifier of the event.

Examples:

```

ticket_id

order_id

transaction_id

```

---

# Fact Table Design Process

Before creating a fact table, define:

## Business Process

Example:

Customer support operations.

---

## Grain

Question:

"What does one row represent?"

Example:

```

One row represents one customer support ticket.

```

---

## Measures

Identify what can be measured.

Example:

```

Resolution Time

Satisfaction Score

SLA Status

```

---

# Types of Fact Tables

## 1. Transaction Fact Tables

Store individual business events.

Example:

```

fact_ticket

```

Each row:

```

One ticket

```

Used for:

- Detailed analysis
- Operational reporting

---

## 2. Periodic Snapshot Fact Tables

Store measurements at regular intervals.

Example:

```

Daily support team performance

```

Used for:

- Trends
- Historical reporting

---

## 3. Accumulating Snapshot Fact Tables

Track the lifecycle of an event.

Example:

Support ticket:

```

Created

Assigned

Resolved

Closed

```

Used for:

- Process analysis
- Workflow optimization

---

# SupportOps Fact Table

The project created:

```

fact_ticket

```

Purpose:

Store support ticket events and measurable operational metrics.

---

## Structure

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

# Why fact_ticket Exists

It allows analysis such as:

## Ticket Volume

Question:

How many tickets were created?

Measure:

```

COUNT(ticket_id)

```

---

## Resolution Performance

Question:

How quickly are tickets resolved?

Measure:

```

AVG(resolution_time_hours)

```

---

## Customer Satisfaction

Question:

How satisfied are customers?

Measure:

```

AVG(satisfaction_score)

```

---

# Dimension Tables

## Definition

Dimension tables contain descriptive attributes used to filter and group facts.

They provide meaning to numerical measurements.

---

# Dimension Characteristics

Dimensions usually contain:

- Primary keys
- Descriptive attributes
- Categories
- Labels

Example:

```

dim_customer

customer_key

customer_name

location

```

---

# Dimension Table Components

## Surrogate Key

Artificial warehouse identifier.

Example:

```

customer_key = 1001

```

---

## Natural Key

Original source identifier.

Example:

```

customer_id = CUST-50001

```

---

## Attributes

Descriptions about the entity.

Example:

```

customer_name

email

region

```

---

# SupportOps Dimension Tables

The project created five dimensions.

---

# 1. dim_customer

## Purpose

Stores customer details.

Structure:

```

dim_customer

customer_key

customer_id

customer_name

email

location

```

---

## Business Questions

Answers:

- Which customers create the most tickets?
- Which locations generate more support demand?
- Which customers need attention?

---

# 2. dim_agent

## Purpose

Stores support agent information.

Structure:

```

dim_agent

agent_key

agent_id

agent_name

```

---

## Business Questions

Answers:

- Which agents handle the most tickets?
- Which agents achieve better SLA performance?
- Which agents need support?

---

# 3. dim_category

## Purpose

Stores ticket issue categories.

Structure:

```

dim_category

category_key

category_name

```

---

## Business Questions

Answers:

- What are the most common customer issues?
- Which categories take longer to resolve?

---

# 4. dim_channel

## Purpose

Stores communication channels.

Structure:

```

dim_channel

channel_key

channel_name

```

---

Examples:

```

Email

Phone

Chat

```

---

## Business Questions

Answers:

- Which channels receive the most requests?
- Which channels have better resolution times?

---

# 5. dim_priority

## Purpose

Stores ticket urgency levels.

Structure:

```

dim_priority

priority_key

priority_level

```

---

Examples:

```

Low

Medium

High

Critical

```

---

## Business Questions

Answers:

- Are critical issues resolved quickly?
- Which priority levels create the most workload?

---

# Fact and Dimension Relationships

A star schema relationship:

```

```
             dim_customer

                  |

                  |
```

dim_agent ---- fact_ticket ---- dim_category

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

# Relationship Rules

## One Dimension to Many Facts

Example:

```

One customer

```
    |

    |
```

Many tickets

```

Database relationship:

```

1 : Many

```

---

# Why Not Store Everything In One Table?

A common beginner approach:

```

support_ticket

customer_name

customer_email

agent_name

category

channel

priority

resolution_time

```

Problems:

- Repeated data
- Larger storage
- Difficult updates
- Poor scalability

---

# Normalized vs Dimensional Design

## Operational Design

Optimized for:

- Transactions
- Updates
- Data entry

---

## Dimensional Design

Optimized for:

- Reporting
- Aggregation
- Analysis

---

# Fact and Dimension Modeling Workflow

Professional approach:

```

Understand Business Process

```
      ↓
```

Define Grain

```
      ↓
```

Identify Facts

```
      ↓
```

Identify Dimensions

```
      ↓
```

Create Relationships

```
      ↓
```

Add Tests

```
      ↓
```

Build Reports

```

---

# Data Types in Fact Tables

Common measures:

## Counts

Example:

```

number_of_tickets

```

---

## Durations

Example:

```

resolution_time_hours

```

---

## Scores

Example:

```

customer_satisfaction_score

```

---

## Financial Measures

Example:

```

revenue

```

---

# Data Types in Dimension Tables

Common attributes:

## Text

Examples:

```

customer_name

category_name

```

---

## Dates

Examples:

```

created_date

signup_date

```

---

## Categories

Examples:

```

priority_level

support_channel

```

---

# Best Practices

## 1. Keep Facts Focused

A fact table should represent one business process.

Good:

```

fact_ticket

```

Bad:

```

fact_everything

```

---

## 2. Avoid Text Columns In Facts

Bad:

```

fact_ticket

customer_name

agent_name

```

Better:

```

fact_ticket

customer_key

agent_key

```

---

## 3. Create Meaningful Dimensions

Dimensions should answer business questions.

---

## 4. Define Grain Clearly

Every fact table should have a documented grain.

---

# Skills Required

## Data Warehousing

Learn:

- Fact tables
- Dimension tables
- Star schemas
- Warehouse design

---

## SQL

Learn:

- Joins
- Aggregations
- Window functions
- CTEs

---

## dbt

Learn:

- Model layers
- Creating marts
- Testing relationships

---

## Business Intelligence

Learn:

- Semantic models
- Metrics
- Dashboard requirements

---

# Resources

## Books

### The Data Warehouse Toolkit

Authors:

Ralph Kimball and Margy Ross

Best resource for:

- Facts
- Dimensions
- Star schemas
- Enterprise analytics


### Fundamentals of Data Engineering

Authors:

Joe Reis and Matt Housley

Covers:

- Modern data platforms
- Data architecture
- Analytics engineering

---

## Courses

Data Engineering Zoomcamp:

https://github.com/DataTalksClub/data-engineering-zoomcamp

Microsoft Power BI Learning:

https://learn.microsoft.com/power-bi/

---

# Summary

Fact and dimension tables form the backbone of analytical systems.

Facts store:

```

Measurements
Events
Transactions

```

Dimensions store:

```

Descriptions
Categories
Context

```

The SupportOps Intelligence Analytics project used:

```

fact_ticket

*

dim_customer

*

dim_agent

*

dim_category

*

dim_channel

*

dim_priority

```

to create a reliable analytical foundation for Power BI reporting.

