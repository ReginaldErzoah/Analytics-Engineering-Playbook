# Supply Chain Analytics Case Study

## Overview

This case study demonstrates how analytics engineers build supply chain analytics solutions to improve operational efficiency, inventory management, supplier performance, and delivery reliability.

The goal is to transform supply chain data into insights that help organizations answer:

- How efficiently are products moving?
- Which suppliers perform best?
- Where are inventory problems occurring?
- How can delivery performance improve?

---

# Business Context

## Company

**GlobalMart Distribution Ltd.**

GlobalMart is a retail distribution company that manages:

- Multiple suppliers
- Warehouses
- Inventory locations
- Customer deliveries

The company operates across several regions and handles thousands of daily orders.

---

# Business Problem

The operations team has difficulty understanding supply chain performance.

Current challenges:

- Inventory shortages occur unexpectedly
- Supplier performance is difficult to measure
- Delivery delays affect customer satisfaction
- Warehouse efficiency is unclear

Leadership wants a centralized supply chain analytics platform.

---

# Business Objectives

The analytics solution should help answer:

## Inventory Management

- Which products are running low?
- Which products are overstocked?
- How quickly does inventory move?

---

## Supplier Performance

- Which suppliers deliver on time?
- Which suppliers have quality issues?
- Which suppliers are most reliable?

---

## Logistics Performance

- Are deliveries meeting expectations?
- Where are delays occurring?
- How can transportation costs be reduced?

---

# Data Sources

The company collects data from multiple operational systems.

---

# Products Table

Stores product information.

Example:

|Column|Description|
|-|-|
|product_id|Product identifier|
|product_name|Product name|
|category|Product category|
|unit_cost|Cost per unit|

---

# Inventory Table

Stores inventory levels.

Example:

|Column|Description|
|-|-|
|product_id|Product identifier|
|warehouse_id|Storage location|
|stock_quantity|Available inventory|
|last_updated|Update timestamp|

---

# Orders Table

Stores customer orders.

Example:

|Column|Description|
|-|-|
|order_id|Order identifier|
|product_id|Product purchased|
|quantity|Units ordered|
|order_date|Purchase date|

---

# Supplier Table

Stores supplier information.

Example:

|Column|Description|
|-|-|
|supplier_id|Supplier identifier|
|supplier_name|Supplier name|
|region|Supplier location|

---

# Shipment Table

Stores delivery information.

Example:

|Column|Description|
|-|-|
|shipment_id|Shipment identifier|
|order_id|Related order|
|delivery_date|Delivery completion date|
|delivery_status|Shipment outcome|

---

# Data Challenges

Supply chain data contains several problems.

---

# Inconsistent Product Information

Problem:

Different systems use different names.

Example:

```
Laptop Pro 15

Laptop-Pro15

LP15
```

Solution:

Create standardized product dimensions.

---

# Missing Shipment Data

Problem:

Some deliveries have incomplete tracking.

Solution:

Implement completeness checks.

---

# Delayed Data Updates

Problem:

Inventory information may not reflect reality.

Solution:

Monitor data freshness.

---

# Analytics Engineering Architecture

The solution follows:

```
Operational Systems

        ↓

Raw Data Warehouse

        ↓

Staging Models

        ↓

Supply Chain Analytics Models

        ↓

Operations Dashboard
```

---

# Data Modeling

Supply chain analytics uses dimensional modeling.

Structure:

```
                Date Dimension

                       |

Product Dimension -- Inventory Fact

                       |

Supplier Dimension -- Shipment Fact

                       |

                  Order Fact
```

---

# Fact Tables

## fact_orders

Stores purchasing activity.

Columns:

```
order_id

product_id

customer_id

quantity

order_date
```

---

## fact_inventory

Stores inventory snapshots.

Columns:

```
product_id

warehouse_id

stock_quantity

snapshot_date
```

---

## fact_shipments

Stores delivery events.

Columns:

```
shipment_id

order_id

supplier_id

delivery_date

status
```

---

# Dimension Tables

## dim_product

Stores product information.

```
product_id

product_name

category

cost
```

---

## dim_supplier

Stores supplier details.

