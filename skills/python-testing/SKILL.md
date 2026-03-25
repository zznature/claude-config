---
name: python-testing
description: pytest essentials for research projects — fixtures, parametrize, mocking LLM/API calls, and coverage.
origin: ECC
---

# Python Testing Patterns (Research Edition)

Practical pytest patterns for a research codebase. Focus: catch regressions fast,
mock expensive external calls (LLM APIs), keep tests deterministic.

## When to Activate

- Writing tests for agents, pipelines, or utilities
- Mocking LLM / API calls in unit tests
- Setting up `conftest.py` fixtures
- Debugging a failing test

---

## Core Structure

```python
def test_positive_case():
    result = my_function("valid input")
    assert result == "expected output"

def test_edge_case_returns_empty():
    assert my_function("") == ""
```

---

## Fixtures

```python
import pytest

@pytest.fixture
def sample_data():
    return {"key": "value", "count": 42}

@pytest.fixture
def tmp_json(tmp_path):
    p = tmp_path / "data.json"
    p.write_text('{"key": "value"}')
    yield p

# shared fixtures → conftest.py
```

### Fixture Scopes

| Scope | Recreated | Use for |
|-------|-----------|---------|
| `function` (default) | each test | mutable state, cheap objects |
| `module` | once per file | read-only shared data |
| `session` | once per run | expensive setup (model load) |

---

## Parametrize

```python
@pytest.mark.parametrize("input_val,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
    ("", ""),
])
def test_uppercase(input_val, expected):
    assert input_val.upper() == expected
```

---

## Mocking LLM / API Calls

Always mock LLM calls in unit tests — never hit real APIs.

```python
from unittest.mock import patch, MagicMock

def _mock_llm(response_text: str) -> MagicMock:
    """Create a mock LLM that returns a fixed response."""
    llm = MagicMock()
    llm.invoke.return_value = MagicMock(content=response_text)
    return llm

def test_agent_returns_output():
    llm = _mock_llm("some structured response")
    state = {"question": "Analyze this", "route": "full_pipeline"}
    result = my_agent(state, llm)
    assert "output_key" in result

# mock a module-level function by full path
@patch("mypackage.agents.module.external_call")
def test_calls_external(mock_call):
    mock_call.return_value = "mocked result"
    ...
    mock_call.assert_called_once()
```

### Mock Side Effects (error paths)

```python
llm = MagicMock()
llm.invoke.side_effect = ConnectionError("timeout")

with pytest.raises(ConnectionError):
    my_agent(state, llm)
```

---

## Exception Testing

```python
def test_missing_config_raises():
    with pytest.raises(ValueError, match="Missing"):
        load_config("nonexistent")

def test_invalid_input_returns_none():
    assert parse("garbage!!!") is None  # should not raise
```

---

## File / Path Tests

Prefer `tmp_path` (built-in, `pathlib.Path` based).

```python
def test_checkpoint_roundtrip(tmp_path):
    ckpt = tmp_path / "checkpoint.jsonl"
    write_checkpoint(ckpt, {"id": "1", "correct": True})

    loaded = load_checkpoint(ckpt)
    assert loaded["1"]["correct"] is True
```

---

## Markers

```python
# pyproject.toml
[tool.pytest.ini_options]
markers = [
    "slow: requires LLM API call or heavy computation",
    "integration: needs external files or services",
]
```

```bash
pytest -m "not slow"        # skip API-dependent tests
pytest -m "not integration" # skip tests needing external resources
```

---

## Key Commands

```bash
pytest -x                              # stop on first failure
pytest -k "formula"                    # run matching tests
pytest --lf                            # re-run last failures
pytest --cov --cov-report=term-missing # coverage report
pytest -v tests/test_graph.py          # verbose single file
```

---

## Quick Reference

| Need | Pattern |
|------|---------|
| Mock LLM response | `MagicMock(); llm.invoke.return_value = MagicMock(content=...)` |
| Mock a function | `@patch("package.module.func")` |
| Test exception | `with pytest.raises(ErrorType, match="msg"):` |
| Temp file | `tmp_path / "file.txt"` fixture |
| Multiple inputs | `@pytest.mark.parametrize("a,b", [...])` |
| Skip slow tests | `pytest -m "not slow"` |

---

**Rule**: if a test hits a real LLM API, it belongs under `@pytest.mark.slow`
and must be skippable in CI.
