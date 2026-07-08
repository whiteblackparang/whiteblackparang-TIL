---
title: "06_String_Methods"
category: "Python"
date: 2026-07-04
tags: ["Python", "String"]
---

# String Methods

Python에서 문자열(String)은 변경할 수 없는(Immutable) 자료형.  
문자열 메서드는 **원본 문자열을 변경하지 않고 새로운 문자열을 반환**한다.

---

## 대문자 변환 `upper()`

- 문자열의 모든 영문자를 대문자로 변환한다.
- 숫자와 특수문자는 그대로 유지된다.

```python
text = "hello python"

print(text.upper())
```

출력

```text
HELLO PYTHON
```

---

## 소문자 변환 `lower()`

- 문자열의 모든 영문자를 소문자로 변환한다.
- 대소문자를 구분하지 않는 비교를 할 때 자주 사용한다.

```python
msg = "HELLO PYTHON"

print(msg.lower())
```

출력

```text
hello python
```

---

## 첫 글자만 대문자 `capitalize()`

- 첫 글자만 대문자로 변환한다.
- 나머지 문자는 모두 소문자로 변환된다.

```python
phrase = "hello PYTHON"

print(phrase.capitalize())
```

출력

```text
Hello python
```

---

## 제목 형식 `title()`

- 각 단어의 첫 글자를 대문자로 변환한다.

```python
sentence = "hello python programming"

print(sentence.title())
```

출력

```text
Hello Python Programming
```

---

## 양쪽 공백 제거 `strip()`

- 문자열의 앞뒤 공백을 제거한다.
- 문자열 중간의 공백은 제거되지 않는다.

```python
raw = "   hello python   "

print(raw.strip())
```

출력

```text
hello python
```

---

## 왼쪽 공백 제거 `lstrip()`

- 문자열 왼쪽의 공백만 제거한다.

```python
text = "   hello"

print(text.lstrip())
```

출력

```text
hello
```

---

## 오른쪽 공백 제거 `rstrip()`

- 문자열 오른쪽의 공백만 제거한다.

```python
text = "hello   "

print(text.rstrip())
```

출력

```text
hello
```

---

## 문자열 치환 `replace()`

- 특정 문자열을 다른 문자열로 변경한다.
- 원본 문자열은 변경되지 않는다.

```python
greeting = "Hello World"

print(greeting.replace("World", "Python"))
```

출력

```text
Hello Python
```

### 여러 문자열 치환

```python
text = "apple apple orange apple"

print(text.replace("apple", "banana"))
```

출력

```text
banana banana orange banana
```

---

## 문자열 나누기 `split()`

- 특정 문자를 기준으로 문자열을 나눈다.
- 결과는 리스트(List)로 반환된다.

```python
row = "kim@example.com,paid,15000"

print(row.split(","))
```

출력

```python
['kim@example.com', 'paid', '15000']
```

---

## 문자열 합치기 `join()`

- 리스트를 하나의 문자열로 합친다.
- 구분자를 지정할 수 있다.

```python
fields = ["kim@example.com", "paid", "15000"]

print("|".join(fields))
```

출력

```text
kim@example.com|paid|15000
```

---

## 문자열 개수 세기 `count()`

- 특정 문자나 문자열이 몇 번 등장하는지 반환한다.

```python
words = "hello hello world hello"

print(words.count("hello"))
```

출력

```text
3
```

---

## 문자열 위치 찾기 `find()`

- 찾은 문자열의 첫 번째 위치(인덱스)를 반환한다.
- 찾지 못하면 `-1`을 반환한다.

```python
code = "Hello Python Programming"

print(code.find("Python"))
```

출력

```text
6
```

### 찾지 못한 경우

```python
text = "Hello World"

print(text.find("Python"))
```

출력

```text
-1
```

---

## 시작 문자 확인 `startswith()`

- 문자열이 특정 문자열로 시작하는지 확인한다.
- 반환값은 `True` 또는 `False`이다.

```python
title = "Python Programming"

print(title.startswith("Python"))
```

출력

```text
True
```

---

## 끝 문자 확인 `endswith()`

- 문자열이 특정 문자열로 끝나는지 확인한다.
- 반환값은 `True` 또는 `False`이다.

```python
file = "script.py"

print(file.endswith(".py"))
```

출력

```text
True
```

---

# 문자열 메서드 활용 예제

사용자의 입력값을 정리하는 예제이다.

```python
raw_line = "  KIM@Example.COM ,  PAID ,  15000원  "

fields = raw_line.split(",")

email = fields[0].strip().lower()
status = fields[1].strip().lower()
amount = int(fields[2].strip().replace("원", ""))

print(email)
print(status)
print(amount)
```

출력

```text
kim@example.com
paid
15000
```

---

# 파일명 정리 예제

```python
raw_file_name = "  Sales Report 2026 FINAL.CSV  "

clean_file_name = (
    raw_file_name
    .strip()
    .lower()
    .replace(" ", "_")
)

print(clean_file_name)
print(clean_file_name.endswith(".csv"))
```

출력

```text
sales_report_2026_final.csv
True
```

---

# 자주 사용하는 문자열 메서드

| 메서드 | 설명 |
|---------|------|
| `upper()` | 모두 대문자로 변환 |
| `lower()` | 모두 소문자로 변환 |
| `capitalize()` | 첫 글자만 대문자로 변환 |
| `title()` | 각 단어의 첫 글자를 대문자로 변환 |
| `strip()` | 양쪽 공백 제거 |
| `lstrip()` | 왼쪽 공백 제거 |
| `rstrip()` | 오른쪽 공백 제거 |
| `replace()` | 문자열 변경 |
| `split()` | 문자열을 리스트로 분리 |
| `join()` | 리스트를 문자열로 합치기 |
| `count()` | 문자열 개수 세기 |
| `find()` | 문자열 위치 찾기 |
| `startswith()` | 시작 문자열 확인 |
| `endswith()` | 끝 문자열 확인 |