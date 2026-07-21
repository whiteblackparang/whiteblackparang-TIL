---
title: "05. Factor : 범주형 변수와 연속형 변수"
category: "R"
date: "2026-07-21"
tags: ["R", "Factor", "Categorical", "Continuous", "Data Type"]
---

# Tutorial 05. Factor : 범주형 변수와 연속형 변수

## 들어가며

데이터 분석에서 숫자만 다루는 것은 아니다. 성별, 지역, 직업, 제품 등과 같이 **일정한 범주(Category)** 를 갖는 데이터도 매우 많이 사용된다.

R에서는 이러한 범주형 데이터를 **Factor**라는 특별한 자료형으로 관리한다. 특히 통계 분석과 머신러닝에서는 범주형 데이터를 Factor로 변환하는 과정이 매우 중요하다.

이번 튜토리얼에서는 다음 내용을 학습한다.

- Factor란 무엇인가?
- 범주형 변수와 연속형 변수의 차이
- Factor 생성 방법
- 명목형(Nominal)과 순서형(Ordinal) Factor
- level과 label 이해하기
- summary(), levels() 함수 활용
- 실제 데이터셋에서 Factor 확인하기

---

# Factor란?

Factor는 **범주형(Categorical) 데이터를 저장하기 위한 R의 자료형**이다.

예를 들어 아래와 같은 데이터는 모두 Factor로 표현할 수 있다.

- 성별
- 혈액형
- 지역
- 학년
- 직급
- 제품 종류

이러한 값들은 숫자가 아니라 **정해진 그룹(범주)** 에 속한다.

예를 들어

```
남성
여성
```

또는

```
서울
부산
대구
인천
```

처럼 미리 정해진 값만 존재한다.

R은 이러한 데이터를 Factor로 저장하면 내부적으로 **Level(범주 목록)** 을 관리하여 메모리를 효율적으로 사용하고 통계 분석도 쉽게 수행할 수 있다.

---

# 범주형 변수와 연속형 변수

데이터는 크게 두 가지 형태로 구분된다.

|구분|설명|예시|
|---|---|---|
|범주형 변수(Categorical)|정해진 그룹 중 하나를 선택|성별, 지역, 직업|
|연속형 변수(Continuous)|숫자의 크기가 의미를 가짐|키, 몸무게, 매출, 나이|

예를 들어

성별

```
남
여
```

은 크기를 비교할 수 없다.

반면

```
175cm
180cm
```

는 크기 비교가 가능하다.

이것이 가장 큰 차이점이다.

---

# Factor 생성하기

Factor는 factor() 함수를 사용한다.

기본 문법은 다음과 같다.

```R
factor(x)
```

예제를 살펴보자.

```R
gender <- c("Male","Female","Female","Male","Male")

factor_gender <- factor(gender)

factor_gender
```

결과

```
[1] Male   Female Female Male   Male
Levels: Female Male
```

문자 벡터가 Factor로 변환되었다.

---

# 자료형 확인하기

class() 함수를 이용하면 자료형을 확인할 수 있다.

```R
class(gender)
```

결과

```
[1] "character"
```

Factor로 변환하면

```R
class(factor_gender)
```

결과

```
[1] "factor"
```

---

# Levels란?

Factor에는 항상 **Level**이 존재한다.

Level은 해당 데이터가 가질 수 있는 모든 범주를 의미한다.

예를 들어

```R
color <- c("Red","Blue","Green","Blue")

factor(color)
```

결과

```
Levels:
Blue
Green
Red
```

R은 기본적으로 알파벳 순서로 Level을 생성한다.

현재 Level만 확인하고 싶다면

```R
levels(factor(color))
```

결과

```
[1] "Blue" "Green" "Red"
```

---

# 명목형(Nominal) Factor

명목형 범주는 **순서가 존재하지 않는 범주**이다.

예를 들어

- 혈액형
- 성별
- 국가
- 브랜드

등이 있다.

예제

```R
fruit <- c("Apple","Banana","Orange","Apple")

factor(fruit)
```

순서는 의미가 없으며 단순히 그룹만 구분한다.

