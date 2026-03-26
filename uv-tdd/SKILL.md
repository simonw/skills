---
name: uv-tdd
description: "Test-driven development workflow for Python projects using uv and pytest. Use when creating a new Python project from scratch, setting up pytest test suites with uv, iterating on Python code with a red-green-refactor TDD cycle, or managing Python dependencies and virtual environments with the uv package manager."
---

# uv-tdd skill

A test-driven development process for Python applications using uv for project management and pytest for testing.

## Project Setup

```bash
mkdir name-of-project
cd name-of-project
uv init
git init  # if not already in a git repo
```

This creates an initial `pyproject.toml`. Add pytest as a dev dependency immediately:

```bash
uv add pytest --dev
```

Add other dependencies as needed:

```bash
uv add httpx
```

## Bootstrap Test

Create a placeholder test to verify the setup works:

```bash
mkdir tests
echo 'def test_add():
    assert 1 + 1 == 2' > tests/test_add.py
```

Run it to confirm the environment is working:

```bash
uv run pytest
```

Delete `test_add.py` once the first real test is written. Do not include it in any commits.

## Running Code

Always execute Python through uv to use the managed environment:

```bash
uv run python -c "..."
uv run pytest
uv run pytest -k name_of_test
```

## TDD Workflow (Red-Green-Refactor)

For every change, follow this cycle:

1. **Red** — Write a failing test first (group related tests in the same test file):
   ```bash
   uv run pytest -k name_of_test  # watch it fail
   ```
2. **Green** — Implement the minimum code to make the test pass:
   ```bash
   uv run pytest -k name_of_test  # watch it pass
   ```
3. **Refactor** — Clean up, then run the full suite to catch regressions:
   ```bash
   uv run pytest  # all tests should pass
   ```
4. **Commit** — Commit implementation, tests, and documentation together as a single commit. Push after every commit if a remote is configured.

If tests fail unexpectedly after implementation, check for import errors (`uv run python -c "import mymodule"`), missing dependencies (`uv add <package>`), or fixture scope issues before debugging test logic.

## Project Documentation

Always create:
- **README.md** — project name as heading plus a short description
- **spec.md** — detailed specification with markdown TODO checklists; update TODOs as work progresses (check off completed items, add new ones)

## Testing Best Practices

- Use and reuse **pytest fixtures** for shared setup, including temporary files scoped to the test run
- Use **`pytest.mark.parametrize`** to avoid duplicated test code across similar cases
- Group related tests in the same file; split into new files when a different domain area is covered
