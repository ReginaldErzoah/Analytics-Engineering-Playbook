# DuckDB With Python

## Overview

DuckDB and Python work extremely well together because they combine two powerful analytics capabilities:

- Python provides data manipulation, automation, and machine learning capabilities.
- DuckDB provides high-performance SQL analytics and database operations.

This combination allows analytics engineers to build complete data workflows:

```

Data Sources
|
↓
Python Data Processing
|
↓
DuckDB Analytics Database
|
↓
SQL Transformations
|
↓
BI Reporting / Machine Learning

````

In the SupportOps Intelligence Analytics project, Python was used for data cleaning, automation, and exporting, while DuckDB served as the analytical storage and querying layer.

---

# Why Combine DuckDB and Python?

Python alone is excellent for:

- Data cleaning
- Feature engineering
- Automation
- Machine learning

However, large analytical operations can become inefficient when performed entirely with Python.

SQL databases are better suited for:

- Aggregations
- Joins
- Filtering
- Grouping
- Analytical transformations

DuckDB provides the SQL layer while Python provides the programming layer.

Together they create a powerful analytics engineering workflow.

---

# Installing DuckDB Python Package

Install DuckDB:

```bash
pip install duckdb
````

Verify installation:

```python
import duckdb

print(duckdb.__version__)
```

---

# Creating a DuckDB Connection

DuckDB supports two connection modes.

## In-Memory Database

Used for temporary analysis.

```python
import duckdb


connection = duckdb.connect()
```

Data disappears when the Python session ends.

---

## Persistent Database

Used for analytics projects.

```python
import duckdb


connection = duckdb.connect(
    "supportops.duckdb"
)
```

This creates:

```
supportops.duckdb
```

The database persists between sessions.

---

# Running SQL From Python

DuckDB allows SQL execution directly inside Python.

Example:

```python
import duckdb


connection = duckdb.connect(
    "supportops.duckdb"
)


result = connection.execute(
    """
    SELECT *
    FROM fact_ticket
    LIMIT 5
    """
)


print(result)
```

---

# Returning Query Results as DataFrames

DuckDB can convert query results directly into Pandas DataFrames.

Example:

```python
import duckdb


df = duckdb.sql(
    """
    SELECT *
    FROM fact_ticket
    LIMIT 10
    """
).df()


print(df.head())
```

This makes DuckDB convenient for analytics notebooks.

---

# Querying Pandas DataFrames With DuckDB

DuckDB can directly query Python variables.

Example:

```python
import pandas as pd
import duckdb


tickets = pd.read_csv(
    "customer_support_tickets_clean.csv"
)


result = duckdb.sql(
    """
    SELECT

        priority_level,

        COUNT(*) AS total_tickets

    FROM tickets

    GROUP BY priority_level
    """
)


print(result)
```

DuckDB automatically detects the DataFrame variable.

---

# Reading CSV Files With Python and DuckDB

Example:

```python
import duckdb


query = """

SELECT *

FROM read_csv_auto(
    'data/customer_support_tickets_clean.csv'
)

LIMIT 10

"""


result = duckdb.sql(query)

print(result)
```

DuckDB handles:

* File reading
* Schema detection
* Query execution

without requiring a separate import step.

---

# Reading Parquet Files

Parquet is commonly used in analytics engineering.

Example:

```python
import duckdb


df = duckdb.sql(
    """

    SELECT *

    FROM 'exports/fact_ticket.parquet'

    LIMIT 10

    """
).df()


print(df)
```

Benefits:

* No loading required
* Faster analytical queries
* Lower storage size

---

# Writing DataFrames Into DuckDB

Python DataFrames can be stored as database tables.

Example:

```python
import pandas as pd
import duckdb


df = pd.read_csv(
    "tickets.csv"
)


connection = duckdb.connect(
    "supportops.duckdb"
)


connection.execute(
    """

    CREATE TABLE tickets AS

    SELECT *

    FROM df

    """
)
```

---

# Building Data Pipelines With DuckDB and Python

