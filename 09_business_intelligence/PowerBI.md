# Power BI

## Overview

Microsoft Power BI is a Business Intelligence platform used to connect, transform, model, analyze, and visualize data.

It enables organizations to turn raw data into interactive reports and dashboards.

Power BI is widely used by:

- Data Analysts
- Business Intelligence Analysts
- Analytics Engineers
- Data Scientists
- Business Users

---

# Power BI Architecture

A typical Power BI workflow:

```
Data Sources

      ↓

Power Query

      ↓

Data Model

      ↓

DAX Calculations

      ↓

Reports

      ↓

Dashboards

      ↓

Business Decisions
```

---

# Power BI Components

Power BI consists of several major components.

---

# 1. Power BI Desktop

Power BI Desktop is the development environment.

Used for:

- Data loading
- Data transformation
- Data modeling
- DAX development
- Report creation

It runs locally on a computer.

---

# 2. Power BI Service

Power BI Service is the cloud platform.

Used for:

- Publishing reports
- Sharing dashboards
- Managing workspaces
- Scheduling refreshes
- Collaboration

---

# 3. Power BI Mobile

Allows users to access dashboards on:

- Phones
- Tablets

---

# 4. Power BI Gateway

A gateway connects on-premise data sources to Power BI Service.

Example:

```
Local SQL Server

        ↓

Gateway

        ↓

Power BI Cloud
```

---

# Connecting Data Sources

Power BI connects to many sources.

Examples:

## Databases

- SQL Server
- PostgreSQL
- MySQL
- Oracle

---

## Files

- Excel
- CSV
- JSON
- XML

---

## Cloud Platforms

- Azure
- Google Cloud
- AWS

---

## Analytics Platforms

- Snowflake
- Databricks
- BigQuery

---

# Power Query

## Overview

Power Query is the data preparation engine in Power BI.

It uses the M language.

Used for:

- Cleaning data
- Transforming columns
- Combining datasets
- Changing data types

---

# Common Power Query Operations

## Remove Columns

Example:

Remove unnecessary fields:

```
Customer_ID

Internal_Code

Created_By
```

---

## Change Data Types

Example:

Convert:

```
Date Text

↓

Date Type
```

---

## Remove Duplicates

Example:

Remove repeated customers.

---

## Merge Queries

Equivalent to SQL JOIN.

Example:

```
Customers

+

Orders

=

Customer Orders
```

---

## Append Queries

Equivalent to SQL UNION.

Example:

```
Sales_2025

+

Sales_2026

=

All Sales
```

---

# Data Modeling in Power BI

A strong data model improves:

- Performance
- Accuracy
- Maintainability

The preferred approach is:

```
Star Schema
```

---

# Star Schema Example

```
              Dim Customer

                    |

Dim Product ---- Fact Sales ---- Dim Date

                    |

              Dim Location
```

---

# Fact Tables

Store measurable business events.

Examples:

```
Sales Transactions

Support Tickets

Website Events
```

Contains:

- Foreign keys
- Measures

Example:

```
Sales_ID

Customer_ID

Product_ID

Revenue

Quantity
```

---

# Dimension Tables

Store descriptive information.

Examples:

## Customer Dimension

```
Customer_ID

Name

Country

Segment
```

---

## Product Dimension

```
Product_ID

Product Name

Category

Price
```

---

# Relationships

Relationships connect tables.

Example:

```
Customers

Customer_ID

      |

      |

Sales

Customer_ID
```

---

# Relationship Types

## One-to-Many

Most common.

Example:

One customer:

```
Customer_ID = 1
```

Many orders:

```
Order 101

Order 102

Order 103
```

---

## Many-to-Many

Requires careful design.

Often avoided because it can create ambiguous filtering.

---

# Measures in Power BI

Measures calculate business logic dynamically.

Example:

```DAX
Total Revenue =

SUM(
Sales[Revenue]
)
```

---

# Common Power BI Measures

## Total Sales

```DAX
Total Sales =

SUM(
Sales[Amount]
)
```

---

## Total Orders

```DAX
Total Orders =

COUNTROWS(
Sales
)
```

---

## Average Order Value

```DAX
Average Order Value =

DIVIDE(

[Total Sales],

[Total Orders]

)
```

---

# Filters in Power BI

Power BI uses filter context.

Filters can come from:

- Visuals
- Pages
- Reports
- Slicers

---

Example:

A report shows:

```
Revenue by Region
```

Power BI automatically calculates:

```
Revenue

for each region
```

---

# Visualizations

Common Power BI visuals:

## Card

Used for:

- KPIs
- Summary values

Example:

```
Revenue

$2M
```

---

## Bar Chart

Used for comparisons.

Example:

```
Revenue by Product
```

---

## Line Chart

Used for trends.

Example:

```
Monthly Revenue
```

---

## Map

Used for geographic analysis.

Example:

```
Sales by Country
```

---

## Table

Used for detailed records.

---

# Dashboard Development Process

Professional workflow:

```
Requirement Gathering

        ↓

Data Preparation

        ↓

Data Modeling

        ↓

Create Measures

        ↓

Build Visuals

        ↓

Validate Results

        ↓

Publish
```

---

# Power BI Performance Optimization

## 1. Use Star Schema

Improves:

- Query speed
- Simplicity

---

## 2. Reduce Columns

Do not load unnecessary data.

---

## 3. Optimize DAX

Avoid:

- Complex calculations
- Repeated expressions

---

## 4. Reduce Visual Count

Too many visuals slow reports.

---

## 5. Use Aggregations

For large datasets:

```
Detailed Data

↓

Summary Tables
```

---

# Power BI Security

## Row-Level Security (RLS)

Controls what users can see.

Example:

Sales managers only see their region.

```
Manager A

↓

Region East

```

```
Manager B

↓

Region West
```

---

# Power BI Deployment Workflow

Development:

```
Power BI Desktop
```

↓

Testing:

```
Workspace
```

↓

Production:

```
Power BI Service
```

---

# Power BI With Analytics Engineering

Modern workflow:

```
Data Sources

↓

ETL / ELT Pipeline

↓

Data Warehouse

↓

dbt Models

↓

Power BI Semantic Model

↓

Reports
```

---

# Example Analytics Project

## Sales Performance Dashboard

Data:

```
Customers

Products

Orders

Dates
```

---

Model:

```
dim_customer

dim_product

dim_date

fact_sales
```

---

KPIs:

```
Revenue

Profit Margin

Orders

Customer Growth
```

---

Visuals:

```
Revenue Trend

Top Products

Regional Sales

Customer Segmentation
```

---

# Common Power BI Interview Questions

## What is Power BI?

Power BI is a BI platform used to transform data into interactive reports and dashboards.

---

## Difference between Power Query and DAX?

Power Query transforms data before loading.

DAX creates calculations after data is loaded into the model.

---

## Why use star schema in Power BI?

Because it improves performance, simplifies relationships, and makes calculations easier.

---

## What is a measure?

A dynamic calculation evaluated based on the current filter context.

---

## What is Row-Level Security?

A feature that restricts users to only the data they are authorized to view.

---

# Key Takeaway

Power BI combines:

```
Data Preparation

+

Data Modeling

+

DAX

+

Visualization

+

Business Understanding
```

A strong Power BI developer does not only create charts.

They build trusted analytical systems that help organizations make better decisions.