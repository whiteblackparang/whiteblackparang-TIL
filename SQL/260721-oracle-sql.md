---
title: "260709 [오라클 SQL] 기초 : 문자열 함수"
category: "SQL"
dialect: "Oracle"
date: 2026-07-21
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 문자열 함수

SQL에서는 데이터를 조회하는 것뿐만 아니라 문자열을 가공하는 작업도 자주 수행한다. 이름의 대소문자를 변경하거나, 문자열의 일부를 추출하고, 특정 문자를 찾아 변경하거나 공백을 제거하는 등 다양한 문자열 함수를 제공한다.

이번 글에서는 Oracle에서 자주 사용하는 문자열 함수인 **UPPER**, **LOWER**, **INITCAP**, **LENGTH**, **SUBSTR**, **INSTR**, **REPLACE**, **LPAD**, **RPAD**, **TRIM** 함수에 대해 정리해보았다.

이번 글에서 다루는 내용은 다음과 같다.

- 대소문자 변환
  - UPPER
  - LOWER
  - INITCAP
- 문자열 길이
  - LENGTH
- 문자열 추출
  - SUBSTR
- 문자열 검색
  - INSTR
- 문자열 변경
  - REPLACE
- 문자열 채우기
  - LPAD
  - RPAD
- 공백 및 문자 제거
  - TRIM
  - LTRIM
  - RTRIM

---

# 1. 대소문자 변환

Oracle에서는 문자열의 대소문자를 쉽게 변환할 수 있는 함수를 제공한다.

- **UPPER** : 모든 문자를 대문자로 변환
- **LOWER** : 모든 문자를 소문자로 변환
- **INITCAP** : 첫 글자는 대문자, 나머지는 소문자로 변환

예를 들어 다음과 같이 사용할 수 있다.

```sql
SELECT UPPER('oracle') FROM DUAL;
SELECT LOWER('ORACLE') FROM DUAL;
SELECT INITCAP('oracle sql') FROM DUAL;
```

실행 결과는 다음과 같다.

```
ORACLE
oracle
Oracle Sql
```

`INITCAP`은 Python의 `capitalize()`와 비슷한 기능으로 이해하면 쉽다.

이 함수들은 사용자 입력값의 형식을 통일하거나 검색 조건을 비교할 때 자주 사용된다.

---

# 2. LENGTH

`LENGTH` 함수는 문자열의 길이를 반환한다.

```sql
SELECT ENAME,
       LENGTH(ENAME)
FROM EMP;
```

실행 결과

|ENAME|LENGTH|
|:---|:---:|
|SMITH|5|
|ALLEN|5|
|WARD|4|

문자열의 길이를 확인하여 입력값을 검증하거나 데이터의 길이를 제한해야 하는 경우 유용하게 사용할 수 있다.

> 참고로 Oracle에서는 하나의 행에 하나의 결과를 반환하는 함수를 **단일행 함수(Single Row Function)** 라고 하며, `LENGTH`도 단일행 함수에 해당한다.

---

# 3. SUBSTR

`SUBSTR` 함수는 문자열의 일부를 추출하는 함수이다.

기본 문법은 다음과 같다.

```sql
SUBSTR(문자열, 시작위치, 길이)
```

예를 들어 직무(JOB)의 앞부분을 추출하면 다음과 같다.

```sql
SELECT JOB,
       SUBSTR(JOB, 1, 2),
       SUBSTR(JOB, 2, 3),
       SUBSTR(JOB, 3, 4)
FROM EMP;
```

실행 결과는 각각

- 첫 번째 문자부터 2글자
- 두 번째 문자부터 3글자
- 세 번째 문자부터 4글자

를 반환한다.

---

## 음수 사용하기

시작 위치를 음수로 지정하면 문자열의 뒤에서부터 계산한다.

```sql
SELECT JOB,
       SUBSTR(JOB, -4, 2),
       SUBSTR(JOB, -4, 3),
       SUBSTR(JOB, -4, 4)
FROM EMP;
```

예를 들어 `MANAGER`라면 뒤에서 네 번째 문자부터 원하는 길이만큼 추출한다.

또한 문자열의 처음을 의미하는 위치를 `LENGTH` 함수와 함께 사용할 수도 있다.

```sql
SELECT JOB,
       SUBSTR(JOB, -LENGTH(JOB), 2)
FROM EMP;
```

`SUBSTR`는 주민등록번호, 전화번호, 날짜 문자열 등 필요한 일부 정보만 추출할 때 자주 사용된다.

---

# 4. INSTR

`INSTR` 함수는 문자열에서 특정 문자의 위치를 찾는 함수이다.

기본 문법은 다음과 같다.

