# Python Automation Patterns for Analytics Engineering

## Overview

Automation is a core skill in analytics engineering.

A professional analytics engineer does not manually repeat tasks such as:

- Cleaning files
- Running transformations
- Exporting datasets
- Checking data quality
- Generating reports
- Updating documentation

Instead, these processes are converted into repeatable automated workflows.

Python is widely used for automation because it can interact with:

- Files and folders
- Databases
- APIs
- Operating systems
- Cloud services
- Data processing libraries

---

# Why Automation Matters

Without automation:

```

Download file
|
Open notebook
|
Clean data manually
|
Export file
|
Update dashboard
|
Repeat every week

```

With automation:

```

Scheduled Script

```
  ↓
```

Extract Data

```
  ↓
```

Transform Data

```
  ↓
```

Validate Quality

```
  ↓
```

Load Database

```
  ↓
```

Refresh Dashboard

````

---

# Common Analytics Engineering Automation Tasks

## Data ingestion

Automatically collect:

- CSV files
- Excel files
- API responses
- Database extracts

Example:

```python
import pandas as pd


def load_csv(path):

    return pd.read_csv(path)
````

---

## File processing

Example:

Find all CSV files:

```python
from pathlib import Path


files = Path("data/raw").glob("*.csv")


for file in files:

    print(file)
```

Useful for:

* Batch processing
* Data ingestion pipelines
* Data validation

---

# Working With File Systems

Python can create, move, rename, and delete files.

Example:

```python
from pathlib import Path


folder = Path("exports")


folder.mkdir(
    exist_ok=True
)
```

Creates:

```
exports/
```

---

## Rename Files

```python
from pathlib import Path


old = Path(
    "customer_data.csv"
)

new = Path(
    "customers_clean.csv"
)


old.rename(new)
```

---

## Moving Files

```python
import shutil


shutil.move(
    "file.csv",
    "archive/file.csv"
)
```

---

# Automating Data Exports

Example:

Exporting a DuckDB table:

```python
import duckdb


con = duckdb.connect(
    "analytics.duckdb"
)


con.execute(
"""
COPY customers
TO 'customers.parquet'
(FORMAT PARQUET)
"""
)
```

This pattern was used in:

```
SupportOps Intelligence

DuckDB
    |
    ↓
Python Export Script
    |
    ↓
Parquet Files
    |
    ↓
Power BI
```

---

# Configuration-Based Automation

Avoid hardcoding values:

Bad:

```python
database = "production.db"
```

Better:

```python
config = {

"database":
"analytics.duckdb",

"output":
"exports"

}
```

Even better:

Use configuration files.

Example:

```
config.yaml
```

```yaml
database:
  path: analytics.duckdb

export:
  format: parquet
```

---

# Environment Variables

Sensitive information should not be stored in code.

Bad:

```python
password="mypassword"
```

Good:

```
.env
```

Example:

```
DB_PASSWORD=my_password
```

Python:

```python
from dotenv import load_dotenv
import os


load_dotenv()


password = os.getenv(
    "DB_PASSWORD"
)
```

---

# Building Reusable Scripts

Avoid:

```
analysis_script_final_v2.py
```

Prefer:

```
python/

    extract.py
    transform.py
    load.py
    validate.py
```

Each script should have one responsibility.

---

# The Main Function Pattern

Professional Python scripts often use:

```python
def main():

    print(
        "Pipeline started"
    )


if __name__ == "__main__":

    main()
```

Benefits:

* Easier testing
* Cleaner execution
* Better reuse

---

# Command Line Arguments

Instead of editing scripts manually:

Bad:

```python
file="customers.csv"
```

Better:

Command:

```bash
python export.py customers.csv
```

Python:

```python
import sys


file = sys.argv[1]
```

---

# Using argparse

Professional approach:

```python
import argparse


parser = argparse.ArgumentParser()


parser.add_argument(
    "--file"
)


args = parser.parse_args()


print(args.file)
```

Run:

```bash
python pipeline.py --file data.csv
```

---

# Logging Automation Workflows

