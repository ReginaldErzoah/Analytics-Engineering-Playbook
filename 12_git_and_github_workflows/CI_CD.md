# CI/CD

## Overview

CI/CD stands for:

```
Continuous Integration

+

Continuous Delivery / Continuous Deployment
```

CI/CD is a software engineering practice that automates the process of:

- Testing code
- Validating changes
- Building applications
- Deploying updates

In analytics engineering, CI/CD helps ensure that:

- SQL transformations are reliable
- dbt models are tested
- Python pipelines work correctly
- Data changes are safely deployed

---

# Why CI/CD Matters

Without CI/CD:

```
Developer Writes Code

        ↓

Manual Testing

        ↓

Manual Deployment

        ↓

Errors Reach Production
```

With CI/CD:

```
Developer Pushes Code

        ↓

Automated Checks

        ↓

Tests Run

        ↓

Deployment

        ↓

Production Update
```

---

# CI/CD Components

A CI/CD pipeline usually contains:

```
Source Control

        ↓

Build

        ↓

Test

        ↓

Validation

        ↓

Deployment

        ↓

Monitoring
```

---

# Continuous Integration (CI)

## Definition

Continuous Integration is the practice of frequently integrating code changes into a shared repository while automatically testing those changes.

---

# CI Workflow

Example:

```
Developer Creates Change

        ↓

Push Code

        ↓

CI Pipeline Starts

        ↓

Run Tests

        ↓

Report Results
```

---

# CI Activities

Common CI tasks:

## Code Testing

Examples:

- Unit tests
- Integration tests

---

## Code Quality Checks

Examples:

- Formatting
- Linting
- Static analysis

---

## Data Validation

Examples:

- Schema checks
- Data quality tests

---

# Continuous Delivery

## Definition

Continuous Delivery ensures that software changes are always ready for deployment.

The deployment step may require manual approval.

Example:

```
Code Tested

        ↓

Deployment Ready

        ↓

Human Approval

        ↓

Production Release
```

---

# Continuous Deployment

## Definition

Continuous Deployment automatically releases validated changes into production.

Example:

```
Merge Code

        ↓

Pipeline Runs

        ↓

Tests Pass

        ↓

Automatic Deployment
```

---

# CI/CD Pipeline Example

A typical pipeline:

```
Developer Pushes Code

        ↓

GitHub Actions

        ↓

Install Dependencies

        ↓

Run Tests

        ↓

Build Application

        ↓

Deploy
```

---

# CI/CD Tools

Common CI/CD platforms:

|Tool|Description|
|-|-|
|GitHub Actions|Automation inside GitHub|
|GitLab CI/CD|Integrated GitLab pipelines|
|Jenkins|Open-source automation server|
|CircleCI|Cloud-based CI/CD|
|Azure DevOps|Microsoft CI/CD platform|

---

# GitHub Actions

## Overview

GitHub Actions allows automation directly inside GitHub repositories.

It uses workflow files:

```
.github/workflows/
```

Example:

```
ci.yml
```

---

# GitHub Actions Workflow Structure

Example:

```yaml
name: Test Pipeline

on:
  push:

jobs:

  test:

    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v4

      - run:
          pytest
```

---

# CI/CD For Analytics Engineering

Analytics engineering pipelines often validate:

- SQL
- dbt
- Python
- Infrastructure

---

# Example Analytics CI Pipeline

```
Developer Updates dbt Model

        ↓

Create Pull Request

        ↓

GitHub Actions Starts

        ↓

Install dbt

        ↓

Run dbt Tests

        ↓

Run SQL Checks

        ↓

Approve Merge

        ↓

Deploy
```

---

# CI/CD With dbt

A dbt CI pipeline may run:

## dbt Compile

Checks SQL generation.

```bash
dbt compile
```

---

## dbt Test

Runs:

- Unique tests
- Not null tests
- Relationship tests

```bash
dbt test
```

---

## dbt Build

Runs models and tests.

```bash
dbt build
```

---

# CI/CD With Python

Python pipelines may include:

## Dependency Installation

```bash
pip install -r requirements.txt
```

---

## Unit Testing

```bash
pytest
```

---

## Code Formatting

Examples:

```
black

ruff

flake8
```

---

# CI/CD Environment Management

Projects commonly use:

```
Development

↓

Testing

↓

Production
```

---

Example:

Development:

```
Developer Workspace
```

Testing:

```
CI Environment
```

Production:

```
Cloud Platform
```

---

# Secrets Management

CI/CD pipelines require secure handling of:

- Passwords
- API keys
- Database credentials

Never store:

```
password = "12345"
```

inside code.

---

Instead use:

```
Environment Variables

Secrets Managers

CI/CD Secrets
```

---

# Example Environment Variable

Python:

```python
import os

password = os.getenv(
"DATABASE_PASSWORD"
)
```

---

# CI/CD Best Practices

## 1. Automate Testing

Every change should be tested automatically.

---

## 2. Keep Pipelines Fast

Slow pipelines reduce productivity.

---

## 3. Fail Early

Stop pipelines when critical issues appear.

---

## 4. Use Separate Environments

Avoid testing directly in production.

---

## 5. Monitor Deployments

Track:

- Success
- Failure
- Runtime
- Errors

---

# Common CI/CD Failures

## Failed Tests

Cause:

```
Code broke existing functionality
```

Solution:

Fix code before merging.

---

## Dependency Issues

Cause:

```
Missing packages
```

Solution:

Maintain requirements files.

---

## Environment Differences

Cause:

```
Works locally but fails in CI
```

Solution:

Use consistent environments.

---

## Secret Errors

Cause:

```
Missing credentials
```

Solution:

Configure CI secrets.

---

# CI/CD Example For Analytics Engineer

Project:

Customer Analytics Platform

Workflow:

```
Developer Updates dbt Model

        ↓

Push Branch

        ↓

Pull Request Created

        ↓

CI Runs:

- dbt compile

- dbt test

- SQL linting

- Python tests

        ↓

Approved

        ↓

Merge

        ↓

Production Deployment
```

---

# Interview Questions

## What is CI/CD?

CI/CD is a set of practices that automate testing, validation, and deployment of code changes.

---

## Difference between Continuous Delivery and Continuous Deployment?

Continuous Delivery prepares changes for release, while Continuous Deployment automatically releases them.

---

## Why use CI/CD in analytics engineering?

To ensure data transformations, pipelines, and analytics code are reliable before reaching production.

---

## What tools have you used for CI/CD?

Examples:

- GitHub Actions
- GitLab CI
- Jenkins
- Azure DevOps

---

# Key Takeaway

CI/CD brings software engineering discipline to analytics workflows.

It enables:

```
Automated Testing

+

Reliable Deployments

+

Faster Collaboration

+

Higher Data Trust
```

A mature analytics engineering team treats data code with the same engineering practices as application code.