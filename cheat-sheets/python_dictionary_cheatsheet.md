# Python Dictionary Cheat Sheet

A dictionary stores unique keys mapped to values. Keys must be hashable.

## Create and access
```python
student = {"name": "Asha", "age": 20}
empty = {}
student = dict(name="Asha", age=20)

student["name"]              # KeyError if absent
student.get("grade")         # None if absent
student.get("grade", "N/A")  # Default if absent
```

## Add, update, remove
```python
student["grade"] = "A"
student.update({"age": 21})
del student["age"]
student.pop("grade", None)
student.clear()
```

## Check and iterate
```python
"name" in student            # Checks keys

for key in student:
    print(key)

for value in student.values():
    print(value)

for key, value in student.items():
    print(key, value)
```

## Useful methods and comprehension
```python
student.keys()
student.values()
student.items()
student.setdefault("city", "Bengaluru")
student.copy()

squares = {n: n * n for n in range(5)}
```

## Count items
```python
counts = {}
for item in ["red", "blue", "red"]:
    counts[item] = counts.get(item, 0) + 1
```

Remember: assigning an existing key updates its value; dictionaries preserve insertion order in modern Python.
