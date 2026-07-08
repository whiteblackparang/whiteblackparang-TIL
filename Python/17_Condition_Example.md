---
title: "17_Condition_example"
category: "Python"
date: 2026-07-08
tags: ["Python", "Condition"]
---

# Condition Example

## if

```python
age = 20

if age >= 18:
    print("Adult")
```

```python
score = 85

if score >= 60:
    print("Pass")
```

---

## if-else

```python
num = 7

if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

```python
age = 15

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

---

## if-elif-else

```python
score = 85

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
else:
    print("F")
```

```python
temperature = 32

if temperature >= 30:
    print("Hot")
elif temperature >= 20:
    print("Warm")
else:
    print("Cold")
```

---

## 비교 연산자

```python
x = 10
y = 20

print(x == y)
print(x != y)
print(x < y)
print(x >= y)
```

```python
password = "1234"

if password == "1234":
    print("Login Success")
else:
    print("Login Failed")
```

---

## and

```python
age = 25
is_member = True

if age >= 18 and is_member:
    print("Discount")
```

```python
score = 90
attendance = 95

if score >= 80 and attendance >= 90:
    print("Excellent")
```

---

## or

```python
is_weekend = False
is_holiday = True

if is_weekend or is_holiday:
    print("Rest Day")
```

```python
vip = False
coupon = True

if vip or coupon:
    print("Discount Available")
```

---

## not

```python
logged_in = False

if not logged_in:
    print("Please Login")
```

```python
is_empty = False

if not is_empty:
    print("Data Exists")
```

---

## in / not in

```python
users = ["Kim", "Lee", "Park"]

if "Kim" in users:
    print("User Found")
```

```python
fruits = ["Apple", "Banana"]

if "Orange" not in fruits:
    print("Not Found")
```

---

## 중첩 조건문

```python
age = 20
has_license = True

if age >= 18:
    if has_license:
        print("Can Drive")
```

```python
logged_in = True
is_admin = False

if logged_in:
    if is_admin:
        print("Admin Page")
    else:
        print("User Page")
```

---

## 삼항 연산자

```python
number = 10

result = "Even" if number % 2 == 0 else "Odd"

print(result)
```

```python
age = 17

status = "Adult" if age >= 18 else "Minor"

print(status)
```

---

## 검증 루프 : 주문 승인 규칙 만들기

```python
stock_count = 5
request_count = 3
paid_amount = 90000
risk_score = 20

if risk_score >= 80:
    order_status = "Review"
elif request_count > stock_count:
    order_status = "Rejected"
elif paid_amount <= 0:
    order_status = "Rejected"
else:
    order_status = "Confirmed"

assert order_status == "Confirmed"

print(order_status)
```

```python
error_count = 3
latency_ms = 850

if error_count >= 10 or latency_ms >= 1000:
    alert_level = "Critical"
elif error_count >= 3 or latency_ms >= 700:
    alert_level = "Warning"
else:
    alert_level = "Normal"

assert alert_level == "Warning"

print(alert_level)
```

---

## Day 13 종합 복습

```python
value = 15

if value > 10:
    print("Big")
```

```python
score = 55

result = "Pass" if score >= 60 else "Fail"

print(result)
```