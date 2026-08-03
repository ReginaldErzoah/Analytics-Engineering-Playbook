# Git & GitHub Open Source Contribution Guide (Beginner → Professional Workflow)

This guide covers the complete workflow for contributing to open-source projects:

* Understanding GitHub projects
* Forking repositories
* Cloning projects
* Creating branches
* Making changes
* Writing commits
* Pushing branches
* Creating Pull Requests (PRs)
* Handling reviews
* Keeping forks updated
* Resolving conflicts
* Advanced Git operations

This is the workflow used in real engineering teams.

---

# 1. Understand the Open Source Contribution Model

When contributing to someone else's project, you usually **do not directly edit their repository**.

The workflow:

```text
Original Repository
        |
        |
        ↓
     Fork
        |
        |
        ↓
 Your GitHub Repository
        |
        |
        ↓
 Clone locally
        |
        |
        ↓
 Create branch
        |
        |
        ↓
 Make changes
        |
        |
        ↓
 Push branch
        |
        |
        ↓
 Pull Request
        |
        |
        ↓
 Maintainer reviews
        |
        |
        ↓
 Merge into original project
```

---

# 2. Find a Project to Contribute To

Good beginner-friendly places:

## GitHub Labels

Search:

```
good first issue
```

or:

```
help wanted
```

Example:

```
https://github.com/search?q=label%3A%22good+first+issue%22
```

---

Before contributing, inspect:

## README.md

Understand:

* What does the project do?
* How is it installed?
* How do they run tests?

---

## CONTRIBUTING.md

Most serious projects have:

```
CONTRIBUTING.md
```

It contains:

* coding style
* branch rules
* commit format
* testing requirements

Example:

```
Before submitting:

1. Run tests
2. Format code
3. Update documentation
4. Create PR
```

---

## Issues

Look for:

```
Open Issues
```

Example:

```
Bug: CSV export fails on empty datasets
```

or

```
Feature: Add JSON output support
```

---

# 3. Fork a Repository

Example project:

```
github.com/project-owner/data-tool
```

Click:

```
Fork
```

GitHub creates:

```
github.com/ReginaldErzoah/data-tool
```

Now:

Original:

```
project-owner/data-tool
```

Your copy:

```
ReginaldErzoah/data-tool
```

---

# 4. Clone Your Fork

Copy your repository URL:

Example:

```bash
git clone https://github.com/ReginaldErzoah/data-tool.git
```

Enter:

```bash
cd data-tool
```

---

Check:

```bash
git remote -v
```

Output:

```
origin
https://github.com/ReginaldErzoah/data-tool.git
```

---

# 5. Add Original Repository as Upstream

This is important.

Your repository:

```
origin
```

Original project:

```
upstream
```

Add:

```bash
git remote add upstream https://github.com/project-owner/data-tool.git
```

Check:

```bash
git remote -v
```

Output:

```
origin
https://github.com/ReginaldErzoah/data-tool.git


upstream
https://github.com/project-owner/data-tool.git
```

---

## Why?

Because you need to regularly sync with the main project.

---

# 6. Create a Development Branch

Never work directly on `main`.

Bad:

```
main
 |
 edits
 |
 commits
```

Professional:

```
main

 |
 |
feature branch

 |
 |
changes
```

---

Create branch:

```bash
git switch -c fix-csv-export
```

Example branches:

```
feature/add-api-support

bugfix/fix-validation-error

docs/update-readme
```

---

Check:

```bash
git branch
```

Output:

```
main

* fix-csv-export
```

---

# 7. Explore the Codebase

Before changing anything:

Run:

```bash
ls
```

Typical project:

```
README.md
src/
tests/
docs/
requirements.txt
pyproject.toml
```

---

Install dependencies:

Python example:

```bash
pip install -r requirements.txt
```

or:

```bash
pip install -e .
```

---

Run tests:

```bash
pytest
```

Example:

```
42 passed
```

Always know the starting point.

---

# 8. Make Your Changes

Example:

Before:

```python
def export_csv(data):
    pass
```

After:

```python
def export_csv(data):
    return data.to_csv()
```

---

# 9. Check Your Changes

See modified files:

```bash
git status
```

Example:

```
modified:
src/export.py
```

---

View changes:

```bash
git diff
```

---

# 10. Add Changes

Specific file:

```bash
git add src/export.py
```

Everything:

```bash
git add .
```

---

Check:

```bash
git status
```

Output:

```
Changes to be committed:

modified src/export.py
```

---

# 11. Create Professional Commits

Bad:

```
changed stuff
fix
update
```

Good:

```
Fix CSV export handling for empty datasets
```

Examples:

```
Add validation for missing columns

Improve error messages for CLI commands

Update installation documentation

Add tests for JSON export
```

---

Commit:

```bash
git commit -m "Fix CSV export handling for empty datasets"
```

---

# 12. Push Your Branch

First push:

```bash
git push -u origin fix-csv-export
```

After:

```bash
git push
```

---

Now GitHub has:

```
main

fix-csv-export
```

---

# 13. Create a Pull Request

Go to GitHub.

You will see:

```
Compare & pull request
```

Click.

---

PR structure:

## Title

Bad:

```
Fix issue
```

Good:

```
Fix CSV export failure when dataset is empty
```

---

## Description

Example:

