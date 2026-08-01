# DAX (Data Analysis Expressions)

## Overview

DAX (Data Analysis Expressions) is a formula language used in Microsoft Power BI, Analysis Services, and Power Pivot.

It is used to:

- Create calculated columns
- Create measures
- Perform advanced calculations
- Analyze data dynamically

DAX is one of the most important skills for Power BI developers and analytics engineers.

---

# DAX in Power BI

Power BI has three major layers:

```
Data Source

      ↓

Data Model

      ↓

DAX Calculations

      ↓

Visualizations
```

DAX works on the data model layer to create analytical logic.

---

# Why DAX Matters

SQL retrieves and transforms data.

DAX creates dynamic business calculations inside the BI model.

Example:

SQL:

```sql
SELECT

SUM(revenue)

FROM sales;
```

DAX:

```DAX
Total Revenue =
SUM(Sales[Revenue])
```

Both calculate revenue, but DAX responds dynamically to:

- Filters
- Slicers
- User interactions

---

# DAX Concepts

The three main concepts:

```
Measures

Calculated Columns

Calculated Tables
```

---

# 1. Measures

## Definition

A measure is a calculation performed when a report visual requests it.

Measures are dynamic.

Example:

```DAX
Total Sales =
SUM(Sales[Amount])
```

If a user filters:

```
Country = Ghana
```

the measure recalculates automatically.

---

# 2. Calculated Columns

## Definition

Calculated columns create new columns during data loading.

Example:

```DAX
Full Name =
Customers[First_Name]
&
" "
&
Customers[Last_Name]
```

Creates:

|First Name|Last Name|Full Name|
|-|-|-|
|John|Mensah|John Mensah|

---

# Measure vs Calculated Column

|Feature|Measure|Calculated Column|
|-|-|-|
|Calculated|At query time|During refresh|
|Storage|No additional storage|Uses storage|
|Dynamic|Yes|No|
|Best for|KPIs|Row-level calculations|

---

# 3. Calculated Tables

Calculated tables create new tables using DAX.

Example:

```DAX
High Value Customers =

FILTER(
Customers,
Customers[Revenue] > 10000
)
```

---

# DAX Syntax

General structure:

```DAX
Name =
Expression
```

Example:

```DAX
Total Customers =
COUNT(Customers[Customer_ID])
```

---

# Basic DAX Functions

## SUM

Adds values.

Example:

```DAX
Total Revenue =
SUM(Sales[Revenue])
```

---

## COUNT

Counts rows.

Example:

```DAX
Total Orders =
COUNT(Sales[Order_ID])
```

---

## COUNTROWS

Counts table rows.

Example:

```DAX
Customers Count =
COUNTROWS(Customers)
```

---

## DISTINCTCOUNT

Counts unique values.

Example:

```DAX
Unique Customers =
DISTINCTCOUNT(
Sales[Customer_ID]
)
```

---

## AVERAGE

Calculates average.

Example:

```DAX
Average Sales =
AVERAGE(
Sales[Revenue]
)
```

---

# Mathematical Calculations

## Addition

```DAX
Total Cost =
Sales[Cost]
+
Sales[Shipping]
```

---

## Percentage Calculation

Example:

Profit Margin:

```DAX
Profit Margin =

DIVIDE(
[Profit],
[Revenue]
)
```

---

# DIVIDE Function

Preferred over `/`.

Example:

Avoid:

```DAX
Profit / Revenue
```

Use:

```DAX
DIVIDE(
Profit,
Revenue
)
```

because it safely handles:

```
Division by zero
```

---

# Filter Context

Filter context determines what data a calculation sees.

Example:

A dashboard shows:

```
Revenue by Country
```

DAX calculates:

```
Revenue

for each country
```

automatically.

---

# Row Context

Row context refers to the current row being evaluated.

Commonly used in calculated columns.

Example:

```DAX
Total Amount =

Sales[Quantity]
*
Sales[Price]
```

Each row gets calculated separately.

---

# CALCULATE Function

CALCULATE is one of the most important DAX functions.

It changes filter context.

Syntax:

```DAX
CALCULATE(
Expression,
Filter
)
```

