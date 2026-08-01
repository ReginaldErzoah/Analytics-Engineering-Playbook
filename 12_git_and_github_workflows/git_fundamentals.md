# Git Fundamentals

## Overview

Git is a distributed version control system used to track changes in files, collaborate with others, and maintain the history of software and analytics projects.

For analytics engineers, Git is essential because modern data projects contain:

- SQL models
- Python scripts
- Data transformation logic
- Documentation
- Configuration files
- Dashboard assets

Git allows engineers to safely develop, experiment, and collaborate without losing work.

---

# Why Git Matters In Analytics Engineering

Without Git:

```

Project Files

↓

Manual Changes

↓

No History

↓

Difficult Recovery

```

---

With Git:

```

Project Files

↓

Tracked Changes

↓

Version History

↓

Collaboration

↓

Reliable Development

```

---

# Core Git Concepts

## Repository

A repository (repo) is a project folder tracked by Git.

Example:

```

analytics-project/

├── README.md

├── models/

├── scripts/

└── docs/

````

When initialized:

```bash
git init
````

Git creates:

```
.git/
```

which stores version information.

---

# Working Directory

The working directory contains your current files.

Example:

```
README.md

models/

scripts/
```

Changes here are not automatically tracked.

---

# Staging Area

The staging area contains changes prepared for a commit.

Example:

```bash
git add README.md
```

Meaning:

```
Working Directory

↓

Staging Area
```

---

# Repository History

The repository stores committed changes.

Example:

```
Commit 1

↓

Commit 2

↓

Commit 3
```

Each commit represents a saved version.

---

# Git Workflow

The basic Git workflow:

```
Modify Files

↓

git add

↓

git commit

↓

git push
```

---

# Installing Git

Check installation:

```bash
git --version
```

Example:

```
git version 2.x.x
```

---

# Git Configuration

Set username:

```bash
git config --global user.name "Your Name"
```

Set email:

```bash
git config --global user.email "email@example.com"
```

Check configuration:

```bash
git config --list
```

---

# Creating A Repository

Navigate to project:

```bash
cd analytics-project
```

Initialize:

```bash
git init
```

Result:

```
Initialized empty Git repository
```

---

# Checking Repository Status

Command:

```bash
git status
```

Shows:

* Modified files
* New files
* Staged files

Example:

```
Untracked files:

README.md
models/
```

---

# Adding Files

Add one file:

```bash
git add README.md
```

Add everything:

```bash
git add .
```

---

# Creating Commits

A commit saves changes permanently.

Example:

```bash
git commit -m "Create customer dimension model"
```

Good commit messages explain:

* What changed
* Why it changed

---

# Good Commit Examples

Good:

```
Add customer analytics model

Create SLA compliance calculation

Fix duplicate ticket records
```

---

Bad:

```
Update files

Changes

Fix stuff
```

---

# Viewing History

View commits:

```bash
git log
```

Short format:

```bash
git log --oneline
```

Example:

```
a91bc2 Add dbt models

72cd91 Create project structure
```

---

# Viewing Changes

See unstaged changes:

```bash
git diff
```

See staged changes:

```bash
git diff --staged
```

---

# Undoing Changes

## Remove Changes From File

Restore:

```bash
git restore filename
```

Example:

```bash
git restore models/customer.sql
```

---

## Remove From Staging

```bash
git restore --staged filename
```

---

# Branches

A branch is an independent development path.

Example:

```
main

|

feature/customer-model
```

---

# Why Use Branches?

Branches allow:

* Safe experimentation
* Parallel development
* Code review

---

# Creating A Branch

Create:

```bash
git branch feature-name
```

Switch:

```bash
git checkout feature-name
```

Modern command:

```bash
git switch feature-name
```

---

# Creating And Switching

Shortcut:

```bash
git checkout -b feature/customer-model
```

or:

```bash
git switch -c feature/customer-model
```

---

# Main Branch

The main branch contains stable code.

Common names:

```
main

master
```

Modern standard:

```
main
```

---

# Merging Branches

Example:

```
feature branch

↓

main
```

Switch to main:

```bash
git switch main
```

Merge:

```bash
git merge feature/customer-model
```

---

