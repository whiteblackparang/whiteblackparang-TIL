---
title: "260710 [오라클 SQL] 기초 : SELECT와 조건 검색"
category: "SQL"
dialect: "Oracle"
date: 2026-07-10
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : SELECT와 조건 검색

`SELECT`는 데이터베이스에서 원하는 데이터를 조회할 때 사용하는 가장
기본적인 SQL 문입니다.

## DESC

테이블의 구조(컬럼명, 자료형, NULL 허용 여부)를 확인합니다.

``` sql
DESC EMP;
DESC DEPT;
```

## SELECT

원하는 컬럼만 조회합니다.

``` sql
SELECT EMPNO, ENAME, DEPTNO
FROM EMP;
```

모든 컬럼 조회

``` sql
SELECT *
FROM EMP;
```

## WHERE

조건에 맞는 데이터만 조회합니다.

``` sql
SELECT *
FROM EMP
WHERE DEPTNO = 30;
```

### AND

모든 조건을 만족해야 합니다.

``` sql
SELECT *
FROM EMP
WHERE DEPTNO = 30
  AND JOB = 'SALESMAN';
```

### OR

조건 중 하나만 만족하면 됩니다.

``` sql
SELECT *
FROM EMP
WHERE DEPTNO = 30
   OR SAL >= 1300;
```

## IN

같은 컬럼을 여러 값과 비교할 때 사용합니다.

``` sql
SELECT *
FROM EMP
WHERE JOB IN ('MANAGER','SALESMAN','CLERK');
```

제외하려면 `NOT IN`을 사용합니다.

``` sql
SELECT *
FROM EMP
WHERE JOB NOT IN ('MANAGER','SALESMAN','CLERK');
```

## LIKE

문자열 검색에 사용하는 연산자입니다.

  패턴    의미               예시
  ------- ------------------ -------
  `A%`    A로 시작           ALLEN
  `%A`    A로 끝남           MARIA
  `%A%`   A를 포함           JAMES
  `_M%`   두 번째 글자가 M   SMITH

``` sql
SELECT *
FROM EMP
WHERE ENAME LIKE '%AM%';
```

## CASE

조건에 따라 다른 값을 반환합니다.

``` sql
SELECT EMPNO,
       ENAME,
       JOB,
       SAL,
       CASE JOB
            WHEN 'MANAGER' THEN SAL * 1.1
            WHEN 'SALESMAN' THEN SAL * 1.05
            WHEN 'ANALYST' THEN SAL
            ELSE SAL * 1.03
       END AS UPSAL
FROM EMP;
```

NULL 여부도 처리할 수 있습니다.

``` sql
SELECT EMPNO,
       ENAME,
       COMM,
       CASE
            WHEN COMM IS NULL THEN '해당사항 없음'
            WHEN COMM = 0 THEN '수당없음'
            WHEN COMM > 0 THEN '수당 : ' || COMM
       END AS COMM_TEXT
FROM EMP;
```

## 연습 문제

사원번호가 7499이고 부서번호가 30인 사원 조회

``` sql
SELECT *
FROM EMP
WHERE EMPNO = 7499
  AND DEPTNO = 30;
```

부서번호가 20이거나 직업이 SALESMAN인 사원 조회

``` sql
SELECT *
FROM EMP
WHERE DEPTNO = 20
   OR JOB = 'SALESMAN';
```

# 핵심 정리

-   `DESC`는 테이블 구조를 확인한다.
-   `SELECT`는 원하는 데이터를 조회한다.
-   `WHERE`는 조건을 지정한다.
-   `AND`는 모든 조건, `OR`는 하나 이상의 조건을 만족해야 한다.
-   `IN`은 여러 값을 한 번에 비교한다.
-   `LIKE`는 문자열 패턴을 검색한다.
-   `CASE`는 SQL의 조건문 역할을 한다.