```
supplier_id

supplier_name

region
```

---

## dim_warehouse

Stores warehouse information.

```
warehouse_id

location

capacity
```

---

## dim_date

Stores calendar details.

```
date_id

day

month

quarter

year
```

---

# Supply Chain Metrics

Analytics engineers create operational KPIs.

---

# 1. Inventory Turnover

Formula:

```
Cost of Goods Sold /

Average Inventory
```

Measures how quickly inventory moves.

---

# 2. Stock Availability Rate

Formula:

```
Available Products /

Total Products × 100
```

---

# 3. Stockout Rate

Formula:

```
Out-of-stock Products /

Total Products × 100
```

---

# 4. Supplier On-Time Delivery Rate

Formula:

```
On-time Deliveries /

Total Deliveries × 100
```

---

# 5. Order Fulfillment Rate

Formula:

```
Completed Orders /

Total Orders × 100
```

---

# 6. Average Delivery Time

Formula:

```
Delivery Date -

Order Date
```

---

# SQL Examples

## Products With Low Inventory

```sql
SELECT

product_id,

stock_quantity

FROM inventory

WHERE stock_quantity < 10;
```

---

## Supplier Performance

```sql
SELECT

supplier_id,

COUNT(*) AS deliveries,

AVG(delivery_days) AS avg_delivery_time

FROM shipments

GROUP BY supplier_id;
```

---

## Monthly Order Volume

```sql
SELECT

DATE_TRUNC('month', order_date) AS month,

COUNT(order_id) AS orders

FROM orders

GROUP BY month;
```

---

# dbt Supply Chain Models

Example structure:

```
models/

staging/

    stg_inventory.sql

    stg_orders.sql

    stg_shipments.sql


intermediate/

    supplier_metrics.sql


marts/

    supply_chain_dashboard.sql
```

---

# Data Quality Tests

Supply chain systems require reliability.

---

## Inventory Validation

Rule:

```
Stock quantity cannot be negative
```

---

## Product Validation

Rule:

```
Every inventory record requires a valid product
```

---

## Shipment Validation

Rule:

```
Every shipment requires an order
```

---

## Delivery Date Validation

Rule:

```
Delivery date cannot occur before order date
```

---

# Dashboard Requirements

A supply chain dashboard should include:

---

# Inventory Overview

Metrics:

- Total inventory
- Stock availability
- Stockout rate

---

# Supplier Performance

Visuals:

- Delivery performance
- Supplier rankings
- Quality metrics

---

# Logistics Performance

Visuals:

- Delivery times
- Late shipments
- Regional performance

---

# Product Movement

Visuals:

- Fast-moving products
- Slow-moving products
- Demand trends

---

# Business Insights Example

## Finding 1

Some products frequently experience stockouts.

Recommendation:

Increase inventory planning.

---

## Finding 2

Certain suppliers have consistently delayed deliveries.

Recommendation:

Review supplier agreements.

---

## Finding 3

Warehouse inventory is higher than demand.

Recommendation:

Reduce excess stock.

---

# Analytics Engineering Deliverables

Final outputs:

```
Supply Chain Models

+

Operational Metrics

+

Quality Tests

+

Performance Dashboards

+

Optimization Insights
```

---

# Tools Used

## Transformation

- SQL
- dbt

---

## Storage

- Snowflake
- BigQuery
- Redshift

---

## Visualization

- Power BI
- Tableau
- Looker

---

# Interview Discussion Points

## How would you improve supply chain visibility?

Answer:

"I would integrate operational data sources into a warehouse, create dimensional models for inventory, suppliers, and shipments, define operational KPIs, and provide dashboards for decision-making."

---

## How would you identify inventory problems?

Answer:

"I would analyze stock levels, demand trends, inventory turnover, and stockout rates to identify shortages and overstock situations."

---

# Key Takeaway

Supply chain analytics engineering converts operational data into insights that improve efficiency.

The process:

```
Supply Chain Data

↓

Data Models

↓

Operational Metrics

↓

Dashboards

↓

Process Improvements
```

Reliable supply chain analytics helps organizations reduce costs, improve deliveries, and optimize operations.