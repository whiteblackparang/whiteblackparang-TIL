---
title: "Variable and DataType"
category: "Python"
date: 2026-07-03
tags: ["Python", "DataType"]
---

# 변수 
변수는 데이터를 저장하기 위햏 사용하는 이름이다. 

```python

df = 'Python'
df

```
---

# 변수 이름 작성법
- 카멜케이스(camelCase)는 첫 단어는 소문자로 시작하고 이후 단어의 첫 글자를 대문자
- 스네이크케이스(snake_case)는 모든 단어를 소문자로 쓰고 밑줄로 연결

- 카멜 케이스(camelCase) : `userName`
- 스네이크 케이스(snake_case) : `user_name`
- Python 공식 스타일 가이드(PEP 8)는 **snake_case**를 권장한다.

---

# 변수 값 변경하기 
- 변수는 언제든 새 값으로 변경가능 
- 변수는 할당된 값을 가짐 

```python

score = 80
score = 95
score

```
---

# 정수 타입 

- 정수는 소수점이 없는 숫자
- 양수, 0, 음수 모두 정수
- 정수 타입은 int로 표시

# 실수 타입 
- 소수점이 있는 숫자
- 실수 타입은 float로 표시

# 문자열 타입 
- 문자열(String)은 따옴표를 사용하여 작성한다
- 문자열 타입은 str로 표시

```python
town = 'Seoul'
town
```

# 불린 타입 (True / False)
- 두 가지 값인 True / False를 가진 타입
- 첫 글자가 반드시 대문자여야 함
- 조건 판단, 비교 연산에서 주로 사용 

```python
active = True
active
```

# Type 함수 
- 변수나 값의 데이터 타입을 확인하는 함수
- 괄호 안에 변수나 값을 넣으면 해당 타입을 반환
- `<class 'int'>`, `<class 'str'>`와 같은 형태로 출력된다.
- 디버깅이나 타입 확인 시 자주 사용한다.
```python
age = 20
type(age)

name = 'Python'
type(name)
```
---

# len 함수
- 문자열의 길이(문자개수)를 반환 
- 공백, 특수문자, 한글 모두 각각 1로 계산
- 빈 문자열의 길이는 0
```python
text = 'Python'
len(text)
```

---
# int 변환 
- 다른 타입을 정수로 변환 
- 문자열은 숫자로만 이루어져 있어야함 

---

# float 변환 
- 다른 타입을 실수로 변환

```python
float(10)
float('3.14')
```

# str 변환
- 다른 타입의 값을 문자열(String)로 변환한다.
- 숫자와 문자열을 함께 사용하거나 출력할 때 자주 사용
- 문자열 연결이나 출력 메시지를 만들 때 사용

```python
str(100)
```

```python
age = 20
str(age)
```

출력 결과

```text
'20'
```
문자열과 숫자를 `+`로 연결하려면 숫자를 문자열로 변환해야 한다.

```python
age = 20

print('나이: ' + str(age))
```

출력 결과

```text
나이: 20
```

# 다중 변수 할당

- 쉼표(`,`)로 구분하여 한 줄에서 여러 변수를 동시에 선언하고 값을 할당
- 왼쪽 변수와 오른쪽 값이 순서대로 대응
- 코드를 간결하게 작성 가능

```python
name, age, city = 'Alice', 25, 'Seoul'

name
age
city
```

또는

```python
name, age, city
```

출력 결과

```text
('Alice', 25, 'Seoul')
```

---

# 같은 값 할당

- `=` 연산자를 연속해서 사용하면 여러 변수에 같은 값을 동시에 할당할 수 있음

```python
a = b = c = 0

a
b
c
```

또는

```python
a, b, c
```

출력 결과

```text
(0, 0, 0)
```

---

# 변수 값 교환하기

- Python은 임시 변수를 만들지 않고 두 변수의 값을 서로 바꿀 수 있다.

```python
a = 10
b = 20

a, b = b, a

a, b
```

출력 결과

```text
(20, 10)
```