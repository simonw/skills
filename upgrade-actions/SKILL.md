---
name: upgrade-actions
description: "Update GitHub Actions workflows to use latest action versions, current Python test matrices, and modern dependency installation patterns. Use when a project's CI workflows reference outdated actions (e.g. actions/checkout@v3), use old Python versions in test matrices, or install dependencies with pip instead of dependency-groups."
---

# Upgrade GitHub Actions workflows

Upgrade GitHub Actions workflows in `.github/workflows/` to the latest standards. Targets `test.yml`, `publish.yml`, and `release.yml` (or equivalent workflow files).

## Step 1: Find Latest Action Versions

Fetch current versions from the actions-latest registry:

```bash
curl -s https://simonw.github.io/actions-latest/versions.txt | grep -E "actions/checkout|actions/setup-python|pypa/gh-action-pypi-publish"
```

Update each `uses:` line to the latest version. For example:

```yaml
# Before
- uses: actions/checkout@v3
- uses: actions/setup-python@v4

# After
- uses: actions/checkout@v4
- uses: actions/setup-python@v5
```

## Step 2: Update Python Test Matrix

Ensure test matrices use current Python versions:

```yaml
strategy:
  matrix:
    python-version: ["3.10", "3.11", "3.12", "3.13", "3.14"]
```

Also verify `requires-python` in `pyproject.toml` is `>= "3.10"`.

## Step 3: Update Dependency Installation

Check if the project uses `dependency-groups.dev` in `pyproject.toml`. If so, replace legacy pip install patterns:

```yaml
# Before
- run: pip install -e '.[test]'

# After
- run: pip install . --group dev
```

## Step 4: Update Cache Keys

Check whether the project uses `setup.py` or `pyproject.toml` and update any `hashFiles()` cache keys to match:

```yaml
# For pyproject.toml projects
key: ${{ runner.os }}-pip-${{ hashFiles('**/pyproject.toml') }}
```

## Step 5: Validate

Push to a branch and verify workflows pass, or check locally with:

```bash
gh workflow list
gh run list --limit 3
```
