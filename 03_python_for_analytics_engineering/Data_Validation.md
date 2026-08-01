# Data Validation With Python

## Overview

Data validation is the process of checking whether data meets expected standards before it is used for analysis, reporting, or machine learning.

The goal of validation is to ensure data is:

- Accurate
- Complete
- Consistent
- Reliable
- Suitable for business use

In analytics engineering, validation prevents incorrect data from reaching:

- Data warehouses
- BI dashboards
- Machine learning models
- Business reports

---

# Why Data Validation Matters

Bad data creates bad decisions.

Example:

A sales dashboard shows:

```
Revenue: $10M
```

But validation discovers:

```
Duplicate transactions inflated revenue by 30%
```

The dashboard is technically correct but business-wise incorrect.

---

# Data Validation Workflow

A typical validation process:

```
Data Source

      ↓

Schema Validation

      ↓

Quality Checks

      ↓

Business Rule Validation

      ↓

Data Acceptance

      ↓

Analytics Usage
```

---

# Types of Data Validation

The main validation categories are:

```
Schema Validation

Completeness Validation

Accuracy Validation

Consistency Validation

Uniqueness Validation

Business Rule Validation
```

---

# 1. Schema Validation

## Definition

Schema validation checks whether data structure matches expectations.

Checks:

- Column names
- Data types
- Required fields
- Number of columns

---

Example expected schema:

```
customer_id    INTEGER

email          STRING

created_date   DATE

revenue        FLOAT
```

---

Python example:

```python
expected_columns = [

"customer_id",

"email",

"revenue"

]


missing_columns = set(

expected_columns

) - set(

df.columns

)


print(missing_columns)
```

---

# Data Type Validation

Check:

```python
df.dtypes
```

Example:

Expected:

```
Revenue → float
```

Received:

```
Revenue → object
```

---

Fix:

```python
df["Revenue"] = pd.to_numeric(

df["Revenue"]

)
```

---

# 2. Completeness Validation

## Definition

Checks whether required data exists.

Example:

Customers must have:

```
Customer ID

Email

Country
```

---

Check missing values:

```python
df.isnull().sum()
```

---

Example:

```python
missing_email = df["email"].isnull().sum()
```

---

Completeness rule:

```
Email cannot be NULL
```

---

# 3. Accuracy Validation

## Definition

Checks whether values are correct.

Examples:

Invalid:

```
Age = -5
```

Valid:

```
Age = 25
```

---

Example:

```python
invalid_age = df[

df["Age"] < 0

]
```

---

# 4. Consistency Validation

## Definition

Ensures data follows the same format.

Example:

Country values:

```
Ghana

GH

ghana
```

Should become:

```
Ghana
```

---

Python:

```python
df["Country"] = (

df["Country"]

.str

.lower()

)
```

---

# 5. Uniqueness Validation

## Definition

Checks whether unique fields contain duplicates.

Examples:

- Customer IDs
- Transaction IDs
- Employee IDs

---

Check duplicates:

```python
duplicates = df[

df.duplicated(

"customer_id"

)

]
```

---

Example rule:

```
customer_id must be unique
```

---

# 6. Business Rule Validation

## Definition

Checks whether data follows business logic.

Examples:

Orders:

```
Quantity > 0
```

Payments:

```
Amount >= 0
```

Dates:

```
Delivery Date >= Order Date
```

---

Example:

```python
invalid_orders = df[

df["Quantity"] <= 0

]
```

---

# Data Quality Dimensions

Data validation supports common data quality dimensions.

---

# Completeness

Question:

```
Is required data present?
```

Example:

Missing customer email.

---

# Accuracy

Question:

```
Is the data correct?
```

Example:

Wrong customer age.

---

# Consistency

Question:

```
Does data follow standards?
```

Example:

Different country formats.

---

# Timeliness

Question:

```
Is data available when needed?
```

Example:

Delayed daily refresh.

---

# Validity

Question:

```
Does data follow rules?
```

Example:

Invalid date format.

---

# Uniqueness

Question:

```
Are duplicate records present?
```

Example:

Duplicate transactions.

---

# Validation Using Pandas

## Check Missing Values

```python
df.isnull().sum()
```

---

## Check Duplicates

```python
df.duplicated().sum()
```

---

## Check Value Ranges

Example:

```python
df["Age"].between(

18,

100

)
```

---

## Check Unique Values

```python
df["Country"].unique()
```

---

# Creating Validation Functions

Reusable validation:

```python
def validate_sales(df):

    assert df["Revenue"].notnull().all()

    assert df["Quantity"].gt(0).all()

    assert df["Order_ID"].is_unique

    return True
```

---

# Using Great Expectations

Great Expectations is a popular data quality framework.

Used for:

- Automated testing
- Data profiling
- Pipeline validation

Example:

Expectation:

```
Revenue column should never be NULL
```

---

# Validation In ETL Pipelines

Example:

```
Extract Data

      ↓

Validate Raw Data

      ↓

Transform Data

      ↓

Validate Output

      ↓

Load Warehouse
```

---

# Data Validation In Analytics Engineering

Common tools:

|Tool|Purpose|
|-|-|
|Pandas|Custom validation scripts|
|Great Expectations|Data quality testing|
|dbt Tests|Warehouse validation|
|Soda|Data monitoring|
|Monte Carlo|Data observability|

---

# dbt Data Tests

Analytics engineers commonly validate warehouse models.

Example:

Unique test:

```yaml
tests:

  - unique

```

---

Not Null test:

```yaml
tests:

  - not_null
```

---

Relationships test:

```yaml
tests:

  - relationships
```

---

# Validation Automation

A production pipeline may:

```
Run Pipeline

      ↓

Execute Tests

      ↓

If Passed

      ↓

Publish Data

```

If failed:

```
Stop Pipeline

      ↓

Send Alert

      ↓

Investigate Issue
```

---

# Common Data Validation Mistakes

## 1. Validating Only At The End

Problem:

Errors spread through the pipeline.

Solution:

Validate early.

---

## 2. Checking Only Missing Values

Problem:

Data can be complete but incorrect.

Example:

```
Revenue = -5000
```

---

## 3. No Business Rules

Technical validation alone is insufficient.

---

## 4. Manual Validation

Manual checks do not scale.

Solution:

Automate validation.

---

# Interview Questions

## What is data validation?

Data validation is the process of ensuring data meets predefined quality and business requirements.

---

## Difference between validation and cleaning?

Cleaning modifies data to fix problems.

Validation checks whether data meets expected standards.

---

## Why validate data pipelines?

To prevent incorrect or unreliable data from reaching downstream users.

---

## What validation tools have you used?

Examples:

- Pandas
- Great Expectations
- dbt tests
- SQL checks

---

# Key Takeaway

Data validation creates trust in analytics systems.

A reliable data workflow requires:

```
Expected Rules

+

Automated Checks

+

Monitoring

+

Quick Feedback
```

Validated data becomes trusted data.