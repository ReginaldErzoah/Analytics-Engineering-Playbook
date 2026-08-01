# Python Environment Management for Analytics Engineering

## Overview

Python is one of the most widely used programming languages in analytics engineering because it enables data engineers and analysts to automate workflows, clean data, build pipelines, perform analysis, and integrate different technologies.

In an analytics engineering project, Python is commonly used alongside tools such as:

- SQL
- DuckDB
- dbt
- Pandas
- Jupyter Notebooks
- Cloud platforms
- Data orchestration tools

A professional analytics engineering workflow requires managing Python environments properly to ensure:

- Dependencies do not conflict
- Projects are reproducible
- Other developers can run the project
- Production deployments remain stable


---

# 1. Why Python Environment Management Matters

A Python environment defines the packages, versions, and configurations available to a project.

Without environment management, projects often encounter problems such as:

- "It works on my machine" issues
- Package version conflicts
- Broken dependencies after updates
- Difficulty reproducing analysis
- Deployment failures


Example:

A project may require:

```
pandas==3.0.5
duckdb==1.5.5
dbt-duckdb==1.10.1
```

Another project may require an older version:

```
pandas==2.2.0
duckdb==1.2.0
```

Installing everything globally can create conflicts.

Virtual environments solve this problem by isolating dependencies per project.


---

# 2. Python Installation

Before creating environments, verify Python installation:

```bash
python --version
```

Example output:

```
Python 3.12.10
```

Check where Python is installed:

```bash
which python
```

Windows:

```bash
where python
```


---

# 3. Virtual Environments

A virtual environment is an isolated Python installation for a specific project.

Each project receives its own:

- Python interpreter
- Installed packages
- Package versions


Example project:

```
analytics-project/

├── .venv/
├── notebooks/
├── python/
├── requirements.txt
└── README.md
```


---

# 4. Creating a Virtual Environment

Create a virtual environment:

```bash
python -m venv .venv
```

This creates:

```
.venv/
```

containing the isolated Python environment.


---

# 5. Activating a Virtual Environment

## Windows Command Prompt

```bash
.venv\Scripts\activate
```


## Windows Git Bash

```bash
source .venv/Scripts/activate
```


## Mac/Linux

```bash
source .venv/bin/activate
```


After activation:

```
(.venv)
```

appears before the terminal prompt.


Example:

```
(.venv) user@computer project $
```


---

# 6. Deactivating an Environment

To leave the environment:

```bash
deactivate
```


---

# 7. Installing Packages

Python packages are installed using `pip`.

Example:

```bash
pip install pandas
```

Multiple packages:

```bash
pip install pandas numpy duckdb
```


---

# 8. Requirements.txt

A `requirements.txt` file records project dependencies.

Example:

```
pandas==3.0.5
numpy==2.5.1
duckdb==1.5.5
dbt-duckdb==1.10.1
seaborn==0.13.2
```

This allows another developer to recreate the environment.


---

# 9. Generating Requirements.txt

After installing packages:

```bash
pip freeze > requirements.txt
```


Example:

```
pip freeze
```

Output:

```
pandas==3.0.5
numpy==2.5.1
duckdb==1.5.5
```


---

# 10. Installing Dependencies from Requirements.txt

A new developer can recreate the environment:

```bash
pip install -r requirements.txt
```


Workflow:

```
Clone repository
        |
        ↓
Create virtual environment
        |
        ↓
Activate environment
        |
        ↓
Install requirements
        |
        ↓
Run project
```


---

# 11. Environment Management in the SupportOps Intelligence Project

The SupportOps Intelligence project used a dedicated environment:

```
dbt-env
```

The environment contained:

## Analytics Libraries

- pandas
- numpy
- matplotlib
- seaborn


## Database Tools

- duckdb


## Analytics Engineering Tools

- dbt-core
- dbt-duckdb


## Development Tools

- Jupyter
- ipykernel


Example:

```bash
(dbt-env)
reginald@computer $
```


---

# 12. Checking Installed Packages

View installed packages:

```bash
pip list
```


View detailed versions:

```bash
pip freeze
```


Find a package:

```bash
pip show pandas
```


---

# 13. Updating Packages

Update a package:

```bash
pip install --upgrade pandas
```


Update all packages:

```bash
pip list --outdated
```


However, production analytics projects should avoid unnecessary upgrades because changes may break pipelines.


---

# 14. Managing Package Versions

Always pin important versions.

Instead of:

```
pandas
```

Use:

```
pandas==3.0.5
```


Version pinning improves:

- Reproducibility
- Stability
- Debugging
- Collaboration


---

# 15. Python Project Structure

A professional analytics engineering project may contain:

```
project/

├── data/
│
├── notebooks/
│
├── python/
│
├── dbt/
│
├── database/
│
├── dashboards/
│
├── docs/
│
├── requirements.txt
│
└── README.md
```


Python scripts should be separated from notebooks.

Example:

```
python/

├── load_data.py
├── transform_data.py
└── export_data.py
```


---

# 16. Jupyter Notebook Environments

Jupyter notebooks require a kernel connected to the correct environment.

Install kernel support:

```bash
pip install ipykernel
```


Register environment:

```bash
python -m ipykernel install --user --name analytics-env
```


Select the kernel inside Jupyter:

```
Kernel → Change Kernel → analytics-env
```


---

# 17. Common Environment Problems

## Wrong Python Interpreter

Problem:

```
ModuleNotFoundError
```

Solution:

Check Python:

```bash
where python
```


Confirm environment activation.


---

## Package Installed but Notebook Cannot Find It

Cause:

Notebook using another kernel.

Solution:

Install kernel:

```bash
python -m ipykernel install --user
```


---

## Requirements File Contains Too Many Packages

`pip freeze` captures everything installed.

For production projects:

Separate:

```
requirements.txt
```

from:

```
requirements-dev.txt
```


Example:

Production:

```
pandas
duckdb
dbt-duckdb
```


Development:

```
jupyter
black
ruff
pytest
```


---

# 18. Best Practices

## Always Use Virtual Environments

Never install project dependencies globally.


## Keep Requirements Updated

Update after major changes:

```bash
pip freeze > requirements.txt
```


## Document Setup Steps

Every repository should explain:

1. Python version
2. Environment creation
3. Dependency installation
4. Running instructions


## Avoid Random Package Installation

Before installing a package:

- Confirm it solves a problem
- Check maintenance status
- Check compatibility


---

# 19. Recommended Learning Resources

## Documentation

Python Virtual Environments:

https://docs.python.org/3/tutorial/venv.html


pip Documentation:

https://pip.pypa.io/


## Books

### Python Crash Course
Author:
Eric Matthes

Focus:
- Python fundamentals
- Projects
- Automation


### Effective Python
Author:
Brett Slatkin

Focus:
- Professional Python practices


## Courses

### Python for Everybody
Author:
Dr. Charles Severance

Platform:
Coursera


### Automate the Boring Stuff with Python

Author:
Al Sweigart

Focus:
- File automation
- Scripts
- Productivity workflows


---

# Summary

Python environment management is a fundamental skill for analytics engineers.

A strong workflow includes:

1. Creating isolated environments
2. Managing dependencies with pip
3. Maintaining requirements files
4. Using correct Jupyter kernels
5. Structuring Python projects professionally
6. Ensuring reproducibility across machines

These practices allow analytics engineers to build reliable, maintainable, and production-ready data systems.