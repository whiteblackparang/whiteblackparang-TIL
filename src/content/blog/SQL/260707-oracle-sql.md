---
title: "260707 [오라클 SQL] 기초 : RDBMS 개념과 SQL 명령어 종류"
category: "SQL"
dialect: "Oracle"
date: "2026-07-07"
tags: ["SQL", "Oracle"]
---

# 오라클 SQL 기초 : RDBMS 개념과 SQL 명령어 종류

## SQL (Structured Query Language)

SQL(Structured Query Language)은 **관계형 데이터베이스(RDBMS)** 에 저장된 데이터를 조회하고 조작하기 위한 **표준 데이터베이스 언어**입니다.

### 특징

- 데이터베이스에 접근하고 데이터를 조회 및 조작하는 표준 언어
- 데이터를 조회(SELECT), 입력(INSERT), 수정(UPDATE), 삭제(DELETE)할 수 있다.
- 데이터베이스 객체(테이블, 뷰 등)를 생성하거나 수정할 수 있다.
- 사용자 권한과 트랜잭션을 관리할 수 있다.
- 1986년 미국 국가표준협회(ANSI)의 표준으로 제정
- 1987년 국제표준화기구(ISO)의 국제 표준으로 채택

---

# DBMS (Database Management System)

DBMS(Database Management System)는 데이터베이스를 생성하고 관리하는 소프트웨어입니다.

데이터를 저장하고 조회하며 수정·삭제할 수 있도록 다양한 기능을 제공합니다.

### 주요 기능

- 데이터 저장 및 관리
- 데이터 조회 및 수정
- 사용자 권한 관리
- 백업 및 복구
- 데이터 무결성 유지
- 동시성(Concurrency) 관리

### 대표적인 DBMS

- Oracle Database
- MySQL
- PostgreSQL
- Microsoft SQL Server(MSSQL)
- SQLite

---

# RDBMS (Relational Database Management System)

RDBMS(Relational Database Management System)는 데이터를 **테이블(Table)** 형태로 저장하고, 테이블 간의 관계(Relation)를 이용하여 데이터를 관리하는 데이터베이스 관리 시스템입니다.

### 특징

- 데이터를 행(Row)과 열(Column)로 구성된 테이블에 저장
- 기본 키(Primary Key)와 외래 키(Foreign Key)를 이용하여 테이블 간 관계를 표현
- SQL을 이용하여 데이터를 관리
- 데이터의 중복을 최소화하고 무결성을 유지

### 주요 구성 요소

|구성 요소|설명|
|---|---|
|Table|데이터를 저장하는 객체|
|Row (Record)|한 개의 데이터|
|Column (Field)|데이터의 속성|
|Primary Key (PK)|행을 구분하는 고유한 값|
|Foreign Key (FK)|다른 테이블과 관계를 연결하는 값|

---

# SQL 명령어 종류

SQL 명령어는 크게 **DML, DDL, DCL, TCL** 네 가지로 구분됩니다.

|구분|의미|설명|
|---|---|---|
|DML|Data Manipulation Language|데이터 조작|
|DDL|Data Definition Language|데이터 정의|
|DCL|Data Control Language|권한 제어|
|TCL|Transaction Control Language|트랜잭션 제어|

---

# DML (Data Manipulation Language)

데이터를 조회하거나 변경하는 명령어입니다.

테이블 내부의 **데이터(행)** 를 관리할 때 사용합니다.

### 주요 명령어

|명령어|설명|
|---|---|
|SELECT|데이터 조회|
|INSERT|데이터 추가|
|UPDATE|데이터 수정|
|DELETE|데이터 삭제|

### 특징

- 데이터를 조회·추가·수정·삭제하는 명령어
- COMMIT 전까지 변경 사항은 확정되지 않는다.
- ROLLBACK으로 마지막 COMMIT 이후의 변경 사항을 취소할 수 있다.

### 예시

