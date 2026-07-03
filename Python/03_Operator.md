---
title: "Operators"
category: "Python"
date: 2026-07-03
tags: ["Python", "Operators"]
---

# Operators 연산자


## 1. 덧셈 연산자
- 두 숫자를 더한다
  - 정수끼리
  - 소수끼리
  - 정수와 소수 섞어서

```python
itemA = 10
itemB = 20
itemA + itemB
```

## 2. 뺄셈 연산자

- 음수 결과도 가능
- 순서가 중요

```python
budget = 100
spent = 35
budget - spent
```
---
## 3. 곱셈 연산자 (`*`)

- `*`는 두 숫자를 곱하는 연산자이다.
- 수학에서는 `×`를 사용하지만, Python에서는 `*`를 사용한다.
- 숫자뿐만 아니라 변수끼리도 곱셈이 가능하다.

```python
3 * 4
```

출력 결과

```text
12
```

변수를 사용한 예제

```python
width = 7
height = 8

width * height
```

출력 결과

```text
56
```
---
---

## 4. 나눗셈 연산자 (`/`)

- `/`는 앞의 숫자를 뒤의 숫자로 나눈다.
- 나눗셈의 결과는 항상 **실수(float)** 로 반환된다.

```python
10 / 2
```

출력 결과

```text
5.0
```

```python
dividend = 10
divisor = 3

dividend / divisor
```

출력 결과

```text
3.3333333333333335
```

## 5. 몫 연산자 (// 기호로 몫을 구함)
- //는 나눗셈의 몫만 구한다
- 소수점 이하는 버림, 정수 부분은 반환 