# File System Management for Analytics Engineering

## Overview

Analytics engineering projects contain many different types of files:

- Raw datasets
- Cleaned datasets
- Database files
- SQL models
- Python scripts
- Documentation
- Dashboard files
- Logs
- Configuration files

Managing these files properly is essential for building maintainable and professional projects.

A well-organized project structure allows engineers to:

- Find files quickly
- Understand project architecture
- Collaborate effectively
- Automate workflows
- Maintain reproducibility

This document covers file system management practices used in analytics engineering projects.

---

# 1. Understanding File Systems

A file system organizes data into:

```
Directories (Folders)

        ↓

Subdirectories

        ↓

Files
```

Example:

```
SupportOps Intelligence/

├── data/

├── database/

├── python/

├── dbt/

├── dashboards/

└── docs/
```

---

# 2. Navigating File Systems

## Current Directory

Command:

```bash
pwd
```

Example output:

```
/Desktop/Projects/SupportOps Intelligence
```

---

## List Files

Basic:

```bash
ls
```

Detailed:

```bash
ls -la
```

Recursive:

```bash
ls -R
```

Example:

```bash
ls -R .
```

Shows the complete project structure.

Used during:

- Project reviews
- Debugging
- Documentation

---

# 3. Creating Project Structures

Professional analytics projects should have clear organization.

Example:

```
analytics_project/

├── data/

│   ├── raw/

│   └── cleaned/

│

├── database/

├── python/

├── sql/

├── notebooks/

├── dashboards/

├── docs/

└── README.md
```

Create using:

```bash
mkdir data database python sql notebooks dashboards docs
```

---

# 4. Creating Nested Folders

Create multiple levels:

```bash
mkdir -p data/raw
```

Creates:

```
data/

└── raw/
```

The `-p` flag creates missing parent directories.

---

# 5. Creating Files

Create an empty file:

```bash
touch README.md
```

Examples:

```bash
touch architecture.md

touch requirements.txt

touch pipeline.py
```

---

# 6. Moving Between Project Areas

Example:

Move into dbt:

```bash
cd dbt
```

Move into models:

```bash
cd models
```

Move back:

```bash
cd ..
```

Return to project root:

```bash
cd ../..
```

---

# 7. Copying Files

Copy:

```bash
cp source destination
```

Example:

```bash
cp README.md docs/
```

Creates:

```
docs/

└── README.md
```

---

Copy folders:

```bash
cp -r folder1 folder2
```

The `-r` means recursive.

---

# 8. Moving and Renaming Files

Move:

```bash
mv file.md docs/
```

Rename:

```bash
mv old.md new.md
```

Example:

```
report_old.md

↓

report.md
```

---

# 9. Removing Files

Delete file:

```bash
rm file.txt
```

Delete folder:

```bash
rm -r folder_name
```

---

Be careful:

Unlike graphical systems, deleted files may not go to a recycle bin.

---

# 10. Managing Analytics Project Files

A typical analytics project contains:

## Data

```
data/

├── raw/

├── cleaned/

└── processed/
```

Purpose:

Store different data stages.

---

## Database

```
database/

└── project.duckdb
```

Purpose:

Local analytical database.

---

## Python

```
python/

├── ingestion.py

├── cleaning.py

└── export.py
```

Purpose:

Data processing scripts.

---

## dbt

```
dbt/

├── models/

├── tests/

├── snapshots/

└── seeds/
```

Purpose:

Analytics transformations.

---

## Documentation

```
docs/

├── architecture.md

├── data_dictionary.md

└── screenshots/
```

Purpose:

Project explanation.

---

# 11. Organizing dbt Projects

Recommended structure:

```
dbt/

├── models/

│   ├── staging/

│   ├── intermediate/

│   └── marts/

│

├── tests/

├── snapshots/

├── seeds/

└── dbt_project.yml
```

---

## Staging

Contains:

