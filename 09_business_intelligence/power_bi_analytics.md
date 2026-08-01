# Power BI Analytics

## Overview

Power BI is a business intelligence and data visualization platform used to transform analytical data models into interactive dashboards and reports.

In analytics engineering workflows, Power BI sits at the final stage of the data pipeline.

The typical flow is:

```

Data Sources

```
  ↓
```

Data Transformation

```
  ↓
```

Data Modeling

```
  ↓
```

Semantic Layer

```
  ↓
```

Power BI Reports

```
  ↓
```

Business Decisions

```

The SupportOps Intelligence Analytics project used Power BI to visualize customer support performance metrics built from:

- DuckDB analytical tables
- dbt dimensional models
- Parquet exports

---

# The Role of Power BI in Analytics Engineering

Power BI is not responsible for:

- Data extraction
- Data cleaning
- Complex transformations

Those tasks belong to:

- Python
- SQL
- dbt
- Data platforms

Power BI focuses on:

- Data modeling
- KPI calculation
- Visualization
- Storytelling

---

# Power BI Architecture

A professional Power BI solution contains:

```

Data Sources

```
  |

  ↓
```

Data Warehouse

```
  |

  ↓
```

Semantic Model

```
  |

  ↓
```

Reports

```
  |

  ↓
```

Dashboards

```

---

# Power BI Components

## 1. Power Query

Used for:

- Data extraction
- Cleaning
- Transformation

Language:

```

M Language

```

Examples:

- Changing data types
- Removing columns
- Combining tables

---

## 2. Data Model

Defines:

- Relationships
- Tables
- Filters

Example:

```

fact_ticket

```
  |

  |
```

dim_customer

````

---

## 3. DAX

Data Analysis Expressions.

Used for:

- Measures
- Calculations
- KPIs

Example:

```DAX
Total Tickets =
COUNT(fact_ticket[ticket_id])
````

---

## 4. Visual Layer

Creates:

* Charts
* Tables
* KPI cards
* Filters

---

# Power BI in SupportOps Intelligence

The Power BI report used:

```
dashboards/

SupportOps Intelligence Analytics.pbix
```

The report was built on the analytical model created using dbt.

---

# Data Model Design

The Power BI model followed a star schema:

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

# Why Star Schema Works Well in Power BI

Power BI performs better when:

* Relationships are simple
* Facts contain metrics
* Dimensions contain descriptions

Benefits:

* Faster filtering
* Cleaner DAX
* Easier maintenance

---

# Connecting Power BI to Data

Power BI can connect to:

## Databases

Examples:

* SQL Server
* PostgreSQL
* DuckDB

---

## Files

Examples:

* CSV
* Excel
* Parquet

---

## Cloud Platforms

Examples:

* Azure
* AWS
* Google Cloud

---

# SupportOps Data Connection

The project exported analytical tables:

```
exports/

fact_ticket.parquet

customers.parquet

agents.parquet

channels.parquet

priorities.parquet
```

These tables were used as Power BI sources.

---

# Power BI Data Modeling Best Practices

## 1. Use Relationships Instead of Merging Everything

Avoid:

```
One large table
```

Prefer:

```
Fact Table

+

Dimension Tables
```

---

## 2. Hide Technical Columns

Example:

Hide:

```
customer_key

agent_key
```

Users usually need:

```
Customer Name

Agent Name
```

---

## 3. Create Measures Instead of Calculated Columns

Prefer:

```DAX
Total Tickets =
COUNT(fact_ticket[ticket_id])
```

Instead of:

Adding unnecessary columns.

---

# Measures vs Calculated Columns

## Measures

Calculated during report interaction.

Example:

```DAX
Average Resolution Time =
AVERAGE(
fact_ticket[resolution_time_hours]
)
```

Advantages:

* Smaller model size
* Dynamic calculations
* Better performance

---

## Calculated Columns

Created during data loading.

Example:

```DAX
Ticket Year =
YEAR(
fact_ticket[created_date]
)
```

Use when:

* Row-level calculations are needed
* Values are reused frequently

---

# Important DAX Concepts

