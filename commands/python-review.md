---
description: Lightweight Python code review for research projects — correctness, safety, and Pythonic clarity.
---

# Python Code Review

Focused review for a **research codebase**: catch real bugs and security issues first,
suggest Pythonic improvements second. Skip production-grade ceremony that slows iteration.

## What This Command Does

1. **Identify scope**: find modified `.py` files via `git diff` (or review all if requested)
2. **Run static analysis**: `ruff check .` and `mypy .`
3. **Scan for security issues**: hardcoded secrets, unsafe `eval`/`exec`, bare excepts
4. **Report issues by severity**: CRITICAL → HIGH → MEDIUM

## Automated Checks

```bash
# Lint + format + import sort
ruff check .
ruff format --check .

# Type checking
mypy .

# Security
bandit -r .

# Tests
pytest --cov --cov-report=term-missing
```

## Severity Levels

### CRITICAL (fix before next run)
- Hardcoded API keys / credentials
- `eval` / `exec` on untrusted input
- Bare `except:` that silently swallows errors
- Unsafe `pickle` deserialization

### HIGH (fix soon — affects correctness)
- Mutable default arguments (`def f(x=[])`)
- Exceptions swallowed without logging
- Missing `with` for file/resource handles
- Public functions without type hints

### MEDIUM (improve when convenient)
- Missing docstrings on public functions
- `print()` where `logging` is more appropriate
- C-style loops that should be comprehensions
- Magic numbers without named constants

> MEDIUM issues are advisory for research code — address in batches, not per-commit.

## Approval Criteria

| Status | Condition |
|--------|-----------|
| Approve | No CRITICAL or HIGH issues |
| Warning | MEDIUM issues only — merge and track |
| Block | Any CRITICAL or HIGH issue |

## Related

- Agent: `agents/python-reviewer.md`
- Style: `rules/research-style.md`
- Skills: `skills/python-patterns/`
