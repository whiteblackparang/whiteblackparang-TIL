---
title: "11. [R] : 함수(Function) 만들기와 활용"
category: "R"
date: 2026-07-22
tags: ["R", "Function", "Programming", "R Basics"]
---

# Tutorial 11. 함수(Function)

## 학습 목표

- 함수(Function)란 무엇인가
- function() 문법 이해하기
- 매개변수(Parameter)와 인수(Argument)
- return() 함수
- 기본값(Default Argument)
- 여러 값을 반환하는 함수
- 실무에서 자주 사용하는 함수 작성 예제

---

# 함수(Function)란?

함수(Function)는 **특정 작업을 수행하도록 만든 코드의 묶음**이다.

같은 코드를 여러 번 작성하는 대신 함수로 만들어 두면 필요한 순간마다 재사용할 수 있다.

예를 들어 평균을 계산하거나, 할인 금액을 계산하거나, 특정 조건을 검사하는 작업을 함수로 만들면 코드가 훨씬 간결해진다.

함수를 사용하는 이유는 다음과 같다.

- 코드의 재사용성이 높아진다.
- 코드가 간결해진다.
- 유지보수가 쉬워진다.
- 동일한 작업을 반복할 필요가 없다.

---

# 함수의 기본 구조

R에서는 `function()`을 이용하여 함수를 만든다.

```r
함수이름 <- function(매개변수){

    실행할 코드

    return(결과)

}
```

가장 기본적인 형태는 다음과 같다.

```r
hello <- function(){

    print("Hello R")

}
```

실행

```r
hello()
```

결과

```
[1] "Hello R"
```

---

# 매개변수(Parameter)

함수는 외부에서 값을 전달받을 수 있다.

이때 전달받는 값을 **매개변수(Parameter)** 라고 한다.

```r
hello <- function(name){

    print(name)

}
```

실행

```r
hello("Kim")
```

결과

```
[1] "Kim"
```

---

# 예제 1. 두 수의 합 구하기

```r
add <- function(a, b){

    a + b

}

add(10, 20)
```

결과

```
[1] 30
```

R에서는 마지막 줄의 결과가 자동으로 반환된다.

---

# return()

반환값을 명확하게 지정하려면 `return()`을 사용할 수 있다.

```r
add <- function(a, b){

    result <- a + b

    return(result)

}
```

실행

```r
add(5, 8)
```

결과

```
[1] 13
```

복잡한 함수를 작성할 때는 `return()`을 사용하는 것이 가독성이 좋다.

---

# 예제 2. 사칙연산 함수

```r
calculator <- function(a, b){

    result <- c(
        Add = a + b,
        Sub = a - b,
        Mul = a * b,
        Div = a / b
    )

    return(result)

}

calculator(10, 5)
```

결과

```
 Add Sub Mul Div
 15   5  50   2
```

---

# 기본값(Default Argument)

매개변수에는 기본값을 지정할 수 있다.

```r
greet <- function(name = "Guest"){

    print(paste("Hello", name))

}
```

실행

```r
greet()
```

결과

```
[1] "Hello Guest"
```

또는

```r
greet("Lee")
```

결과

```
[1] "Hello Lee"
```

기본값을 사용하면 인수를 생략할 수 있다.

---

# 여러 개의 값을 반환하기

R에서는 벡터나 리스트를 이용하여 여러 값을 함께 반환할 수 있다.

## 벡터 반환

```r
score <- function(){

    c(90, 85, 100)

}

score()
```

결과

```
[1] 90 85 100
```

---

## 리스트 반환

```r
student <- function(){

    list(

        Name = "Kim",
        Age = 25,
        Score = 95

    )

}

student()
```

결과

```
$Name
[1] "Kim"

$Age
[1] 25

$Score
[1] 95
```

실무에서는 여러 정보를 함께 반환할 때 리스트를 많이 사용한다.

---

# 조건문을 사용하는 함수

```r
grade <- function(score){

    if(score >= 90){

        return("A")

    }else if(score >= 80){

        return("B")

    }else{

        return("C")

    }

}
```

실행

```r
grade(95)
```

결과

```
[1] "A"
```

---

# 반복문을 사용하는 함수

```r
sum_number <- function(n){

    total <- 0

    for(i in 1:n){

        total <- total + i

    }

    return(total)

}
```

실행

```r
sum_number(10)
```

결과

```
[1] 55
```

---

# 실무 예제 1. 할인 가격 계산

```r
discount <- function(price, rate){

    result <- price * (1 - rate)

    return(result)

}
```

실행

```r
discount(100000, 0.2)
```

결과

```
[1] 80000
```

---

# 실무 예제 2. BMI 계산 함수

```r
bmi <- function(weight, height){

    weight / (height^2)

}
```

실행

```r
bmi(70, 1.75)
```

결과

```
[1] 22.86
```

---

# 실무 예제 3. 평균 계산 함수

```r
mean_score <- function(score){

    mean(score)

}

score <- c(80, 85, 90, 100)

mean_score(score)
```

결과

```
[1] 88.75
```

---

# 함수 안에서 사용하는 지역 변수

함수 내부에서 만든 변수는 함수 밖에서 사용할 수 없다.

```r
test <- function(){

    x <- 100

    print(x)

}

test()
```

결과

```
[1] 100
```

하지만

```r
print(x)
```

결과

```
Error : object 'x' not found
```

이처럼 함수 내부 변수는 지역 변수(Local Variable)이다.

---

# 함수 작성 시 좋은 습관

- 함수는 하나의 기능만 수행하도록 작성한다.
- 함수 이름은 기능을 알 수 있게 작성한다.
- 너무 긴 함수는 여러 개로 나누는 것이 좋다.
- 반복되는 코드는 함수로 만든다.
- 반환값은 가능한 한 명확하게 작성한다.

---

# 자주 사용하는 내장 함수

| 함수 | 설명 |
|------|------|
| mean() | 평균 계산 |
| sum() | 합계 계산 |
| max() | 최댓값 |
| min() | 최솟값 |
| length() | 길이 계산 |
| paste() | 문자열 연결 |
| round() | 반올림 |
| sqrt() | 제곱근 계산 |

이 함수들도 모두 R에서 제공하는 함수이다.

---

# 마무리
함수는 반복되는 코드를 하나로 묶어 재사용할 수 있게 해주는 중요한 기능이다.

데이터 분석뿐만 아니라 머신러닝, 자동화, 패키지 개발에서도 함수 작성 능력은 필수적인 역량이다.