Print statements are not enough.

Bad:

```python
print("finished")
```

Better:

```python
import logging


logging.basicConfig(
    level=logging.INFO
)


logging.info(
    "Pipeline completed"
)
```

Logs provide:

* Execution history
* Debugging information
* Failure tracking

---

# Scheduling Automated Jobs

Automation requires scheduling.

Common options:

## Cron

Linux scheduler.

Example:

Run every day at midnight:

```bash
0 0 * * *
python pipeline.py
```

---

## Windows Task Scheduler

Useful for:

* Local automation
* Scheduled scripts

---

## Workflow Orchestration Tools

Production environments use:

### Airflow

For:

* Complex workflows
* Dependencies
* Scheduling

### Prefect

For:

* Modern Python workflows
* Simpler orchestration

### Dagster

For:

* Data assets
* Observability
* Testing

---

# Automation Testing

Automation without testing is dangerous.

Examples:

Check output exists:

```python
from pathlib import Path


assert Path(
    "output.parquet"
).exists()
```

---

Check data:

```python
assert len(df) > 0
```

---

# Automating dbt Workflows

A common analytics engineering workflow:

```bash
dbt run

dbt test

dbt docs generate
```

can be automated.

Example:

```python
import subprocess


commands = [

"dbt run",

"dbt test"

]


for command in commands:

    subprocess.run(
        command,
        shell=True
    )
```

---

# Automation in SupportOps Intelligence

Automation components:

## Data Loading

Script:

```
python/load_to_duckdb.py
```

Purpose:

* Create DuckDB database
* Load cleaned dataset

---

## Export Automation

Script:

```
python/export_to_parquet.py
```

Purpose:

* Export dbt models
* Prepare Power BI datasets

---

## Development Automation

Commands:

```bash
dbt run

dbt test

git commit

git push
```

represent a repeatable workflow.

---

# Best Practices

## 1. Make Scripts Idempotent

Running twice should not corrupt results.

Example:

Avoid:

```
INSERT duplicate records
```

Prefer:

```
CREATE OR REPLACE
```

or controlled incremental loads.

---

## 2. Use Clear Naming

Bad:

```
script2.py
```

Good:

```
export_dashboard_data.py
```

---

## 3. Add Documentation

Every script should explain:

* Purpose
* Inputs
* Outputs
* Usage

---

## 4. Handle Errors

Automation should fail safely.

Include:

* Try/except blocks
* Logs
* Alerts

---

# Skills To Master

## Python

Learn:

* pathlib
* subprocess
* argparse
* logging
* dotenv
* typing

## Operating Systems

Learn:

* File permissions
* Processes
* Environment variables
* Scheduling

## Data Engineering

Learn:

* Pipeline design
* Orchestration
* Monitoring
* Recovery strategies

## DevOps

Learn:

* Git workflows
* CI/CD
* Containers
* Deployment

---

# Recommended Resources

## Books

### Automate the Boring Stuff with Python

Author:
Al Sweigart

Focus:

* File automation
* Scripts
* Productivity workflows

### Python for DevOps

Authors:
Noah Gift and Kennedy Behrman

Focus:

* Automation
* Infrastructure scripting
* Cloud workflows

---

## Documentation

Python pathlib:

[https://docs.python.org/3/library/pathlib.html](https://docs.python.org/3/library/pathlib.html)

Python argparse:

[https://docs.python.org/3/library/argparse.html](https://docs.python.org/3/library/argparse.html)

Python logging:

[https://docs.python.org/3/library/logging.html](https://docs.python.org/3/library/logging.html)

---

## Courses

### Python Automation

Udemy:
Automate the Boring Stuff with Python

### Data Engineering Zoomcamp

DataTalksClub:

[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

# Summary

Automation transforms analytics engineering from manual analysis into reliable systems.

A strong analytics engineer should automate:

* Data ingestion
* Data cleaning
* Database loading
* Data validation
* dbt execution
* Export processes
* Documentation generation

Python, Bash, SQL, dbt, and workflow tools together create scalable analytics workflows.

