---
description: 'Python Scripting, Data Science, and Backend Standards.'
applyTo: '**.py, **.ipynb, **.requirements.txt, **.toml'
---

# Python Development Guidelines & Standards

## Your Mission
Enforce strict typing, modern Python practices, and cross-platform compatibility. You are fighting against the "it's just a script" mentality to ensure production-quality Python code.

## Introduction
Python is powerful but prone to "spaghetti code" without discipline. These instructions mandate strict type hinting, modern path handling, and robust testing to ensure our code is maintainable and safe on Windows and Linux alike.

## Python Fundamentals & Architecture

### Distinctive Style
- **PEP 8:** Adherence is mandatory. Use formatting tools like `black`.
- **Typing:** Strict Type Hints are **MANDATORY** (`typing`, `pydantic`). Untyped code is legacy code.
- **Data Models:** Use `@dataclass` for clear, memory-efficient data structures.

### Mandatory Typing
**Example - Strong Typing:**
```python
from typing import List, Optional

def process_items(items: List[str]) -> Optional[int]:
    if not items:
        return None
    return len(items)
```

## Safety & Security

### Path Safety (Windows/Linux)
- **The Rule:** ❌ Banish `os.path.join` to the shadow realm.
- **The Solution:** ✅ `pathlib` is non-negotiable.

**Example - Pathlib:**
```python
from pathlib import Path

# 🛡️ Works safely on Windows and Linux
base_dir = Path("data")
file_path = base_dir / "user_logs" / "latest.log"

if file_path.exists():
    content = file_path.read_text()
```

### Environment Management
- Assume `venv` usage on Windows (`.\venv\Scripts\activate`).
- Manage dependencies via `requirements.txt` or `pyproject.toml`.

## Testing & Quality Assurance

### Frameworks
- **Runner:** `pytest`.
- **Linting:** `pylint` or `ruff`.

### Patterns
- **Fixtures:** Use `conftest.py` for shared setup.
- **Parametrization:** Use `@pytest.mark.parametrize` for data-driven tests. avoids code duplication.

**Example - Parametrized Test:**
```python
import pytest

@pytest.mark.parametrize("input_val,expected", [
    (1, 2),
    (2, 4),
    (10, 20),
])
def test_doubler(input_val, expected):
    assert (input_val * 2) == expected
```

## Anti-patterns
- **Wildcard Imports:** `from module import *` is forbidden. It pollutes the namespace.
- **Mutable Defaults:** Never use mutable default arguments (`def func(a=[]):`).
- **Print Debugging:** Use the `logging` module, not `print()`.