```sql
SELECT *
FROM EMP;

INSERT INTO EMP
VALUES (9000, 'KIM', 'CLERK', 7902, SYSDATE, 2000, NULL, 10);

UPDATE EMP
SET SAL = 3000
WHERE EMPNO = 9000;

DELETE FROM EMP
WHERE EMPNO = 9000;
```

---

# DDL (Data Definition Language)

데이터베이스 객체를 생성하거나 수정, 삭제하는 명령어입니다.

테이블과 같은 **객체(Object)** 의 구조를 관리할 때 사용합니다.

### 주요 명령어

|명령어|설명|
|---|---|
|CREATE|객체 생성|
|ALTER|객체 수정|
|DROP|객체 삭제|
|TRUNCATE|모든 데이터 삭제(구조 유지)|
|RENAME|객체 이름 변경|

### 특징

- 테이블 구조를 관리하는 명령어
- 실행 즉시 자동 COMMIT된다.
- ROLLBACK으로 취소할 수 없다.

### 예시

```sql
CREATE TABLE TEST (
    ID NUMBER,
    NAME VARCHAR2(30)
);

ALTER TABLE TEST
ADD AGE NUMBER;

RENAME TEST TO TEST2;

TRUNCATE TABLE TEST2;

DROP TABLE TEST2;
```

---

# DCL (Data Control Language)

데이터베이스 사용자에게 권한을 부여하거나 회수하는 명령어입니다.

### 주요 명령어

|명령어|설명|
|---|---|
|GRANT|권한 부여|
|REVOKE|권한 회수|

### 예시

```sql
GRANT SELECT ON EMP TO SCOTT;

REVOKE SELECT ON EMP FROM SCOTT;
```

### 특징

- 사용자 권한을 관리한다.
- 권한 변경 시 자동 COMMIT된다.

---

# TCL (Transaction Control Language)

트랜잭션(Transaction)을 관리하는 명령어입니다.

트랜잭션은 하나의 작업 단위를 의미하며, 여러 개의 SQL 문을 하나의 논리적인 작업으로 처리합니다.

### 주요 명령어

|명령어|설명|
|---|---|
|COMMIT|변경 사항 저장|
|ROLLBACK|변경 사항 취소|
|SAVEPOINT|중간 저장 지점 생성|

### 예시

```sql
UPDATE EMP
SET SAL = SAL * 1.1;

SAVEPOINT SP1;

UPDATE EMP
SET COMM = 100;

ROLLBACK TO SP1;

COMMIT;
```

### 특징

- DML에서 수행한 작업을 관리한다.
- COMMIT 이후에는 ROLLBACK이 불가능하다.
- SAVEPOINT를 이용하면 특정 시점까지만 되돌릴 수 있다.

---

# SQL 명령어 비교

|구분|대상|대표 명령어|자동 COMMIT|ROLLBACK|
|---|---|---|---|---|
|DML|데이터|SELECT, INSERT, UPDATE, DELETE|아니오|가능|
|DDL|객체|CREATE, ALTER, DROP, TRUNCATE, RENAME|예|불가능|
|DCL|권한|GRANT, REVOKE|예|불가능|
|TCL|트랜잭션|COMMIT, ROLLBACK, SAVEPOINT|-|-|

---

# 핵심 정리

- SQL은 관계형 데이터베이스를 다루기 위한 표준 언어이다.
- DBMS는 데이터베이스를 저장하고 관리하는 시스템이다.
- RDBMS는 데이터를 테이블 형태로 저장하고 관계를 이용하여 관리하는 DBMS이다.
- SQL 명령어는 DML, DDL, DCL, TCL 네 가지로 구분된다.
- DML은 데이터를 관리하며 COMMIT과 ROLLBACK의 영향을 받는다.
- DDL은 테이블과 같은 객체를 관리하며 실행 즉시 자동 COMMIT된다.
- DCL은 사용자 권한을 부여하거나 회수한다.
- TCL은 트랜잭션을 저장하거나 취소하고 작업 단위를 관리한다.
- Oracle에서는 DDL과 DCL 실행 시 자동으로 COMMIT이 수행된다.