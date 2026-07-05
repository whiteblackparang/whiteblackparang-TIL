---
title: "List Methods"
category: "Python"
date: 2026-07-04
tags: ["Python", "List"]
---

---
title: "List Methods"
category: "Python"
date: 2026-07-04
tags: ["Python", "List"]
---

# 리스트 메서드 (List Methods)

## 리스트 메서드란?

리스트 메서드(Method)는 리스트 객체가 제공하는 기능입니다.

- 리스트의 요소를 추가, 삭제, 정렬, 검색할 수 있습니다.
- 대부분의 메서드는 원본 리스트를 직접 변경합니다.
- 대부분의 메서드는 반환값이 `None`입니다.
- `리스트.메서드()` 형태로 호출합니다.

---

## append()

리스트의 **끝에 요소 하나를 추가**합니다.

- 한 번에 하나의 요소만 추가합니다.
- 원본 리스트가 변경됩니다.
- 가장 많이 사용하는 메서드입니다.

**문법**

```python
리스트.append(값)
```

---

## insert()

원하는 위치에 요소를 삽입합니다.

- 기존 요소들은 뒤로 밀립니다.
- 위치를 직접 지정할 수 있습니다.

**문법**

```python
리스트.insert(인덱스, 값)
```

---

## remove()

특정 값을 찾아 삭제합니다.

- 첫 번째로 찾은 값만 삭제합니다.
- 값이 없으면 오류가 발생합니다.

**문법**

```python
리스트.remove(값)
```

---

## pop()

인덱스의 요소를 삭제하고 반환합니다.

- 인덱스를 생략하면 마지막 요소를 삭제합니다.
- 삭제한 값을 변수에 저장할 수 있습니다.

**문법**

```python
리스트.pop()
리스트.pop(인덱스)
```

---

## clear()

리스트의 모든 요소를 삭제합니다.

- 리스트는 유지됩니다.
- 내용만 빈 리스트가 됩니다.

**문법**

```python
리스트.clear()
```

---

## extend()

다른 리스트의 모든 요소를 추가합니다.

- 리스트를 합칠 때 사용합니다.
- 원본 리스트가 변경됩니다.

**문법**

```python
리스트.extend(다른리스트)
```

---

## sort()

리스트를 정렬합니다.

- 기본은 오름차순입니다.
- `reverse=True`를 사용하면 내림차순입니다.
- 원본 리스트가 변경됩니다.

**문법**

```python
리스트.sort()
리스트.sort(reverse=True)
```

---

## reverse()

리스트의 순서를 뒤집습니다.

- 정렬이 아닙니다.
- 순서만 반대로 변경합니다.

**문법**

```python
리스트.reverse()
```

---

## copy()

리스트의 얕은 복사본을 만듭니다.

- 새로운 리스트가 생성됩니다.
- 원본과 복사본은 서로 영향을 주지 않습니다.

**문법**

```python
새리스트 = 기존리스트.copy()
```

---

## count()

특정 값의 개수를 셉니다.

- 리스트는 변경되지 않습니다.
- 개수만 반환합니다.

**문법**

```python
리스트.count(값)
```

---

## index()

특정 값의 위치를 찾습니다.

- 첫 번째 위치만 반환합니다.
- 값이 없으면 오류가 발생합니다.

**문법**

```python
리스트.index(값)
```

---

## append()

```python
fruits = ['사과', '바나나']

fruits.append('오렌지')
fruits.append('포도')

print(fruits)
```

---

## insert()

```python
colors = ['빨강', '파랑']

colors.insert(1, '초록')

print(colors)
```

---

## remove()

```python
items = ['사과', '바나나', '오렌지', '바나나']

items.remove('바나나')

print(items)
```

---

## pop()

```python
nums = [10, 20, 30, 40]

last = nums.pop()

print(last)
print(nums)
```

---

## clear()

```python
nums = [1, 2, 3, 4]

nums.clear()

print(nums)
```

---

## extend()

```python
base = [1, 2, 3]
extra = [4, 5, 6]

base.extend(extra)

print(base)
```

---

## sort()

```python
data = [5, 2, 8, 1, 9]

data.sort()

print(data)
```

---

## reverse()

```python
chars = ['A', 'B', 'C', 'D']

chars.reverse()

print(chars)
```

---

## copy()

```python
orig = [1, 2, 3]

copy_list = orig.copy()

copy_list.append(4)

print(orig)
print(copy_list)
```

---

## count()

```python
nums = [1, 2, 3, 2, 4, 2]

print(nums.count(2))
```

---

## index()

```python
fruits = ['사과', '바나나', '오렌지', '바나나']

print(fruits.index('바나나'))
```

---

## 실전 예제

```python
orderQueue = ['ORD-1001', 'ORD-1002']

orderQueue.append('ORD-1003')
orderQueue.insert(0, 'URG-9001')

processedOrder = orderQueue.pop(0)
orderQueue.remove('ORD-1002')

print(processedOrder)
print(orderQueue)
```