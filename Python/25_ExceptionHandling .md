---
title: "25_ExceptionHandling "
category: "Python"
date: 2026-07-13
tags: ["Python", "ExceptionHandling"]
---

# 예외처리 (Exception Handling)

파이썬에서 프로그램 실행 중 발생하는 오류를 어떻게 처리하는지 다룬다. 입력값을 정하고, 처리 로직을 실행하고, 출력 결과를 확인하는 흐름을 작은 코드 조각으로 하나씩 연결해서 익힌다.

## 흐름

1. **try-except 기본 입력 확인** — 문자열, 숫자, 변수 같은 예제 값과 필요한 조건을 먼저 정한다.
2. **특정 예외 처리 실행** — 기초 문법 코드를 실행해서 중간 결과를 확인한다.
3. **여러 예외 처리 결과 검증** — 출력 또는 마지막 표현식 결과를 기준으로 실행 결과를 비교한다.
4. **예외처리 재사용** — 완성한 코드를 작은 자동화 스크립트에 붙일 수 있도록 정리한다.

정리하면, 문자열·숫자·변수 같은 예제 값을 먼저 확인한 뒤 기초 문법에 맞는 코드를 고르고, 결과를 출력 또는 마지막 표현식 기준으로 즉시 점검하고, 완료한 코드는 이후 자동화 스크립트에 다시 활용할 수 있다.

---

## 1. try-except 기본 — 에러 처리하기

### 왜 배우는가
프로그램은 실행 도중 예상하지 못한 상황(0으로 나누기, 존재하지 않는 파일 열기, 잘못된 타입 연산 등)을 만나면 그 자리에서 멈춰버린다. `try-except`는 이런 상황에서 프로그램이 멈추지 않고 계속 동작하게 만드는 기본 도구다. 초보자 입장에서는 "에러가 나면 프로그램이 죽는다"는 두려움을 없애주는 첫 문법이고, 실무자 입장에서는 사용자 입력 처리나 외부 API 호출처럼 실패 가능성이 있는 모든 코드에 반드시 들어가는 기본 패턴이다.

### 상세 설명
- `try` 블록 안에 "실패할 수도 있는 코드"를 넣는다.
- `try` 블록에서 에러가 발생하면, 그 즉시 `except` 블록으로 넘어가서 실행된다.
- `try` 블록에서 에러가 발생하지 않으면 `except` 블록은 아예 실행되지 않는다.
- 이 구조 덕분에 프로그램이 중단되지 않고 계속 실행된다.
- **핵심 원칙**: `try-except`는 "예상되는 에러"를 처리할 때 쓴다. 무슨 에러가 날지 전혀 예상되지 않는 코드에 무작정 씌우는 용도가 아니다.


```python
try:
    result = 10 / 0
except ZeroDivisionError:
    result = 'Error occurred'

result
```

**코드 읽는 순서**
1. `try` 블록의 `10 / 0`이 실행된다.
2. 0으로 나누는 연산이라 `ZeroDivisionError`가 발생한다.
3. 에러가 발생했으므로 `except ZeroDivisionError:` 블록으로 넘어간다.
4. `result`에 `'Error occurred'`가 대입된다.
5. 마지막 줄 `result`를 실행하면 `'Error occurred'`가 출력된다.

---

## 2. 특정 예외 처리 — 예외 타입 지정

### 왜 배우는가
`except:`만 써서 모든 에러를 다 잡아버리면, 정작 내가 미처 예상하지 못한 심각한 버그까지 조용히 숨어버린다. `except` 뒤에 정확한 예외 타입을 적으면, "내가 예상한 에러만" 처리하고 나머지는 그대로 드러나게 만들 수 있다. 실무에서는 이 구분이 디버깅 시간을 크게 줄여준다.

