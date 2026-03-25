---
name: python-patterns
description: Pythonic idioms and best practices for research Python code — type hints, error handling, comprehensions, and anti-patterns to avoid.
origin: ECC
---

# Python Patterns (Research Edition)

Quick reference for writing clear, correct Python in a research codebase.
Prioritize readability and correctness; skip production-grade ceremony.

## When to Activate

- Writing or reviewing Python code
- Refactoring an existing module
- Unsure whether a pattern is idiomatic

---

## Type Hints

Use built-in generics (Python 3.9+); fall back to `typing` only when needed.

```python
# preferred (3.9+)
def process(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# nullable parameter
from typing import Optional
def find(key: str) -> Optional[str]:
    ...

# union shorthand (3.10+)
def load(path: str) -> dict | None:
    ...
```

---

## Error Handling

Catch specific exceptions; always chain when re-raising.

```python
# good
def load_config(path: str) -> dict:
    try:
        with open(path) as f:
            return json.load(f)
    except FileNotFoundError as e:
        raise RuntimeError(f"Config not found: {path}") from e
    except json.JSONDecodeError as e:
        raise ValueError(f"Invalid JSON: {path}") from e

# bad — bare except swallows everything silently
try:
    risky()
except:
    pass
```

---

## Context Managers

Always use `with` for files and any resource that needs cleanup.

```python
# good
with open("data.txt") as f:
    data = f.read()

# bad
f = open("data.txt")
data = f.read()
f.close()  # skipped on exception
```

---

## Comprehensions & Generators

Prefer comprehensions for simple transforms; generators for large data.

```python
# list comprehension
active = [u.name for u in users if u.is_active]

# generator (lazy — no intermediate list)
total = sum(x ** 2 for x in range(1_000_000))

# dict comprehension
lengths = {s: len(s) for s in words}

# when logic is complex, use a plain loop instead of nesting comprehensions
```

---

## Dataclasses

Use `@dataclass` instead of plain classes for data containers.

```python
from dataclasses import dataclass, field

@dataclass
class Result:
    smiles: str
    score: float
    tags: list[str] = field(default_factory=list)
```

---

## Decorators

Keep decorators simple; always use `functools.wraps`.

```python
import functools

def retry(times: int = 3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(times):
                try:
                    return func(*args, **kwargs)
                except Exception:
                    if attempt == times - 1:
                        raise
        return wrapper
    return decorator
```

---

## Anti-Patterns

| Bad | Good |
|-----|------|
| `def f(x=[])` mutable default | `def f(x=None)` then `if x is None: x = []` |
| `type(x) == list` | `isinstance(x, list)` |
| `x == None` | `x is None` |
| `from module import *` | explicit imports |
| string concat in loop: `s += x` | `"".join(items)` |
| bare `except:` | `except SpecificError as e:` |

---

## Tooling

```bash
ruff check .          # lint + import sort
ruff format .         # auto-format
mypy .                # type checking
pytest -x             # run tests, stop on first failure
```

---

**Rule of thumb**: code should be obvious to a collaborator reading it six months later.
