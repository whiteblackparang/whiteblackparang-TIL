---
title: "260715 [오라클 SQL] 기초 : 집합 연산자와 GROUP BY"
category: "SQL"
dialect: "Oracle"
date: 2026-07-15
tags: ["SQL", "Oracle"]
---

#### 오라클 SQL 기초

1. 집합 연산자(Set Operator)
    - UNION
    - UNION ALL
    - MINUS
    - INTERSECT

2. CONCAT
    - 문자열 연결하기

3. GROUP BY 심화
    - HAVING
    - ROLLUP
    - CUBE

4. 실무에서 기억할 점

5. 마무리

---

## ① 집합 연산자 (Set Operator)

집합 연산자는 두 개 이상의 SELECT 결과를 하나로 합칠 때 사용한다. JOIN이 테이블과 테이블을 옆으로 붙이는 **가로 병합**이라면, 집합 연산자는 SELECT 결과와 결과를 위아래로 붙이는 **세로 병합**이라고 이해하면 쉽다.

집합 연산자를 사용하려면 아래 두 조건을 반드시 지켜야 한다.

- 각 SELECT 문의 **컬럼 개수**가 같아야 한다.
- 대응되는 컬럼의 **데이터 타입**이 서로 호환되어야 한다.

컬럼명이 달라도 실행은 되지만, 결과 컬럼명은 첫 번째 SELECT 문을 기준으로 표시된다.

### UNION

**설명**
UNION은 두 SELECT 결과를 합치되, **중복된 행을 제거**한다. 내부적으로 정렬(SORT)과 중복 제거 작업이 함께 일어나기 때문에 UNION ALL보다 처리 비용이 더 든다.

**예제**

```sql
SELECT EMPNO, ENAME, DEPTNO
FROM EMP
WHERE DEPTNO = 10
UNION
SELECT EMPNO, ENAME, DEPTNO
FROM EMP
WHERE JOB = 'MANAGER';
```

10번 부서 직원과 MANAGER 직급 직원을 합쳐서 보여주되, 두 조건에 모두 해당해 중복으로 나온 행은 한 번만 표시된다.

**주의사항**
- 중복 제거를 위해 결과를 정렬하므로, 데이터가 많을수록 성능 저하가 발생할 수 있다.
- ORDER BY는 마지막 SELECT 문 뒤에 한 번만 작성한다.

**실행 결과**
DEPTNO, EMPNO 등 기준으로 정렬된 상태에서 중복이 제거된 행 목록이 반환된다.

### UNION ALL

**차이점**
UNION ALL은 UNION과 달리 **중복 제거와 정렬을 하지 않는다.** 두 결과를 있는 그대로 이어 붙이기만 하므로 속도가 더 빠르다.

```sql
SELECT EMPNO, ENAME, DEPTNO
FROM EMP
WHERE DEPTNO = 10
UNION ALL
SELECT EMPNO, ENAME, DEPTNO
FROM EMP
WHERE JOB = 'MANAGER';
```

**언제 사용하는지**
- 두 결과 집합에 애초에 중복이 생길 수 없다는 것을 알고 있을 때
- 중복이 있어도 상관없거나, 오히려 중복 건수 자체가 의미 있는 데이터일 때 (예: 로그 데이터 합산)

**실무 팁**
결과에 중복이 없다는 확신이 있다면 습관적으로 UNION ALL을 먼저 고려하는 것이 좋다. UNION은 "중복 제거가 꼭 필요한 경우"에만 제한적으로 쓰는 것이 성능 관리 측면에서 유리하다.

### MINUS

**차집합**
MINUS는 첫 번째 SELECT 결과에서 두 번째 SELECT 결과와 겹치는 행을 제외하고 반환한다. 수학의 차집합(A - B)과 동일한 개념이다.

```sql
SELECT EMPNO, ENAME
FROM EMP
WHERE DEPTNO = 10
MINUS
SELECT EMPNO, ENAME
FROM EMP
WHERE JOB = 'MANAGER';
```

10번 부서 직원 중에서 MANAGER 직급인 사람을 제외한 나머지가 결과로 반환된다.

**사용 사례**
- "A 테이블에는 있지만 B 테이블에는 없는 데이터"를 찾을 때
- 마이그레이션 후 데이터 누락 여부를 검증할 때
- 특정 조건을 만족하지 않는 예외 케이스를 걸러낼 때

### INTERSECT

**교집합**
INTERSECT는 두 SELECT 결과에 **공통으로 존재하는 행**만 반환한다. 수학의 교집합(A ∩ B) 개념이다.

```sql
SELECT EMPNO, ENAME
FROM EMP
WHERE DEPTNO = 10
INTERSECT
SELECT EMPNO, ENAME
FROM EMP
WHERE JOB = 'MANAGER';
```

10번 부서에 속하면서 동시에 MANAGER 직급인 직원만 결과로 나온다.

**사용 사례**
- 두 조건을 모두 만족하는 대상만 뽑고 싶을 때
- 서로 다른 두 시스템에서 공통으로 존재하는 데이터를 검증할 때

### 집합 연산자 비교

| 연산자 | 중복 제거 | 설명 |
|---|---|---|
| UNION | O | 합집합 |
| UNION ALL | X | 합집합 |
| MINUS | - | 차집합 |
| INTERSECT | O | 교집합 |

---

## ② CONCAT

문자열을 연결할 때 사용하는 함수다. 두 개의 문자열을 하나로 이어 붙이며, 중첩해서 사용하면 세 개 이상의 문자열도 연결할 수 있다.

