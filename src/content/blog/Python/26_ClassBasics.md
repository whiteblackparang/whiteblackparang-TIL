---
title: "26_ClassBasics"
category: "Python"
date: 2026-07-13
tags: ["Python", "OOP", "Class"]
---

# 클래스 기초 (Class Basics)

파이썬에서 데이터와 기능을 하나로 묶는 도구인 클래스를 어떻게 정의하고 사용하는지 다룬다. 입력값을 정하고, 처리 로직을 실행하고, 출력 결과를 확인하는 흐름을 작은 코드 조각으로 하나씩 연결해서 익힌다.

## 흐름

1. **클래스 정의 입력 확인** — 문자열, 숫자, 변수 같은 예제 값과 필요한 조건을 먼저 정한다.
2. **`__init__` 메서드 처리 실행** — 기초 문법 코드를 실행해서 중간 결과를 확인한다.
3. **`self` 이해하기 결과 검증** — 출력 또는 마지막 표현식 결과를 기준으로 실행 결과를 비교한다.
4. **클래스 기초 재사용** — 완성한 코드를 작은 자동화 스크립트에 붙일 수 있도록 정리한다.

정리하면, 문자열·숫자·변수 같은 예제 값을 먼저 확인한 뒤 기초 문법에 맞는 코드를 고르고, 결과를 출력 또는 마지막 표현식 기준으로 즉시 점검하고, 완료한 코드는 이후 자동화 스크립트에 다시 활용할 수 있다.

---

## 1. 클래스 정의 — class 키워드

### 왜 배우는가
클래스는 "객체를 어떻게 만들지" 미리 정해놓은 설계도다. 초보자 입장에서는 숫자나 문자열 같은 기본 자료형만으로는 표현하기 힘든, "이름도 있고 나이도 있고 인사도 하는 사람" 같은 복잡한 대상을 다루기 위한 첫걸음이다. 실무자 입장에서는 서로 관련 있는 데이터와 기능을 하나의 단위로 묶어서 코드를 정리하는 기본 도구다.

### 상세 설명
- 클래스는 객체를 만드는 설계도다.
- `class` 키워드로 클래스를 정의하고, 클래스명 뒤에 괄호 `()`를 붙여서 인스턴스(실제 객체)를 생성한다.
- 클래스명은 대문자로 시작하는 것이 관례다(예: `Dog`, `Order`, `Employee`).
- 클래스는 데이터와 기능을 하나로 묶는 도구다.

```python
class Cat:
    pass

myCat = Cat()
type(myCat)
```

**코드 읽는 순서**
1. `class Cat:` 으로 아무 내용도 없는 빈 클래스를 정의한다. `pass`는 "지금은 내용이 없다"는 뜻이다.
2. `Cat()`을 호출해서 `Cat` 클래스의 인스턴스를 하나 만들고 `myCat`에 대입한다.
3. `type(myCat)`을 실행하면 `myCat`이 어떤 클래스로부터 만들어졌는지 확인할 수 있다.
4. 마지막 줄을 실행하면 `<class '__main__.Cat'>`이 출력된다.

---

## 2. `__init__` 메서드 — 초기화 메서드

### 왜 배우는가
빈 클래스만으로는 인스턴스마다 서로 다른 데이터를 담을 수 없다. `__init__`은 인스턴스를 만드는 순간 "이 인스턴스는 어떤 초기 값을 가질지" 자동으로 정해주는 자리다. 실무에서는 객체를 생성하자마자 필요한 값(이름, 가격, 상태 등)을 바로 채워 넣을 때 반드시 쓰는 문법이다.

### 상세 설명
- `__init__` 메서드는 인스턴스가 생성될 때 자동으로 호출되는 특별한 메서드다.
- 인스턴스의 초기 속성값을 설정하는 데 사용한다.
- 첫 번째 매개변수는 항상 `self`다.
- `__init__`은 생성자처럼 동작하지만 엄밀히 말하면 "초기화 메서드"다.

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

