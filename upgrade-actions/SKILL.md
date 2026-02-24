---
name: upgrade-actions
description: Upgrade GitHub Actions workflows
---

# Upgrade GitHub Actions workflows

Upgrade GitHub Actions workflows to the latest standard.

Look in `.github/workflows/` for `test.yml` and `publish.yml` or `release.yml`.

First make sure they are running the latest versions of e.g. `actions/checkout` and `actions/setup-python`.

Consult https://simonw.github.io/actions-latest/versions.txt with curl and grep to find the latest versions.

Next make sure they use current versions of Python in their test matrices. Right now that should be:

    ["3.10", "3.11", "3.12", "3.13", "3.14"]

Check if the project uses `setup.py` or `pyproject.toml` and update any cache keys to reflect that.

Also check if the project uses `dependency-groups.dev` in `pyproject.toml` - if it does then any pip install lines
in the workflows should use this pattern:

    pip install . --group dev
