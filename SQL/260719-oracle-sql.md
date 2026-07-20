---
title: "260719 [오라클 SQL] 기초 : JOIN 기초(Oracle JOIN)"
category: "SQL"
dialect: "Oracle"
date: 2026-07-19
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : JOIN 기초(Oracle JOIN)
실제 데이터베이스에서는 원하는 정보가 하나의 테이블에만 저장되어 있는 경우는 드물다.

예를 들어 직원의 이름은 `EMP` 테이블에 저장되어 있지만, 부서명과 근무 지역은 `DEPT` 테이블에 저장되어 있다. 이처럼 여러 테이블에 흩어져 있는 데이터를 하나의 결과로 조회하기 위해 사용하는 기능이 **JOIN**이다.

집합 연산자(UNION, MINUS, INTERSECT)가 여러 SELECT 문의 결과를 **세로(행)** 방향으로 합치는 기능이라면, JOIN은 여러 테이블을 **가로(열)** 방향으로 연결하는 기능이다.

이번 글에서 다루는 내용

- JOIN이란?
- Cartesian Product
- Oracle JOIN
- 테이블 별칭(Alias)
- INNER JOIN
- SELF JOIN
- Oracle OUTER JOIN(+)

---

# 1. JOIN이란?

JOIN은 두 개 이상의 테이블을 공통된 컬럼을 기준으로 연결하여 하나의 결과 집합으로 조회하는 기능이다.

예를 들어 `EMP` 테이블에는 직원 정보가 저장되어 있다.

|EMPNO|ENAME|DEPTNO|
|:---:|:---:|:---:|
|7369|SMITH|20|

반면 부서 정보는 `DEPT` 테이블에 저장되어 있다.

|DEPTNO|DNAME|LOC|
|:---:|:---:|:---:|
|20|RESEARCH|DALLAS|

직원의 이름과 부서명을 함께 조회하려면 두 테이블을 연결해야 한다.

```sql
SELECT E.ENAME,
       D.DNAME,
       D.LOC
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

결과는 다음과 같이 출력된다.

|ENAME|DNAME|LOC|
|:---:|:---:|:---:|
|SMITH|RESEARCH|DALLAS|

이처럼 **공통된 컬럼(DEPTNO)을 기준으로 여러 테이블을 연결하는 작업**을 JOIN이라고 한다.

---

# 2. Cartesian Product

JOIN에서 가장 많이 발생하는 실수는 **JOIN 조건을 작성하지 않는 것**이다.

예를 들어 다음과 같이 두 테이블만 조회하면

```sql
SELECT *
FROM EMP, DEPT;
```

EMP의 모든 행과 DEPT의 모든 행이 서로 연결된다.

이를 **Cartesian Product(카티션 곱)** 라고 한다.

예를 들어

- EMP : 14행
- DEPT : 4행

이라면 결과는

```
14 × 4 = 56행
```

이 출력된다.

원하는 결과가 아니라면 반드시 JOIN 조건을 작성해야 한다.

---

# 3. Oracle JOIN

Oracle의 기존 JOIN 문법은 `FROM` 절에 여러 테이블을 작성하고 `WHERE` 절에서 연결 조건을 지정한다.

```sql
SELECT *
FROM EMP,
     DEPT
WHERE EMP.DEPTNO = DEPT.DEPTNO
ORDER BY EMP.DEPTNO;
```

또는 별칭을 사용하면 더욱 읽기 쉬운 SQL을 작성할 수 있다.

```sql
SELECT E.ENAME,
       E.JOB,
       D.LOC
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

두 SQL은 동일한 결과를 반환하지만, 별칭(Alias)을 사용하면 코드의 가독성이 좋아진다.

---

# 4. 테이블 별칭(Alias)

JOIN에서는 동일한 컬럼명이 여러 테이블에 존재하는 경우가 많다.

예를 들어 `EMP`와 `DEPT`에는 모두 `DEPTNO` 컬럼이 존재한다.

이럴 때는 어느 테이블의 컬럼인지 명확하게 지정해야 한다.