pen = Product('Pen', 1500)
pen.name, pen.price
```

**코드 읽는 순서**
1. `Product('Pen', 1500)`을 호출하면 `__init__(self, name, price)`가 자동으로 실행된다.
2. 이때 `self`는 새로 만들어지는 인스턴스 자신이고, `name`에는 `'Pen'`, `price`에는 `1500`이 전달된다.
3. `self.name = name`, `self.price = price`로 이 값들이 인스턴스에 저장된다.
4. `pen.name, pen.price`를 실행하면 `('Pen', 1500)`이 출력된다.

---

## 3. `self` 이해하기 — 인스턴스 자신을 가리키는 참조

### 왜 배우는가
클래스 안에서 정의한 메서드가 "지금 이 인스턴스"의 데이터를 읽거나 바꾸려면, 그 인스턴스 자신을 가리킬 방법이 필요하다. `self`가 바로 그 역할을 한다. 이 개념을 정확히 이해해야 이후 인스턴스 메서드, 인스턴스 속성을 헷갈리지 않고 쓸 수 있다.

### 상세 설명
- `self`는 인스턴스 자신을 가리키는 참조다.
- 메서드의 첫 번째 매개변수로 `self`를 받고, `self.속성명`으로 인스턴스 속성에 접근한다.
- 메서드를 `인스턴스.메서드명()` 형태로 호출하면 `self`는 자동으로 전달되므로, 호출할 때는 `self`를 직접 넘기지 않는다.
- `self`는 관례적인 이름이며 다른 이름으로 바꿔도 문법적으로는 동작한다. 하지만 `self`를 사용하는 것이 표준이다.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increase(self):
        self.count = self.count + 1

c = Counter()
c.increase()
c.increase()
c.count
```

**코드 읽는 순서**
1. `Counter()`를 호출하면 `__init__`이 실행되어 `self.count`가 `0`으로 초기화된다.
2. `c.increase()`를 호출하면 `increase(self)`의 `self` 자리에 `c`가 자동으로 전달된다.
3. `self.count = self.count + 1`이 실행되어 `c.count`가 `1`이 된다.
4. `c.increase()`를 한 번 더 호출하면 `c.count`가 `2`가 된다.
5. 마지막 줄 `c.count`를 실행하면 `2`가 출력된다.

---

## 4. 인스턴스 메서드 — 클래스의 함수

### 왜 배우는가
클래스가 데이터만 가지고 있으면 그냥 자료 보관함일 뿐이다. 클래스 안에 "이 데이터로 무엇을 할지"에 해당하는 함수, 즉 메서드를 정의하면 데이터와 동작을 한 곳에서 관리할 수 있다. 실무에서는 이 메서드가 곧 그 객체가 "할 수 있는 일"을 정의한다.

### 상세 설명
- 인스턴스 메서드는 클래스 내부에 정의된 함수다.
- 첫 번째 매개변수로 `self`를 받고, `인스턴스.메서드명()`으로 호출한다.
- 메서드는 인스턴스의 속성을 읽거나 수정할 수 있다.
- 메서드는 인스턴스의 동작을 정의한다.

```python
class Speaker:
    def __init__(self, name):
        self.name = name

    def announce(self):
        return self.name + ' says hello'

s = Speaker('Robot')
s.announce()
```

**코드 읽는 순서**
1. `Speaker('Robot')`을 호출하면 `self.name`이 `'Robot'`로 저장된다.
2. `s.announce()`를 호출하면 `announce(self)`가 실행되고, `self`에는 `s`가 전달된다.
3. `self.name + ' says hello'`가 계산되어 `'Robot says hello'`가 반환된다.
4. 마지막 줄을 실행하면 `'Robot says hello'`가 출력된다.

---

## 5. 인스턴스 속성 — 인스턴스의 데이터

### 왜 배우는가
같은 클래스로 만든 인스턴스라도 이름이나 나이 같은 값은 서로 달라야 한다. 인스턴스 속성은 "같은 틀에서 나왔지만 서로 다른 값을 가지는" 데이터를 표현하는 방법이다. 실무 데이터 모델링에서 "개별 객체가 저마다 가지는 상태"를 다룰 때 기본이 되는 개념이다.

### 상세 설명
- 인스턴스 속성은 각 인스턴스가 가지는 고유한 데이터다.
- `self.속성명`으로 정의하고 접근한다.
- 같은 클래스로 여러 인스턴스를 만들어도, 각 인스턴스는 독립적인 속성값을 가진다.
- 속성은 인스턴스의 상태를 나타낸다.

