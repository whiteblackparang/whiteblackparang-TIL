---
title: "24_파일입출력"
category: "Python"
date: 2026-07-12
tags: ["Python", "Function"]
---

## 파일입출력
파일입출력(File I/O)은 **데이터를 저장하고 다시 불러오는 핵심 기술**입니다.  
실무에서는 로그, 리포트, 데이터 저장 등 거의 모든 자동화에서 사용됩니다.

핵심 흐름:

- 입력값 정의 → 파일 열기 → 읽기/쓰기 → 결과 확인 → 재사용

---

#### 전체 흐름 (실무 기준)

1. 파일 경로 및 입력값 정의  
2. 파일 열기 (open / with)  
3. 읽기 또는 쓰기 실행  
4. 결과 검증 후 재사용 구조로 정리  

---

#### 1. 파일 열기 (open)

파일을 열고 작업할 준비를 합니다.

```python
f = open('test.txt', 'w')
f.write('Hello World')
f.close()

'File created'
```

✔ 핵심 포인트

open(파일경로, 모드)
반드시 close() 필요

---

#### 2. 파일 읽기 (read)

파일 내용을 가져옵니다.

```python
writer = open('data.txt', 'w')
writer.write('Line 1\nLine 2\nLine 3')
writer.close()

reader = open('data.txt', 'r')
text = reader.read()
reader.close()

text
```

주요 메서드
- read() → 전체 문자열
- readline() → 한 줄
- readlines() → 리스트

#### 3. 파일 쓰기 (write)

파일에 데이터를 저장합니다.

```python
fileObj = open('message.txt', 'w')
count = fileObj.write('Hello Python')
fileObj.close()

count  # 12
```

✔ 핵심 포인트

- 반환값 = 작성한 문자 수
- 'w' → 기존 내용 삭제 후 새로 작성

---

#### 4. with 문 (실무 표준)

파일을 자동으로 닫아줍니다 (가장 권장)

```python
with open('test.txt', 'w') as outFile:
    outFile.write('With statement')

with open('test.txt', 'r') as inFile:
    data = inFile.read()

data
```

✔ 장점

- close 자동 처리
- 예외 발생 시에도 안전

---

#### 5. 줄 단위 순회 (대용량 처리 핵심)

파일을 한 줄씩 읽습니다.

```python
with open('items.txt', 'w') as creator:
    creator.write('Apple\nBanana\nCherry')

with open('items.txt', 'r') as processor:
    lineCount = 0
    for line in processor:
        lineCount = lineCount + 1

lineCount  # 3
```

#### 파일 모드 

파일을 어떻게 열지 결정합니다.

#### 기본 모드 표

| 모드 | 의미 |
|------|------|
| r | 읽기 (파일 없으면 에러) |
| w | 쓰기 (기존 내용 삭제 후 새로 작성) |
| a | 추가 (기존 내용 유지 + 뒤에 이어쓰기) |
| r+ | 읽기 + 쓰기 (파일 존재해야 함) |

---

