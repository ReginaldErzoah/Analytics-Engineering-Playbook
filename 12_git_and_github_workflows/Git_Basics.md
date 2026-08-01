# Git Basics

## Overview

Git is a distributed version control system used to track, manage, and collaborate on changes in software projects.

For analytics engineers, Git is used to manage:

- SQL models
- dbt projects
- Python scripts
- Data pipelines
- Documentation
- Configuration files

Git provides a history of changes and enables teams to work together safely.

---

# Why Learn Git?

Without Git:

```
Manual File Copies

↓

Confusion

↓

Lost Changes

↓

Difficult Collaboration
```

With Git:

```
Track Changes

↓

Review History

↓

Collaborate

↓

Deploy Reliably
```

---

# Git vs GitHub

## Git

Git is the version control system installed on your computer.

It manages:

- Commits
- Branches
- History
- Changes

---

## GitHub

GitHub is an online platform that hosts Git repositories.

It provides:

- Remote repositories
- Pull requests
- Code reviews
- Collaboration tools

---

# Installing Git

Check installation:

```bash
git --version
```

Example output:

```
git version 2.45.0
```

---

# Configuring Git

Set username:

```bash
git config --global user.name "Your Name"
```

---

Set email:

```bash
git config --global user.email "your@email.com"
```

---

View configuration:

```bash
git config --list
```

---

# Creating a Repository

## Initialize Git Repository

Navigate to project:

```bash
cd analytics-project
```

Initialize:

```bash
git init
```

Creates:

```
.git/
```

The `.git` folder stores version history.

---

# Checking Repository Status

Command:

```bash
git status
```

Shows:

- Modified files
- Untracked files
- Staged files

Example:

```
Untracked files:

models/customer.sql
```

---

# Git Workflow

The basic Git lifecycle:

```
Edit Files

    ↓

git status

    ↓

git add

    ↓

git commit

    ↓

git push
```

---

# Adding Files

## Add One File

```bash
git add README.md
```

---

## Add Multiple Files

```bash
git add file1.sql file2.sql
```

---

## Add Everything

```bash
git add .
```

---

# Understanding Staging

Git has three areas:

```
Working Directory

        ↓

Staging Area

        ↓

Repository
```

---

Example:

You edit:

```
customer_model.sql
```

Status:

```
Modified
```

After:

```bash
git add customer_model.sql
```

Status:

```
Staged
```

After:

```bash
git commit
```

Status:

```
Saved in history
```

---

# Creating Commits

A commit saves changes permanently.

Example:

```bash
git commit -m "Create customer analytics model"
```

---

# Good Commit Messages

Good:

```
Add customer dimension model
```

```
Fix revenue calculation logic
```

```
Update dbt tests
```

---

Bad:

```
changes
```

```
update
```

```
stuff
```

---

# Viewing Commit History

Basic:

```bash
git log
```

---

Compact:

```bash
git log --oneline
```

Example:

```
a83f21 Add sales model

b92d10 Fix validation test
```

---

# Viewing Changes

Before staging:

```bash
git diff
```

Shows:

```
Changes not staged
```

---

After staging:

```bash
git diff --staged
```

Shows:

```
Changes ready for commit
```

---

# Undoing Changes

## Discard File Changes

Return file to last commit:

```bash
git checkout -- file.sql
```

Modern command:

```bash
git restore file.sql
```

---

# Unstage Files

Remove from staging:

```bash
git restore --staged file.sql
```

The file remains modified.

---

# Removing Files

Delete file:

```bash
git rm file.sql
```

Commit:

```bash
git commit -m "Remove unused file"
```

---

# Branch Basics

Branches allow independent development.

View branches:

```bash
git branch
```

---

Create branch:

```bash
git branch feature/customer-model
```

---

Switch branch:

```bash
git checkout feature/customer-model
```

Modern:

```bash
git switch feature/customer-model
```

---

Create and switch:

```bash
git checkout -b feature/customer-model
```

---

# Main Branch

The main branch represents stable code.

Example:

```
main

|

├── feature/new-dashboard

├── feature/new-model

└── bugfix/error
```

---

# Merging Branches

Example:

Switch to main:

```bash
git checkout main
```

Merge:

```bash
git merge feature/customer-model
```

---

# Remote Repositories

A remote connects local Git to GitHub.

View remotes:

```bash
git remote -v
```

---

Add remote:

```bash
git remote add origin https://github.com/user/project.git
```

---

# Pushing Code

Send commits to GitHub:

```bash
git push origin main
```

---

Set upstream:

```bash
git push -u origin main
```

After this:

```bash
git push
```

is enough.

---

# Pulling Changes

Download updates:

```bash
git pull
```

Equivalent:

```
git fetch

+

git merge
```

---

# Fetch vs Pull

## Git Fetch

Downloads changes but does not merge.

```bash
git fetch
```

---

## Git Pull

Downloads and merges.

```bash
git pull
```

---

# Cloning Repositories

Copy remote repository:

```bash
git clone repository_url
```

Example:

```bash
git clone https://github.com/user/project.git
```

---

# .gitignore

A `.gitignore` file tells Git what not to track.

Example:

```
.env

__pycache__/

*.csv

.venv/
```

---

Common ignored files:

- Passwords
- Environment variables
- Temporary files
- Large datasets

---

# Git Tags

Tags mark important versions.

Example:

```bash
git tag v1.0.0
```

---

Push tag:

```bash
git push origin v1.0.0
```

---

# Git for Analytics Engineering Example

Project:

```
analytics_project/

├── models/

│   └── customers.sql

├── tests/

├── macros/

├── scripts/

└── README.md
```

Workflow:

```
Create Feature Branch

↓

Build dbt Model

↓

Run Tests

↓

Commit Changes

↓

Create Pull Request

↓

Merge
```

---

# Common Git Mistakes

## 1. Committing Directly To Main

Problem:

Can break production code.

Solution:

Use branches.

---

## 2. Huge Commits

Problem:

Difficult to review.

Solution:

Commit small changes.

---

## 3. Forgetting git status

Problem:

Files may not be tracked.

Solution:

Check status frequently.

---

## 4. Committing Sensitive Data

Never commit:

```
Passwords

API Keys

Credentials
```

---

# Interview Questions

## What is Git?

Git is a distributed version control system used to track and manage changes in files.

---

## Difference between Git and GitHub?

Git is the version control tool, while GitHub hosts Git repositories online.

---

## What is a commit?

A commit is a saved snapshot of changes in a Git repository.

---

## What is a branch?

A branch is an independent line of development used to work on changes safely.

---

## Difference between git pull and git fetch?

Fetch downloads changes without merging, while pull downloads and merges changes.

---

# Key Takeaway

Git is the foundation of collaborative analytics engineering.

Mastering Git means understanding:

```
Repositories

+

Commits

+

Branches

+

Merging

+

Collaboration
```

Good Git practices make analytics projects reliable, maintainable, and production-ready.