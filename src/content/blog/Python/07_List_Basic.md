---
title: "07_List_Basic"
category: "Python"
date: 2026-07-04
tags: ["Python", "List"]
---

# List Basic

리스트(List)는 여러 개의 값을 **순서대로 저장**하는 자료형이다.

- 대괄호(`[]`)를 사용하여 생성한다.
- 하나의 리스트에 숫자, 문자열, Boolean 등 다양한 자료형을 함께 저장할 수 있다.
- 각 요소는 **인덱스(Index)** 를 통해 접근한다.
- 리스트는 **수정 가능한(Mutable)** 자료형이다.

---

## 리스트 생성

리스트는 여러 개의 값을 하나의 변수에 저장할 수 있다.

```python
items = [1, 2, 3, 4, 5]
print(items)
```

출력

```text
[1, 2, 3, 4, 5]
```

### 예제

```python
fruits = ["사과", "바나나", "포도"]
print(fruits)
```

출력

```text
['사과', '바나나', '포도']
```

---

## 다양한 리스트 생성

리스트에는 다양한 자료형을 저장할 수 있다.

```python
nums = [10, 20, 30]
```

### 문자열 리스트

```python
colors = ["red", "green", "blue"]
```

### 여러 자료형 저장

```python
data = [1, "Python", True, 3.14]
```

### 빈 리스트 생성

```python
empty = []
```

---

## 리스트 인덱싱(Indexing)

리스트의 요소는 **0부터 시작하는 인덱스**를 이용하여 접근한다.

```python
basket = ["사과", "바나나", "오렌지", "포도"]

print(basket[0])
```

출력

```text
사과
```

### 예제

```python
numbers = [10, 20, 30]

print(numbers[2])
```

출력

```text
30
```

---

## 음수 인덱싱

음수 인덱스를 사용하면 뒤에서부터 접근할 수 있다.

```python
values = [10, 20, 30, 40, 50]

print(values[-1])
```

출력

```text
50
```

### 예제

```python
fruits = ["사과", "바나나", "포도"]

print(fruits[-2])
```

출력

```text
바나나
```

---

## 리스트 슬라이싱(Slicing)

슬라이싱은 리스트의 일부를 잘라 새로운 리스트를 만든다.

```python
rainbow = ["빨강", "주황", "노랑", "초록", "파랑", "남색", "보라"]

print(rainbow[:3])
```

출력

```text
['빨강', '주황', '노랑']
```

### 예제

```python
numbers = [1, 2, 3, 4, 5]

print(numbers[1:4])
```

출력

```text
[2, 3, 4]
```

---

## 슬라이싱 Step

`시작:끝:간격(step)` 형태로 사용할 수 있다.

```python
sequence = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(sequence[::2])
```

출력

```text
[0, 2, 4, 6, 8]
```

### 리스트 뒤집기

```python
print(sequence[::-1])
```

출력

```text
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

---

## 리스트 수정

리스트는 생성 후에도 값을 변경할 수 있다.

```python
fruits = ["사과", "바나나", "오렌지"]

fruits[1] = "포도"

print(fruits)
```

출력

```text
['사과', '포도', '오렌지']
```

### 예제

```python
numbers = [1, 2, 3]

numbers[0] = 100

print(numbers)
```

출력

```text
[100, 2, 3]
```

---

## 리스트 길이 `len()`

`len()` 함수는 리스트의 요소 개수를 반환한다.

```python
goods = ["사과", "바나나", "오렌지", "포도", "딸기"]

print(len(goods))
```

출력

```text
5
```

### 예제

```python
numbers = [1, 2, 3]

print(len(numbers))
```

출력

```text
3
```

---

## 리스트 연결

`+` 연산자를 이용하여 리스트를 합칠 수 있다.

```python
front = [1, 2, 3]
back = [4, 5, 6]

print(front + back)
```

출력

```text
[1, 2, 3, 4, 5, 6]
```

### 예제

```python
a = ["A", "B"]
b = ["C", "D"]

print(a + b)
```

출력

```text
['A', 'B', 'C', 'D']
```

---

## 리스트 반복

`*` 연산자를 사용하면 리스트를 여러 번 반복할 수 있다.

```python
base = [1, 2, 3]

print(base * 3)
```

출력

```text
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

### 예제

```python
zeros = [0] * 5

print(zeros)
```

출력

```text
[0, 0, 0, 0, 0]
```

---

## 포함 여부 확인 `in`, `not in`

리스트 안에 특정 값이 있는지 확인한다.

```python
bucket = ["사과", "바나나", "오렌지"]

print("바나나" in bucket)
```

출력

```text
True
```

### 예제

```python
numbers = [10, 20, 30]

print(50 not in numbers)
```

출력

```text
True
```

---

# 예제

```python
product_names = ["노트북", "마우스", "키보드", "모니터"]
stock_counts = [5, 20, 0, 8]

print(product_names[0])
print(stock_counts[-1])
print(len(product_names))
```

출력

```text
노트북
8
4
```

---

# 슬라이싱 활용 예제

```python
daily_tasks = ["메일 확인", "견적서 작성", "고객 연락", "보고서 정리"]

urgent_tasks = daily_tasks[:2]
later_tasks = daily_tasks[2:]

print(urgent_tasks)
print(later_tasks)
```

출력

```text
['메일 확인', '견적서 작성']
['고객 연락', '보고서 정리']
```

---

# 자주 사용하는 리스트 기능

| 기능 | 설명 |
|------|------|
| `[]` | 리스트 생성 |
| `list[index]` | 요소 접근 |
| `list[-1]` | 마지막 요소 접근 |
| `list[start:end]` | 슬라이싱 |
| `list[start:end:step]` | 간격을 두고 슬라이싱 |
| `len(list)` | 리스트 길이 |
| `list1 + list2` | 리스트 연결 |
| `list * n` | 리스트 반복 |
| `in` | 요소 포함 여부 확인 |
| `not in` | 요소 미포함 여부 확인 |

---

# 핵심 정리

- 리스트는 여러 개의 값을 순서대로 저장하는 자료형
- 인덱스는 **0부터 시작**
- 음수 인덱스를 사용하면 뒤에서부터 접근할 수 있음
- 슬라이싱은 원본 리스트를 변경하지 않고 새로운 리스트를 반환
- 리스트는 값을 수정할 수 있는 **Mutable** 자료형
- `len()`으로 리스트 길이를 확인할 수 있음
- `+`는 리스트를 연결하고, `*`는 리스트를 반복
- `in`, `not in`으로 특정 요소의 포함 여부를 확인할 수 있음