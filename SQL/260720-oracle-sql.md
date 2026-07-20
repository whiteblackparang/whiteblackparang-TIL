---
title: "260720 [오라클 SQL] 기초 : ANSI JOIN"
category: "SQL"
dialect: "Oracle"
date: 2026-07-20
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : ANSI JOIN
기존 Oracle JOIN은 `FROM` 절에 여러 테이블을 나열하고 `WHERE` 절에서 JOIN 조건을 작성하는 방식이다. 하지만 이 문법은 Oracle에 종속적인 부분이 있으며, 다른 DBMS에서는 사용할 수 없는 문법도 존재한다.

이를 해결하기 위해 SQL 표준에서는 **ANSI JOIN** 문법을 제공한다. ANSI JOIN은 Oracle뿐만 아니라 MySQL, PostgreSQL, SQL Server 등 대부분의 관계형 데이터베이스에서 공통적으로 사용할 수 있는 표준 JOIN 방식이다.

이번 글에서 다루는 내용

- ANSI JOIN이란?
- NATURAL JOIN
- USING
- ON
- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
- 3개 이상의 테이블 JOIN

---

# 1. ANSI JOIN이란?

ANSI JOIN은 SQL 표준에서 정의한 JOIN 문법이다.

기존 Oracle JOIN은 `WHERE` 절에 JOIN 조건을 함께 작성했지만, ANSI JOIN은 `JOIN`과 `ON` 키워드를 사용하여 JOIN 조건을 명확하게 구분한다.

예를 들어 Oracle JOIN은 다음과 같이 작성한다.

```sql
SELECT E.ENAME,
       D.DNAME
FROM EMP E,
     DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

같은 내용을 ANSI JOIN으로 작성하면 다음과 같다.

```sql
SELECT E.ENAME,
       D.DNAME
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

결과는 동일하지만, JOIN 조건이 별도로 구분되어 있어 SQL의 가독성이 좋아진다.

현재 대부분의 프로젝트에서는 ANSI JOIN 문법을 기본으로 사용한다.

---

# 2. NATURAL JOIN

`NATURAL JOIN`은 두 테이블에서 **이름이 같은 컬럼을 자동으로 찾아 JOIN**하는 방식이다.

```sql
SELECT E.EMPNO,
       E.ENAME,
       DEPTNO,
       D.DNAME,
       D.LOC
FROM EMP E
NATURAL JOIN DEPT D;
```

위 SQL에서는 `EMP`와 `DEPT`에 공통으로 존재하는 `DEPTNO` 컬럼을 자동으로 찾아 JOIN을 수행한다.

별도의 JOIN 조건을 작성하지 않아도 되므로 문법이 간단한 것이 장점이다.

하지만 의도하지 않은 동일한 컬럼까지 JOIN 조건으로 사용될 수 있다는 단점이 있다.

따라서 실무에서는 사용 빈도가 매우 낮으며, 명시적으로 조건을 작성하는 `USING`이나 `ON`을 사용하는 경우가 많다.

---

# 3. USING

`USING`은 두 테이블의 **공통 컬럼을 직접 지정**하여 JOIN하는 방식이다.

```sql
SELECT E.EMPNO,
       E.ENAME,
       DEPTNO,
       D.DNAME,
       D.LOC
FROM EMP E
JOIN DEPT D
USING (DEPTNO);
```

`USING`은 컬럼 이름이 동일한 경우에만 사용할 수 있다.

또한 `USING`으로 지정한 컬럼은 이미 공통 컬럼으로 인식되기 때문에 `SELECT` 절에서도 테이블 별칭을 붙이지 않는다.

```sql
SELECT DEPTNO
```

처럼 작성해야 하며,

```sql
SELECT E.DEPTNO
```

와 같이 작성하면 오류가 발생한다.

---

# 4. ON

`ON`은 ANSI JOIN에서 가장 많이 사용하는 방식이다.

JOIN 조건을 자유롭게 작성할 수 있으며, 컬럼 이름이 서로 달라도 사용할 수 있다.

```sql
SELECT E.EMPNO,
       E.ENAME,
       E.DEPTNO,
       D.DNAME,
       D.LOC
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

`ON`은 JOIN 조건을 명확하게 표현할 수 있고, 복잡한 조건도 자유롭게 작성할 수 있기 때문에 실무에서 가장 많이 사용되는 JOIN 문법이다.

---

# 5. LEFT OUTER JOIN

LEFT OUTER JOIN은 **왼쪽 테이블의 모든 데이터를 유지**하면서 오른쪽 테이블의 데이터를 연결한다.

조건이 일치하지 않는 경우에는 오른쪽 테이블의 컬럼이 `NULL`로 출력된다.

```sql
SELECT E1.ENAME,
       E1.JOB,
       E1.MGR,
       E2.EMPNO,
       E2.ENAME
