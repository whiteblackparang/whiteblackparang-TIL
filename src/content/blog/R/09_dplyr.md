---
title: "09. [R] : dplyr 기초 - 데이터 전처리와 핵심 함수"
category: "R"
date: 2026-07-22
tags: ["R", "dplyr", "Data Preparation", "Data Cleaning"]
---

# Tutorial 09. dplyr 기초 - 데이터 전처리와 핵심 함수

## 학습 목표

이번 튜토리얼에서는 다음 내용을 학습한다.

- dplyr 패키지란 무엇인가
- dplyr 설치 및 불러오기
- 파이프 연산자(`%>%`) 이해하기
- select() 함수
- filter() 함수
- mutate() 함수
- arrange() 함수
- summarise() 함수
- group_by() 함수

---

# dplyr이란?

`dplyr`은 **R에서 데이터를 가공(전처리)하기 위해 가장 많이 사용하는 패키지**이다.

실제 데이터 분석 프로젝트에서는 모델을 만드는 시간보다 데이터를 정리하고 가공하는 시간이 훨씬 많이 소요된다. 현업에서는 전체 분석 시간의 70~80%를 데이터 전처리에 사용한다고 할 정도이다.

예를 들어 다음과 같은 작업을 수행해야 한다.

- 필요한 열만 선택하기
- 조건에 맞는 데이터 추출하기
- 새로운 변수 생성하기
- 데이터 정렬하기
- 그룹별 통계 계산하기
- 여러 데이터셋 결합하기

이러한 작업을 쉽고 직관적으로 수행하도록 만든 패키지가 **dplyr**이다.

특히 SQL의 `SELECT`, `WHERE`, `ORDER BY`, `GROUP BY`와 매우 비슷한 문법을 제공하기 때문에 SQL을 공부한 사람이라면 빠르게 익힐 수 있다.

---

# dplyr 설치

처음 한 번만 설치하면 된다.

```r
install.packages("dplyr")
```

설치가 완료되면 라이브러리를 불러온다.

```r
library(dplyr)
```

---

# 실습 데이터 준비

이번 튜토리얼에서는 R에 기본으로 포함된 **mtcars** 데이터셋을 사용한다.

```r
head(mtcars)
```

출력 예시

```
                   mpg cyl disp  hp drat    wt
Mazda RX4         21.0   6  160 110 3.90 2.620
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875
Datsun 710        22.8   4  108  93 3.85 2.320
Hornet 4 Drive    21.4   6  258 110 3.08 3.215
```

---

# 파이프 연산자(%>%)

dplyr을 사용할 때 가장 많이 사용하는 문법이 **파이프(Pipe)** 이다.

```r
데이터 %>%
    함수1() %>%
    함수2() %>%
    함수3()
```

왼쪽의 결과를 오른쪽 함수의 첫 번째 인수로 전달한다.

예를 들어

```r
mtcars %>%
    head()
```

은 아래 코드와 동일하다.

```r
head(mtcars)
```

하지만 함수가 여러 개 연결될수록 `%>%`를 사용하는 코드가 훨씬 읽기 쉽다.

---

# select()

## 개념

`select()`는 **필요한 열(Column)만 선택**하는 함수이다.

가장 많이 사용하는 함수 중 하나이다.

## 문법

```r
select(data, column1, column2, ...)
```

---

## 예제 1. 특정 열 선택

```r
mtcars %>%
    select(mpg, hp)
```

결과

```
    mpg   hp
1  21.0 110
2  21.0 110
3  22.8  93
...
```

---

## 예제 2. 여러 열 선택

```r
mtcars %>%
    select(mpg, cyl, hp, wt)
```

---

## 예제 3. 열 제외하기

```r
mtcars %>%
    select(-hp)
```

`-` 기호는 해당 열을 제외한다.

---

# filter()

## 개념

`filter()`는 **조건에 맞는 행(Row)만 추출**하는 함수이다.

SQL의 WHERE 절과 같은 역할을 한다.

## 문법

```r
filter(data, 조건)
```

---

## 예제 1. 연비가 25 이상인 차량

```r
mtcars %>%
    filter(mpg >= 25)
```

---

## 예제 2. 실린더가 6개인 차량

```r
mtcars %>%
    filter(cyl == 6)
```

---

## 예제 3. 여러 조건 사용