### 상세 설명
- `except` 뒤에 예외 타입(`ZeroDivisionError`, `ValueError`, `TypeError` 등)을 지정할 수 있다.
- 지정한 타입의 에러만 처리하고, 그 외의 에러는 그대로 발생시켜서 프로그램이 멈추게 둔다.
- 이렇게 하면 "내가 미리 생각해둔 실패 상황"과 "전혀 예상하지 못한 버그"를 구분할 수 있다.
- 예외 타입을 지정하면 의도하지 않은 에러를 놓치지 않는다.

```python
try:
    num = 10 / 0
except ZeroDivisionError:
    num = 'Cannot divide by zero'

num
```
---

## 3. 여러 예외 처리 — 다양한 에러 대응

### 왜 배우는가
현실의 코드는 한 가지 방식으로만 실패하지 않는다. 예를 들어 사용자가 입력한 문자열을 숫자로 바꾸는 코드는 "숫자가 아닌 값이 들어올 수도 있고(ValueError)", "변환은 됐지만 0으로 나누게 될 수도 있다(ZeroDivisionError)". 이렇게 서로 다른 실패 상황마다 다른 대응이 필요할 때 여러 개의 `except`를 나란히 쓴다.

### 상세 설명
- `except` 블록을 여러 개 나열해서 서로 다른 예외를 각각 다르게 처리할 수 있다.
- 파이썬은 `except`를 위에서부터 순서대로 검사하며, 가장 먼저 일치하는 `except` 하나만 실행한다.
- **주의**: 구체적인 예외(더 좁은 범위)를 먼저 배치하고, 일반적인 예외(더 넓은 범위)를 나중에 배치해야 한다. 순서가 반대면 구체적인 예외 블록에 절대 도달하지 못하는 상황이 생긴다.

```python
try:
    val = int('10')
    ans = val / 2
except ValueError:
    ans = 'Invalid number'
except ZeroDivisionError:
    ans = 'Cannot divide'

ans
```

**코드 읽는 순서**
1. `int('10')`은 문자열 `'10'`을 정수로 바꾸는 데 성공하므로 에러가 없다. `val`은 `10`이 된다.
2. `val / 2`는 `10 / 2 = 5.0`이라 이것도 에러가 없다. `ans`는 `5.0`이 된다.
3. `try` 블록 전체가 에러 없이 끝났으므로 `except` 블록은 실행되지 않는다.
4. 마지막 줄 `ans`를 실행하면 `5.0`이 출력된다.

---

## 4. finally 절 — 항상 실행되는 코드

### 왜 배우는가
파일을 열었는데 중간에 에러가 나서 파일을 닫는 코드까지 건너뛰어 버리면, 파일이 계속 열린 상태로 남는 문제가 생긴다. `finally`는 "에러가 나든 안 나든 반드시 실행되어야 하는 뒷정리 코드"를 넣는 자리다.

### 상세 설명
- `finally` 블록은 `try` 블록에서 에러가 발생했는지 여부와 관계없이 항상 실행된다.
- 파일 닫기, 데이터베이스 연결 해제, 임시 자원 정리 같은 마무리 작업에 주로 사용한다.
- 실무 팁: 파이썬의 `with` 문을 사용하면 `finally` 없이도 자원을 자동으로 정리해준다. 파일이나 네트워크 연결처럼 "정리가 필요한 자원"을 다룰 때는 `with` 문을 우선적으로 고려한다.

```python
try:
    calc = 10 / 2
except ZeroDivisionError:
    calc = 0
finally:
    status = 'Completed'

calc, status
```

**코드 읽는 순서(초보자용)**
1. `10 / 2 = 5.0`이라 에러 없이 `calc`는 `5.0`이 된다.
2. 에러가 없었으므로 `except` 블록은 건너뛴다.
3. 에러 여부와 상관없이 `finally` 블록은 항상 실행되므로 `status = 'Completed'`가 대입된다.
4. 마지막 줄에서 `(calc, status)`, 즉 `(5.0, 'Completed')`가 출력된다.
---

## 5. raise로 예외 발생 — 의도적으로 에러 발생시키기