```markdown
## Problem

CSV export fails when the dataset contains zero rows.

## Solution

Added validation before exporting.

## Testing

- Added unit tests
- Ran pytest successfully

## Related Issue

Fixes #123
```

---

# 14. Pull Request Review Process

Maintainer may:

## Approve

```
Approved ✅
```

Merge.

---

## Request Changes

Example:

```
Please add tests for this edge case.
```

You update:

```bash
git add .
git commit -m "Add empty dataset test"
git push
```

Your PR automatically updates.

---

# 15. Keeping Your Fork Updated

Your fork becomes outdated.

Example:

Original:

```
main
 |
new commits
```

Your:

```
main
 |
old commits
```

---

Fetch upstream:

```bash
git fetch upstream
```

---

Switch main:

```bash
git switch main
```

---

Merge:

```bash
git merge upstream/main
```

---

Push update:

```bash
git push origin main
```

---

# 16. Handling Merge Conflicts

Example:

You changed:

```python
timeout = 30
```

Original changed:

```python
timeout = 60
```

Git cannot decide.

You see:

```
<<<<<<< HEAD

timeout = 30

=======

timeout = 60

>>>>>>> upstream/main
```

Choose correct version.

Then:

```bash
git add file.py
```

Commit:

```bash
git commit -m "Resolve merge conflict"
```

---

# 17. Rebasing (Professional Skill)

Instead of:

```
main

A---B---C


feature

A---B---D
```

Merge creates:

```
A---B---C---M
       \
        D
```

Rebase creates cleaner history:

```
A---B---C---D
```

---

Update branch:

```bash
git fetch upstream

git rebase upstream/main
```

---

# 18. Git Stash

Useful when you need to temporarily save work.

Example:

You are coding:

```
unfinished changes
```

Need to switch branches.

Save:

```bash
git stash
```

Switch:

```bash
git switch main
```

Return:

```bash
git stash pop
```

---

# 19. Git Tags and Releases

Used for software versions.

Example:

```
v1.0.0
v1.1.0
v2.0.0
```

Create:

```bash
git tag v1.0.0
```

Push:

```bash
git push origin v1.0.0
```

---

# 20. GitHub Actions (CI/CD)

Most open-source projects automatically run:

* tests
* linting
* security checks

Example:

```
Push Code

↓

GitHub Actions

↓

pytest

↓

Deploy
```

File:

```
.github/workflows/test.yml
```

Example:

```yaml
name: Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

---

# 21. Professional Open Source Workflow

Daily workflow:

```bash
# Update local repository

git switch main

git pull upstream main


# Create task branch

git switch -c feature/new-feature


# Work


# Check

git status


# Commit

git add .

git commit -m "Add feature"


# Push

git push -u origin feature/new-feature


# Create PR
```

---

# 22. Contribution Types Beyond Code

Open source needs:

## Documentation

Examples:

* Fix README
* Add tutorials
* Improve examples

## Testing

Examples:

* Add missing tests
* Improve coverage

## Bug Reports

Good issue:

```
Expected:
CSV export creates file

Actual:
File is empty

Steps:
1.
2.
3.

Environment:
Python 3.12
```

---

## Community Support

Examples:

* Answer GitHub discussions
* Help beginners
* Review PRs

---

# 23. Recommended Git Books

## 1. Pro Git ⭐⭐⭐⭐⭐

Author:

**Scott Chacon & Ben Straub**

Free:

[https://git-scm.com/book/en/v2](https://git-scm.com/book/en/v2)

Best Git reference.

Learn:

* internals
* branching
* merging
* workflows

---

## 2. Git Pocket Guide

Author:

**Richard E. Silverman**

Good quick reference.

---

## 3. Version Control with Git

Author:

**Jon Loeliger & Matthew McCullough**

More advanced.

---

# YouTube Resources

## 1. freeCodeCamp Git & GitHub Course

Search:

```
freeCodeCamp Git and GitHub Full Course
```

Good beginner foundation.

---

## 2. TechWorld with Nana

Search:

```
TechWorld with Nana Git Tutorial
```

Excellent explanations of:

* branching
* merge conflicts
* workflows

---

## 3. The Net Ninja

Search:

```
Git and GitHub Tutorial The Net Ninja
```

Very beginner friendly.

---

## 4. Fireship

Search:

```
Git explained Fireship
```

Fast overview.

---

## 5. GitHub Official Channel

[https://www.youtube.com/@GitHub](https://www.youtube.com/@GitHub)

Learn:

* GitHub Actions
* open source workflows
* collaboration

---

# Practice Projects For You

Given your goals (Analytics Engineering + Open Source), practice with:

## Contribution 1

Improve documentation in:

* dbt projects
* DuckDB tools
* Python data libraries

## Contribution 2

Fix a small bug.

Example:

```
good first issue:
Improve error message
```

## Contribution 3

Add tests.

Example:

```
Add test for empty dataframe handling
```

---

# Your Git Skill Target

For your career goals, you should be comfortable with:

✅ git init
✅ clone
✅ fork
✅ remote management
✅ branches
✅ merge
✅ rebase
✅ stash
✅ conflict resolution
✅ pull requests
✅ code reviews
✅ GitHub Actions
✅ releases
✅ contributing to open source

This level is enough to operate like a professional contributor on projects such as dbt packages, DuckDB tools, Airbyte connectors, and Python data libraries.
