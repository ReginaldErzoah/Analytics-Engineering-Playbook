# Terminal Fundamentals for Analytics Engineering

## Overview

The terminal is one of the most important tools for analytics engineers because modern data projects involve:

- Running scripts
- Managing environments
- Executing database commands
- Running transformation pipelines
- Managing files
- Using version control
- Automating workflows

A professional analytics engineer should be comfortable working without relying entirely on graphical interfaces.

This document covers the terminal skills required to build projects like **SupportOps Intelligence Analytics**.

---

# 1. Understanding the Terminal

The terminal is a command-line interface (CLI) that allows users to interact directly with the operating system.

Instead of clicking through folders and applications, commands are used.

Example:

Graphical action:

```
Open project folder
```

Terminal equivalent:

```bash
cd project_folder
```

---

# 2. Understanding the Shell

A shell is the program that interprets commands.

Common shells:

| Shell | Platform |
|-|-|
| Bash | Linux, macOS, Git Bash |
| PowerShell | Windows |
| Zsh | macOS/Linux |

For this project:

Environment:

```
Git Bash
```

Shell:

```
Bash
```

---

# 3. Navigating the File System

## Checking Current Location

Command:

```bash
pwd
```

Purpose:

Shows the current directory.

Example:

```
/Desktop/Resources/Python Practice/SupportOps Intelligence
```

---

# Listing Files

Command:

```bash
ls
```

Shows files and folders.

Example:

```
data/
dbt/
python/
README.md
```

---

Detailed listing:

```bash
ls -la
```

Shows:

- Hidden files
- Permissions
- Metadata

---

# Changing Directories

Command:

```bash
cd folder_name
```

Example:

```bash
cd dbt
```

Moves into the dbt directory.

---

Move back:

```bash
cd ..
```

Example:

```
dbt/

↓

SupportOps Intelligence/
```

---

Go to home directory:

```bash
cd ~
```

---

# 4. Creating Files and Folders

## Create Folder

Command:

```bash
mkdir folder_name
```

Example:

```bash
mkdir dashboards
```

---

Create multiple folders:

```bash
mkdir data database docs
```

---

## Create Empty File

Command:

```bash
touch filename.md
```

Example:

```bash
touch README.md
```

---

Create multiple files:

```bash
touch file1.md file2.md file3.md
```

---

# 5. Viewing File Contents

## Display Entire File

Command:

```bash
cat filename
```

Example:

```bash
cat requirements.txt
```

---

## View Large Files

Command:

```bash
less filename
```

Useful for:

- Logs
- Large CSV files
- Documentation

---

## View First Lines

Command:

```bash
head filename
```

Example:

```bash
head customer_support_tickets.csv
```

---

## View Last Lines

Command:

```bash
tail filename
```

Useful for:

- Logs
- Pipeline output

---

# 6. Editing Files From Terminal

Common terminal editors:

## Nano

Simple editor:

```bash
nano README.md
```

Save:

```
CTRL + O
ENTER
```

Exit:

```
CTRL + X
```

---

## Vim

Advanced editor:

```bash
vim filename
```

Common in Linux environments.

---

# 7. File Operations

## Copy Files

Command:

```bash
cp source destination
```

Example:

```bash
cp README.md docs/
```

---

## Move Files

Command:

```bash
mv source destination
```

Example:

```bash
mv report.md docs/
```

---

## Rename Files

Same command:

```bash
mv old_name.md new_name.md
```

Example:

```bash
mv report.txt report.md
```

---

## Delete Files

Command:

```bash
rm filename
```

Example:

```bash
rm temporary_file.txt
```

---

## Delete Folder

Command:

```bash
rm -r folder_name
```

Be careful because deletion is permanent.

---

# 8. Searching Files

## Search Text

Command:

```bash
grep "text" filename
```

Example:

```bash
grep "customer" schema.yml
```

---

Search recursively:

```bash
grep -r "customer" .
```

Useful for:

- Finding SQL references
- Finding configuration values

---

# 9. Understanding Paths

## Relative Path

Starts from current location.

Example:

```
docs/readme.md
```

---

## Absolute Path

Full location.

Example:

```
C:/Users/reginald/project/docs/readme.md
```

---

# 10. Working With Python From Terminal

## Check Python Version

Command:

```bash
python --version
```

Example:

```
Python 3.12
```

---

## Run Python Script

Command:

