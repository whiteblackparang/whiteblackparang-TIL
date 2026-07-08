---
title: "15_Methods_example"
category: "Python"
date: 2026-07-08
tags: ["Python", "Dictionary"]
---

# Dictionary Methods Example

## get()

```python
config = {"host": "localhost", "port": 8080}

print(config.get("host"))
print(config.get("user", "admin"))
```

```python
user = {"name": "Kim"}

print(user.get("name"))
print(user.get("age", 0))
```

---

## keys()

```python
student = {
    "name": "Kim",
    "age": 20,
    "major": "Computer"
}

print(student.keys())
```

```python
fruit = {
    "apple": 3,
    "banana": 5
}

for key in fruit.keys():
    print(key)
```

---

## values()

```python
student = {
    "name": "Kim",
    "age": 20,
    "major": "Computer"
}

print(student.values())
```

```python
scores = {
    "math": 90,
    "english": 85,
    "science": 95
}

for value in scores.values():
    print(value)
```

---

## items()

```python
student = {
    "name": "Kim",
    "age": 20
}

for key, value in student.items():
    print(key, value)
```

```python
product = {
    "name": "Laptop",
    "price": 1200
}

print(list(product.items()))
```

---

## update()

```python
user = {
    "name": "Kim",
    "age": 20
}

extra = {
    "city": "Seoul",
    "job": "Developer"
}

user.update(extra)

print(user)
```

```python
score = {"math": 80}

score.update({"math": 95})

print(score)
```

---

## pop()

```python
stock = {
    "apple": 10,
    "banana": 5,
    "orange": 7
}

removed = stock.pop("banana")

print(removed)
print(stock)
```

```python
data = {"a": 1}

print(data.pop("b", 0))
```

---

## popitem()

```python
user = {
    "name": "Kim",
    "age": 20,
    "city": "Seoul"
}

print(user.popitem())
print(user)
```

```python
sample = {
    "x": 10,
    "y": 20
}

last = sample.popitem()

print(last)
```

---

## clear()

```python
numbers = {
    "one": 1,
    "two": 2
}

numbers.clear()

print(numbers)
```

```python
data = {"name": "Kim"}

data.clear()

print(len(data))
```

---

## setdefault()

```python
user = {
    "name": "Kim"
}

user.setdefault("age", 20)

print(user)
```

```python
counter = {}

counter.setdefault("apple", 0)
counter["apple"] += 1

print(counter)
```