---
title: "260716 [오라클 SQL] 기초 : 집합 연산자와 GROUP BY 심화"
category: "SQL"
dialect: "Oracle"
date: 2026-07-16
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 집합 연산자와 GROUP BY 심화

이번 글에서는 다음과 같은 내용을 정리한다.

- 집합 연산자(Set Operator)
  - UNION
  - UNION ALL
  - MINUS
  - INTERSECT

- 문자열 연결(CONCAT)

- GROUP BY 심화
  - HAVING
  - ROLLUP
  - CUBE

---

# 1. 집합 연산자(Set Operator)

집합 연산자는 두 개 이상의 SELECT 문의 실행 결과를 하나의 결과 집합으로 합칠 때 사용하는 기능이다.

JOIN이 여러 테이블을 **가로(열)** 방향으로 연결하는 기능이라면, 집합 연산자는 여러 SELECT 결과를 **세로(행)** 방향으로 연결한다는 차이가 있다.

집합 연산자를 사용할 때는 반드시 다음 조건을 만족해야 한다.

- SELECT 문의 컬럼 개수가 동일해야 한다.
- 같은 위치의 컬럼 데이터 타입이 서로 호환되어야 한다.
- 컬럼의 순서도 동일해야 한다.

이 조건을 만족하지 않으면 오류가 발생하거나 의도하지 않은 결과가 출력될 수 있다.

---

## UNION

`UNION`은 두 개 이상의 SELECT 결과를 합친 후 **중복된 행을 제거**하여 반환한다.

예를 들어 10번 부서와 20번 부서의 사원 정보를 함께 조회하려면 다음과 같이 작성한다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10

UNION

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 20;
```

두 SELECT 문의 결과가 하나의 결과 집합으로 합쳐지며, 동일한 행이 존재한다면 한 번만 출력된다.

---

### 컬럼 순서는 반드시 동일해야 한다.

집합 연산자는 컬럼 이름이 아니라 **위치(Position)** 를 기준으로 데이터를 연결한다.

다음과 같이 컬럼 순서를 변경하면 데이터 타입이 서로 맞지 않아 오류가 발생한다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10

UNION

SELECT EMPNO, SAL, ENAME, DEPTNO
FROM EMP
WHERE DEPTNO = 20;
```

두 번째 SELECT에서는 `SAL`과 `ENAME`의 위치가 바뀌었기 때문에 숫자와 문자열을 비교하게 되어 오류가 발생한다.

---

### 데이터 타입이 같다면 오류가 발생하지 않을 수도 있다.

주의해야 할 점은 데이터 타입이 같으면 오류가 발생하지 않는 경우도 있다는 것이다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10

UNION

SELECT EMPNO, ENAME, DEPTNO, SAL
FROM EMP
WHERE DEPTNO = 20;
```

`SAL`과 `DEPTNO`는 모두 숫자형이므로 SQL은 오류를 발생시키지 않는다.

하지만 실제 결과에서는 급여와 부서번호가 서로 바뀌어 출력되므로 원하는 결과를 얻을 수 없다.

따라서 집합 연산자를 사용할 때는 **컬럼 개수뿐만 아니라 컬럼의 순서까지 반드시 동일하게 작성해야 한다.**

---

### 중복 데이터는 자동으로 제거된다.

동일한 SELECT 문을 UNION으로 연결하면 결과는 한 번만 출력된다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10

UNION

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10;
```

`UNION`은 중복을 제거한 뒤 결과를 반환하기 때문이다.

---

## UNION ALL

`UNION ALL`은 `UNION`과 동일하게 여러 SELECT 결과를 합치지만 **중복을 제거하지 않는다.**

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10

UNION ALL

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10;
```

위 SQL을 실행하면 동일한 데이터도 모두 출력된다.

즉,

- `UNION` : 중복 제거 후 출력
- `UNION ALL` : 중복 제거 없이 모두 출력

이라는 차이가 있다.

실무에서는 중복 제거 과정이 필요하지 않다면 `UNION`보다 `UNION ALL`을 사용하는 것이 성능 면에서 유리하다.

---

## MINUS

`MINUS`는 첫 번째 SELECT 결과에서 두 번째 SELECT 결과를 제외한 행만 반환하는 **차집합 연산자**이다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10
   OR DEPTNO = 20

MINUS

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10;
```

위 SQL은 10번 부서와 20번 부서를 모두 조회한 뒤, 10번 부서의 데이터를 제외한다.

즉, 결과에는 **20번 부서의 데이터만 남게 된다.**

특정 조건의 데이터를 제외하고 조회해야 하는 경우 유용하게 사용할 수 있다.

---

## INTERSECT

`INTERSECT`는 두 SELECT 결과에서 **공통으로 존재하는 데이터만 반환하는 교집합 연산자**이다.

```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10
   OR DEPTNO = 20

INTERSECT

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE DEPTNO = 10;
```

첫 번째 SELECT에는 10번과 20번 부서가 모두 포함되어 있고,

두 번째 SELECT에는 10번 부서만 존재하므로

결과는 **10번 부서의 데이터만 출력**된다.

---

## 집합 연산자 비교

|연산자|설명|중복 제거|
|:---|:---|:---:|
|UNION|합집합|O|
|UNION ALL|합집합|X|
|MINUS|차집합|-|
|INTERSECT|교집합|O|

집합 연산자는 모두 여러 SELECT 결과를 하나로 합친다는 공통점이 있지만, 반환하는 결과의 의미는 서로 다르다.

따라서 원하는 결과에 맞는 연산자를 선택하여 사용하는 것이 중요하다.

---

# 2. 문자열 연결(CONCAT)

`CONCAT` 함수는 두 개의 문자열을 하나로 연결하는 함수이다.

EMPNO와 ENAME을 하나의 문자열로 출력하려면 다음과 같이 사용할 수 있다.

```sql
SELECT CONCAT(EMPNO, ENAME)
FROM EMP;
```

실행 결과는 다음과 같다.

```
7369SMITH
7499ALLEN
```

두 문자열 사이에 구분자를 넣고 싶다면 `CONCAT`을 중첩하여 사용할 수 있다.

```sql
SELECT CONCAT(EMPNO, CONCAT(':', ENAME))
FROM EMP;
```

실행 결과

```
7369:SMITH
7499:ALLEN
```

다만 Oracle에서는 `CONCAT`보다 문자열 연결 연산자인 `||`를 사용하는 경우가 더 많다.

```sql
SELECT EMPNO || ':' || ENAME
FROM EMP;
```

두 방법 모두 동일한 결과를 반환하며, 실무에서는 `||` 연산자가 더 간결하여 자주 사용된다.
