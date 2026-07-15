---
title: "260712 [오라클 SQL] 기초 : 데이터 조회와 정렬"
category: "SQL"
dialect: "Oracle"
date: 2026-07-12
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 데이터 조회와 정렬

## DISTINCT

중복된 데이터를 제거하여 조회합니다.

``` sql
SELECT DISTINCT DEPTNO, JOB
FROM EMP;
```

## 열과 연산

조회한 컬럼에 산술 연산을 적용할 수 있습니다.

``` sql
SELECT ENAME, SAL * 12
FROM EMP;
```

급여를 12배 하여 연봉을 계산할 수 있습니다.

## 비교 연산자

같지 않음을 표현합니다.

-   `!=`
-   `<>`
-   `^=`

``` sql
SELECT *
FROM EMP
WHERE SAL <> 3000
  AND JOB <> 'SALESMAN';
```

또는

``` sql
SELECT *
FROM EMP
WHERE NOT SAL = 3000;
```

## BETWEEN

범위 검색에 사용합니다.

``` sql
SELECT *
FROM EMP
WHERE SAL BETWEEN 2000 AND 3000;
```

제외는 `NOT BETWEEN`을 사용합니다.

## AS

별칭(Alias)을 지정합니다.

``` sql
SELECT ENAME,
       SAL * 12 AS ANNSAL
FROM EMP;
```

## ORDER BY

오름차순

``` sql
SELECT *
FROM EMP
ORDER BY SAL;
```

내림차순

``` sql
SELECT *
FROM EMP
ORDER BY SAL DESC;
```

복합 정렬

``` sql
SELECT *
FROM EMP
ORDER BY DEPTNO ASC, SAL DESC;
```

## 종합 예제

``` sql
SELECT EMPNO, ENAME
FROM EMP
WHERE DEPTNO = 30
   OR JOB = 'SALESMAN'
ORDER BY ENAME;
```

# 핵심 정리

-   DISTINCT는 중복을 제거한다.
-   산술 연산으로 조회 데이터를 가공할 수 있다.
-   BETWEEN은 범위 검색에 사용한다.
-   AS는 별칭을 지정한다.
-   ORDER BY는 결과를 정렬한다.