- Source cleaning
- Column renaming
- Type conversions

Example:

```
stg_ticket.sql
```

---

## Intermediate

Contains:

- Business logic
- Reusable transformations

Example:

```
int_ticket_metrics.sql
```

---

## Marts

Contains:

- Fact tables
- Dimension tables
- Reporting models

Example:

```
fact_ticket.sql

dim_customer.sql
```

---

# 12. Managing Generated Files

Many tools create generated files.

Examples:

dbt:

```
target/
logs/
```

Python:

```
__pycache__/
```

Jupyter:

```
.ipynb_checkpoints/
```

These usually should not be committed.

---

# 13. Using .gitignore

`.gitignore` tells Git which files to ignore.

Example:

```
target/

logs/

__pycache__/

.env

*.duckdb
```

---

Common ignored analytics files:

## Database Files

```
*.duckdb
*.sqlite
```

---

## Temporary Files

```
*.tmp
*.log
```

---

## Python Cache

```
__pycache__/
```

---

## Environment Files

```
.env
```

---

# 14. Empty Folders and .gitkeep

Git does not track empty folders.

Example:

```
tests/

snapshots/

seeds/
```

If you need to preserve them:

Create:

```bash
touch folder/.gitkeep
```

Example:

```bash
touch tests/.gitkeep
```

---

However, unnecessary empty folders can be removed.

---

# 15. File Naming Best Practices

Use:

- lowercase
- underscores
- descriptive names

Good:

```
customer_support_tickets.csv

fact_ticket.sql

data_dictionary.md
```

Avoid:

```
FinalReport2.xlsx

newfile.sql

DataFile.csv
```

---

# 16. Managing Project Versions

Projects evolve.

Example:

Version history:

```
Project v1

↓

Project v2

↓

Project v3
```

Git manages these changes.

---

# 17. Separating Code and Data

Do not mix:

```
python/

customers.csv
```

Better:

```
python/

clean_data.py


data/

customers.csv
```

---

Principle:

```
Code transforms data

Data is stored separately
```

---

# 18. File Management Workflow Used in SupportOps Intelligence

The project followed:

```
Create Project Structure

↓

Add Raw Data

↓

Clean Data

↓

Create Database

↓

Create dbt Project

↓

Create Models

↓

Create Documentation

↓

Create Dashboard

↓

Prepare Repository
```

Final structure:

```
SupportOps Intelligence/

├── dashboards/

├── data/

├── database/

├── dbt/

├── docs/

├── exports/

├── notebooks/

├── python/

├── README.md

└── requirements.txt
```

---

# 19. Professional Repository Structure

A strong analytics repository should communicate:

## Data Flow

Where data comes from.

---

## Code

How transformations happen.

---

## Documentation

Why decisions were made.

---

## Outputs

What users consume.

---

Example:

```
project/

├── data/

├── models/

├── scripts/

├── docs/

├── dashboards/

└── README.md
```

---

# 20. Recommended File Management Checklist

Before sharing a project:

## Structure

- [ ] Folders have clear names
- [ ] Files are organized
- [ ] No unnecessary duplicates

---

## Data

- [ ] Raw and processed data separated
- [ ] Sensitive data removed
- [ ] Large files ignored

---

## Code

- [ ] Scripts organized
- [ ] Requirements documented
- [ ] Configuration separated

---

## Documentation

- [ ] README exists
- [ ] Architecture documented
- [ ] Data dictionary available

---

# Recommended Resources

## Books

### The Linux Command Line

Author:

William Shotts

---

### Data Engineering with Python

Author:

Paul Crickard

---

## Documentation

Git Documentation:

https://git-scm.com/doc

Linux File System:

https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

---

# Summary

File system management is a fundamental skill for analytics engineers.

A clean structure improves:

- Development speed
- Collaboration
- Debugging
- Documentation
- Deployment

Professional analytics engineers treat project organization as part of engineering quality, not just personal preference.