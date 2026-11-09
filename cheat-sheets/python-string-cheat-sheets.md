# Python Strings Cheat Sheet

## 1. What is a string?
A string (`str`) is an immutable sequence of Unicode characters.

```python
name = "Raghu"
message = 'Hello, Python!'
paragraph = """First line
Second line"""
```

- Strings are ordered and support indexing, slicing, and iteration.
- Strings are **immutable**: individual characters cannot be changed in place.
- `len(text)` returns the number of characters.

## 2. Creating strings and escape sequences

```python
single = 'Hello'
double = "Hello"
multi_line = """Line one
Line two"""
number_text = str(123)
```

| Escape | Meaning |
|---|---|
| `\n` | New line |
| `\t` | Tab |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |
| `\r` | Carriage return |

```python
print("Hello\nWorld")
print("Name:\tRaghu")
path = r"C:\Users\Raghu\Documents"  # Raw string
```

Raw strings (`r"..."`) are useful for paths and regular expressions because backslashes are mostly treated literally.

## 3. Indexing

Positive indexing starts at `0`; negative indexing starts at `-1` from the end.

```python
word = "Python"

word[0]    # 'P'
word[2]    # 't'
word[-1]   # 'n'
word[-2]   # 'o'
```

An invalid index raises `IndexError`.

## 4. Slicing

Syntax: `text[start:stop:step]`. The start is included; the stop is excluded.

```python
word = "Python"

word[0:2]    # 'Py'
word[2:]     # 'thon'
word[:4]     # 'Pyth'
word[:]      # 'Python'
word[::2]    # 'Pto'
word[::-1]   # 'nohtyP' (reverse)
word[-3:]    # 'hon'
```

Omitted values use defaults. Slices safely handle out-of-range bounds.

## 5. Immutability

You cannot assign to an index:

```python
word = "Python"
# word[0] = "J"  # TypeError
```

Create a new string instead:

```python
new_word = "J" + word[1:]  # "Jython"
```

String methods return new strings; they do not modify the original.

## 6. Concatenation and repetition

```python
"Hello" + " " + "World"  # 'Hello World'
"Hi! " * 3                # 'Hi! Hi! Hi! '
```

Use `join()` to combine many strings:

```python
words = ["Python", "is", "fun"]
sentence = " ".join(words)  # 'Python is fun'
```

## 7. Important string methods

### Case conversion

```python
text = "hello PYTHON"

text.upper()       # 'HELLO PYTHON'
text.lower()       # 'hello python'
text.capitalize()  # 'Hello python'
text.title()       # 'Hello Python'
text.swapcase()    # 'HELLO python'
text.casefold()    # Stronger normalization for caseless matching
```

### Removing whitespace or characters

```python
text = "  hello  "

text.strip()       # 'hello' (both ends)
text.lstrip()      # 'hello  ' (left)
text.rstrip()      # '  hello' (right)
```

`strip(chars)` removes any of the specified characters from either end, not an exact prefix or suffix.

### Searching and counting

```python
text = "banana"

text.find("na")    # 2; returns -1 if absent
text.rfind("na")   # 4; searches from right
text.index("na")   # 2; raises ValueError if absent
text.count("a")    # 3
```

Use `find()` when a missing match is normal; use `index()` when absence should be an error.

### Replacing

```python
text = "I like Java"

text.replace("Java", "Python")  # 'I like Python'
text.replace("a", "A", 1)       # Replace at most one occurrence
```

### Prefix and suffix checks

```python
filename = "report.pdf"

filename.startswith("report")  # True
filename.endswith(".pdf")      # True
```

Both methods accept a tuple of possible prefixes or suffixes.

### Splitting and partitioning

```python
"red,green,blue".split(",")
# ['red', 'green', 'blue']

"one two".split()       # Splits on whitespace
"one\ntwo".splitlines()  # Splits into lines

"user@example.com".partition("@")
# ('user', '@', 'example.com')
```

`partition()` returns a three-item tuple: before, separator, and after. If the separator is missing, the remaining string is in the first item.

## 8. String testing methods

These methods return `True` or `False`.