```python
class Player:
    def __init__(self, name, level):
        self.name = name
        self.level = level

player1 = Player('Yuna', 12)
player2 = Player('Minho', 7)
player1.level, player2.level
```

**코드 읽는 순서**
1. `Player('Yuna', 12)`를 호출하면 `player1`은 `name='Yuna'`, `level=12`를 가진다.
2. `Player('Minho', 7)`을 호출하면 `player2`는 `name='Minho'`, `level=7`을 가진다.
3. 두 인스턴스는 같은 클래스에서 만들어졌지만 속성값은 서로 독립적이다.
4. `player1.level, player2.level`을 실행하면 `(12, 7)`이 출력된다.

---

## 6. 여러 인스턴스 — 클래스로부터 여러 객체 생성

### 왜 배우는가
클래스 하나만 잘 정의해두면, 그 뒤로는 같은 구조를 가진 객체를 필요한 만큼 계속 찍어낼 수 있다. 이것이 클래스가 "재사용 가능한 코드 템플릿"으로 불리는 이유다. 실무에서는 회원, 주문, 게시글처럼 같은 형태를 가진 데이터가 여러 개 있을 때 이 방식을 사용한다.

### 상세 설명
- 하나의 클래스로부터 여러 개의 인스턴스를 생성할 수 있다.
- 각 인스턴스는 같은 구조(같은 속성과 메서드)를 가지지만, 독립적인 데이터를 저장한다.
- 클래스는 설계도이고, 인스턴스는 그 설계도로 실제로 만들어진 객체다.
- 클래스는 재사용 가능한 코드 템플릿이다.

```python
class Book:
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages

book1 = Book('파이썬 입문', 320)
book2 = Book('데이터 분석 실무', 410)
book1.title, book2.title
```

**코드 읽는 순서**
1. `Book` 클래스 하나로 `book1`, `book2` 두 개의 인스턴스를 각각 만든다.
2. `book1`은 `title='파이썬 입문'`, `pages=320`을 가진다.
3. `book2`는 `title='데이터 분석 실무'`, `pages=410`을 가진다.
4. `book1.title, book2.title`을 실행하면 `('파이썬 입문', '데이터 분석 실무')`가 출력된다.

---

## 7. 현업 흐름 검증 — 장바구니 객체로 상태와 합계 관리하기

### 왜 배우는가
지금까지 배운 개념(`__init__`, `self`, 인스턴스 메서드, 인스턴스 속성)을 하나로 합쳐서, 실무에서 실제로 쓰는 형태에 가깝게 연습하는 단계다. 장바구니처럼 "여러 항목이 담기고, 합계가 계산되고, 결제 여부라는 상태가 바뀌는" 데이터는 딕셔너리 여러 개로 따로 관리하면 실수가 늘어난다. 클래스로 묶으면 관련 데이터와 규칙을 한 곳에서 안전하게 관리할 수 있다.

### 상세 설명
- 클래스의 핵심은 관련 데이터와 행동(메서드)을 한 곳에 묶는 것이다.
- 항목을 추가할 때 가격과 수량이 올바른지 그 자리에서 검증하면, 잘못된 데이터가 애초에 들어오지 못하게 막을 수 있다.
- `checkout()` 같은 메서드로 "결제 가능한 상태인지"를 판단하는 규칙을 클래스 안에 함께 두면, 이 규칙을 클래스 밖에서 매번 다시 작성할 필요가 없다.

```python
class Cart:
    def __init__(self, cartId):
        self.cartId = cartId
        self.items = []
        self.status = 'open'

    def addItem(self, name, price, quantity=1):
        if price <= 0:
            raise ValueError('price must be positive')
        if quantity <= 0:
            raise ValueError('quantity must be positive')

        self.items.append({
            'name': name,
            'price': price,
            'quantity': quantity,
        })

    def total(self):
        amount = 0
        for item in self.items:
            amount += item['price'] * item['quantity']
        return amount

    def checkout(self):
        if not self.items:
            raise ValueError('empty cart cannot be checked out')
        self.status = 'checked_out'

cartA = Cart('C-001')
cartB = Cart('C-002')

cartA.addItem('notebook', 3000, 2)
cartA.addItem('pen', 1500, 3)
cartB.addItem('bag', 25000, 1)

assert cartA.total() == 10500
assert cartB.total() == 25000
assert cartA.items != cartB.items

cartA.checkout()
assert cartA.status == 'checked_out'
assert cartB.status == 'open'

try:
    cartB.addItem('broken item', -500)
except ValueError as exc:
    assert 'price' in str(exc)

print('장바구니 객체 흐름 통과')
```

