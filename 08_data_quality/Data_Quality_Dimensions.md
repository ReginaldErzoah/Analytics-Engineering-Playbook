# Data Quality Dimensions

## Overview

Data quality dimensions are the different ways organizations measure whether data is trustworthy and suitable for business use.

A dataset can be considered high quality only when it satisfies multiple dimensions.

For example, a customer support dataset may be:

- Complete but inaccurate
- Accurate but outdated
- Consistent but duplicated

Therefore, data quality must be evaluated from multiple perspectives.

---

# Core Data Quality Dimensions

The most common data quality dimensions are:

```
Accuracy

Completeness

Consistency

Validity

Uniqueness

Timeliness

Integrity
```

---

# 1. Accuracy

## Definition

Accuracy measures whether data correctly represents the real-world object or event it describes.

The question:

> "Is this data correct?"

---

## Example

Real customer information:

```
Customer Name:

John Mensah

Location:

Accra
```

Database:

|customer_name|location|
|-|-|
|John Mensah|Accra|

High accuracy.

---

Incorrect:

|customer_name|location|
|-|-|
|John Mensah|Kumasi|

The data exists but represents the wrong information.

---

## Causes of Inaccuracy

Common causes:

- Manual entry errors
- Incorrect data imports
- Outdated information
- Integration issues

---

## Improving Accuracy

Methods:

- Validation rules
- Reference data checks
- Source system improvements
- User input restrictions

---

# 2. Completeness

## Definition

Completeness measures whether all required data is available.

The question:

> "Is anything missing?"

---

## Example

Customer table:

|customer_id|email|phone|
|-|-|-|
|101|john@email.com|0240000000|
|102|null|0200000000|

The second record is incomplete.

---

## Measuring Completeness

Example:

Required fields:

```
customer_id

email

created_date
```

Check:

```sql
SELECT *

FROM customers

WHERE email IS NULL;
```

---

## Causes of Missing Data

Examples:

- Failed integrations
- Optional fields
- User mistakes
- System limitations

---

# 3. Consistency

## Definition

Consistency ensures that data follows the same rules across different systems.

The question:

> "Does the same data mean the same thing everywhere?"

---

## Example

CRM System:

```
Country:

Ghana
```

Analytics Database:

```
Country:

GH
```

Potential inconsistency.

---

## Types of Consistency

## Format Consistency

Example:

Dates:

System A:

```
2026-01-01
```

System B:

```
01/01/2026
```

---

## Value Consistency

Example:

Customer status:

System A:

```
Active
```

System B:

```
ACTIVE
```

---

## Improving Consistency

Methods:

- Standard definitions
- Data dictionaries
- Transformation rules
- Master data management

---

# 4. Validity

## Definition

Validity checks whether data follows predefined rules and formats.

The question:

> "Does this value follow the expected requirements?"

---

## Examples

Age:

Valid:

```
35
```

Invalid:

```
-10
```

---

Email:

Valid:

```
user@email.com
```

Invalid:

```
user@email
```

---

## Validation Rules

Examples:

Range:

```
Satisfaction Score:

1 - 5
```

Format:

```
Email pattern
```

Allowed values:

```
Status:

Open

Closed

Pending
```

---

# 5. Uniqueness

## Definition

Uniqueness ensures that records are not duplicated.

The question:

> "Does each record represent one unique entity?"

---

## Example

Customer table:

|customer_id|name|
|-|-|
|101|John|
|101|John|

Problem:

Duplicate customer.

---

## Detecting Duplicates

SQL:

```sql
SELECT

customer_id,

COUNT(*)

FROM customers

GROUP BY customer_id

HAVING COUNT(*) > 1;
```

---

## Improving Uniqueness

Methods:

- Primary keys
- Unique constraints
- Deduplication pipelines

---

# 6. Timeliness

## Definition

Timeliness measures whether data is available and updated when required.

The question:

> "Is the data available at the right time?"

---

## Example

Business requirement:

```
Daily sales dashboard

available at 8 AM
```

Actual:

```
Available at 5 PM
```

The data may be accurate but not timely.

---

## Timeliness Metrics

Examples:

- Data freshness
- Pipeline delay
- Update frequency

---

# 7. Integrity

## Definition

Integrity ensures relationships between datasets remain correct.

The question:

> "Are connections between data entities maintained?"

---

## Example

Orders table:

|order_id|customer_id|
|-|-|
|1001|50|

Customers table:

|customer_id|
|-|
|50|

Relationship is valid.

---

Invalid:

Orders:

|order_id|customer_id|
|-|-|
|1001|999|

Customer 999 does not exist.

---

## Integrity Checks

Examples:

- Foreign key validation
- Relationship tests
- Referential integrity checks

---

# 8. Availability

## Definition

Availability measures whether data can be accessed when needed.

The question:

> "Can users access the data?"

---

Examples:

High availability:

```
Dashboard available 24/7
```

Low availability:

```
Dashboard unavailable every morning
```

---

# 9. Traceability

## Definition

Traceability measures whether users can understand where data came from.

The question:

> "Can we track the origin of this data?"

---

Example:

Dashboard metric:

```
Customer Satisfaction Score
```

Should be traceable to:

```
Support Tickets Table

↓

Transformation Model

↓

Dashboard Metric
```

---

# Data Quality Dimension Summary

|Dimension|Question|Example Issue|
|-|-|-|
|Accuracy|Is it correct?|Wrong customer location|
|Completeness|Is anything missing?|Missing email|
|Consistency|Is it standardized?|Different country formats|
|Validity|Does it follow rules?|Invalid date|
|Uniqueness|Are records duplicated?|Duplicate customers|
|Timeliness|Is it available on time?|Late dashboard refresh|
|Integrity|Are relationships correct?|Missing customer reference|
|Availability|Can users access it?|Dashboard downtime|
|Traceability|Can we track the source?|Unknown metric calculation|

---

# Data Quality Dimensions in Analytics Engineering

Analytics engineers apply these dimensions through:

- dbt tests
- SQL checks
- Data contracts
- Documentation
- Monitoring systems

Example:

Customer Support Analytics:

|Quality Dimension|Implementation|
|-|-|
|Completeness|not_null tests|
|Uniqueness|unique key tests|
|Validity|accepted_values tests|
|Integrity|relationship tests|
|Timeliness|freshness monitoring|
|Traceability|dbt documentation|

---

# Interview Questions

## What are the main dimensions of data quality?

Accuracy, completeness, consistency, validity, uniqueness, timeliness, and integrity.

---

## Can data be complete but inaccurate?

Yes.

Example:

A customer address exists but contains the wrong location.

---

## Why is timeliness important?

Because outdated data can lead to incorrect operational decisions.

---

## How do you measure data quality?

Through automated tests, validation rules, monitoring, and business checks.

---

# Key Takeaway

High-quality data is not only about having data.

It is about having data that is:

```
Correct

Complete

Consistent

Valid

Unique

Available

Reliable
```

Trusted analytics begins with trusted data.