```python
"Python".isalpha()       # Letters only
"12345".isdigit()        # Digit characters
"12345".isdecimal()      # Decimal digit characters
"abc123".isalnum()       # Letters or digits only
"hello".islower()        # Lowercase
"HELLO".isupper()        # Uppercase
"   ".isspace()          # Whitespace only
"Hello World".istitle()  # Title case
```

Important differences:
- `isdecimal()` is the narrowest of these numeric checks.
- `isdigit()` accepts some digit characters beyond decimal digits.
- `isnumeric()` accepts an even wider range of numeric characters.
- These methods return `False` for an empty string.

## 9. String formatting

### f-strings (recommended for most cases)

```python
name = "Raghu"
age = 25

print(f"My name is {name} and I am {age}.")
print(f"Next year I will be {age + 1}.")
```

### Number formatting

```python
price = 1234.567

f"{price:.2f}"    # '1234.57'
f"{price:,.2f}"   # '1,234.57'
f"{0.25:.0%}"     # '25%'
f"{42:05d}"       # '00042'
```

### Other formatting styles

```python
"Hello, {}!".format("Raghu")
"Name: %s, Score: %d" % ("Raghu", 95)
```

Prefer f-strings in modern Python.

## 10. Useful built-in functions

```python
text = "Python"

len(text)       # 6
min(text)       # Smallest character by Unicode code point
max(text)       # Largest character by Unicode code point
sorted(text)    # Sorted list of characters
list(text)      # List of characters
tuple(text)     # Tuple of characters

str(100)        # '100'
int("100")      # 100
float("10.5")   # 10.5
```

`int()` and `float()` raise `ValueError` for invalid numeric text.

## 11. Iterating and checking membership

```python
word = "Python"

for character in word:
    print(character)

for index, character in enumerate(word):
    print(index, character)

"Python" in "Learn Python"       # True
"Java" not in "Learn Python"     # True
```

## 12. Common string patterns

### Reverse a string

```python
text = "Python"
reversed_text = text[::-1]
```

### Check for a palindrome

```python
word = "level"
is_palindrome = word == word[::-1]
```

### Count words

```python
sentence = "Python is easy to learn"
word_count = len(sentence.split())
```

### Count a character

```python
text = "programming"
number_of_m = text.count("m")
```

### Join a list into a string

```python
letters = ["P", "y", "t", "h", "o", "n"]
word = "".join(letters)
```

## 13. Common mistakes to avoid

- Trying `text[index] = value`; strings are immutable.
- Forgetting that the stop index in a slice is excluded.
- Using `index()` when a match may be absent; it raises `ValueError`.
- Assuming `strip("abc")` removes the exact substring `"abc"`; it removes any of those characters from both ends.
- Forgetting that `split()` returns a list.
- Repeatedly using `+` in a loop to build a large string; collect pieces and use `"".join(pieces)`.
- Confusing `isdecimal()`, `isdigit()`, and `isnumeric()`.
- Comparing user input without considering case and surrounding whitespace.

## 14. Quick revision table

| Task | Syntax |
|---|---|
| Length | `len(text)` |
| First character | `text[0]` |
| Last character | `text[-1]` |
| Slice | `text[start:stop:step]` |
| Reverse | `text[::-1]` |
| Lowercase / uppercase | `text.lower()` / `text.upper()` |
| Trim ends | `text.strip()` |
| Replace | `text.replace(old, new)` |
| Find substring | `text.find(value)` |
| Count substring | `text.count(value)` |
| Split into list | `text.split(separator)` |
| Join list into string | `separator.join(items)` |
| Starts / ends with | `text.startswith(value)` / `text.endswith(value)` |
| Contains substring | `value in text` |
| Format value | `f"{value}"` |

## 15. Quick practice

Try these without running the code first:

1. What is the output of `"Python"[1:4]`?
2. What does `"Python"[::-1]` return?
3. What is the difference between `find()` and `index()`?
4. Why does `text[0] = "A"` raise an error?
5. What does `"a,b,c".split(",")` return?
6. How do you combine `["I", "love", "Python"]` into one sentence?
7. What is the difference between `strip()` and `replace()`?

---

**Remember:** Strings are immutable sequences. Be comfortable with indexing, slicing, common methods, splitting and joining, and f-string formatting. These cover most everyday string operations.
