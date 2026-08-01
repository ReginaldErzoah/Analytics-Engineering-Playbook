# Marketing Analytics Case Study

## Overview

This case study demonstrates how analytics engineers build marketing analytics systems to measure campaign effectiveness, customer acquisition, and marketing performance.

The goal is to transform marketing data into insights that help organizations understand:

- Which campaigns perform best
- Which channels generate customers
- How much customer acquisition costs
- Whether marketing investments generate returns

---

# Business Context

## Company

**GrowthHub Digital**

GrowthHub Digital is an online subscription business that uses multiple marketing channels to acquire customers.

The company invests in:

- Social media advertising
- Email campaigns
- Search advertising
- Affiliate marketing
- Content marketing

---

# Business Problem

The marketing team runs many campaigns but struggles to measure performance accurately.

Current challenges:

- Campaign data exists in different platforms
- Marketing costs are difficult to track
- Customer attribution is unclear
- ROI calculations are inconsistent

---

# Business Objectives

The analytics solution should help answer:

## Campaign Performance

- Which campaigns generate the most conversions?
- Which campaigns have the highest ROI?
- Which channels perform best?

---

## Customer Acquisition

- How many customers were acquired?
- How much does acquisition cost?
- Which sources bring valuable customers?

---

## Marketing Investment

- Where should marketing budget be allocated?
- Which campaigns should be expanded?
- Which campaigns should stop?

---

# Data Sources

The company collects data from multiple platforms.

---

# Campaign Table

Stores campaign information.

Example:

|Column|Description|
|-|-|
|campaign_id|Campaign identifier|
|campaign_name|Campaign name|
|channel|Marketing channel|
|start_date|Campaign start date|
|end_date|Campaign end date|

---

# Ad Performance Table

Stores advertising metrics.

Example:

|Column|Description|
|-|-|
|campaign_id|Campaign identifier|
|impressions|Number of views|
|clicks|Number of clicks|
|cost|Advertising spend|

---

# Customer Acquisition Table

Stores customer sources.

Example:

|Column|Description|
|-|-|
|customer_id|Customer identifier|
|campaign_id|Acquisition campaign|
|signup_date|Registration date|
|source|Marketing source|

---

# Sales Table

Stores customer purchases.

Example:

|Column|Description|
|-|-|
|order_id|Order identifier|
|customer_id|Customer|
|revenue|Purchase value|

---

# Data Challenges

Marketing data often contains complexity.

---

# Attribution Problems

Problem:

A customer interacts with multiple campaigns.

Example:

```
Google Ad

↓

Email Campaign

↓

Purchase
```

Question:

Which campaign receives credit?

---

Solution:

Create attribution models.

---

# Different Data Formats

Problem:

Platforms store metrics differently.

Example:

```
Facebook Ads

Google Ads

Email Platform
```

Solution:

Standardize data models.

---

# Duplicate Customers

Problem:

Same customer appears across platforms.

Solution:

Identity matching.

---

# Analytics Engineering Architecture

The solution follows:

```
Marketing Platforms

        ↓

Raw Warehouse Tables

        ↓

Staging Models

        ↓

Marketing Analytics Models

        ↓

Performance Dashboards
```

---

# Data Modeling

Marketing analytics uses a dimensional model.

Structure:

```
             Date Dimension

                  |

Campaign Dimension

                  |

       Marketing Fact Table

                  |

          Customer Dimension
```

---

# Fact Tables

## fact_campaign_performance

Stores campaign activity.

Columns:

```
campaign_id

date_id

impressions

clicks

cost

conversions
```

---

## fact_customer_acquisition

Stores customer acquisition events.

Columns:

```
customer_id

campaign_id

signup_date

source
```

---

# Dimension Tables

## dim_campaign

Stores campaign details.

```
campaign_id

campaign_name

channel

campaign_type
```

---

## dim_customer

Stores customer information.

```
customer_id

location

segment
```

---

## dim_date

Stores calendar information.

```
date_id

month

quarter

year
```

---

# Marketing Metrics

