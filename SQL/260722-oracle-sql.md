---
title: "260722 [오라클 SQL] 기초 : 숫자 함수와 날짜 함수, 형 변환"
category: "SQL"
dialect: "Oracle"
date: "2026-07-22"
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 숫자 함수와 날짜 함수, 형 변환

SQL에서는 데이터를 조회하는 것뿐만 아니라 숫자를 계산하고, 날짜를 처리하며, 데이터 타입을 변환하는 작업도 자주 수행한다.

Oracle에서는 이를 위해 다양한 내장 함수를 제공한다. 이번 글에서는 숫자 계산에 사용하는 함수부터 날짜 함수, 다중행 함수(Aggregate Function), 그리고 형 변환 함수까지 정리해보았다.

이번 글에서 다루는 내용은 다음과 같다.

- DUAL 테이블
- 숫자 함수
  - ROUND
  - TRUNC
  - CEIL
  - FLOOR
  - MOD
- 다중행 함수
  - SUM
  - COUNT
  - MAX
  - MIN
  - AVG
- 날짜 함수
  - SYSDATE
  - ADD_MONTHS
- 형 변환
  - TO_CHAR
  - TO_NUMBER
  - TO_DATE

---

# 1. DUAL 테이블

`DUAL`은 Oracle에서 제공하는 **가상의 테이블**이다.

실제 데이터를 저장하는 테이블이 아니라, 함수 실행이나 간단한 계산 결과를 확인하기 위해 사용하는 **1행 1열의 임시 테이블**이다.

예를 들어 간단한 계산은 다음과 같이 수행할 수 있다.

```sql
SELECT 1 + 1
FROM DUAL;
```

결과

```
2
```

`SYSDATE`, 문자열 함수, 숫자 함수 등을 테스트할 때도 대부분 `DUAL` 테이블을 사용한다.

---

# 2. 숫자 함수

## ROUND

`ROUND`는 숫자를 **반올림**하는 함수이다.

```sql
SELECT ROUND(1234.5678, 1)
FROM DUAL;

SELECT ROUND(1234.5678, 2)
FROM DUAL;
```

결과

```
1234.6

1234.57
```

두 번째 인수는 소수점 이하 몇 자리까지 표시할지를 의미한다.

---

## TRUNC

`TRUNC`는 소수점을 **버림**하는 함수이다.

```sql
SELECT TRUNC(1234.5678, 1)
FROM DUAL;
```

결과

```
1234.5
```

반올림하지 않고 지정한 자리에서 잘라낸다.

---

## CEIL

`CEIL`은 숫자를 **올림하여 가장 가까운 정수**를 반환한다.

```sql
SELECT CEIL(1234.5678)
FROM DUAL;
```

결과

```
1235
```

---

## FLOOR

`FLOOR`는 숫자를 **내림하여 가장 가까운 정수**를 반환한다.

```sql
SELECT FLOOR(1234.5678)
FROM DUAL;
```

결과

```
1234
```

---

## MOD

`MOD`는 나눗셈의 **나머지**를 반환한다.

```sql
SELECT MOD(15, 2)
FROM DUAL;
```

결과

```
1
```

짝수·홀수 판별이나 특정 주기의 데이터를 처리할 때 자주 사용된다.

---

# 3. 다중행 함수(Aggregate Function)

다중행 함수는 여러 행을 하나의 결과로 집계하는 함수이다.

대표적인 함수는 다음과 같다.

- SUM
- COUNT
- MAX
- MIN
- AVG

---

## SUM

`SUM`은 숫자의 합계를 계산한다.

```sql
SELECT SUM(SAL)
FROM EMP;
```

주의할 점은 일반 컬럼과 집계 함수를 함께 사용할 수 없다는 것이다.

```sql
SELECT ENAME,
       SAL,
       SUM(SAL)
FROM EMP;
```

위 SQL은 `GROUP BY`가 없기 때문에 오류가 발생한다.

NULL은 합계에서 제외된다.

```sql
SELECT SUM(COMM)
FROM EMP;
```

---

### NVL과 함께 사용하기

수당(COMM)이 NULL이라면 계산 결과도 NULL이 된다.

이를 방지하기 위해 `NVL`을 함께 사용하는 경우가 많다.

```sql
SELECT ENAME,
       SAL,
       SAL + NVL(COMM, 0)
FROM EMP;
```

NULL을 0으로 변환하여 정상적으로 계산할 수 있다.

---

## COUNT

`COUNT`는 데이터의 개수를 반환한다.

```sql
SELECT COUNT(ENAME)
FROM EMP;
```

30번 부서의 사원 수를 조회하려면 다음과 같이 작성한다.

```sql
SELECT COUNT(*)
FROM EMP
WHERE DEPTNO = 30;
```

추가 수당(COMM)을 받는 직원 수는 다음과 같이 조회할 수 있다.

```sql
SELECT COUNT(*)
FROM EMP
WHERE COMM IS NOT NULL;
```

---

## MAX와 MIN

최댓값과 최솟값을 구하는 함수이다.

```sql
SELECT MAX(SAL),
       MIN(SAL)
FROM EMP
WHERE DEPTNO = 30;
```

날짜에도 사용할 수 있다.

```sql
SELECT MAX(HIREDATE),
       MIN(HIREDATE)
FROM EMP
WHERE DEPTNO = 30;
```