---

# 순서형(Ordinal) Factor

순서형은 이름 그대로 **순서가 존재하는 범주**이다.

예를 들어

```
매우 만족
만족
보통
불만족
매우 불만족
```

또는

```
초급
중급
고급
```

처럼 크기나 순서가 존재한다.

R에서는 ordered=TRUE를 사용한다.

```R
level <- c("Beginner","Intermediate","Advanced")

study <- c(
"Intermediate",
"Beginner",
"Advanced",
"Intermediate"
)

factor(
study,
levels=level,
ordered=TRUE
)
```

결과

```
Levels:
Beginner < Intermediate < Advanced
```

이제 R은 Beginner보다 Intermediate가 크다는 것을 이해한다.

---

# summary() 함수

Factor에서는 각 범주가 몇 번 등장했는지 매우 자주 확인한다.

이를 위해 summary() 함수를 사용한다.

```R
gender <- factor(c(
"Male",
"Female",
"Female",
"Male",
"Male"
))

summary(gender)
```

결과

```
Female 2
Male   3
```

범주별 빈도를 자동으로 계산해준다.

데이터 분석에서 가장 많이 사용하는 함수 중 하나이다.

---

# labels 사용하기

labels 옵션을 사용하면 표시되는 이름을 변경할 수 있다.

```R
gender <- c(0,1,1,0)

factor(
gender,
levels=c(0,1),
labels=c("Female","Male")
)
```

결과

```
Female
Male
Male
Female
```

데이터베이스에는 숫자로 저장되어 있어도 사람이 읽기 쉬운 이름으로 변경할 수 있다.

---

# 실제 데이터셋에서 확인하기

R에는 mtcars라는 내장 데이터셋이 존재한다.

```R
dataset <- mtcars

class(dataset$mpg)
```

결과

```
[1] "numeric"
```

mpg는 연속형 변수이다.

반면 cyl을 Factor로 변경하면

```R
dataset$cyl <- factor(dataset$cyl)

class(dataset$cyl)
```

결과

```
[1] "factor"
```

이처럼 숫자로 저장되어 있더라도 **의미가 범주라면 Factor로 변환하는 것이 일반적**이다.

예를 들어 실린더 개수(4, 6, 8)는 크기를 계산하기보다 자동차 종류를 구분하는 값으로 사용하는 경우가 많다.

---

# Factor가 중요한 이유

R에서는 대부분의 통계 함수가 범주형 데이터를 자동으로 Factor로 인식한다.

또한 머신러닝 알고리즘도 범주형 변수는 대부분 Factor 형태를 요구한다.

예를 들어 다음과 같은 분석에서 매우 자주 사용된다.

- 회귀분석
- ANOVA
- 의사결정나무
- 랜덤포레스트
- 로지스틱 회귀
- 분류(Classification) 모델

Factor를 사용하면 모델이 문자 데이터를 범주로 올바르게 이해할 수 있으며, 각 범주별 비교와 분석도 손쉽게 수행할 수 있다.

---

# Character와 Factor의 차이

|Character|Factor|
|---------|------|
|단순 문자열 저장|범주형 데이터 저장|
|순서 없음|Level 관리 가능|
|메모리 사용량 큼|메모리 효율적|
|통계 분석 활용 제한|통계 분석에 적합|

초보자는 Character와 Factor를 혼동하기 쉽지만, **문자 데이터를 분석 대상으로 사용할 경우에는 Factor로 변환하는 습관을 들이는 것이 좋다.**

---

# 정리

- Factor는 범주형 데이터를 저장하는 자료형이다.
- 범주형 변수와 연속형 변수는 목적과 특성이 다르다.
- factor() 함수로 문자 데이터를 Factor로 변환할 수 있다.
- 명목형은 순서가 없고, 순서형은 ordered=TRUE를 사용한다.
- levels() 함수는 범주(Level)를 확인하며, summary() 함수는 범주별 빈도를 계산한다.
- 머신러닝과 통계 분석에서는 Character보다 Factor를 사용하는 경우가 훨씬 많다.
