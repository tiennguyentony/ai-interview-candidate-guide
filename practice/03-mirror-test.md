# 3. The test that agrees with everything

A video app shows how long a video is, like `1h 05m`. You asked an AI to
write tests for this function:

```python
def format_duration(seconds: int) -> str:
    return f"{seconds // 3600}h {seconds % 3600 // 60:02d}m"
```

It wrote:

```python
import pytest
from player.format import format_duration

@pytest.mark.parametrize("seconds", [0, 59, 60, 3599, 3600, 3900, 90000])
def test_format_duration(seconds):
    expected = f"{seconds // 3600}h {seconds % 3600 // 60:02d}m"
    assert format_duration(seconds) == expected


def test_returns_a_string():
    assert isinstance(format_duration(3900), str)
```

All tests pass.

## Your task

1. What do these tests actually prove? Could they ever fail?
2. If the function had a bug in its formula, would these tests catch it?
3. Rewrite the tests so they would catch real bugs. Which cases matter?
4. Run the function on a few inputs yourself. Is there anything about its
   behaviour you'd question?

**Hint, only if stuck:** where does `expected` come from?
