# Automation with Shell Scripts

## Overview

Shell scripting is the practice of writing a sequence of terminal commands into a reusable script file that can be executed automatically.

For analytics engineering projects, shell scripts help automate repetitive tasks such as:

* Creating project folder structures
* Setting up development environments
* Running data pipelines
* Executing dbt commands
* Running tests
* Generating documentation
* Exporting datasets
* Managing project files

Instead of manually typing multiple commands every time, a script allows engineers to execute a complete workflow with a single command.

---

# Why Automation Matters in Analytics Engineering

Analytics engineering involves many repeated processes:

1. Extracting or loading data
2. Cleaning and transforming datasets
3. Running SQL models
4. Testing data quality
5. Generating reports
6. Publishing outputs

Manually executing every step creates risks:

* Human errors
* Forgotten steps
* Inconsistent environments
* Slower development cycles

Automation creates reliable and repeatable workflows.

---

# Shell Script Basics

A shell script is usually stored with the `.sh` extension.

Example:

```
pipeline.sh
```

The first line specifies the interpreter:

```bash
#!/bin/bash
```

This is called the shebang.

It tells the operating system to execute the file using Bash.

---

# Creating a Shell Script

Create a file:

```bash
touch pipeline.sh
```

Open it:

```bash
nano pipeline.sh
```

Example:

```bash
#!/bin/bash

echo "Starting analytics pipeline"

echo "Pipeline completed"
```

Run the script:

```bash
bash pipeline.sh
```

---

# Making Scripts Executable

A script can also be executed directly.

Give it permission:

```bash
chmod +x pipeline.sh
```

Run:

```bash
./pipeline.sh
```

---

# Variables in Bash

Variables store reusable values.

Example:

```bash
PROJECT_NAME="SupportOps Intelligence"

echo $PROJECT_NAME
```

Output:

```
SupportOps Intelligence
```

Variables are useful for:

* File paths
* Database names
* Environment names
* Dates
* Configuration values

---

# Working With Paths

Example:

```bash
PROJECT_ROOT="/projects/supportops"

echo $PROJECT_ROOT
```

Using variables makes scripts easier to modify.

Instead of changing a path in multiple places, update it once.

---

# Conditional Statements

Scripts can make decisions.

Example:

```bash
if [ -f "data.csv" ]
then
    echo "File exists"
else
    echo "File not found"
fi
```

Common checks:

```bash
-f
```

Checks if a file exists.

```bash
-d
```

Checks if a directory exists.

---

# Loops in Bash

Loops repeat commands.

Example:

```bash
for file in *.csv
do
    echo $file
done
```

This prints every CSV file in the current directory.

---

# Automating Analytics Engineering Workflows

A common analytics engineering workflow:

```
Raw Data
    |
    v
Python Cleaning
    |
    v
DuckDB Loading
    |
    v
dbt Transformation
    |
    v
dbt Testing
    |
    v
Data Export
    |
    v
BI Dashboard
```

This workflow can be automated using Bash.

---

# Example: Running a dbt Pipeline

Example script:

```bash
#!/bin/bash

echo "Starting dbt pipeline"

cd dbt

dbt run

dbt test

echo "dbt pipeline completed successfully"
```

Instead of manually running:

```bash
dbt run
```

and:

```bash
dbt test
```

the entire workflow runs automatically.

---

# Example: Project Setup Automation

During the Analytics Engineering Playbook project, shell commands were used to create multiple folders and documentation files.

Instead of manually creating:

```
01_analytics_engineering_foundations
02_terminal_and_bash
03_python_for_analytics_engineering
...
```

a Bash script can automate the setup.

Example:

```bash
mkdir project_name

cd project_name

mkdir data dbt docs dashboards exports

touch README.md requirements.txt
```

This creates a repeatable project template.

---

# Environment Automation

Bash can automate Python environment setup.

Example:

```bash
python -m venv analytics-env

source analytics-env/bin/activate

pip install -r requirements.txt
```

For Windows:

```bash
analytics-env\Scripts\activate
```

---

# Combining Bash With Python

A common pattern is using Bash as the workflow controller.

Example:

```bash
#!/bin/bash

python python/load_data.py

python python/clean_data.py

python python/export_data.py
```

The Bash script controls the order while Python handles complex logic.

---

# Bash Automation Best Practices

## 1. Use Clear Names

Bad:

```
run.sh
```

Better:

```
run_dbt_pipeline.sh
```

---

## 2. Add Comments

Example:

```bash
# Run dbt transformations

dbt run
```

Comments make scripts easier to maintain.

---

## 3. Handle Errors

Example:

```bash
dbt run || exit 1
```

If dbt fails, the script stops.

---

## 4. Avoid Hardcoding Paths

Bad:

```bash
cd C:/Users/name/project
```

Better:

```bash
PROJECT_ROOT=$(pwd)
```

---

## 5. Store Scripts Separately

Professional projects often use:

```
scripts/

    setup_project.sh

    run_pipeline.sh

    export_data.sh
```

---

# Bash Skills Required for Analytics Engineers

An analytics engineer should understand:

## File Management

Commands:

```bash
ls
cd
mkdir
touch
rm
cp
mv
```

---

## Data Pipeline Execution

Commands:

```bash
python script.py

dbt run

dbt test
```

---

## Environment Management

Commands:

```bash
python -m venv

pip install

pip freeze
```

---

## Version Control

Commands:

```bash
git status

git add .

git commit

git push
```

---

# Recommended Learning Resources

## Books

**The Linux Command Line**

* Author: William Shotts

A complete guide to terminal usage, shell commands, and scripting.

**Classic Shell Scripting**

* Authors: Arnold Robbins and Nelson H. F. Beebe

A deeper guide to Bash scripting.

---

## Courses

**Linux Command Line Basics**

* Linux Foundation

**Bash Scripting and Shell Programming**

* Udemy courses by Jason Cannon

---

## YouTube Channels

* NetworkChuck
* freeCodeCamp
* Corey Schafer
* The Linux Foundation

---

# Practical Exercises

To master shell scripting:

1. Create a script that creates a project folder structure.

2. Create a script that:

   * activates a Python environment
   * installs dependencies
   * runs tests

3. Create a script that:

   * runs dbt models
   * runs dbt tests
   * exports tables

4. Create a complete analytics pipeline launcher:

```
./run_pipeline.sh
```

that executes the entire workflow from raw data to dashboard-ready outputs.

---

# Key Takeaway

Shell scripting is not about replacing Python or SQL.

In analytics engineering:

* SQL transforms data
* Python processes complex logic
* dbt manages transformations
* DuckDB stores and queries analytical data
* Bash connects everything into repeatable workflows

Strong analytics engineers understand how to automate the entire data lifecycle.
