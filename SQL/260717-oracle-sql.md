---
title: "260717 [오라클 SQL] 기초 : GROUP BY 심화(HAVING, ROLLUP, CUBE)"
category: "SQL"
dialect: "Oracle"
date: 2026-07-17
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : GROUP BY 심화(HAVING, ROLLUP, CUBE)

이번에는 `GROUP BY`를 조금 더 효율적으로 활용할 수 있는 **HAVING**, **ROLLUP**, **CUBE**에 대해 정리

`GROUP BY`는 동일한 값을 가진 데이터를 하나의 그룹으로 묶어 집계하는 기능이다. 여기에 HAVING, ROLLUP, CUBE를 함께 사용하면 조건에 맞는 그룹만 조회하거나 부분합과 전체합까지 손쉽게 구할 수 있다.

이번 글에서는 다음 내용을 다룬다.

- HAVING
- ROLLUP
- CUBE

---

# 1. HAVING

`HAVING`은 **GROUP BY로 생성된 그룹에 조건을 적용하는 절**이다.

일반적으로 조건을 지정할 때는 `WHERE` 절을 사용하지만, 집계 함수의 결과를 기준으로 조건을 지정할 때는 `HAVING`을 사용해야 한다.

예를 들어 부서와 직무별 평균 급여가 2,000 이상인 그룹만 조회하려면 다음과 같이 작성한다.

```sql
SELECT DEPTNO,
       JOB,
       ROUND(AVG(SAL), 2)
FROM EMP
GROUP BY DEPTNO, JOB
HAVING AVG(SAL) >= 2000
ORDER BY DEPTNO;
```

위 SQL은 먼저 `DEPTNO`와 `JOB`으로 그룹을 생성한 뒤, 평균 급여가 2,000 이상인 그룹만 결과로 출력한다.

---

## WHERE와 HAVING의 차이

`WHERE`와 `HAVING`은 모두 조건을 지정하는 역할을 하지만 적용되는 시점이 다르다.

- **WHERE** : 그룹화 이전에 개별 행을 필터링
- **HAVING** : 그룹화 이후 생성된 그룹을 필터링

예를 들어 평균 급여가 2,000 이상인 부서를 조회하고 싶다면 다음과 같이 `HAVING`을 사용해야 한다.

```sql
SELECT DEPTNO,
       AVG(SAL)
FROM EMP
GROUP BY DEPTNO
HAVING AVG(SAL) >= 2000;
```

반대로 `WHERE AVG(SAL)`처럼 집계 함수를 사용하면 오류가 발생한다.

---

## SQL 실행 순서

HAVING을 이해하려면 SQL의 실행 순서를 함께 알아두는 것이 좋다.

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
```

`WHERE`는 그룹이 만들어지기 전에 실행되고, `HAVING`은 그룹이 생성된 이후 실행된다.

따라서 집계 함수의 결과를 조건으로 사용할 때는 반드시 `HAVING`을 사용해야 한다.

---

# 2. ROLLUP

`ROLLUP`은 GROUP BY 결과에 **부분합(Subtotal)** 과 **전체합(Grand Total)** 을 자동으로 추가해주는 기능이다.

보고서나 통계 자료를 만들 때 자주 사용된다.

일반적인 GROUP BY는 그룹별 결과만 출력한다.

```sql
SELECT DEPTNO,
       JOB,
       COUNT(*),
       MAX(SAL),
       SUM(SAL),
       AVG(SAL)
FROM EMP
GROUP BY DEPTNO, JOB;
```

위 SQL은 부서와 직무별 집계 결과만 반환한다.

---

## ROLLUP 사용

같은 SQL에 `ROLLUP`을 적용하면 부분합이 함께 출력된다.

```sql
SELECT DEPTNO,
       JOB,
       COUNT(*),
       MAX(SAL),
       SUM(SAL),
       AVG(SAL)
FROM EMP
GROUP BY ROLLUP(DEPTNO, JOB);
```

실행 결과에는 다음과 같은 정보가 포함된다.

- 부서 + 직무별 집계
- 부서별 합계
- 전체 합계

즉, 여러 개의 집계 SQL을 작성하지 않아도 하나의 SQL로 다양한 집계 결과를 얻을 수 있다.

---

## ROLLUP의 특징

예를 들어 `ROLLUP(DEPTNO, JOB)`을 사용하면 다음 순서대로 집계가 생성된다.

```text
DEPTNO + JOB

↓

DEPTNO

↓

전체 합계
```

상위 그룹으로 올라가면서 부분합을 계산한다고 이해하면 된다.

그래서 **계층적인 집계**를 수행할 때 매우 유용하다.

---

# 3. CUBE

`CUBE`는 `ROLLUP`보다 한 단계 확장된 집계 기능이다.

ROLLUP은 계층 구조를 기준으로 부분합을 생성하지만,

CUBE는 **지정한 모든 컬럼의 가능한 조합에 대해 집계 결과를 생성**한다.

예제는 다음과 같다.

```sql
SELECT DEPTNO,
       JOB,
       COUNT(*),
       MAX(SAL),
       SUM(SAL),
       AVG(SAL)
FROM EMP
GROUP BY CUBE(DEPTNO, JOB)
ORDER BY DEPTNO, JOB;
```

---

## CUBE와 ROLLUP의 차이

ROLLUP은 계층적으로 부분합을 계산한다.

```text
DEPTNO + JOB

↓

DEPTNO

↓

전체 합계
```

반면 CUBE는 가능한 모든 조합을 계산한다.

```text
DEPTNO + JOB

DEPTNO

JOB

전체 합계
```

즉, ROLLUP보다 더 많은 집계 결과를 생성한다.

---

## 언제 사용할까?

ROLLUP은 다음과 같은 경우에 적합하다.

- 부서별 매출
- 월별 매출
- 연도별 매출
- 보고서의 부분합

반면 CUBE는 여러 기준으로 분석하는 경우 유용하다.

예를 들어

- 부서별 분석
- 직무별 분석
- 부서 + 직무 분석
- 전체 분석

처럼 다양한 관점에서 데이터를 확인할 수 있다.

---

## ROLLUP과 CUBE 비교

|구분|ROLLUP|CUBE|
|:---|:---:|:---:|
|부분합|O|O|
|전체합|O|O|
|모든 조합 집계|X|O|
|계층형 집계|O|X|

데이터를 여러 차원으로 분석해야 한다면 CUBE를, 계층적인 집계를 수행한다면 ROLLUP을 사용하는 것이 적합하다.

---

# 마무리

이번 글에서는 GROUP BY를 더욱 효율적으로 활용하기 위한 HAVING, ROLLUP, CUBE에 대해 정리했다.

- **HAVING**은 그룹화된 결과에 조건을 적용할 때 사용한다.
- **ROLLUP**은 부분합과 전체합을 자동으로 생성한다.
- **CUBE**는 가능한 모든 조합의 집계 결과를 생성한다.

이 기능들은 데이터 분석과 보고서 작성에서 자주 사용되는 만큼 GROUP BY와 함께 익혀두면 다양한 집계 SQL을 더욱 효율적으로 작성할 수 있다.