```sql
INSTR(문자열, 찾을문자, 시작위치, 몇번째)
```

예제를 살펴보면 다음과 같다.

```sql
SELECT INSTR('HELLO, ORACLE!', 'L')
FROM DUAL;
```

결과

```
3
```

첫 번째 `L`의 위치를 반환한다.

---

검색 시작 위치를 지정할 수도 있다.

```sql
SELECT INSTR('HELLO, ORACLE!', 'L', 5)
FROM DUAL;
```

5번째 문자부터 검색을 시작한다.

또한 몇 번째 문자를 찾을 것인지도 지정할 수 있다.

```sql
SELECT INSTR('HELLO, ORACLE!', 'L', 2, 3)
FROM DUAL;
```

위 SQL은 두 번째 문자부터 검색을 시작하여 세 번째 `L`의 위치를 반환한다.

문자열 안에서 특정 문자나 단어의 위치를 찾아야 할 때 자주 사용하는 함수이다.

---

# 5. REPLACE

`REPLACE` 함수는 특정 문자열을 다른 문자열로 변경한다.

기본 문법은 다음과 같다.

```sql
REPLACE(문자열, 기존문자열, 변경문자열)
```

예를 들어 전화번호의 `-`를 공백으로 변경하면 다음과 같다.

```sql
SELECT REPLACE('010-1234-5678', '-', ' ')
FROM DUAL;
```

결과

```
010 1234 5678
```

공백뿐만 아니라 다른 문자로도 변경할 수 있으며, 문자열 전처리 작업에서 자주 사용된다.

---

# 6. LPAD와 RPAD

`LPAD`와 `RPAD`는 문자열의 길이를 지정한 후 부족한 부분을 원하는 문자로 채우는 함수이다.

- **LPAD** : 왼쪽을 채운다.
- **RPAD** : 오른쪽을 채운다.

예제를 살펴보면 다음과 같다.

```sql
SELECT LPAD('ORACLE', 10, '#')
FROM DUAL;
```

결과

```
####ORACLE
```

오른쪽을 채우는 경우는 다음과 같다.

```sql
SELECT RPAD('ORACLE', 8, '-')
FROM DUAL;
```

결과

```
ORACLE--
```

전화번호 마스킹에도 활용할 수 있다.

```sql
SELECT RPAD('010-1234-', 13, '*')
FROM DUAL;
```

결과

```
010-1234-****
```

실무에서는 개인정보 보호를 위해 주민등록번호나 전화번호 일부를 가릴 때 자주 사용된다.

---

# 7. TRIM

`TRIM` 함수는 문자열의 공백이나 특정 문자를 제거하는 함수이다.

Python의 `strip()` 함수와 비슷한 기능이라고 생각하면 이해하기 쉽다.

Oracle에서는 다음과 같은 함수들을 제공한다.

- **TRIM** : 양쪽 공백 제거
- **LTRIM** : 왼쪽 공백 제거
- **RTRIM** : 오른쪽 공백 제거

예제를 살펴보면 다음과 같다.

```sql
SELECT TRIM('   ORACLE!   ')
FROM DUAL;
```

결과

```
ORACLE!
```

왼쪽 공백만 제거하려면

```sql
SELECT LTRIM('   ORACLE!   ')
FROM DUAL;
```

오른쪽 공백만 제거하려면

```sql
SELECT RTRIM('   ORACLE!   ')
FROM DUAL;
```

처럼 사용할 수 있다.

특정 문자도 제거할 수 있다.

```sql
SELECT RTRIM('ORACLE!!!', '!')
FROM DUAL;
```

결과

```
ORACLE
```

데이터를 정제하거나 불필요한 공백과 특수문자를 제거할 때 매우 유용한 함수이다.

---

# 마무리

이번 글에서는 Oracle에서 자주 사용하는 문자열 함수들을 정리했다.

- **UPPER**, **LOWER**, **INITCAP**은 문자열의 대소문자를 변환한다.
- **LENGTH**는 문자열의 길이를 반환한다.
- **SUBSTR**는 문자열의 일부를 추출한다.
- **INSTR**는 특정 문자열의 위치를 찾는다.
- **REPLACE**는 문자열을 다른 문자열로 변경한다.
- **LPAD**, **RPAD**는 지정한 길이에 맞게 문자열을 채운다.
- **TRIM**, **LTRIM**, **RTRIM**은 공백이나 특정 문자를 제거한다.

문자열 함수는 SQL에서 데이터를 조회한 이후 원하는 형태로 가공하는 데 가장 많이 사용하는 함수 중 하나이다. 여러 함수를 함께 조합하면 다양한 문자열 처리 작업을 효율적으로 수행할 수 있으므로 기본 사용법을 익혀두는 것이 좋다.