**코드 읽는 순서**
1. `Cart('C-001')`, `Cart('C-002')`로 서로 다른 장바구니 `cartA`, `cartB`를 각각 생성한다. 생성 시점에는 `items`가 빈 리스트, `status`가 `'open'`이다.
2. `cartA.addItem(...)`을 두 번 호출해서 `notebook`, `pen`을 담는다. 가격과 수량이 모두 양수라 에러 없이 `items` 리스트에 추가된다.
3. `cartB.addItem('bag', 25000, 1)`로 `bag` 하나를 담는다.
4. `cartA.total()`은 `3000×2 + 1500×3 = 10500`을 계산해서 반환하고, `cartB.total()`은 `25000`을 반환한다. `assert`로 두 값이 예상과 같은지 검증한다.
5. `cartA.items != cartB.items`로 두 인스턴스의 데이터가 서로 독립적임을 확인한다.
6. `cartA.checkout()`을 호출하면 `items`가 비어 있지 않으므로 `status`가 `'checked_out'`으로 바뀐다. `cartB`는 아직 `checkout()`을 호출하지 않았으므로 `status`가 그대로 `'open'`이다.
7. `cartB.addItem('broken item', -500)`은 가격이 음수라 `ValueError`가 발생하고, `except` 블록에서 에러 메시지 안에 `'price'`가 포함되어 있는지 확인한다.
8. 모든 `assert`를 통과하면 마지막 줄에서 `'장바구니 객체 흐름 통과'`가 출력된다.

---

## 8. Day 22 종합 복습 — 클래스 기초 마스터하기

### 왜 배우는가
1~7번에서 따로 배운 개념(클래스 정의, `__init__`, `self`, 인스턴스 메서드, 인스턴스 속성, 여러 인스턴스, 현업 흐름 검증)을 한 번에 복습하면서 스스로 얼마나 이해했는지 점검하는 단계다.

### 상세 설명
Day 22에서 배운 클래스 기초 문법을 난이도별로 복습한다. 기본 미션부터 시작해서 심화 미션까지 순서대로 도전할 수 있다. 각 미션은 서로 독립적으로 실행 가능하므로, 꼭 순서대로 풀지 않아도 괜찮다.

```python
class Robot:
    def __init__(self, name):
        self.name = name
        self.battery = 100

    def work(self, cost):
        if cost > self.battery:
            raise ValueError('not enough battery')
        self.battery -= cost
        return self.battery

# 기본 미션: 빈 클래스로 인스턴스 만들고 타입 확인하기
class Animal:
    pass

pet = Animal()
petType = type(pet).__name__

# 심화 미션: 배터리가 부족할 때 에러 메시지 확인하기
bot = Robot('R2')
bot.work(30)

try:
    bot.work(90)
except ValueError as exc:
    petType = str(exc)

petType
```

---

## 전체 핵심 요약

| 문법 | 언제 쓰는가 | 한 줄 요약 |
|---|---|---|
| `class` | 데이터와 기능을 하나로 묶고 싶을 때 | 객체를 만드는 설계도를 정의한다 |
| `__init__` | 인스턴스를 만들자마자 초기값을 채우고 싶을 때 | 생성 시점에 자동으로 실행되는 초기화 메서드다 |
| `self` | 메서드 안에서 "이 인스턴스 자신"을 가리키고 싶을 때 | 인스턴스 자신에 대한 참조다 |
| 인스턴스 메서드 | 인스턴스가 할 수 있는 동작을 정의하고 싶을 때 | 클래스 안에 정의한 함수다 |
| 인스턴스 속성 | 인스턴스마다 서로 다른 데이터를 저장하고 싶을 때 | `self.속성명`으로 저장하는 개별 데이터다 |
| 여러 인스턴스 | 같은 구조의 데이터가 여러 개 필요할 때 | 하나의 클래스로 여러 객체를 찍어낸다 |
| 클래스 + 검증 로직 | 상태가 바뀌고 규칙이 있는 데이터를 다룰 때 | 데이터와 규칙을 한 곳에서 안전하게 관리한다 |