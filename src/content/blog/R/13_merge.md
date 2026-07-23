---
title: "13. [R] Merge: 완전(Full) & 부분(Partial) 대응"
category: "R"
date: 2026-07-23
tags: ["R", "Data Frame", "Merge"]
---

# 데이터 프레임 결합(Merge): 완전(Full) & 부분(Partial) 대응

## 들어가며

실제 데이터 분석에서는 하나의 데이터만 사용하는 경우보다 여러 개의 데이터셋을 함께 사용하는 경우가 훨씬 많다.

예를 들어 다음과 같은 데이터를 각각 보유하고 있다고 가정해 보자.

- 고객 정보
- 주문 내역
- 상품 정보
- 회원 등급
- 지역 정보

이처럼 서로 다른 데이터셋을 **공통된 기준(Key)** 으로 연결하여 하나의 데이터셋으로 만드는 작업을 **병합(Merge)** 이라고 한다.

R에서는 `merge()` 함수를 이용하여 손쉽게 데이터 프레임을 결합할 수 있으며, SQL의 JOIN과 매우 유사한 방식으로 동작한다.

---

- Merge란 무엇인가?
- `merge()` 함수 사용법
- 완전 대응(Full Match)
- 서로 다른 Key 이름으로 병합하기
- 부분 대응(Partial Match)
- `all.x`, `all.y`, `all` 옵션 이해하기
- 실무 활용 예제

---

# Merge란?

Merge는 **공통된 Key 값을 기준으로 두 개 이상의 데이터 프레임을 하나로 합치는 작업**이다.

예를 들어 아래와 같은 두 개의 데이터가 있다고 가정하자.

### 고객 정보

| ID | 이름 |
|----|------|
| 1 | Kim |
| 2 | Lee |
| 3 | Park |

### 구매 정보

| ID | 상품 |
|----|------|
| 1 | Laptop |
| 2 | Mouse |
| 3 | Keyboard |

ID를 기준으로 병합하면 다음과 같은 데이터가 만들어진다.

| ID | 이름 | 상품 |
|----|------|------|
|1|Kim|Laptop|
|2|Lee|Mouse|
|3|Park|Keyboard|

---

# merge() 함수

R에서 데이터 프레임을 병합하는 기본 함수는 `merge()`이다.

```r
merge(x, y, by)
```

## 주요 인수

|인수|설명|
|---|---|
|x|첫 번째 데이터 프레임|
|y|두 번째 데이터 프레임|
|by|공통 Key|
|by.x|첫 번째 데이터의 Key|
|by.y|두 번째 데이터의 Key|
|all.x|왼쪽 데이터 모두 유지|
|all.y|오른쪽 데이터 모두 유지|
|all|양쪽 데이터 모두 유지|

---

# 예제 데이터 만들기

감독 정보와 영화 정보를 각각 만들어 보자.

```r
producers <- data.frame(
  surname = c(
    "Spielberg",
    "Scorsese",
    "Hitchcock",
    "Tarantino",
    "Polanski"
  ),
  nationality = c(
    "US",
    "US",
    "UK",
    "US",
    "Poland"
  ),
  stringsAsFactors = FALSE
)

movies <- data.frame(
  surname = c(
    "Spielberg",
    "Scorsese",
    "Hitchcock",
    "Hitchcock",
    "Spielberg",
    "Tarantino",
    "Polanski"
  ),
  title = c(
    "Jaws",
    "Goodfellas",
    "Psycho",
    "Vertigo",
    "Saving Private Ryan",
    "Pulp Fiction",
    "The Pianist"
  ),
  stringsAsFactors = FALSE
)
```

---

# 완전 대응(Full Match)

기본적으로 `merge()`는 두 데이터에 모두 존재하는 Key만 병합한다.

```r
m1 <- merge(
  producers,
  movies,
  by = "surname"
)

m1
```

### 결과

```
      surname nationality                title
1  Hitchcock          UK              Psycho
2  Hitchcock          UK             Vertigo
3   Polanski      Poland         The Pianist
4   Scorsese          US          Goodfellas
5  Spielberg          US               Jaws
6  Spielberg          US Saving Private Ryan
7 Tarantino          US       Pulp Fiction
```

### 확인

