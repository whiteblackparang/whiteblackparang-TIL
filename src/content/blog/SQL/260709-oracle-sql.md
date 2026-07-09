---
title: "260709 [오라클 SQL] 기초 : 데이터 사전과 데이터 관리"
category: "SQL"
dialect: "Oracle"
date: 2026-07-09
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : 데이터 사전과 데이터 관리

## 데이터 사전(Data Dictionary)

데이터 사전(Data Dictionary)은 오라클 데이터베이스를 구성하고 관리하기 위한 **메타데이터(Metadata)** 를 저장하는 시스템 테이블입니다.

사용자가 직접 수정할 수는 없지만, **데이터 사전 뷰(Data Dictionary View)** 를 통해 필요한 정보를 조회할 수 있습니다.

### 데이터 사전 뷰 접두어

|접두어|설명|
|---|---|
|`USER_`|현재 접속한 사용자가 소유한 객체(테이블, 인덱스 등)를 조회|
|`ALL_`|현재 사용자가 접근 권한을 가진 모든 객체를 조회|
|`DBA_`|데이터베이스 관리자(DBA) 권한이 있는 사용자만 조회 가능|

> 일반 계정(예: SCOTT)은 `DBA_` 뷰를 조회할 수 없습니다.

### USER_TABLES 조회

```sql
SELECT table_name
FROM user_tables;
```

### ALL_TABLES 조회

```sql
SELECT owner,
       table_name
FROM all_tables;
```

---

# CREATE TABLE

## 기존 테이블을 복사하여 생성하기

### 테이블 구조와 데이터 모두 복사

```sql
CREATE TABLE CO_DEPT_SRC
AS
SELECT *
FROM DEPT;
```

### 테이블 구조만 복사

조회 결과가 없도록 `WHERE 1 <> 1` 조건을 사용하면 데이터는 복사되지 않고 **테이블 구조만 복사**됩니다.

```sql
CREATE TABLE CO_EMP_SRC
AS
SELECT *
FROM EMP
WHERE 1 <> 1;
```

---

# INSERT

## 데이터 입력

### 모든 컬럼에 데이터 입력

```sql
INSERT INTO CO_DEPT_SRC
VALUES (70, 'WEB', NULL);
```

```sql
INSERT INTO CO_DEPT_SRC
VALUES (80, 'MOBILE', '');
```

> Oracle에서는 빈 문자열(`''`)을 **NULL**로 처리합니다.

### 일부 컬럼만 지정하여 입력

지정하지 않은 컬럼은 자동으로 **NULL**이 입력됩니다.

```sql
INSERT INTO CO_DEPT_SRC (DEPTNO, DNAME)
VALUES (90, 'CLOUD');
```

---

## INSERT 시 자주 발생하는 오류

|오류 유형|설명|대표 오류|
|---|---|---|
|컬럼 개수 부족|VALUES 개수가 컬럼보다 적음|ORA-00947|
|컬럼 개수 초과|VALUES 개수가 컬럼보다 많음|ORA-00913|
|자료형 불일치|숫자 컬럼에 문자 입력 등|ORA-01722|
|자릿수 초과|NUMBER의 허용 자릿수 초과|ORA-01438|

---

# UPDATE

## 모든 행 수정

`WHERE` 절을 생략하면 **테이블의 모든 행**이 수정됩니다.

```sql
UPDATE CO_DEPT_SRC
SET LOC = 'SEOUL';
```

> UPDATE를 실행하기 전에 반드시 WHERE 조건을 확인하는 습관을 가지는 것이 좋습니다.

---

## 특정 행만 수정

```sql
UPDATE CO_DEPT_SRC
SET DNAME = 'DATABASE',
    LOC = 'BUSAN'
WHERE DEPTNO = 40;
```

---

## 서브쿼리를 이용한 수정

다른 테이블의 값을 조회하여 수정할 수도 있습니다.

```sql
UPDATE CO_DEPT_SRC
SET (DNAME, LOC) =
(
    SELECT DNAME,
           LOC
    FROM DEPT
    WHERE DEPTNO = 40
)
WHERE DEPTNO = 40;
```