# Merge Conflicts

A conflict happens when Git cannot automatically combine changes.

Example:

Two people modify:

```
customer.sql
```

differently.

Git asks you to choose the correct version.

---

# Remote Repositories

A remote repository is a Git repository stored elsewhere.

Examples:

* GitHub
* GitLab
* Bitbucket

---

# Connecting GitHub

Create remote:

```bash
git remote add origin repository-url
```

Check remote:

```bash
git remote -v
```

---

# Pushing Code

Upload commits:

```bash
git push origin main
```

First push:

```bash
git push -u origin main
```

---

# Pulling Changes

Download updates:

```bash
git pull
```

Used when:

* Working with teams
* Updating local repository

---

# Cloning A Repository

Download existing repository:

```bash
git clone repository-url
```

Example:

```bash
git clone https://github.com/user/project.git
```

---

# Git Ignore

`.gitignore` tells Git which files not to track.

Example:

```
.env

__pycache__/

*.csv

db/*.duckdb
```

---

# Why Ignore Files?

Avoid committing:

* Passwords
* Large datasets
* Temporary files
* Generated outputs

---

# Analytics Engineering .gitignore Example

```
# Python

__pycache__/

*.pyc


# Environment

.env


# Virtual environments

venv/

dbt-env/


# Data

data/raw/

*.csv


# Databases

*.duckdb


# Logs

logs/
```

---

# Git Tags

Tags mark important versions.

Example:

```bash
git tag v1.0.0
```

Useful for:

* Releases
* Project milestones

---

# Git Stash

Temporarily save changes.

Example:

```bash
git stash
```

Restore:

```bash
git stash pop
```

Useful when:

* Switching branches
* Testing something quickly

---

# Git Reset

Move repository history.

Soft reset:

```bash
git reset --soft HEAD~1
```

Removes commit but keeps files.

---

Hard reset:

```bash
git reset --hard HEAD~1
```

Removes commit and changes.

Use carefully.

---

# Professional Git Workflow

A common workflow:

```
main

↓

feature branch

↓

Development

↓

Testing

↓

Merge

↓

Production
```

---

# Git Workflow For Analytics Projects

Example:

Create feature:

```bash
git switch -c feature/new-kpi
```

Develop:

```
Create SQL model

Add tests

Update documentation
```

Commit:

```bash
git add .

git commit -m "Add SLA KPI model"
```

Push:

```bash
git push origin feature/new-kpi
```

Merge after review.

---

# GitHub Pull Requests

A pull request allows:

* Code review
* Discussion
* Automated testing

Workflow:

```
Feature Branch

↓

Pull Request

↓

Review

↓

Merge
```

---

# Git Best Practices

## Commit Frequently

Avoid:

```
One huge commit
```

Prefer:

```
Small meaningful commits
```

---

## Write Clear Messages

Future you should understand:

```
Why was this change made?
```

---

## Never Commit Secrets

Never upload:

```
passwords

API keys

credentials
```

---

## Keep Main Stable

Only merge tested code.

---

# Git Skills For Analytics Engineers

Must know:

## Beginner

* init
* add
* commit
* status
* log
* clone

---

## Intermediate

* Branching
* Merging
* Pull requests
* Conflicts

---

## Advanced

* Rebase
* Git hooks
* CI/CD integration
* Release workflows

---

# Resources

## Books

### Pro Git

Author:

Scott Chacon and Ben Straub

Free:

[https://git-scm.com/book/](https://git-scm.com/book/)

Focus:

* Complete Git reference

### GitHub For Developers

Focus:

* Collaboration workflows

---

## Courses

GitHub Skills:

[https://skills.github.com/](https://skills.github.com/)

Learn Git Branching:

[https://learngitbranching.js.org/](https://learngitbranching.js.org/)

Git Documentation:

[https://git-scm.com/doc](https://git-scm.com/doc)

---

# Summary

Git is the foundation of professional analytics engineering workflows.

The essential workflow:

```
Create Repository

↓

Create Branch

↓

Develop

↓

Commit Changes

↓

Push To GitHub

↓

Review

↓

Merge
```

Mastering Git allows analytics engineers to build reliable, collaborative, and maintainable data projects.