FROM EMP E1
LEFT OUTER JOIN EMP E2
ON E1.MGR = E2.EMPNO;
```

`OUTER` 키워드는 생략할 수 있으므로 일반적으로는 다음과 같이 작성한다.

```sql
LEFT JOIN
```

---

# 6. RIGHT OUTER JOIN

RIGHT OUTER JOIN은 **오른쪽 테이블의 모든 데이터를 유지**한다.

```sql
SELECT E1.ENAME,
       E1.JOB,
       E1.MGR,
       E2.EMPNO,
       E2.ENAME
FROM EMP E1
RIGHT OUTER JOIN EMP E2
ON E1.MGR = E2.EMPNO;
```

조건이 일치하지 않는 데이터도 오른쪽 테이블을 기준으로 모두 조회할 수 있다.

---

# 7. FULL OUTER JOIN

FULL OUTER JOIN은 **양쪽 테이블의 모든 데이터를 반환**한다.

조건이 일치하는 데이터는 JOIN하여 출력하고,

일치하지 않는 데이터는 각각 `NULL`을 포함한 상태로 함께 출력한다.

```sql
SELECT E1.ENAME,
       E1.JOB,
       E1.MGR,
       E2.EMPNO,
       E2.ENAME
FROM EMP E1
FULL OUTER JOIN EMP E2
ON E1.MGR = E2.EMPNO;
```

세 가지 OUTER JOIN을 비교하면 다음과 같다.

|JOIN 종류|반환되는 데이터|
|:---|:---|
|LEFT JOIN|왼쪽 테이블 전체|
|RIGHT JOIN|오른쪽 테이블 전체|
|FULL OUTER JOIN|양쪽 테이블 전체|

---

# 8. 3개 이상의 테이블 JOIN

JOIN은 두 개의 테이블뿐만 아니라 여러 개의 테이블도 연결할 수 있다.

기존 Oracle 방식은 다음과 같이 작성한다.

```sql
FROM TABLE1,
     TABLE2,
     TABLE3
WHERE TABLE1.COL = TABLE2.COL
AND TABLE2.COL = TABLE3.COL;
```

ANSI JOIN은 다음과 같이 `JOIN`을 이어서 작성한다.

```sql
FROM TABLE1
JOIN TABLE2
ON TABLE1.COL = TABLE2.COL
JOIN TABLE3
ON TABLE2.COL = TABLE3.COL;
```

테이블이 하나 추가될 때마다 JOIN 조건도 하나씩 추가된다고 생각하면 이해하기 쉽다.

예를 들어 네 개의 테이블을 연결한다면 다음과 같이 작성할 수 있다.

```sql
FROM A
JOIN B
ON A.COL = B.COL
JOIN C
ON B.COL = C.COL
JOIN D
ON C.COL = D.COL;
```

복잡한 SQL에서는 JOIN 순서와 별칭을 명확하게 작성하면 가독성을 높일 수 있다.

---

# ANSI JOIN과 Oracle JOIN 비교

|구분|Oracle JOIN|ANSI JOIN|
|:---|:---|:---|
|JOIN 조건|WHERE 절|ON 절|
|가독성|보통|높음|
|표준 SQL|X|O|
|다른 DBMS 사용|제한적|가능|
|실무 사용 빈도|낮음|높음|

현재는 대부분의 프로젝트에서 ANSI JOIN을 사용하며, Oracle의 기존 JOIN 문법은 기존 시스템 유지보수나 자격증 시험에서 주로 접할 수 있다.

---

# 마무리

이번 글에서는 ANSI JOIN의 기본 문법과 다양한 JOIN 방식을 정리했다.

- **NATURAL JOIN**은 공통 컬럼을 자동으로 찾아 JOIN한다.
- **USING**은 공통 컬럼을 직접 지정하여 JOIN한다.
- **ON**은 가장 많이 사용하는 JOIN 방식으로 조건을 자유롭게 작성할 수 있다.
- **LEFT JOIN**, **RIGHT JOIN**, **FULL OUTER JOIN**은 조건이 일치하지 않는 데이터까지 함께 조회할 수 있다.
- ANSI JOIN은 대부분의 관계형 데이터베이스에서 사용할 수 있는 표준 문법이며, 현재 실무에서 가장 널리 사용되는 JOIN 방식이다.