Analytics engineers create marketing KPIs.

---

# 1. Impressions

Definition:

Number of times an advertisement was displayed.

---

# 2. Click Through Rate (CTR)

Formula:

```
CTR =

Clicks / Impressions × 100
```

Example:

```
10,000 impressions

500 clicks

CTR = 5%
```

---

# 3. Conversion Rate

Formula:

```
Conversions /

Clicks × 100
```

Measures how effectively clicks become customers.

---

# 4. Customer Acquisition Cost (CAC)

Formula:

```
Marketing Spend /

New Customers Acquired
```

Example:

```
$10,000 spend

500 customers

CAC = $20
```

---

# 5. Return On Marketing Investment (ROMI)

Formula:

```
Revenue Generated - Marketing Cost

/

Marketing Cost
```

---

# 6. Customer Lifetime Value To CAC Ratio

Formula:

```
CLV / CAC
```

Measures customer profitability.

---

# SQL Examples

## Campaign Revenue

```sql
SELECT

campaign_id,

SUM(revenue) AS total_revenue

FROM sales

GROUP BY campaign_id;
```

---

## Campaign Conversion Rate

```sql
SELECT

campaign_id,

conversions / clicks AS conversion_rate

FROM campaign_metrics;
```

---

## Marketing Spend By Channel

```sql
SELECT

channel,

SUM(cost) AS total_spend

FROM campaigns

GROUP BY channel;
```

---

# dbt Marketing Models

Example structure:

```
models/

staging/

    stg_campaigns.sql

    stg_ads.sql


intermediate/

    campaign_metrics.sql


marts/

    marketing_performance.sql
```

---

# Data Quality Tests

Marketing data requires validation.

---

## Campaign ID Validation

Rule:

```
Every metric must have a valid campaign
```

---

## Cost Validation

Rule:

```
Advertising cost cannot be negative
```

---

## Conversion Validation

Rule:

```
Conversions cannot exceed clicks
```

---

## Freshness Tests

Rule:

```
Marketing data updated daily
```

---

# Dashboard Requirements

A marketing analytics dashboard should include:

---

# Campaign Overview

Metrics:

- Total spend
- Total conversions
- Revenue generated
- ROI

---

# Channel Performance

Visuals:

- Revenue by channel
- CAC by channel
- Conversion rates

---

# Campaign Comparison

Visuals:

- Best-performing campaigns
- Worst-performing campaigns
- Spending trends

---

# Customer Acquisition

Visuals:

- New customers over time
- Acquisition sources
- Customer quality

---

# Business Insights Example

## Finding 1

Paid search generates customers at a lower acquisition cost.

Recommendation:

Increase investment in high-performing channels.

---

## Finding 2

Some campaigns generate clicks but few conversions.

Recommendation:

Improve landing pages or targeting.

---

## Finding 3

High acquisition cost reduces profitability.

Recommendation:

Optimize campaign spending.

---

# Analytics Engineering Deliverables

Final outputs:

```
Marketing Data Models

+

Campaign Metrics

+

Attribution Models

+

Quality Tests

+

Marketing Dashboards
```

---

# Tools Used

## Data Transformation

- SQL
- dbt

---

## Data Sources

- Google Ads
- Meta Ads
- Email platforms
- CRM systems

---

## Visualization

- Power BI
- Tableau
- Looker

---

# Interview Discussion Points

## How would you measure campaign success?

Answer:

"I would define metrics such as CTR, conversion rate, CAC, revenue contribution, and ROMI, then build models that connect campaign activity to customer outcomes."

---

## How would you handle marketing attribution?

Answer:

"I would create attribution models such as first-touch, last-touch, or multi-touch attribution depending on business requirements."

---

# Key Takeaway

Marketing analytics engineering transforms campaign data into measurable business outcomes.

The process:

```
Marketing Data

↓

Data Models

↓

Campaign Metrics

↓

Performance Analysis

↓

Marketing Decisions
```

A strong marketing analytics platform helps organizations spend smarter, acquire customers efficiently, and maximize growth.