### 왜 배우는가
지금까지는 파이썬이 "자동으로" 발생시키는 에러(0으로 나누기 등)를 다뤘다. 하지만 실무에서는 "나이가 음수면 안 된다", "이메일 형식이 아니면 안 된다" 같은, 파이썬이 스스로는 알 수 없는 비즈니스 규칙 위반도 에러로 취급해야 할 때가 많다. `raise`는 이럴 때 직접 에러를 만들어서 발생시키는 키워드다.

### 상세 설명
- `raise` 키워드로 원하는 시점에 원하는 예외를 직접 발생시킬 수 있다.
- 조건이 맞지 않을 때 강제로 에러를 일으켜서, 프로그램을 중단시키거나 그 에러를 호출한 쪽으로 전달한다.
- 함수의 입력값이 올바른지 검증하는 용도로 자주 사용한다. 예를 들어 함수 맨 앞에서 입력값을 검사하고, 조건에 맞지 않으면 바로 `raise`로 알려준다.

```python
def checkAge(age):
    if age < 0:
        raise ValueError('Age cannot be negative')
    return age

try:
    valid = checkAge(-5)
except ValueError:
    valid = 'Invalid age'

valid
```

**코드 읽는 순서(초보자용)**
1. `checkAge(-5)`를 호출한다.
2. 함수 내부에서 `age < 0`(즉 `-5 < 0`)이 참이므로 `raise ValueError('Age cannot be negative')`가 실행되어 에러가 발생한다.
3. 이 에러는 함수를 호출한 `try` 블록으로 전달된다.
4. `except ValueError:` 블록이 실행되어 `valid`에 `'Invalid age'`가 대입된다.
5. 마지막 줄 `valid`를 실행하면 `'Invalid age'`가 출력된다.

---

## 6. 예외 정보 — 에러 메시지 받기

### 왜 배우는가
에러가 발생했다는 사실만 아는 것보다, "정확히 왜 실패했는지" 메시지를 확인할 수 있으면 문제를 훨씬 빠르게 찾을 수 있다. 특히 로그를 남기거나 사용자에게 실패 이유를 안내해야 하는 실무 코드에서는 이 정보가 필수적이다.

### 상세 설명
- `except 예외타입 as 변수:` 형식으로 쓰면, 발생한 예외 객체 자체를 변수에 담아서 사용할 수 있다.
- 이 예외 객체 안에는 에러 메시지와 관련 정보가 들어 있다.
- `str(err)`처럼 문자열로 변환하면 사람이 읽을 수 있는 에러 메시지를 얻는다.
- 예외 메시지를 확인하면 문제의 원인을 파악하기 쉬워진다.

```python
try:
    bad = 10 / 0
except ZeroDivisionError as err:
    bad = str(err)

bad
```

**코드 읽는 순서(초보자용)**
1. `10 / 0`에서 `ZeroDivisionError`가 발생한다.
2. `as err`로 이 예외 객체를 `err`라는 이름으로 받는다.
3. `str(err)`로 에러 메시지를 문자열로 바꿔서 `bad`에 대입한다.
4. 마지막 줄 `bad`를 실행하면 `'division by zero'`와 같은 에러 메시지 문자열이 출력된다.

---

## 7. 실패를 설명하는 예외 처리

### 왜 배우는가
지금까지 배운 문법을 하나로 합쳐서, 실무에서 실제로 쓰는 형태에 가깝게 연습하는 단계다. 좋은 예외 처리는 단순히 "에러를 조용히 숨겨서 프로그램을 안 죽게 만드는 것"이 아니다. 어떤 입력이, 왜 실패했는지를 명확히 설명하고, 정말 복구가 가능한 곳에서만 복구를 시도하는 것이 원칙이다.