A typical analytics engineering pipeline:

```
Extract
  |
  ↓
Python Script
  |
  ↓
Clean Data
  |
  ↓
Load Into DuckDB
  |
  ↓
dbt Transformation
  |
  ↓
Analytics Tables
  |
  ↓
Power BI
```

---

# Example Project Pipeline

## Step 1: Extract Data

Python loads source data.

```python
import pandas as pd


df = pd.read_csv(
    "customer_support_tickets.csv"
)
```

---

## Step 2: Clean Data

Example:

```python
df["customer_email"] = (
    df["customer_email"]
    .str.lower()
)
```

---

## Step 3: Load Into DuckDB

```python
import duckdb


connection = duckdb.connect(
    "supportops.duckdb"
)


connection.execute(
    """

    CREATE OR REPLACE TABLE raw_tickets AS

    SELECT *

    FROM df

    """
)
```

---

## Step 4: Transform With dbt

dbt models query DuckDB tables:

```
raw_tickets

      ↓

stg_ticket

      ↓

int_ticket_metrics

      ↓

fact_ticket
```

---

# Automating DuckDB Workflows

Python scripts can automate:

* Database creation
* Data loading
* Exports
* Quality checks

Example structure:

```
python/

├── load_data.py

├── clean_data.py

├── export_tables.py

└── run_pipeline.py
```

---

# Exporting DuckDB Results Using Python

Example:

```python
from pathlib import Path
import duckdb


database = "supportops.duckdb"

output = Path(
    "exports/fact_ticket.parquet"
)


connection = duckdb.connect(database)


connection.execute(
    f"""

    COPY fact_ticket

    TO '{output}'

    (FORMAT PARQUET)

    """
)


connection.close()
```

This pattern was used in SupportOps Intelligence Analytics.

---

# Error Handling in DuckDB Python Pipelines

Production scripts should handle failures.

Example:

```python
import duckdb


try:

    connection = duckdb.connect(
        "supportops.duckdb"
    )

    connection.execute(
        "SELECT COUNT(*) FROM fact_ticket"
    )


except Exception as error:

    print(error)


finally:

    connection.close()
```

---

# Environment Management

Analytics projects should use virtual environments.

Example:

```bash
python -m venv analytics-env
```

Activate:

Windows:

```bash
analytics-env\Scripts\activate
```

Install dependencies:

```bash
pip install duckdb pandas
```

Save:

```bash
pip freeze > requirements.txt
```

---

# DuckDB Python Skills To Master

## Python Fundamentals

Learn:

* Functions
* Modules
* Classes
* File handling
* Error handling

---

## Pandas

Learn:

* Data cleaning
* Data transformation
* Missing values
* Data types
* Feature engineering

---

## SQL Integration

Learn:

* Executing SQL from Python
* Parameterized queries
* Query automation
* Database connections

---

## Data Engineering Patterns

Learn:

* ETL pipelines
* ELT pipelines
* Batch processing
* Data validation
* Logging

---

# Resources

## Documentation

DuckDB Python API:

[https://duckdb.org/docs/api/python/overview](https://duckdb.org/docs/api/python/overview)

Pandas Documentation:

[https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)

---

## Books

### Python for Data Analysis

Author:

Wes McKinney

Focus:

* Pandas
* Data manipulation
* Data workflows

### Effective Python

Author:

Brett Slatkin

Focus:

* Writing professional Python code

---

## Courses

### Data Analysis With Python

freeCodeCamp

[https://www.freecodecamp.org/learn/data-analysis-with-python/](https://www.freecodecamp.org/learn/data-analysis-with-python/)

### Data Engineering Zoomcamp

DataTalksClub

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

# Summary

DuckDB with Python creates a complete analytics engineering environment.

Python handles:

* Data extraction
* Cleaning
* Automation
* Pipeline execution

DuckDB handles:

* Storage
* SQL analytics
* Transformations
* Analytical workloads

Together they allow analytics engineers to build scalable, reproducible, and production-style data workflows locally before moving to cloud platforms.
