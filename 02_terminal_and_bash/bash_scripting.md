# Bash Scripting for Analytics Engineering

## Overview

Bash scripting allows analytics engineers to automate repetitive tasks by combining multiple terminal commands into reusable programs.

Instead of manually executing:

```bash
python load_to_duckdb.py

dbt run

dbt test

python export_to_parquet.py
```

an engineer can create one script:

```bash
run_pipeline.sh
```

that executes the entire workflow automatically.

Bash scripting is especially useful for:

- Data pipeline automation
- Environment setup
- Project initialization
- File management
- Deployment workflows
- Scheduled tasks

---

# 1. What is Bash?

Bash (Bourne Again Shell) is a command interpreter used to execute commands on Unix-based systems and environments such as Git Bash.

A Bash script is simply a file containing terminal commands.

Example:

```bash
#!/bin/bash

echo "Hello Analytics Engineering"
```

---

# 2. Creating a Bash Script

Create a file:

```bash
touch script.sh
```

Example:

```bash
touch run_pipeline.sh
```

Open:

```bash
nano run_pipeline.sh
```

---

# 3. The Shebang

Every Bash script should begin with:

```bash
#!/bin/bash
```

This tells the system which interpreter should execute the script.

Example:

```bash
#!/bin/bash

echo "Running pipeline"
```

---

# 4. Running Bash Scripts

A script needs execution permission.

Linux/macOS:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

---

In Git Bash on Windows:

```bash
bash script.sh
```

---

# 5. Printing Messages

Use:

```bash
echo
```

Example:

```bash
echo "Starting data pipeline"
```

Output:

```
Starting data pipeline
```

Useful for:

- Progress tracking
- Debugging
- Logging

---

# 6. Variables

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

---

# 7. Project Paths

Variables are useful for paths.

Example:

```bash
PROJECT_ROOT="/Desktop/Projects/Analytics"

cd $PROJECT_ROOT
```

Instead of repeating long paths.

---

# 8. Command Substitution

Store command results.

Example:

```bash
DATE=$(date)

echo $DATE
```

Output:

```
Fri Aug 1 2026
```

Useful for:

- File naming
- Logging
- Reports

---

# 9. User Inputs

Scripts can accept inputs.

Example:

```bash
echo "Enter project name"

read PROJECT

echo "Creating $PROJECT"
```

---

# 10. Conditional Statements

Bash supports decision-making.

Example:

```bash
if [ -f "README.md" ]

then

    echo "README exists"

else

    echo "README missing"

fi
```

---

Common conditions:

Check file:

```bash
-f
```

Check folder:

```bash
-d
```

Compare values:

```bash
=
```

---

# 11. Loops

Loops repeat commands.

Example:

```bash
for file in *.csv

do

    echo $file

done
```

Output:

```
customers.csv
tickets.csv
sales.csv
```

---

# 12. Functions

Functions organize scripts.

Example:

```bash
load_data(){

    python load_to_duckdb.py

}
```

Run:

```bash
load_data
```

---

# 13. Building an Analytics Pipeline Script

A typical analytics engineering workflow:

```
Raw Data

↓

Python Cleaning

↓

Database Loading

↓

dbt Transformation

↓

Testing

↓

Export
```

Can become:

```
run_pipeline.sh
```

Example:

```bash
#!/bin/bash


echo "Starting Analytics Pipeline"


echo "Loading data"

python python/load_to_duckdb.py


echo "Running dbt"

cd dbt

dbt run


echo "Testing models"

dbt test


cd ..


echo "Exporting data"

python python/export_to_parquet.py


echo "Pipeline completed"
```

---

# 14. Creating Project Setup Scripts

Bash can create project structures automatically.

Example:

```bash
mkdir analytics_project

cd analytics_project

mkdir data database python notebooks docs dashboards
```

Instead of manually creating folders.

---

# 15. Generating Documentation Structure

Example:

```bash
mkdir docs

touch docs/architecture.md

touch docs/data_dictionary.md

touch docs/business_metrics.md
```

Useful when starting new projects.

---

# 16. Environment Setup Script

A script can prepare a new machine.

Example:

```bash
#!/bin/bash


python -m venv analytics-env


source analytics-env/Scripts/activate


pip install -r requirements.txt
```

---

# 17. Error Handling

Good scripts should stop when errors occur.

Use:

```bash
set -e
```

Example:

```bash
#!/bin/bash

set -e

python load_data.py

dbt run
```

If loading fails:

```
dbt run
```

will not execute.

---

# 18. Logging Output

Save output into files.

Example:

```bash
python pipeline.py > logs/pipeline.log
```

Append:

```bash
python pipeline.py >> logs/pipeline.log
```

---

# 19. Example: SupportOps Pipeline Automation

A production-style workflow:

File:

```
run_supportops_pipeline.sh
```

Content:

```bash
#!/bin/bash

set -e


echo "Activating environment"

source dbt-env/Scripts/activate


echo "Loading data"

python python/load_to_duckdb.py


echo "Building dbt models"

cd dbt

dbt run


echo "Running tests"

dbt test


cd ..


echo "Exporting analytical tables"

python python/export_to_parquet.py


echo "Pipeline finished successfully"
```

---

# 20. Bash + Git Workflow

Bash can automate Git actions.

Example:

```bash
git add .

git commit -m "Update analytics models"

git push
```

Combined:

```bash
#!/bin/bash

git add .

git commit -m "Automated update"

git push
```

---

# 21. Bash Best Practices

## Use Comments

Good:

```bash
# Load cleaned data into DuckDB
python load_to_duckdb.py
```

---

## Use Meaningful Names

Good:

```
run_pipeline.sh
setup_project.sh
export_data.sh
```

Avoid:

```
script1.sh
test.sh
```

---

## Avoid Hard-Coding Paths

Bad:

```bash
cd C:/Users/name/Desktop/project
```

Better:

```bash
PROJECT_ROOT=$(pwd)
```

---

## Handle Failures

Use:

```bash
set -e
```

---

## Keep Scripts Small

Instead of:

```
huge_script.sh
```

Use:

```
setup.sh

pipeline.sh

export.sh
```

---

# 22. Bash Skills Required for Analytics Engineers

A professional analytics engineer should understand:

## Basic Commands

- cd
- ls
- mkdir
- touch
- cp
- mv
- rm

---

## Script Concepts

- Variables
- Functions
- Conditions
- Loops
- Inputs
- Error handling

---

## Automation

- Running pipelines
- Creating project templates
- Managing files
- Scheduling tasks

---

## Data Workflow Automation

- Python execution
- dbt execution
- Database commands
- Git operations

---

# Recommended Resources

## Books

### The Linux Command Line

Author:

William Shotts

---

### Bash Cookbook

Authors:

Carl Albing, JP Vossen, Cameron Newham

---

## Courses

### Bash Scripting Full Course

freeCodeCamp

https://www.youtube.com/@freecodecamp

---

### Shell Scripting Tutorial

https://linuxcommand.org/

---

## Documentation

GNU Bash Manual:

https://www.gnu.org/software/bash/manual/

---

# Summary

Bash scripting transforms manual workflows into repeatable automation.

For analytics engineering, Bash enables engineers to:

- Build projects faster
- Automate pipelines
- Reduce human error
- Create reproducible workflows
- Connect different tools together

The SupportOps Intelligence Analytics project can eventually be automated using Bash to execute the complete workflow:

```
Clean Data

↓

Load Database

↓

Run dbt

↓

Test Models

↓

Export Data

↓

Update Reports
```