---

Example:

Total Sales:

```DAX
Total Sales =
SUM(
Sales[Amount]
)
```

Sales only for Ghana:

```DAX
Ghana Sales =

CALCULATE(

[Total Sales],

Customers[Country]="Ghana"

)
```

---

# Time Intelligence

DAX provides functions for analyzing time periods.

Common calculations:

- Year-to-date
- Previous year
- Month-to-date
- Quarter comparison

---

# Date Table

Time intelligence requires a proper date table.

Example:

```
Dim_Date

Date

Year

Month

Quarter
```

---

# Year-To-Date Sales

Example:

```DAX
YTD Sales =

TOTALYTD(

[Total Sales],

Date[Date]

)
```

---

# Previous Year Sales

```DAX
Previous Year Sales =

CALCULATE(

[Total Sales],

SAMEPERIODLASTYEAR(
Date[Date]
)

)
```

---

# Growth Percentage

Example:

```DAX
Sales Growth =

DIVIDE(

[Total Sales]
-
[Previous Year Sales],

[Previous Year Sales]

)
```

---

# Ranking

Example:

Rank products by revenue:

```DAX
Product Rank =

RANKX(

ALL(Product),

[Total Revenue]

)
```

---

# Variables in DAX

Variables improve readability.

Example:

Without variable:

```DAX
Profit Margin =

DIVIDE(

SUM(Sales[Profit]),

SUM(Sales[Revenue])

)
```

---

With variable:

```DAX
Profit Margin =

VAR Profit =
SUM(Sales[Profit])

VAR Revenue =
SUM(Sales[Revenue])

RETURN

DIVIDE(
Profit,
Revenue
)
```

---

# DAX and Data Modeling

DAX works best with good models.

Recommended:

```
Fact Tables

        ↓

Dimension Tables

        ↓

Relationships

        ↓

Measures
```

---

Example:

Star Schema:

```
              Dim Customer

                    |

Dim Product ---- Fact Sales ---- Dim Date

                    |

              Dim Location
```

---

# Common DAX Mistakes

## 1. Creating Too Many Calculated Columns

Problem:

- Increases file size
- Slows refresh

Prefer measures where possible.

---

## 2. Poor Data Modeling

DAX cannot fix a bad model.

Good:

```
Star Schema
```

Bad:

```
Many-to-many relationships everywhere
```

---

## 3. Ignoring Filter Context

Many DAX problems come from misunderstanding filters.

---

# DAX Best Practices

## Use Measures For Business Metrics

Example:

```
Revenue

Profit

Customer Count
```

---

## Name Measures Clearly

Good:

```
Total Revenue
```

Bad:

```
Measure1
```

---

## Use Variables

Benefits:

- Easier debugging
- Better readability
- Better performance

---

## Create a Dedicated Measures Table

Example:

```
_Measures

    Total Revenue

    Profit Margin

    Customer Count
```

---

# Example: Customer Analytics Dashboard

Dataset:

```
Sales

Customers

Products

Dates
```

Measures:

## Total Revenue

```DAX
Total Revenue =
SUM(Sales[Amount])
```

---

## Customer Count

```DAX
Customers =
DISTINCTCOUNT(
Sales[Customer_ID]
)
```

---

## Average Order Value

```DAX
AOV =

DIVIDE(

[Total Revenue],

[Total Orders]

)
```

---

# Interview Questions

## What is DAX?

DAX is a formula language used in Power BI to create calculations and analytical expressions.

---

## Difference between measures and calculated columns?

Measures calculate dynamically during analysis, while calculated columns are created during data refresh.

---

## What does CALCULATE do?

CALCULATE changes the filter context under which an expression is evaluated.

---

## Why is data modeling important for DAX?

Because DAX relies on relationships and filter propagation within the data model.

---

# Key Takeaway

DAX transforms a Power BI model into an analytical engine.

A strong DAX foundation requires:

```
Data Modeling

+

Filter Context Understanding

+

Business Logic

+

Performance Optimization
```

Good DAX is not about writing complex formulas.

It is about creating accurate, reusable, and meaningful business metrics.