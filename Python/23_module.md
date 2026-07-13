---
title: "23_module and import"
category: "Python"
date: 2026-07-11
tags: ["Python", "Function"]
---
# 모듈과 import

---

#### 0. 전체 구조 한눈에 보기

파이썬에서 모듈과 import는 이렇다.

1. 모듈: 기능을 담은 파일
2. import: 모듈 가져오기
3. from import: 필요한 것만 가져오기
4. as: 이름 변경
5. 표준 라이브러리 활용
6. 여러 모듈 조합 → 실무 로직 구성

---

#### 1. 모듈(Module)이란?

## 개념

함수, 변수, 클래스 등을 모아놓은 파이썬 파일

예:

* math.py
* random.py
* datetime.py

---

## 왜 사용하는가

* 코드 재사용
* 기능 분리
* 유지보수 용이

---

## 핵심 구조

하나의 파일 = 하나의 모듈

---

#### 2. import 기본

## 개념

모듈 전체를 가져오는 방법

```python
import math

math.sqrt(16)
```

---

## 특징

* 모듈 전체를 가져옴
* 사용할 때 `모듈명.함수명` 형태

---

## 장점

* 명확함 (어디서 온 함수인지 알 수 있음)
* 충돌 방지

---

## 단점

* 코드가 길어짐

---

## 실무 기준

기본은 import 사용이 가장 안전하다.

---

#### 3. from import

## 개념

모듈에서 특정 함수만 가져오기

```python
from math import sqrt

sqrt(25)
```

---

## 특징

* 모듈명 없이 바로 사용 가능
* 필요한 것만 가져옴

---

## 장점

* 코드 간결
* 가독성 증가

---

## 단점

* 출처가 불명확할 수 있음
* 이름 충돌 가능

---

## 실무 기준

* 많이 쓰는 함수만 제한적으로 사용
* 대규모 코드에서는 주의 필요

---

#### 4. as 별칭 (Alias)

## 개념

모듈 또는 함수 이름을 변경

```python
import math as m

m.sqrt(36)
```

또는

```python
from math import sqrt as s

s(49)
```

---

## 사용하는 이유

1. 긴 이름 축약
2. 이름 충돌 방지
3. 코드 가독성 개선

---

## 실무 기준

* 짧고 의미 있는 이름 사용
* 일반적으로 널리 쓰이는 관례 존재

예:

* numpy → np
* pandas → pd

---

#### 5. import 방식 비교

| 방식                         | 사용 형태       | 특징    |
| -------------------------- | ----------- | ----- |
| import math                | math.sqrt() | 가장 안전 |
| from math import sqrt      | sqrt()      | 간결    |
| import math as m           | m.sqrt()    | 짧음    |
| from math import sqrt as s | s()         | 매우 간결 |

---

#### 6. math 모듈

## 주요 기능

* sqrt: 제곱근
* ceil: 올림
* floor: 내림
* pow: 거듭제곱
* pi: 원주율

```python
from math import sqrt, ceil

sqrt(16)
ceil(3.2)
```

---

## 실무 포인트

* math.pow → float 반환
* ** 연산자 → 정수 유지 가능

---

#### 7. random 모듈

## 주요 기능

* randint(a, b): 정수 난수
* random(): 0~1 실수
* choice(): 리스트 선택
* shuffle(): 리스트 섞기

```python
from random import randint

randint(1, 100)
```

---

## 실무 포인트

* 실행마다 결과가 달라짐
* 테스트 시 seed 설정 고려

---

#### 8. datetime 모듈

## 주요 기능

* datetime.now(): 현재 시간
* strptime(): 문자열 → 날짜 변환
* strftime(): 날짜 → 문자열 변환

```python
from datetime import datetime

now = datetime.now()
now.year, now.month
```

---

## 실무 포인트

* 로그 처리
* 데이터 시간 분석
* ETL 파이프라인에서 필수

---

#### 9. import 동작 원리

## 실행 방식

1. 처음 import 시 모듈 로드
2. 메모리에 캐싱됨
3. 이후 재사용

---

## 특징

* 한 번만 실행됨
* 성능 효율적

---

#### 10. import 에러 이해

## 대표 에러

* ModuleNotFoundError
* ImportError
* NameError

---

## 해결 방법

1. 모듈 설치 여부 확인
2. 경로 확인
3. 이름 오타 확인
4. alias 사용 여부 확인

---

#### 11. 실무에서 import 설계 방식

## 기본 원칙

1. 파일 상단에 import 모아서 작성
2. 표준 라이브러리 → 외부 라이브러리 → 내부 모듈 순서
3. 필요한 것만 import

---

## 나쁜 예

```python
def func():
    import math
    return math.sqrt(16)
```

---

## 좋은 예

```python
import math

def func():
    return math.sqrt(16)
```

---

#### 12. 실무 import 조합 패턴

##### 예제 구조

입력 → 처리 → 출력

```python
from datetime import datetime
from statistics import mean

def summarize(logs):
    times = [log["time"] for log in logs]
    return {
        "count": len(times),
        "avg": mean(times),
        "date": datetime.now().strftime("%Y-%m-%d")
    }
```

---

##### 핵심 포인트

* 여러 모듈을 조합
* 각 모듈 역할 명확히 구분
* 작은 함수로 연결

---

#### 13. 실무 관점 핵심 정리

* import는 단순 문법이 아니라 "도구 선택"
* 필요한 기능만 가져오는 것이 중요
* 모듈 조합 능력이 곧 생산성

---

#### 14. 핵심 요약

* 모듈: 기능 묶음 파일
* import: 전체 가져오기
* from import: 일부 가져오기
* as: 이름 변경
* 표준 라이브러리: 기본 도구

---

# 16. 결론

좋은 코드는 다음을 따른다.

* 필요한 모듈만 명확하게 선택한다
* import 구조를 일관되게 유지한다
* 모듈을 조합해 작은 단위로 로직을 만든다

import는 단순 문법이 아니라 문제 해결을 위한 도구 선택 과정이다.
