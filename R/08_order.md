---
title: "08. [R] 기초 : order() 함수를 이용한 데이터 프레임 정렬"
category: "R"
date: 2026-07-21
tags: ["R", "DataFrame", "order", "sort", "정렬"]
---

# Tutorial 08. order() 함수를 이용한 데이터 프레임 정렬

## 학습 목표

- `order()` 함수의 개념
- 오름차순 / 내림차순 정렬
- 여러 개의 컬럼 기준으로 정렬
- 문자형과 Factor 정렬
- `NA`가 포함된 데이터 정렬
- `sort()`와 `order()`의 차이
- 실무에서 많이 사용하는 정렬 예제

---

# order() 함수란?

데이터 분석에서는 원하는 기준으로 데이터를 **정렬(Sort)** 하는 작업이 매우 자주 발생합니다.

예를 들어

- 매출이 높은 상품부터 확인
- 시험 점수 순으로 학생 정렬
- 가입일 기준 정렬
- 여러 조건(부서 → 급여)으로 정렬

등이 모두 정렬 작업입니다.

R에서는 데이터를 정렬할 때 가장 많이 사용하는 함수가 바로 **order()** 입니다.

> order()는 **정렬된 값 자체를 반환하는 것이 아니라**
> **정렬되어야 하는 위치(Index)를 반환**하는 함수입니다.

그래서 보통 데이터프레임과 함께 사용합니다.

```r
df[order(df$컬럼), ]
```

---

# order() 기본 문법

```r
order(..., decreasing = FALSE, na.last = TRUE)
```

### 주요 인수

|인수|설명|
|---|---|
|...|정렬 기준이 되는 벡터|
|decreasing|TRUE이면 내림차순|
|na.last|NA를 마지막으로 보낼지 여부|

기본값은

```r
decreasing = FALSE
```

즉 **오름차순**입니다.

---

# order()와 sort()의 차이

많은 사람들이 처음에 헷갈리는 부분입니다.

### sort()

값 자체를 정렬합니다.

```r
x <- c(8,4,6,1)

sort(x)
```

결과

```
1 4 6 8
```

---

### order()

정렬되어야 하는 **위치(index)** 를 반환합니다.

```r
x <- c(8,4,6,1)

order(x)
```

결과

```
4 2 3 1
```

의미

```
1번째 위치 → 8
2번째 위치 → 4
3번째 위치 → 6
4번째 위치 → 1

정렬하면

1(4번째)
4(2번째)
6(3번째)
8(1번째)
```

따라서

```r
x[order(x)]
```

를 수행하면

```
1 4 6 8
```

이 됩니다.

실무에서는 **데이터프레임 전체를 함께 이동해야 하므로 대부분 order()를 사용합니다.**

---

# 예제 데이터 생성

```r
student <- data.frame(
  name = c("Kim","Lee","Park","Choi","Jung"),
  score = c(82,95,74,88,95),
  age = c(23,21,24,22,21)
)

student
```

결과

|name|score|age|
|---|---:|---:|
|Kim|82|23|
|Lee|95|21|
|Park|74|24|
|Choi|88|22|
|Jung|95|21|

---

# 오름차순 정렬

점수가 낮은 순으로 정렬합니다.

```r
student[order(student$score), ]
```

결과

|name|score|
|---|---:|
|Park|74|
|Kim|82|
|Choi|88|
|Lee|95|
|Jung|95|

---

# 내림차순 정렬

높은 점수부터 보고 싶다면

```r
student[order(student$score, decreasing = TRUE), ]
```

결과

|name|score|
|---|---:|
|Lee|95|
|Jung|95|
|Choi|88|
|Kim|82|
|Park|74|

---

# 여러 컬럼 기준으로 정렬

실무에서는 하나의 컬럼만으로 정렬하는 경우보다

> "점수는 높은 순, 나이는 어린 순"

처럼 여러 조건을 사용하는 경우가 많습니다.

예제

```r
student[order(-student$score, student$age), ]
```

결과

|name|score|age|
|---|---:|---:|
|Lee|95|21|
|Jung|95|21|
|Choi|88|22|
|Kim|82|23|
|Park|74|24|

