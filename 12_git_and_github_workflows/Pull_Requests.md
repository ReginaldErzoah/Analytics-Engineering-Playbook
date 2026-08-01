# Pull Requests

## Overview

A Pull Request (PR) is a mechanism used in Git-based workflows to propose, review, discuss, and merge changes from one branch into another.

Pull Requests are a critical part of collaborative software development because they provide:

- Code review
- Quality checks
- Collaboration
- Documentation of changes
- Automated testing

In analytics engineering, Pull Requests are commonly used for reviewing:

- SQL models
- dbt transformations
- Python pipelines
- Data quality tests
- Documentation changes

---

# Why Pull Requests Matter

Without Pull Requests:

```
Developer Changes Code

        ↓

Directly Pushes To Production

        ↓

Possible Errors
```

With Pull Requests:

```
Developer Creates Branch

        ↓

Makes Changes

        ↓

Opens Pull Request

        ↓

Review + Testing

        ↓

Merge Safely
```

---

# Pull Request Workflow

A typical workflow:

```
Create Feature Branch

        ↓

Develop Changes

        ↓

Commit Changes

        ↓

Push Branch

        ↓

Open Pull Request

        ↓

Code Review

        ↓

CI Checks

        ↓

Approval

        ↓

Merge
```

---

# Creating a Pull Request

## Step 1: Create Branch

Example:

```bash
git checkout -b feature/customer-model
```

---

## Step 2: Make Changes

Example:

Create:

```
models/customer_metrics.sql
```

---

## Step 3: Commit Changes

```bash
git add .

git commit -m "Add customer metrics model"
```

---

## Step 4: Push Branch

```bash
git push origin feature/customer-model
```

---

## Step 5: Open Pull Request

On GitHub:

```
Repository

↓

Pull Requests

↓

New Pull Request
```

---

# Pull Request Components

A good Pull Request contains:

```
Title

Description

Code Changes

Testing Information

Reviewers
```

---

# Pull Request Title

Titles should describe the change.

Good:

```
Add customer lifetime value model
```

Bad:

```
Update files
```

---

# Pull Request Description

A good description explains:

- What changed
- Why it changed
- How it was tested

Example:

```
## Changes

Added customer lifetime value calculation.

## Testing

- dbt tests passed
- SQL query validated

## Impact

Adds customer profitability analysis.
```

---

# Code Review

Code review is the process where teammates examine changes before merging.

Reviewers check:

- Code quality
- Logic correctness
- Performance
- Security
- Documentation

---

# Analytics Engineering Code Review Example

SQL model:

```sql
SELECT

customer_id,

SUM(revenue) AS total_revenue

FROM orders

GROUP BY customer_id;
```

Reviewer checks:

Questions:

```
Is the aggregation correct?

Are duplicates handled?

Are tests included?

Is documentation updated?
```

---

# Pull Request Reviews

Common review outcomes:

## Approved

Changes are acceptable.

```
Ready to merge
```

---

## Request Changes

Problems need fixing.

Example:

```
Add missing data validation test
```

---

## Comment Only

Provides suggestions without blocking merge.

---

# Pull Requests and CI/CD

Pull Requests often trigger automated checks.

Example:

```
Open PR

      ↓

GitHub Actions Runs

      ↓

Run Tests

      ↓

Build Project

      ↓

Report Results
```

---

# CI Checks For Analytics Projects

Examples:

## dbt Tests

```bash
dbt test
```

Checks:

- Unique values
- Missing values
- Relationships

---

## SQL Formatting

Example:

```
SQLFluff
```

Checks:

- SQL style
- Formatting rules

---

## Python Tests

Example:

```bash
pytest
```

Checks:

- Functions
- Data processing logic

---

# Pull Request Approval Rules

Teams may require:

- Minimum reviewers
- Passing tests
- No conflicts
- Branch protection

Example:

```
main branch

Requires:

✓ 2 approvals

✓ CI passed

✓ No merge conflicts
```

---

# Merge Strategies

## Merge Commit

Keeps all branch history.

Example:

```
main

|

merge commit

|

feature commits
```

---

## Squash Merge

Combines all PR commits into one.

Example:

Before:

```
Commit 1

Commit 2

Commit 3
```

After:

```
One clean commit
```

---

## Rebase Merge

Places commits directly on top of the target branch.

Creates a cleaner history.

---

# Pull Requests In dbt Projects

Example:

A developer creates:

```
models/marts/customer_sales.sql
```

Workflow:

```
Create Branch

↓

Build Model

↓

Add Schema Tests

↓

Run dbt Test

↓

Open PR

↓

Review Model Logic

↓

Merge
```

---

# Good Pull Request Practices

## 1. Keep PRs Small

Better:

```
Add customer model
```

than:

```
Rewrite entire warehouse
```

---

## 2. Explain Business Context

Analytics changes should explain:

```
Why does this metric exist?
```

---

## 3. Include Tests

Every important change should include validation.

---

## 4. Update Documentation

Keep:

- README
- Data dictionaries
- Model descriptions

updated.

---

## 5. Review Your Own PR First

Before requesting review:

Check:

```bash
git diff main
```

---

# Common Pull Request Mistakes

## 1. Large Unfocused PRs

Problem:

Hard to review.

Solution:

Split into smaller changes.

---

## 2. Missing Description

Problem:

Reviewers do not understand the change.

---

## 3. No Testing Evidence

Problem:

Changes cannot be trusted.

---

## 4. Ignoring Review Comments

Problem:

Reduces collaboration quality.

---

# Pull Requests vs Direct Commits

|Direct Commit|Pull Request|
|-|-|
|Fast|Controlled|
|No review|Peer review|
|Higher risk|Lower risk|
|Less documentation|Clear history|

---

# Pull Requests In Analytics Engineering Teams

A mature analytics team uses PRs for:

```
SQL Changes

+

dbt Models

+

Python Scripts

+

Dashboard Logic

+

Documentation
```

---

# Interview Questions

## What is a Pull Request?

A Pull Request is a request to merge changes from one branch into another after review and validation.

---

## Why use Pull Requests?

They improve code quality through collaboration, review, and automated testing.

---

## What should a good Pull Request contain?

A clear title, description, testing details, and relevant context.

---

## How do Pull Requests relate to CI/CD?

Pull Requests trigger automated tests and validation before changes are merged and deployed.

---

# Key Takeaway

Pull Requests are the bridge between development and production.

They provide:

```
Collaboration

+

Review

+

Testing

+

Safe Deployment
```

For analytics engineers, strong Pull Request practices ensure that data transformations remain reliable and trustworthy.