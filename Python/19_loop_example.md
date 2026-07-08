---
title: "19_Loop_example"
category: "Python"
date: 2026-07-08
tags: ["Python", "Loop"]
---

## for 리스트 순회

```python
numbers = [10, 20, 30]

for number in numbers:
    print(number)
```

---

## for 문자열 순회

```python
text = "Python"

for char in text:
    print(char)
```

---

## for 딕셔너리 순회

```python
student = {
    "name": "Kim",
    "age": 20
}

for key, value in student.items():
    print(key, value)
```

---

## range()

```python
print(list(range(5)))
print(list(range(1, 6)))
print(list(range(0, 10, 2)))
```

---

## range와 for

```python
for i in range(1, 6):
    print(i)
```

---

## while

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

---

## break

```python
numbers = [1, 2, 3, 4, 5]

for number in numbers:
    if number == 3:
        break
    print(number)
```

---

## continue

```python
for number in range(1, 6):
    if number == 3:
        continue
    print(number)
```

---

## for-else

```python
numbers = [2, 4, 6]

for number in numbers:
    if number == 5:
        print("Found")
        break
else:
    print("Not Found")
```

---

## while-else

```python
count = 0

while count < 3:
    print(count)
    count += 1
else:
    print("Finished")
```

---

## 중첩 반복문

```python
colors = ["Red", "Blue"]
sizes = ["S", "M"]

for color in colors:
    for size in sizes:
        print(color, size)
```

---

## 실무 반복 처리 루프

```python
orders = [
    {"id": 1, "status": "paid", "price": 10000},
    {"id": 2, "status": "cancelled", "price": 8000},
    {"id": 3, "status": "paid", "price": 15000}
]

total = 0

for order in orders:
    if order["status"] != "paid":
        continue

    total += order["price"]

print(total)
```

---

```python
numbers = [1, 2, 3, 4, 5]

total = 0

for number in numbers:
    total += number

print(total)
```