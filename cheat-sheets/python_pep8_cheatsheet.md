# Python PEP 8 Cheat Sheet

PEP 8 is Python's style guide for readable, consistent code.

## Naming
```python
student_name = "Asha"       # variables: snake_case

def calculate_total():      # functions: snake_case
    pass

class StudentRecord:        # classes: CapWords
    pass

MAX_RETRIES = 3             # constants: UPPER_SNAKE_CASE
```

## Formatting reminders
- Indent with 4 spaces; avoid tabs.
- Keep lines generally to 79 characters.
- Use two blank lines between top-level definitions.
- Put spaces around operators: `total = price + tax`.
- Keep imports near the top, grouped: standard library, third-party, local.
- Avoid extra spaces inside brackets: `items[0]`.

## Docstrings
```python
def area(width, height):
    """Return the area of a rectangle."""
    return width * height
```

Write comments to explain why, not simply repeat what the code says.
