---
title: "21_Function_example"
category: "Python"
date: 2026-07-08
tags: ["Python", "Function"]
---

# 함수 정의

```python
def greet():
    return "Hello"

print(greet())
```

---

# return

```python
def add():
    result = 10 + 20
    return result

print(add())
```

---

# 매개변수 1개

```python
def double(num):
    return num * 2

print(double(5))
```

---

# 매개변수 여러 개

```python
def add(a, b):
    return a + b

print(add(10, 20))
```

---

# 여러 값 반환

```python
def min_max(a, b):
    if a < b:
        return a, b
    return b, a

small, big = min_max(10, 5)

print(small)
print(big)
```

---

# 매개변수가 없는 함수

```python
def get_pi():
    return 3.14159

print(get_pi())
```

---

# return이 없는 함수

```python
def hello():
    print("Hello Python")

result = hello()

print(result)
```

---

# 함수와 조건문

```python
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    return "F"

print(get_grade(85))
```

---

# 함수와 반복문

```python
def sum_list(numbers):
    total = 0

    for number in numbers:
        total += number

    return total

print(sum_list([1, 2, 3, 4, 5]))
```

---

# 함수 재사용

```python
def square(n):
    return n * n

print(square(3))
print(square(5))
print(square(7))
```

---

# 실무 함수 설계 루프

```python
def calculate_discount(price, is_member):
    if is_member:
        return int(price * 0.9)
    return price

price = calculate_discount(50000, True)

assert price == 45000

print(price)
```