정렬 순서는

1. score 내림차순
2. score가 같으면 age 오름차순

입니다.

---

# 문자형 정렬

문자도 알파벳 순으로 정렬됩니다.

```r
student[order(student$name), ]
```

결과

```
Choi
Jung
Kim
Lee
Park
```

---

# Factor 정렬

Factor는 **Levels 순서**를 기준으로 정렬됩니다.

```r
grade <- factor(
    c("Silver","Gold","Bronze","Gold"),
    levels = c("Bronze","Silver","Gold")
)

order(grade)
```

결과

```
3 1 2 4
```

Levels가

```
Bronze
↓

Silver
↓

Gold
```

순서이기 때문입니다.

---

# NA가 있는 경우

```r
score <- c(80, NA, 95, 60, NA)

score[order(score)]
```

결과

```
60
80
95
NA
NA
```

기본적으로

```
na.last = TRUE
```

입니다.

NA를 앞에 두고 싶다면

```r
order(score, na.last = FALSE)
```

를 사용합니다.

---

# order()는 원본 데이터를 변경하지 않는다

다음 코드를 보겠습니다.

```r
student[order(student$score), ]
```

정렬된 결과는 출력되지만

```r
student
```

를 다시 출력하면

원래 순서 그대로입니다.

정렬 결과를 저장하려면

```r
student <- student[order(student$score), ]
```

처럼 다시 대입해야 합니다.

---

# 실무 예제 ① 매출 높은 순 정렬

```r
sales <- data.frame(
  product = c("A","B","C","D"),
  sales = c(520,730,410,680)
)

sales[order(sales$sales, decreasing = TRUE), ]
```

결과

|상품|매출|
|---|---:|
|B|730|
|D|680|
|A|520|
|C|410|

---

# 실무 예제 ② 여러 조건 정렬

```r
employee <- data.frame(
  name = c("Kim","Lee","Park","Choi","Jung"),
  dept = c("HR","Sales","Sales","HR","Sales"),
  salary = c(3500,4200,3900,4100,4200)
)

employee
```

부서 기준으로 정렬하고

같은 부서에서는 급여가 높은 순으로 정렬합니다.

```r
employee[
    order(employee$dept,
          -employee$salary),
]
```

결과

|name|dept|salary|
|---|---|---:|
|Choi|HR|4100|
|Kim|HR|3500|
|Lee|Sales|4200|
|Jung|Sales|4200|
|Park|Sales|3900|

---

# order()와 dplyr::arrange()

최근에는 `dplyr` 패키지의 `arrange()`도 많이 사용됩니다.

```r
library(dplyr)

student %>%
    arrange(score)
```

내림차순

```r
student %>%
    arrange(desc(score))
```

`arrange()`는 파이프(`%>%`)와 함께 사용할 수 있어 가독성이 좋으며, 데이터 분석 프로젝트에서 자주 사용됩니다.

다음 튜토리얼에서는 `dplyr` 패키지와 `arrange()`를 포함한 데이터 조작 함수들을 자세히 살펴봅니다.

---

# 정리

|함수|설명|
|---|---|
|sort()|벡터 값 자체를 정렬|
|order()|정렬 순서를 반환|
|decreasing=TRUE|내림차순|
|na.last=TRUE|NA를 마지막으로 이동|
|order(a, b)|여러 컬럼 기준 정렬|
|df[order(df$x), ]|데이터프레임 정렬의 기본 형태|

---

# 핵심 요약

- `order()`는 데이터프레임을 정렬할 때 가장 많이 사용하는 함수이다.
- 반환값은 정렬된 값이 아니라 **행의 순서(Index)** 이다.
- 오름차순은 기본값이며 `decreasing = TRUE`로 내림차순 정렬이 가능하다.
- 여러 컬럼을 동시에 지정하여 다중 정렬을 수행할 수 있다.
- 문자형은 알파벳 순, Factor는 `levels` 순서대로 정렬된다.
- 원본 데이터는 변경되지 않으므로 저장하려면 다시 대입해야 한다.
- 실무에서는 `order()`와 함께 `dplyr::arrange()`도 매우 자주 사용된다.