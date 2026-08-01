# dbt Deployment

## Overview

Deployment is the process of moving dbt transformations from development environments into production environments where business users consume trusted analytics.

A production analytics engineering workflow ensures that:

- Code changes are reviewed
- Tests pass
- Data models build successfully
- Dashboards receive reliable data

Typical workflow:

```
Developer Environment

        ↓

Git Repository

        ↓

CI/CD Pipeline

        ↓

Production dbt Environment

        ↓

Production Data Warehouse

        ↓

BI Tools
```

---

# Why dbt Deployment Matters

Without a deployment process:

- Analysts manually modify production SQL
- Errors reach business dashboards
- Data quality problems go unnoticed
- Changes are difficult to track

Deployment introduces:

- Version control
- Automation
- Testing
- Reliability

---

# Development vs Production Environment

## Development Environment

Used by analytics engineers for:

- Writing models
- Testing changes
- Experimenting with SQL

Example:

```
dev_reginald.customer_support_models
```

---

## Production Environment

Used by business users.

Contains:

- Official models
- Certified metrics
- Dashboard datasets

Example:

```
prod.analytics.fact_ticket_metrics
```

---

# Typical dbt Deployment Workflow

## Step 1: Create Feature Branch

Example:

```bash
git checkout -b add-ticket-metrics
```

Developer makes changes:

```
models/

fact_ticket_metrics.sql
```

---

## Step 2: Test Locally

Run:

```bash
dbt test
```

Compile:

```bash
dbt compile
```

Build:

```bash
dbt build
```

---

## Step 3: Commit Changes

Example:

```bash
git add .

git commit -m "Add ticket metrics model"
```

---

## Step 4: Push Code

```bash
git push origin add-ticket-metrics
```

---

## Step 5: Create Pull Request

Team reviews:

- SQL logic
- Data quality
- Documentation
- Performance

---

## Step 6: CI Pipeline Runs

Automated checks:

```
Install dependencies

 ↓

Run dbt tests

 ↓

Compile models

 ↓

Validate changes
```

---

## Step 7: Merge to Main

After approval:

```
Feature Branch

        ↓

Main Branch
```

---

## Step 8: Production Deployment

dbt runs production jobs:

```
dbt run

dbt test
```

---

# dbt Cloud Deployment

dbt Cloud provides managed deployment.

Features:

- Scheduled jobs
- Environment management
- Job monitoring
- CI/CD integration
- Documentation hosting

---

# dbt Jobs

A job defines automated dbt execution.

Example:

Daily analytics refresh:

```
Schedule:

Every day 06:00 AM

Commands:

dbt build
```

---

# Common Production Job

Example:

```bash
dbt build --target prod
```

This runs:

```
Models

Tests

Snapshots

Seeds
```

---

# dbt Build Command

`dbt build` is commonly used in production.

It combines:

```bash
dbt run

+

dbt test

+

dbt snapshot

+

dbt seed
```

---

# CI/CD with dbt

CI/CD means:

## Continuous Integration

Automatically test code changes.

Example:

Developer creates PR.

Pipeline checks:

```
SQL compiles

Tests pass

Models build
```

---

## Continuous Deployment

Automatically deploy approved changes.

Example:

```
Merged PR

        ↓

Production dbt job runs
```

---

# Example GitHub Actions Workflow

File:

```
.github/workflows/dbt.yml
```

Example:

```yaml
name: dbt CI

on:

 pull_request:


jobs:

 test:

  runs-on: ubuntu-latest

  steps:

  - checkout code

  - install dbt

  - run dbt deps

  - run dbt build
```

---

# Deployment Environments

A professional setup usually contains:

```
Development

↓

Staging

↓

Production
```

---

# Development

Purpose:

Testing new ideas.

Example:

```
dev_schema
```

---

# Staging

Purpose:

Pre-production validation.

Example:

```
staging_schema
```

---

# Production

Purpose:

Business reporting.

Example:

```
analytics_schema
```

---

# Managing Environment Variables

Production systems store sensitive information separately.

Examples:

- Database credentials
- API keys
- Cloud secrets

Never commit:

```
passwords

tokens

credentials
```

into Git.

---

# Deployment Monitoring

Production teams monitor:

## Job Status

Example:

```
Success

Failed

Running
```

---

## Runtime

Example:

```
Job normally takes:

15 minutes

Today:

2 hours
```

---

## Data Freshness

Example:

```
Latest ticket data:

Yesterday

Expected:

Today
```

---

# Deployment Best Practices

## 1. Always Use Version Control

Never directly edit production SQL.

---

## 2. Automate Testing

Every change should validate:

- Schema tests
- Business rules
- Data quality

---

## 3. Use Code Reviews

Review:

- SQL logic
- Performance
- Documentation

---

## 4. Separate Environments

Avoid:

```
Developer changes

directly affecting production
```

---

## 5. Monitor Failures

Investigate:

- Failed models
- Missing data
- Slow queries

---

# Example: Customer Support Analytics Deployment

Development:

```
Developer modifies:

fact_ticket_metrics.sql
```

↓

Testing:

```
dbt test
```

↓

GitHub:

```
Pull Request
```

↓

CI:

```
Run dbt build
```

↓

Production:

```
fact_ticket_metrics table updated
```

↓

Power BI:

```
Customer Support Dashboard refreshed
```

---

# Interview Questions

## What happens when deploying dbt models?

Code changes are tested, reviewed, merged, and executed in a production environment.

---

## What is dbt build?

A command that runs models, tests, seeds, and snapshots together.

---

## Why use CI/CD with dbt?

To automatically validate and safely deploy analytics changes.

---

## What is the difference between development and production environments?

Development is for building and testing; production serves official business analytics.

---

## Why should analysts not edit production tables directly?

Because it bypasses testing, version control, and governance.

---

# Key Takeaway

Deployment turns dbt projects from personal SQL scripts into reliable production data systems.

A mature analytics engineering workflow includes:

✅ Version control  
✅ Automated testing  
✅ Code review  
✅ CI/CD pipelines  
✅ Production monitoring  

The goal is not only to build models — it is to deliver trustworthy analytics continuously.