---

# 트랜잭션(Transaction)

트랜잭션은 데이터베이스에서 하나의 작업 단위를 의미합니다.

Oracle에서는 DML(`INSERT`, `UPDATE`, `DELETE`) 실행 후 `COMMIT` 하기 전까지는 변경 사항이 임시 상태입니다.

## ROLLBACK

마지막 COMMIT 이후의 변경 사항을 모두 취소합니다.

```sql
ROLLBACK;
```

## COMMIT

현재까지의 변경 사항을 데이터베이스에 영구 저장합니다.

```sql
COMMIT;
```

---

# DELETE

## 행(Row) 삭제

테이블 구조는 유지하고 데이터만 삭제합니다.

```sql
DELETE FROM CO_EMP_SRC
WHERE JOB = 'MANAGER';
```

`WHERE` 절을 생략하면 모든 데이터가 삭제됩니다.

```sql
DELETE FROM CO_EMP_SRC;
```

---

# DROP TABLE

## 테이블 삭제

테이블의 구조와 데이터를 모두 삭제합니다.

```sql
DROP TABLE CO_EMP_SRC;
```

> `DROP TABLE`은 DDL(Data Definition Language)이므로 실행과 동시에 자동으로 COMMIT됩니다.

---

# DELETE와 DROP의 차이

|DELETE|DROP|
|---|---|
|데이터만 삭제|테이블 자체 삭제|
|테이블 구조 유지|테이블 구조 삭제|
|WHERE 사용 가능|WHERE 사용 불가|
|ROLLBACK 가능(COMMIT 전)|ROLLBACK 불가|
|DML|DDL|

---

# DELETE와 TRUNCATE의 차이

|DELETE|TRUNCATE|
|---|---|
|행 단위 삭제|테이블 전체 삭제|
|WHERE 사용 가능|WHERE 사용 불가|
|ROLLBACK 가능(COMMIT 전)|ROLLBACK 불가|
|속도가 상대적으로 느림|속도가 빠름|
|DML|DDL|

---

# DML과 DDL

## DML (Data Manipulation Language)

데이터를 조회하거나 수정하는 명령어입니다.

- SELECT
- INSERT
- UPDATE
- DELETE

특징

- COMMIT이 필요
- ROLLBACK 가능

---

## DDL (Data Definition Language)

데이터베이스 객체를 생성하거나 수정하는 명령어입니다.

- CREATE
- ALTER
- DROP
- TRUNCATE
- RENAME

특징

- 실행 즉시 자동 COMMIT
- ROLLBACK 불가

---

# 자주 사용하는 명령어

## 테이블 구조 확인

```sql
DESC EMP;
```

또는

```sql
DESCRIBE EMP;
```

---

## 현재 사용자가 가진 테이블 조회

```sql
SELECT table_name
FROM user_tables;
```

---

## 접근 가능한 모든 테이블 조회

```sql
SELECT owner,
       table_name
FROM all_tables;
```

---

# 핵심 정리

- 데이터 사전은 데이터베이스의 메타데이터를 저장하는 시스템 정보이다.
- `USER_`, `ALL_`, `DBA_` 데이터 사전 뷰를 통해 정보를 조회할 수 있다.
- `CREATE TABLE AS SELECT`를 이용하면 기존 테이블을 쉽게 복사할 수 있다.
- `WHERE 1 <> 1`을 사용하면 구조만 복사할 수 있다.
- Oracle에서는 빈 문자열(`''`)을 NULL로 처리한다.
- INSERT 시 컬럼 개수와 자료형을 반드시 확인한다.
- UPDATE와 DELETE에서 WHERE 절을 생략하면 모든 행이 변경 또는 삭제된다.
- COMMIT은 변경 사항을 저장하고, ROLLBACK은 마지막 COMMIT 이후의 변경 사항을 취소한다.
- DROP과 TRUNCATE는 DDL이므로 실행 즉시 자동 COMMIT된다.
- DELETE는 DML, DROP과 TRUNCATE는 DDL이라는 차이를 기억한다.