```bash
python script.py
```

Example:

```bash
python load_to_duckdb.py
```

Used in SupportOps:

```
python/load_to_duckdb.py

python/export_to_parquet.py
```

---

# 11. Virtual Environments

Virtual environments isolate project dependencies.

Example:

```
Project A

numpy 1.x


Project B

numpy 2.x
```

Both can exist separately.

---

Create environment:

```bash
python -m venv dbt-env
```

---

Activate environment:

Windows:

```bash
dbt-env\Scripts\activate
```

Git Bash:

```bash
source dbt-env/Scripts/activate
```

---

Deactivate:

```bash
deactivate
```

---

# 12. Installing Packages

Install package:

```bash
pip install package_name
```

Example:

```bash
pip install pandas
```

---

Install requirements:

```bash
pip install -r requirements.txt
```

---

Check installed packages:

```bash
pip list
```

---

Export dependencies:

```bash
pip freeze > requirements.txt
```

Used in this project to document:

- pandas
- numpy
- duckdb
- dbt
- seaborn
- matplotlib

---

# 13. Running dbt Commands

dbt commands are executed from terminal.

---

Run models:

```bash
dbt run
```

---

Run specific models:

```bash
dbt run --select fact_ticket
```

---

Run tests:

```bash
dbt test
```

---

Full rebuild:

```bash
dbt run --full-refresh
```

Used when:

- Changing table structures
- Rebuilding models

---

Preview models:

```bash
dbt show --select fact_ticket
```

Example:

```
Previewing node 'fact_ticket'
```

---

# 14. Managing Processes

Sometimes programs continue running in the background.

Example:

Jupyter kernels.

---

List processes:

```bash
tasklist
```

---

Filter Python processes:

```bash
tasklist | grep python
```

---

Inspect process commands:

```bash
wmic process where "name='python.exe'" get ProcessId,CommandLine
```

Useful for:

- Debugging
- Finding stuck notebooks
- Managing environments

---

# 15. Environment Variables

Environment variables store configuration values.

Examples:

```
DATABASE_PATH

API_KEY

USERNAME
```

View variables:

```bash
echo $VARIABLE_NAME
```

---

Common project file:

```
.env
```

Example:

```
DATABASE_PATH=data/database.duckdb
```

---

# 16. Running SQL Tools From Terminal

Example:

DuckDB CLI:

```bash
duckdb database/supportops.duckdb
```

Then:

```sql
SHOW TABLES;
```

---

# 17. Project Debugging Using Terminal

Common debugging commands:

Check files:

```bash
ls -R
```

Check logs:

```bash
cat logs/dbt.log
```

Check package:

```bash
pip show package_name
```

Check running processes:

```bash
tasklist
```

---

# 18. Terminal Workflow Used in SupportOps Intelligence

Typical workflow:

```
Open Terminal

↓

Navigate to Project

↓

Activate Environment

↓

Run Python Scripts

↓

Load Database

↓

Run dbt Models

↓

Run dbt Tests

↓

Export Data

↓

Update Git
```

Example:

```bash
cd SupportOps Intelligence

source dbt-env/Scripts/activate

python python/load_to_duckdb.py

cd dbt

dbt run

dbt test

cd ..

python python/export_to_parquet.py
```

---

# 19. Essential Terminal Skills Checklist

An Analytics Engineer should know:

## Navigation

- pwd
- ls
- cd

## File Management

- mkdir
- touch
- cp
- mv
- rm

## Inspection

- cat
- head
- tail
- grep

## Python

- python
- pip
- virtual environments

## Data Tools

- dbt commands
- DuckDB CLI

## Debugging

- logs
- processes
- environment variables

## Automation

- bash scripts

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

### Linux Command Line Basics

freeCodeCamp

https://www.freecodecamp.org/

---

### Bash Scripting Tutorial

https://linuxcommand.org/

---

## Documentation

Bash Reference:

https://www.gnu.org/software/bash/manual/

Git Bash:

https://git-scm.com/

---

# Summary

Terminal proficiency is a core skill for analytics engineers.

It enables engineers to:

- Build projects efficiently
- Automate workflows
- Debug problems
- Manage environments
- Execute data pipelines

The SupportOps Intelligence Analytics project used terminal skills extensively for:

- Creating project structures
- Running Python scripts
- Managing dbt workflows
- Debugging processes
- Managing Git repositories