## Aggregations

Examples:

COUNT

```DAX
Ticket Count =
COUNT(fact_ticket[ticket_id])
```

---

SUM

```DAX
Total Hours =
SUM(
fact_ticket[resolution_time_hours]
)
```

---

AVERAGE

```DAX
Average Satisfaction =
AVERAGE(
fact_ticket[satisfaction_score]
)
```

---

# Filter Context

DAX calculations respond to filters.

Example:

A KPI:

```
Total Tickets
```

can change based on:

* Agent
* Channel
* Priority
* Date

---

# Time Intelligence

Power BI commonly analyzes:

* Daily trends
* Monthly trends
* Year comparisons

Requires:

```
dim_date
```

Example:

```
2026 January

2026 February

2026 March
```

---

# Power BI Report Design Principles

A good dashboard should:

* Answer business questions
* Highlight important metrics
* Avoid unnecessary complexity

---

# Dashboard Structure

A professional report usually contains:

## Page 1: Executive Summary

Purpose:

Provide overall performance.

Contains:

* KPI cards
* Trend charts
* High-level insights

---

## Page 2: Operational Performance

Purpose:

Analyze support operations.

Contains:

* Agent performance
* Resolution metrics
* SLA analysis

---

## Page 3: Customer Analysis

Purpose:

Understand customer support demand.

Contains:

* Customer trends
* Ticket categories
* Support channels

---

# SupportOps Report Pages

The project created:

## Page 1

```
Summary Page
```

Focus:

Overall support performance.

---

## Page 2

```
Agent Performance Page
```

Focus:

Support team analysis.

---

## Page 3

```
Customer Tickets Page
```

Focus:

Customer behavior and ticket patterns.

---

# Common Power BI Visuals

## KPI Cards

Used for:

* Total tickets
* Average resolution time
* SLA percentage

---

## Bar Charts

Used for:

* Category comparison
* Agent ranking

---

## Line Charts

Used for:

* Trends over time

---

## Donut Charts

Used for:

* Distribution analysis

Examples:

* Ticket channels
* Priorities

---

## Tables

Used for:

* Detailed records
* Rankings

---

# Power BI Performance Optimization

## 1. Reduce Data Size

Remove unnecessary columns.

---

## 2. Use Star Schemas

Avoid complex relationships.

---

## 3. Optimize DAX

Avoid expensive calculations.

---

## 4. Use Measures

Reduce model size.

---

## 5. Import Mode Optimization

Power BI Import mode provides:

* Fast queries
* Compression
* Better performance

---

# Power BI Security

## Row Level Security (RLS)

Controls data access.

Example:

Agents only see their own tickets.

---

# Deployment Workflow

Professional workflow:

```
Develop Report

       ↓

Test Metrics

       ↓

Publish Dataset

       ↓

Schedule Refresh

       ↓

Monitor Usage
```

---

# Power BI Skills Required

## Data Modeling

Learn:

* Star schemas
* Relationships
* Semantic models

---

## DAX

Learn:

* Measures
* Filter context
* Time intelligence

---

## Visualization

Learn:

* Dashboard design
* Data storytelling
* User experience

---

## Business Analysis

Learn:

* KPI development
* Requirement gathering
* Stakeholder communication

---

# Resources

## Books

### The Definitive Guide to DAX

Authors:

Marco Russo and Alberto Ferrari

Recommended for:

* Advanced DAX
* Performance optimization

### Storytelling with Data

Author:

Cole Nussbaumer Knaflic

Recommended for:

* Visualization principles
* Communication

---

## Courses

Microsoft Power BI Learning:

[https://learn.microsoft.com/power-bi/](https://learn.microsoft.com/power-bi/)

SQLBI:

[https://www.sqlbi.com/](https://www.sqlbi.com/)

---

# Summary

Power BI is the final presentation layer of an analytics engineering system.

A strong Power BI implementation requires:

```
Clean Data

+

Reliable Data Models

+

Correct Measures

+

Effective Visualization
```

The SupportOps Intelligence Analytics project used Power BI to transform dbt-created analytical models into a professional customer support analytics solution.

