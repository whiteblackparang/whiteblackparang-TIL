---
title: "260711 [오라클 SQL] 기초 : 서브쿼리와 인라인 뷰"
category: "SQL"
dialect: "Oracle"
date: 2026-07-11
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 서브쿼리와 인라인 뷰

서브쿼리(Subquery)는 **쿼리 안에 포함된 또 다른 쿼리**입니다. 메인
쿼리가 실행되기 전에 먼저 실행되어 그 결과를 메인 쿼리에서 사용합니다.

## 서브쿼리란?

예를 들어 **JONES보다 급여를 많이 받는 사원**을 찾는다고 가정해 봅시다.

먼저 JONES의 급여를 확인합니다.

``` sql
SELECT SAL
FROM EMP
WHERE ENAME = 'JONES';
```

결과를 이용해 하나의 SQL로 작성하면 다음과 같습니다.

``` sql
SELECT ENAME, SAL
FROM EMP
WHERE SAL >
(
    SELECT SAL
    FROM EMP
    WHERE ENAME = 'JONES'
);
```

> 서브쿼리는 먼저 실행되고, 반환된 값을 메인 쿼리가 사용합니다.

## 또 다른 예제

ALLEN보다 늦게 입사한 사원

``` sql
SELECT *
FROM EMP
WHERE HIREDATE >
(
    SELECT HIREDATE
    FROM EMP
    WHERE ENAME = 'ALLEN'
);
```

ALLEN보다 많은 추가 수당(COMM)을 받는 사원

``` sql
SELECT *
FROM EMP
WHERE COMM >
(
    SELECT COMM
    FROM EMP
    WHERE ENAME = 'ALLEN'
);
```

> 비교하는 컬럼과 서브쿼리의 반환 자료형은 같아야 합니다.

# ANY, SOME, ALL

여러 개의 값을 반환하는 서브쿼리에서 사용하는 연산자입니다.

## ANY (SOME)

하나라도 조건을 만족하면 TRUE입니다.

`SOME`은 `ANY`와 동일한 의미입니다.

``` sql
SELECT *
FROM EMP
WHERE SAL = ANY
(
    SELECT MAX(SAL)
    FROM EMP
    GROUP BY DEPTNO
);
```

### 예제

30번 부서의 최대 급여보다 작은 사원

``` sql
SELECT *
FROM EMP
WHERE SAL < ANY
(
    SELECT MAX(SAL)
    FROM EMP
    WHERE DEPTNO = 30
);
```

## ALL

모든 결과를 만족해야 TRUE입니다.

``` sql
SELECT *
FROM EMP
WHERE SAL > ALL
(
    SELECT MAX(SAL)
    FROM EMP
    WHERE DEPTNO = 30
);
```

## ANY와 ALL 비교

  연산자      의미
  ----------- ---------------------------
  ANY(SOME)   여러 값 중 하나 이상 만족
  ALL         모든 값을 만족

# 다중열 서브쿼리

여러 컬럼을 동시에 비교할 수 있습니다.

각 부서에서 가장 높은 급여를 받는 사원

``` sql
SELECT *
FROM EMP
WHERE (DEPTNO, SAL) IN
(
    SELECT DEPTNO,
           MAX(SAL)
    FROM EMP
    GROUP BY DEPTNO
);
```

# 인라인 뷰

FROM 절에 서브쿼리를 작성하여 하나의 임시 테이블처럼 사용하는
방법입니다.

``` sql
SELECT E10.EMPNO,
       E10.ENAME,
       D.DNAME,
       D.LOC
FROM
(
    SELECT *
    FROM EMP
    WHERE DEPTNO = 10
) E10,
(
    SELECT *
    FROM DEPT
) D
WHERE E10.DEPTNO = D.DEPTNO;
```

인라인 뷰를 사용하면 복잡한 조회를 여러 단계로 나누어 가독성을 높일 수
있습니다.

# 핵심 정리

-   서브쿼리는 쿼리 안의 또 다른 쿼리이다.
-   메인 쿼리보다 먼저 실행된다.
-   반환 자료형과 비교 대상 자료형은 같아야 한다.
-   `ANY(SOME)`은 하나 이상 만족하면 된다.
-   `ALL`은 모든 값을 만족해야 한다.
-   다중열 서브쿼리는 여러 컬럼을 동시에 비교한다.
-   인라인 뷰는 FROM절에서 서브쿼리를 테이블처럼 사용하는 기법이다.