```r
dim(m1)
```

```
[1] 7 3
```

병합된 데이터는 **7행 3열**이다.

---

# 서로 다른 Key 이름으로 병합하기

Key 이름이 서로 달라도 병합할 수 있다.

먼저 영화 데이터의 컬럼명을 변경한다.

```r
colnames(movies)[1] <- "director"
```

병합한다.

```r
m2 <- merge(
  producers,
  movies,
  by.x = "surname",
  by.y = "director"
)

head(m2)
```

결과는 이전과 동일하다.

동일한 데이터인지 확인해 보자.

```r
identical(m1, m2)
```

```
[1] TRUE
```

즉,

- Key 이름이 달라도
- Key 값만 같으면
- 정상적으로 병합된다.

---

# 부분 대응(Partial Match)

실제 데이터에서는 항상 모든 Key가 일치하지는 않는다.

새로운 감독을 추가해 보자.

```r
new_producer <- data.frame(
  surname = "Lucas",
  nationality = "US",
  stringsAsFactors = FALSE
)

producers2 <- rbind(
  producers,
  new_producer
)
```

현재 Lucas는 영화 데이터에 존재하지 않는다.

---

# all.x = TRUE

왼쪽 데이터는 모두 유지하고 싶다면 다음과 같이 작성한다.

```r
m3 <- merge(
  producers2,
  movies,
  by.x = "surname",
  by.y = "director",
  all.x = TRUE
)

m3
```

### 결과

```
      surname nationality                title
1  Hitchcock          UK              Psycho
2  Hitchcock          UK             Vertigo
3      Lucas          US                 <NA>
4   Polanski      Poland         The Pianist
5   Scorsese          US          Goodfellas
6  Spielberg          US               Jaws
7  Spielberg          US Saving Private Ryan
8 Tarantino          US       Pulp Fiction
```

Lucas는 영화 정보가 없으므로 `NA`가 입력된다.

---

# all.y = TRUE

반대로 오른쪽 데이터를 모두 유지하려면

```r
merge(
  producers,
  movies,
  by.x = "surname",
  by.y = "director",
  all.y = TRUE
)
```

을 사용한다.

영화 데이터는 모두 유지되고,

감독 정보가 없는 경우에는 `NA`가 입력된다.

---

# all = TRUE

양쪽 데이터를 모두 유지하려면

```r
merge(
  producers2,
  movies,
  by.x = "surname",
  by.y = "director",
  all = TRUE
)
```

을 사용한다.

이는 SQL의 **FULL OUTER JOIN**과 동일한 개념이다.

---

# 병합 옵션 비교

|옵션|설명|
|---|---|
|기본 merge()|양쪽 모두 존재하는 데이터만 반환|
|all.x=TRUE|왼쪽 데이터 모두 유지|
|all.y=TRUE|오른쪽 데이터 모두 유지|
|all=TRUE|양쪽 데이터 모두 유지|

---

# 실무에서 Merge는 언제 사용할까?

Merge는 거의 모든 데이터 분석 프로젝트에서 사용된다.

대표적인 예는 다음과 같다.

- 고객 정보 + 주문 내역
- 주문 내역 + 상품 정보
- 직원 정보 + 부서 정보
- 회원 정보 + 로그인 기록
- 지역 코드 + 행정구역 정보

여러 데이터셋을 하나의 분석용 데이터셋으로 만드는 과정은 데이터 분석에서 가장 중요한 작업 중 하나이다.

---

# 정리

이번 튜토리얼에서는 데이터 프레임 병합 방법을 학습하였다.

- `merge()` 함수로 데이터 프레임을 결합한다.
- `by`는 공통 Key를 지정한다.
- `by.x`, `by.y`는 서로 다른 Key 이름을 연결한다.
- 기본 `merge()`는 공통 Key만 반환한다.
- `all.x = TRUE`는 왼쪽 데이터를 모두 유지한다.
- `all.y = TRUE`는 오른쪽 데이터를 모두 유지한다.
- `all = TRUE`는 양쪽 데이터를 모두 유지한다.

Merge는 SQL의 JOIN과 매우 유사한 기능이며, 실제 데이터 분석 프로젝트에서 가장 자주 사용하는 데이터 처리 기술 중 하나이다.