```sql
SELECT CONCAT(ENAME, JOB) AS NAME_JOB
FROM EMP;

-- 세 개 이상 연결 시 중첩 사용
SELECT CONCAT(CONCAT(ENAME, ' - '), JOB) AS NAME_JOB
FROM EMP;
```

Oracle에서는 CONCAT 함수 대신 `||` 연산자를 훨씬 더 자주 사용한다. `||`는 인자 개수 제한 없이 여러 문자열을 한 번에 연결할 수 있어서 실무에서는 CONCAT보다 선호도가 높다.

```sql
SELECT EMPNO || ':' || ENAME AS EMP_INFO
FROM EMP;
```

---

## ③ GROUP BY 심화

### HAVING

**왜 필요한지**
GROUP BY로 그룹을 만든 뒤, 그 **그룹 단위로 조건을 걸고 싶을 때** HAVING을 사용한다. 예를 들어 "부서별 평균 급여가 300만 원 이상인 부서만 보고 싶다"와 같은 조건은 그룹화가 끝난 이후에만 판단할 수 있다.

**WHERE와 차이**
- WHERE는 그룹화 이전, **개별 행**을 대상으로 조건을 거른다.
- HAVING은 그룹화 이후, **그룹(집계 결과)**을 대상으로 조건을 거른다.
- 따라서 HAVING 절에는 SUM, AVG, COUNT 같은 집계 함수를 조건으로 쓸 수 있지만, WHERE 절에는 쓸 수 없다.

**예제**

```sql
SELECT DEPTNO, AVG(SAL) AS AVG_SAL
FROM EMP
WHERE JOB <> 'PRESIDENT'
GROUP BY DEPTNO
HAVING AVG(SAL) >= 3000000
ORDER BY AVG_SAL DESC;
```

**실행 순서**

```
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
ORDER BY
```

작성 순서와 실제 실행 순서가 다르다는 점이 핵심이다. WHERE로 먼저 불필요한 행을 걸러낸 뒤 그룹을 만들고, HAVING으로 그룹을 한 번 더 거른 다음, 마지막에 정렬한다.

### ROLLUP

**설명**
ROLLUP은 GROUP BY 절에 지정한 컬럼을 기준으로 **소계(부분합)와 총계를 자동으로 함께 생성**해 주는 확장 기능이다. 컬럼 순서에 따라 계층적으로 합계를 만들어 준다는 점이 특징이다.

**부분합 생성**

```sql
SELECT DEPTNO, JOB, SUM(SAL) AS SUM_SAL
FROM EMP
GROUP BY ROLLUP(DEPTNO, JOB);
```

**결과 해석**
위 쿼리는 다음 세 단계의 집계를 한 번에 만들어 준다.

- DEPTNO + JOB별 소계
- DEPTNO별 소계 (JOB 컬럼은 NULL로 표시)
- 전체 총계 (DEPTNO, JOB 모두 NULL로 표시)

즉 상세 → 중간 소계 → 전체 총계로 이어지는 계층형 보고서를 한 번의 쿼리로 만들 수 있다.

### CUBE

**설명**
CUBE는 ROLLUP보다 한 단계 더 나아가, GROUP BY에 지정한 컬럼들의 **가능한 모든 조합**에 대해 집계를 만들어 준다.

**모든 조합 생성**

```sql
SELECT DEPTNO, JOB, SUM(SAL) AS SUM_SAL
FROM EMP
GROUP BY CUBE(DEPTNO, JOB);
```

**ROLLUP과 차이**
- ROLLUP(DEPTNO, JOB): DEPTNO+JOB, DEPTNO, 전체 총계 → 계층 순서대로 소계
- CUBE(DEPTNO, JOB): DEPTNO+JOB, DEPTNO, JOB, 전체 총계 → 컬럼 조합을 전부 생성

즉 ROLLUP은 "왼쪽에서 오른쪽으로" 순서를 지키며 부분합을 만들지만, CUBE는 순서와 무관하게 모든 조합의 집계를 만든다.

**언제 사용하는지**
- 부서별 소계처럼 계층 구조가 뚜렷한 보고서 → ROLLUP
- 부서별, 직급별을 각각 따로도 보고 싶고 교차 분석도 필요한 다차원 분석 → CUBE

| 구분 | ROLLUP | CUBE |
|---|---|---|
| 생성 범위 | 부분합 | 모든 조합 |
| 성격 | 계층형 집계 | 전체 집계 |

---

## ④ 실무에서 기억할 점

- UNION보다 UNION ALL이 더 빠르다. 중복이 없다는 확신이 있으면 UNION ALL을 우선 고려한다.
- HAVING은 그룹화 이후 조건이다. 집계 함수 조건은 WHERE가 아니라 HAVING에 써야 한다.
- WHERE로 먼저 데이터를 줄이는 것이 성능상 유리하다. GROUP BY 대상 행 자체를 줄여야 집계 비용도 줄어든다.
- ROLLUP은 보고서 작성에서 많이 사용된다. 부서별 소계, 월별 소계처럼 계층형 합계가 필요할 때 적합하다.
- CUBE는 다차원 분석에서 많이 사용된다. 여러 기준을 교차해서 살펴봐야 하는 분석성 리포트에 적합하다.

---

## ⑤ 마무리

이번 글에서는

- 집합 연산자
- 문자열 연결
- GROUP BY 심화

를 정리했다. UNION 계열과 MINUS, INTERSECT는 서로 다른 SELECT 결과를 합치거나 비교하는 상황에서, ROLLUP과 CUBE는 집계 보고서를 만드는 상황에서 자주 쓰이는 만큼, 각각의 차이를 정확히 구분해 두면 실무에서 쿼리를 짤 때 훨씬 수월해진다.