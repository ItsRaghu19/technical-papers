# Python Arrays Cheatsheet

In everyday Python, “array” often means a `list`. This sheet covers lists and briefly notes `array.array` and NumPy.

## Create lists
```python
numbers = [10, 20, 30]
mixed = [1, "hello", True]
empty = []
zeros = [0] * 5
values = list(range(5))  # [0, 1, 2, 3, 4]
```
Lists are ordered, mutable, and may contain mixed types.

## Access and slice
```python
items = ["a", "b", "c", "d"]
items[0]      # first: "a"
items[-1]     # last: "d"
items[1:3]    # ["b", "c"], end excluded
items[:2]     # first two
items[2:]     # index 2 onward
items[::2]    # every second item
items[::-1]   # reversed copy
```
Indexing starts at zero; an invalid index raises `IndexError`.

## Add and update
```python
items[1] = "B"
items.append("e")          # add one at end
items.extend(["f", "g"])   # add multiple
items.insert(1, "new")     # insert at index
```

## Remove
```python
items.remove("B")  # first matching value; ValueError if absent
last = items.pop() # remove and return last
first = items.pop(0)
del items[0]       # delete by index
items.clear()      # remove all
```

## Search, count, and sort
```python
3 in numbers
numbers.index(20)
numbers.count(10)
numbers.sort()                  # changes list
numbers.sort(reverse=True)
new_list = sorted(numbers)      # returns a new list
```

## Iterate and transform
```python
for value in numbers:
    print(value)

for index, value in enumerate(numbers):
    print(index, value)

squares = [n * n for n in numbers]
evens = [n for n in numbers if n % 2 == 0]
```

## Copying
```python
original = [1, 2, 3]
copied = original.copy()  # or original[:]
```
`alias = original` does not copy: both names refer to the same list. A shallow copy still shares nested mutable objects; use `copy.deepcopy()` for a recursive copy when needed.

## Nested lists
```python
matrix = [[1, 2, 3], [4, 5, 6]]
matrix[0][1]  # 2

# Independent rows:
matrix = [[0] * 3 for _ in range(2)]
```
Avoid `[[0] * 3] * 2` when rows must be independent; it repeats references to one inner list.

## Handy operations
| Operation | Purpose |
|---|---|
| `len(items)` | Number of elements |
| `sum(numbers)` | Sum |
| `min(items)`, `max(items)` | Minimum/maximum |
| `list(reversed(items))` | Reversed list |
| `items + other` | Concatenate |
| `items * 3` | Repeat contents |

## List vs. array types
- **`list`**: built-in, flexible, general-purpose sequence.
- **`array.array`**: standard-library typed array, usually for compact values of one type.
- **NumPy `ndarray`**: third-party numeric arrays, useful for vectorized and multidimensional computation.

```python
from array import array
scores = array("i", [10, 20, 30])  # signed integer array
scores.append(40)
```
