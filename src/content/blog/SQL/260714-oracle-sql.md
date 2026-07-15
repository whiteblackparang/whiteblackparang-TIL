---
title: "260714 [오라클 SQL] 기초 : ALTER와 테이블 관리"
category: "SQL"
dialect: "Oracle"
date: 2026-07-14
tags: ["SQL", "Oracle"]
---
# 오라클 SQL 기초 : ALTER와 테이블 관리

`ALTER TABLE`은 이미 만들어진 테이블의 구조(컬럼, 제약조건, 이름 등)를 바꾸는 DDL 명령어다. 데이터를 다루는 DML(INSERT, UPDATE, DELETE)과 달리, 테이블의 "설계도" 자체를 수정한다는 점을 먼저 기억해두면 이후 내용이 훨씬 잘 이해된다.

---

## 1. 컬럼 관리

### ADD - 컬럼 추가

``` sql
ALTER TABLE EMP_ALTER
ADD HP VARCHAR2(20);
```

기존 테이블에 새 컬럼을 추가한다. 이미 데이터가 있는 테이블이라면, 추가된 컬럼의 기존 행 값은 모두 NULL로 채워진다.

여러 컬럼을 한 번에 추가하려면 괄호로 묶어서 콤마로 구분한다.

``` sql
ALTER TABLE EMP_ALTER
ADD (ADDR VARCHAR2(100), EMAIL VARCHAR2(50));
```

### RENAME COLUMN - 컬럼 이름 변경

``` sql
ALTER TABLE EMP_ALTER
RENAME COLUMN HP TO TEL;
```

컬럼의 이름만 바꾸고 데이터 타입이나 값은 그대로 유지된다.

### MODIFY - 컬럼 정의 수정

``` sql
ALTER TABLE EMP_ALTER
MODIFY EMPNO NUMBER(5);
```

컬럼의 데이터 타입, 크기, 제약조건 등을 수정할 때 사용한다. NOT NULL 제약을 추가할 때도 MODIFY를 쓴다.

``` sql
ALTER TABLE EMP_ALTER
MODIFY EMPNO NOT NULL;
```

> 이미 데이터가 들어 있는 컬럼의 타입/크기를 줄이거나 NOT NULL로 바꾸려 하면, 기존 데이터가 조건을 만족하지 못할 경우 오류가 발생할 수 있다.

MODIFY도 여러 컬럼을 괄호로 묶어 한 번에 처리할 수 있다.

``` sql
ALTER TABLE EMP_ALTER
MODIFY (EMPNO NUMBER(5), ENAME VARCHAR2(30));
```

### DROP COLUMN - 컬럼 삭제

``` sql
ALTER TABLE EMP_ALTER
DROP COLUMN TEL;
```

컬럼과 해당 컬럼의 모든 데이터를 즉시, 완전히 삭제한다.

### SET UNUSED - 논리적 컬럼 삭제 (대용량 테이블용)

대용량 테이블에서 `DROP COLUMN`을 바로 실행하면 모든 행의 데이터를 재구성해야 해서 부하가 크고 시간이 오래 걸린다. 이럴 때는 컬럼을 "사용 안 함" 상태로만 표시해두고, 나중에 시스템이 한가할 때 실제로 삭제하는 방법을 쓴다.

``` sql
-- 1단계: 논리적으로만 숨김 (즉시 완료, 부하 적음)
ALTER TABLE EMP_ALTER SET UNUSED COLUMN TEL;

-- 2단계: 실제 삭제 (나중에 별도로 실행)
ALTER TABLE EMP_ALTER DROP UNUSED COLUMNS;
```

SET UNUSED 처리된 컬럼은 조회/사용이 불가능해지지만, DROP UNUSED COLUMNS를 실행하기 전까지는 디스크 공간을 계속 차지한다.

---

## 2. 제약조건(CONSTRAINT) 관리

ALTER TABLE의 또 다른 핵심 용도는 제약조건을 추가하거나 삭제하는 것이다.

``` sql
-- 기본 키(PRIMARY KEY) 추가
ALTER TABLE EMP_ALTER
ADD CONSTRAINT PK_EMP_ALTER PRIMARY KEY (EMPNO);

-- 제약조건 삭제
ALTER TABLE EMP_ALTER
DROP CONSTRAINT PK_EMP_ALTER;
```

제약조건에 이름을 직접 지정해두면(`PK_EMP_ALTER`처럼) 나중에 어떤 제약조건인지 식별하고 삭제하기 쉬워진다.

---

## 3. 테이블 이름 변경

두 가지 방법이 있으며 결과는 동일하다.

``` sql
-- 방법 1: RENAME 문
RENAME EMP_ALTER TO EMP_RENAME;

-- 방법 2: ALTER TABLE 문
ALTER TABLE EMP_ALTER RENAME TO EMP_RENAME;
```

---

## 4. 데이터/테이블 삭제

### TRUNCATE - 데이터만 빠르게 삭제

``` sql
TRUNCATE TABLE EMP_RENAME;
```

테이블 구조는 유지하고 모든 데이터를 빠르게 삭제한다.

> TRUNCATE는 DDL이므로 실행과 동시에 자동 커밋된다. 따라서 `DELETE`와 달리 **ROLLBACK으로 되돌릴 수 없다.** 이 점이 DELETE와 TRUNCATE의 가장 결정적인 차이다.

| 구분 | DELETE | TRUNCATE |
|---|---|---|
| 분류 | DML | DDL |
| ROLLBACK | 가능 | 불가능 (자동 커밋) |
| 속도 | 상대적으로 느림 | 빠름 |
| WHERE 조건 | 사용 가능 | 사용 불가 (전체 삭제만) |

### DROP TABLE - 테이블 자체를 삭제

``` sql
DROP TABLE EMP_RENAME;
```

테이블 구조와 데이터를 모두 삭제한다.

#### 휴지통(Recycle Bin)을 통한 복구

Oracle은 `DROP TABLE`을 실행해도 데이터를 바로 지우지 않고 휴지통(Recycle Bin)에 보관한다. 실수로 테이블을 삭제했더라도 복구가 가능하다.

``` sql
-- 삭제된 테이블 복구
FLASHBACK TABLE EMP_RENAME TO BEFORE DROP;
```

휴지통을 거치지 않고 즉시, 완전히 삭제하려면 `PURGE` 옵션을 사용한다.

``` sql
DROP TABLE EMP_RENAME PURGE;
```

---

# 핵심 정리

- `ALTER TABLE`은 테이블의 **구조**를 변경하는 DDL 명령어다.
- 컬럼 관리: `ADD`(추가), `MODIFY`(수정), `RENAME COLUMN`(이름 변경), `DROP COLUMN`(삭제)
- 대용량 테이블의 컬럼 삭제는 `SET UNUSED` → `DROP UNUSED COLUMNS` 순서로 부하를 줄일 수 있다.
- 제약조건은 `ADD CONSTRAINT` / `DROP CONSTRAINT`로 추가·삭제한다.
- 테이블 이름 변경은 `RENAME`문 또는 `ALTER TABLE ... RENAME TO`로 가능하다.
- `TRUNCATE`는 데이터만 삭제하며, **DDL이라 자동 커밋되어 ROLLBACK이 불가능**하다.
- `DROP TABLE`은 테이블 자체를 삭제하지만, 기본적으로 휴지통에 보관되어 `FLASHBACK TABLE`로 복구할 수 있다. 즉시 완전 삭제하려면 `PURGE` 옵션을 사용한다.