가장 최근 입사일과 가장 오래된 입사일을 확인할 수 있다.

---

## AVG

`AVG`는 평균값을 계산한다.

```sql
SELECT ROUND(AVG(SAL), 2)
FROM EMP
WHERE DEPTNO = 30;
```

추가 수당의 평균도 계산할 수 있다.

```sql
SELECT ROUND(AVG(COMM), 2)
FROM EMP
WHERE DEPTNO = 30;
```

NULL 값은 평균 계산에서 자동으로 제외된다.

---

# 4. 날짜 함수

## SYSDATE

`SYSDATE`는 현재 시스템 날짜와 시간을 반환한다.

```sql
SELECT SYSDATE
FROM DUAL;
```

날짜 연산도 가능하다.

```sql
SELECT SYSDATE - 1
FROM DUAL;

SELECT SYSDATE + 1
FROM DUAL;
```

- `-1` : 하루 전
- `+1` : 하루 후

---

## TO_CHAR와 함께 날짜 출력

날짜는 원하는 형식으로 출력할 수 있다.

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS')
FROM DUAL;
```

월과 요일도 출력할 수 있다.

```sql
SELECT
    TO_CHAR(SYSDATE, 'MM'),
    TO_CHAR(SYSDATE, 'MON'),
    TO_CHAR(SYSDATE, 'MONTH'),
    TO_CHAR(SYSDATE, 'DD'),
    TO_CHAR(SYSDATE, 'DY'),
    TO_CHAR(SYSDATE, 'DAY')
FROM DUAL;
```

시간 형식도 다양하게 표현할 수 있다.

```sql
SELECT
    TO_CHAR(SYSDATE, 'HH24:MI:SS'),
    TO_CHAR(SYSDATE, 'HH12:MI:SS AM'),
    TO_CHAR(SYSDATE, 'HH:MI:SS P.M.')
FROM DUAL;
```

|형식|설명|
|---|---|
|HH24|24시간|
|HH12|12시간|
|MI|분|
|SS|초|
|AM / PM|오전·오후|

---

## ADD_MONTHS

`ADD_MONTHS`는 날짜에 원하는 개월 수를 더한다.

```sql
SELECT ADD_MONTHS(SYSDATE, 3)
FROM DUAL;
```

결과는 현재 날짜에서 **3개월 후** 날짜가 반환된다.

---

# 5. 형 변환 함수

Oracle은 자동 형 변환을 지원하지만, 필요한 경우 명시적으로 형 변환을 수행하는 것이 좋다.

대표적인 함수는 다음과 같다.

- TO_CHAR
- TO_NUMBER
- TO_DATE

---

## TO_CHAR

숫자나 날짜를 문자열로 변환한다.

```sql
SELECT TO_CHAR(SYSDATE,
               'YYYY/MM/DD HH24:MI:SS')
FROM DUAL;
```

숫자의 표시 형식도 변경할 수 있다.

```sql
SELECT SAL,
       TO_CHAR(SAL, '999,999'),
       TO_CHAR(SAL, 'L999,999')
FROM EMP;
```

- `999,999` : 천 단위 콤마
- `L` : 지역 통화 기호 표시

---

## TO_NUMBER

문자열을 숫자로 변환한다.

Oracle은 `'500'`처럼 숫자로 변환 가능한 문자열은 자동 형 변환을 수행한다.

```sql
SELECT SAL,
       SAL + '500'
FROM EMP;
```

하지만 쉼표가 포함된 문자열은 자동 변환되지 않는다.

```sql
SELECT SAL,
       SAL + TO_NUMBER('5,000', '999,999')
FROM EMP;
```

`TO_NUMBER`를 사용하면 정상적으로 계산할 수 있다.

---

## TO_DATE

문자열을 날짜 타입으로 변환한다.

```sql
SELECT TO_DATE('2018-07-14',
               'YYYY-MM-DD')
FROM DUAL;
```

날짜 비교에서도 많이 사용된다.

```sql
SELECT *
FROM EMP
WHERE HIREDATE >
      TO_DATE('1981-06-01', 'YYYY-MM-DD');
```

문자열을 날짜로 변환한 후 비교하는 것이 명확하고 안전한 방법이다.

---

# 마무리

이번 글에서는 Oracle에서 자주 사용하는 숫자 함수와 날짜 함수, 형 변환 함수에 대해 정리했다.

- **DUAL**은 함수와 계산을 테스트하기 위한 가상 테이블이다.
- **ROUND**, **TRUNC**, **CEIL**, **FLOOR**, **MOD**는 숫자를 계산하거나 가공할 때 사용한다.
- **SUM**, **COUNT**, **MAX**, **MIN**, **AVG**는 여러 행을 하나의 결과로 집계하는 다중행 함수이다.
- **SYSDATE**와 **ADD_MONTHS**를 사용하면 날짜를 쉽게 계산할 수 있다.
- **TO_CHAR**, **TO_NUMBER**, **TO_DATE**를 활용하면 데이터 타입을 명시적으로 변환할 수 있다.

숫자 함수와 날짜 함수, 형 변환 함수는 SQL에서 매우 자주 사용하는 기본 기능이다. 각 함수의 특징과 사용법을 익혀두면 데이터를 보다 정확하고 효율적으로 처리할 수 있다.