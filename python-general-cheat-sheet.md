# Python General Cheatsheet

## Running Python
```bash
python --version
python script.py
python
```
Exit the REPL with `exit()` or `Ctrl+D`.

## Variables and types
```python
name = "Ravi"       # str
age = 21            # int
height = 1.75       # float
active = True       # bool
value = None        # NoneType
```
Python is dynamically typed. Check with `type(x)` or `isinstance(x, SomeType)`.

## Input and output
```python
name = input("Name: ")        # input() returns str
age = int(input("Age: "))
print(f"{name} is {age} years old")
```

## Operators
| Kind | Operators |
|---|---|
| Arithmetic | `+ - * / // % **` |
| Comparison | `== != < <= > >=` |
| Logical | `and or not` |
| Assignment | `= += -= *= /=` |
| Membership | `in`, `not in` |
| Identity | `is`, `is not` |

`/` is true division; `//` is floor division. Use `==` for value equality and `is None` to check for `None`.

## Conditionals
```python
if score >= 90:
    grade = "A"
elif score >= 75:
    grade = "B"
else:
    grade = "C"
```
Indentation defines code blocks.

## Loops
```python
for item in items:
    print(item)

for i in range(5):  # 0 through 4
    print(i)

while condition:
    do_work()
```
`break` exits a loop, `continue` skips to its next iteration, and `pass` is a placeholder.

## Functions
```python
def add(a, b=0):
    # Return the sum.
    return a + b

result = add(3, 4)
```
A function without an explicit `return` returns `None`.

## Useful built-ins
| Built-in | Purpose |
|---|---|
| `len(x)` | Number of items |
| `range(n)` | Integers from 0 to n-1 |
| `enumerate(items)` | Index and item while iterating |
| `zip(a, b)` | Iterate over sequences together |
| `sorted(items)` | Return a sorted list |
| `sum(nums)` | Sum numeric values |
| `min(x)`, `max(x)` | Smallest/largest |
| `any(x)`, `all(x)` | Test truthiness of any/all items |

## Comprehensions
```python
squares = [n * n for n in numbers]
evens = [n for n in numbers if n % 2 == 0]
```

## Exceptions
```python
try:
    number = int(input("Number: "))
except ValueError:
    print("Enter a valid integer.")
else:
    print("You entered", number)
finally:
    print("Finished.")
```
Prefer catching specific exceptions.

## Imports and main guard
```python
import math
from pathlib import Path

def main():
    print(math.sqrt(25))

if __name__ == "__main__":
    main()
```

## File handling
```python
with open("notes.txt", "r", encoding="utf-8") as file:
    contents = file.read()

with open("output.txt", "w", encoding="utf-8") as file:
    file.write("Hello\n")
```
Modes: `r` read, `w` overwrite, `a` append, `x` create exclusively. `with` closes the file automatically.

## Quick reminders
- Strings are immutable; lists are mutable.
- `#` starts a comment; triple-quoted strings commonly document modules, classes, and functions.
- Use descriptive `snake_case` names.