### 상세 설명
- 숫자 변환, 필수 값 검증, 파일 읽기처럼 실패할 수 있는 지점마다 좁은 범위의 예외 타입을 지정해서 처리한다.
- `raise ... from exc` 구문을 쓰면, 원래 발생했던 예외(`exc`)를 새로운 예외의 원인으로 연결해서 함께 보여줄 수 있다. 이렇게 하면 "진짜 원인이 무엇이었는지" 추적이 쉬워진다.
- `assert` 문으로 예상한 값과 실제 값이 같은지 코드 안에서 바로 검증한다. 사람이 눈으로 결과를 확인하는 대신, 코드가 스스로 검증하게 만드는 방식이다.

```python
def parseQuantity(text):
    try:
        quantity = int(text)
    except ValueError as exc:
        raise ValueError(f'quantity must be integer: {text!r}') from exc

    if quantity <= 0:
        raise ValueError('quantity must be positive')
    return quantity

assert parseQuantity('3') == 3

try:
    parseQuantity('3개')
except ValueError as exc:
    quantityProblem = str(exc)

assert 'integer' in quantityProblem
quantityProblem
```

**코드 읽는 순서(초보자용)**
1. `parseQuantity('3')`을 호출하면 `int('3')`이 성공해서 `3`을 반환한다. `assert parseQuantity('3') == 3`은 예상과 실제가 같으므로 통과한다.
2. `parseQuantity('3개')`를 호출하면 `int('3개')`가 실패해서 `ValueError`가 발생한다.
3. 함수 내부의 `except ValueError as exc:`가 이 에러를 받아서, 원래 에러(`exc`)를 원인으로 삼아 더 친절한 메시지의 새 `ValueError`를 다시 `raise`한다.
4. 이 새 에러는 함수 밖의 `try-except`로 전달되어 `quantityProblem`에 에러 메시지 문자열이 저장된다.
5. `assert 'integer' in quantityProblem`으로 메시지 안에 `'integer'`라는 단어가 들어 있는지 검증한다.
6. 마지막 줄 `quantityProblem`을 실행하면 `"quantity must be integer: '3개'"`와 같은 메시지가 출력된다.

---

## 8. 예외 처리 마스터하기 복습

### 왜 배우는가
1~7번에서 따로 배운 개념(`try-except` 기본, 예외 타입 지정, 여러 예외 처리, `finally`, `raise`, 예외 정보 확인, 검증 루프)을 한 번에 복습하면서 스스로 얼마나 이해했는지 점검하는 단계다.

```python
def safeDivide(a, b):
    try:
        divResult = a / b
    except ZeroDivisionError as exc:
        raise ZeroDivisionError(f'cannot divide {a} by zero') from exc
    finally:
        print('division attempt finished')
    return divResult

# 기본 미션: 정상 나눗셈
divResult = safeDivide(10, 2)

# 심화 미션: 0으로 나누는 상황을 예외 메시지로 확인
try:
    safeDivide(10, 0)
except ZeroDivisionError as exc:
    divResult = str(exc)

divResult
```

---

## 전체 핵심 요약

| 문법 | 언제 쓰는가 | 한 줄 요약 |
|---|---|---|
| `try-except` | 실패할 수 있는 코드를 실행할 때 | 에러가 나도 프로그램이 멈추지 않게 한다 |
| `except 특정타입` | 예상한 에러만 골라서 처리하고 싶을 때 | 예상 밖 에러까지 숨기지 않는다 |
| 여러 개의 `except` | 실패 원인이 여러 가지일 때 | 원인별로 다르게 대응한다 |
| `finally` | 에러 여부와 상관없이 꼭 해야 할 일이 있을 때 | 뒷정리를 보장한다 |
| `raise` | 규칙 위반을 직접 에러로 알려야 할 때 | 원하는 시점에 원하는 에러를 만든다 |
| `except ... as 변수` | 에러의 구체적인 이유를 확인하고 싶을 때 | 에러 메시지를 코드에서 활용한다 |
| `assert` + 좁은 예외 처리 | 코드가 스스로 결과를 검증하게 하고 싶을 때 | 사람 대신 코드가 확인한다 |