```r
mtcars %>%
    filter(cyl == 4, mpg >= 25)
```

또는

```r
mtcars %>%
    filter(cyl == 4 & mpg >= 25)
```

---

# mutate()

## 개념

`mutate()`는 **새로운 열을 생성하거나 기존 열을 수정**하는 함수이다.

분석 과정에서 파생변수를 만들 때 가장 많이 사용한다.

## 문법

```r
mutate(data, 새로운열 = 계산식)
```

---

## 예제 1. 새로운 열 생성

```r
mtcars %>%
    mutate(weight_kg = wt * 453.592)
```

---

## 예제 2. 연비 등급 생성

```r
mtcars %>%
    mutate(
        grade = ifelse(mpg >= 20,
                       "High",
                       "Low")
    )
```

결과

```
mpg    grade
21.0   High
21.0   High
15.2   Low
```

---

# arrange()

## 개념

`arrange()`는 데이터를 **정렬**하는 함수이다.

기본값은 오름차순이다.

## 문법

```r
arrange(data, column)
```

---

## 예제 1. 연비 오름차순

```r
mtcars %>%
    arrange(mpg)
```

---

## 예제 2. 연비 내림차순

```r
mtcars %>%
    arrange(desc(mpg))
```

---

## 예제 3. 여러 기준 정렬

```r
mtcars %>%
    arrange(cyl, desc(mpg))
```

먼저 실린더 수를 기준으로 정렬한 뒤 같은 실린더 안에서는 연비가 높은 순으로 정렬한다.

---

# summarise()

## 개념

`summarise()`는 데이터를 **하나의 통계값으로 요약**하는 함수이다.

평균, 합계, 최대값, 최소값 등을 계산할 때 사용한다.

## 문법

```r
summarise(data, 결과 = 함수())
```

---

## 예제 1. 평균 연비

```r
mtcars %>%
    summarise(mean_mpg = mean(mpg))
```

결과

```
mean_mpg
20.09
```

---

## 예제 2. 여러 통계 계산

```r
mtcars %>%
    summarise(
        평균 = mean(mpg),
        최대 = max(mpg),
        최소 = min(mpg),
        개수 = n()
    )
```

---

# group_by()

## 개념

`group_by()`는 데이터를 그룹으로 묶는 함수이다.

대부분 `summarise()`와 함께 사용한다.

SQL의 GROUP BY와 같은 역할을 한다.

---

## 예제 1. 실린더별 평균 연비

```r
mtcars %>%
    group_by(cyl) %>%
    summarise(
        평균연비 = mean(mpg)
    )
```

결과

```
cyl   평균연비
4     26.66
6     19.74
8     15.10
```

---

## 예제 2. 여러 통계 계산

```r
mtcars %>%
    group_by(cyl) %>%
    summarise(
        차량수 = n(),
        평균마력 = mean(hp),
        평균무게 = mean(wt)
    )
```

---

# 함수 조합 예제

dplyr의 가장 큰 장점은 여러 함수를 자연스럽게 연결할 수 있다는 점이다.

다음 코드는 실린더가 6개 이상인 차량만 선택한 뒤 연비가 높은 순으로 정렬하고 필요한 열만 출력한다.

```r
mtcars %>%
    filter(cyl >= 6) %>%
    arrange(desc(mpg)) %>%
    select(mpg, cyl, hp)
```

이처럼 하나의 분석 과정을 위에서 아래로 순서대로 작성할 수 있어 코드의 가독성이 매우 높다.

---

# 자주 사용하는 dplyr 함수

| 함수 | 설명 |
|------|------|
| select() | 필요한 열 선택 |
| filter() | 조건에 맞는 행 추출 |
| mutate() | 새로운 변수 생성 |
| arrange() | 데이터 정렬 |
| summarise() | 통계 요약 |
| group_by() | 그룹별 집계 |

---

# 마무리

이번 튜토리얼에서는 **dplyr의 핵심 함수**를 학습하였다.

데이터 분석에서는 대부분 다음과 같은 순서로 작업을 수행한다.

1. 필요한 열 선택 (`select()`)
2. 조건에 맞는 데이터 추출 (`filter()`)
3. 새로운 변수 생성 (`mutate()`)
4. 데이터 정렬 (`arrange()`)
5. 그룹 생성 (`group_by()`)
6. 통계 계산 (`summarise()`)