```sql
EMP.DEPTNO
DEPT.DEPTNO
```

하지만 매번 테이블 이름을 모두 작성하면 SQL이 길어질 수 있다.

그래서 일반적으로 별칭(Alias)을 사용한다.

```sql
EMP E
DEPT D
```

이후에는 다음과 같이 간단하게 사용할 수 있다.

```sql
SELECT E.ENAME,
       D.DNAME
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

실무에서도 별칭 사용은 거의 필수라고 볼 수 있다.

---

# 5. INNER JOIN(등가 조인)

가장 많이 사용하는 JOIN 방식이다.

Oracle에서는 **INNER JOIN**, **EQUI JOIN**, **SIMPLE JOIN**을 같은 의미로 사용하는 경우가 많다.

두 테이블에서 조건이 일치하는 데이터만 조회한다.

```sql
SELECT E.ENAME,
       E.JOB,
       E.DEPTNO,
       D.DNAME,
       D.LOC
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

JOIN 조건과 일반 조건을 함께 사용할 수도 있다.

```sql
SELECT E.ENAME,
       E.JOB,
       E.DEPTNO,
       D.DNAME,
       D.LOC
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND SAL >= 3000;
```

즉, JOIN으로 테이블을 연결한 뒤 필요한 조건을 추가로 지정할 수 있다.

---

# 6. 범위를 이용한 JOIN

JOIN은 항상 `=` 연산자로만 연결하는 것은 아니다.

조건에 따라 범위를 이용하여 JOIN할 수도 있다.

대표적인 예가 `SALGRADE` 테이블이다.

```sql
SELECT *
FROM EMP E,
     SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

급여가 어느 등급에 속하는지 확인할 때 사용하는 대표적인 JOIN 예제이다.

---

# 7. SELF JOIN

SELF JOIN은 하나의 테이블을 자기 자신과 JOIN하는 방법이다.

대표적인 예가 직원과 관리자 관계이다.

EMP 테이블에는 관리자의 사번(MGR)이 저장되어 있다.

관리자의 이름도 EMP 테이블에 있기 때문에 EMP 테이블을 두 번 사용해야 한다.

```sql
SELECT E1.ENAME,
       E1.JOB,
       E2.ENAME AS MANAGER
FROM EMP E1,
     EMP E2
WHERE E1.MGR = E2.EMPNO;
```

여기서

- `E1` : 직원
- `E2` : 관리자

역할을 수행한다.

같은 테이블을 두 번 사용하는 경우에는 반드시 서로 다른 별칭을 사용해야 한다.

---

# 8. Oracle OUTER JOIN(+)

INNER JOIN은 조건이 일치하는 데이터만 조회한다.

하지만 관리자가 없는 직원도 함께 조회하고 싶다면 OUTER JOIN을 사용해야 한다.

Oracle에서는 `(+)` 기호를 이용하여 OUTER JOIN을 작성했다.

```sql
SELECT E1.EMPNO,
       E1.ENAME,
       E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
FROM EMP E1,
     EMP E2
WHERE E1.MGR = E2.EMPNO(+)
ORDER BY E1.EMPNO;
```

`(+)`가 붙은 테이블은 조건이 일치하지 않아도 결과에 포함된다.

다만 `(+)` 문법은 Oracle에서만 사용할 수 있는 전용 문법이다.

현재는 대부분의 데이터베이스에서 사용할 수 있는 **ANSI JOIN**을 사용하는 것이 일반적이다.

---

# 마무리

- JOIN은 여러 테이블을 공통된 컬럼으로 연결하는 기능이다.
- JOIN 조건이 없으면 Cartesian Product가 발생한다.
- Oracle JOIN은 `FROM`과 `WHERE` 절을 이용하여 작성한다.
- SELF JOIN은 하나의 테이블을 두 번 사용하는 JOIN 방식이다.
- Oracle OUTER JOIN은 `(+)` 기호를 사용하지만 현재는 ANSI JOIN을 사용하는 것이 일반적이다.