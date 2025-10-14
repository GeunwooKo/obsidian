# Python 기초 가이드 (반도체 엔지니어용)

## 목차
- [[#Python 시작하기]]
- [[#기본 문법]]
- [[#데이터 타입]]
- [[#제어 구조]]
- [[#함수]]
- [[#파일 처리]]
- [[#모듈과 패키지]]
- [[#반도체 업무 예제]]

---

## Python 시작하기

### Python이란?

Python은 배우기 쉽고 강력한 프로그래밍 언어입니다.

**반도체 업무에서의 활용:**
- 데이터 분석 및 시각화
- 자동화 스크립트
- 테스트 데이터 처리
- 통계 분석
- 머신러닝

### 설치

```bash
# Anaconda 설치 (권장)
# https://www.anaconda.com/download

# Python 버전 확인
python --version

# pip 업그레이드
pip install --upgrade pip
```

### 첫 프로그램

```python
# hello.py
print("Hello, Semiconductor!")

# 실행
# python hello.py
```

---

## 기본 문법

### 주석

```python
# 단일 행 주석

"""
여러 줄 주석
(Docstring)
"""

'''
이것도 여러 줄 주석
'''
```

### 변수

```python
# 변수 선언 (타입 지정 불필요)
wafer_id = "W001"
yield_value = 95.5
is_passed = True

# 여러 변수 동시 할당
x, y, z = 1, 2, 3

# 같은 값 할당
a = b = c = 0
```

### 출력

```python
# 기본 출력
print("Hello")

# 여러 값 출력
print("Wafer:", wafer_id, "Yield:", yield_value)

# f-string (Python 3.6+)
print(f"Wafer {wafer_id} has yield {yield_value}%")

# format 메서드
print("Wafer {} has yield {:.2f}%".format(wafer_id, yield_value))
```

### 입력

```python
# 사용자 입력
name = input("이름을 입력하세요: ")

# 숫자 입력
number = int(input("숫자를 입력하세요: "))
```

---

## 데이터 타입

### 숫자

```python
# 정수 (int)
wafer_count = 25
lot_number = 1001

# 실수 (float)
yield_rate = 95.5
temperature = 850.0

# 복소수
complex_num = 3 + 4j

# 연산
a = 10 + 5      # 15 (덧셈)
b = 10 - 5      # 5 (뺄셈)
c = 10 * 5      # 50 (곱셈)
d = 10 / 5      # 2.0 (나눗셈)
e = 10 // 3     # 3 (몫)
f = 10 % 3      # 1 (나머지)
g = 10 ** 2     # 100 (거듭제곱)
```

### 문자열

```python
# 문자열 생성
wafer_id = "W001"
lot_id = 'LOT2025001'
long_text = """여러 줄
문자열"""

# 문자열 연산
full_id = wafer_id + "_" + lot_id  # "W001_LOT2025001"
repeat = "AB" * 3                   # "ABABAB"

# 문자열 메서드
text = "  Hello World  "
print(text.strip())         # "Hello World" (공백 제거)
print(text.upper())         # "  HELLO WORLD  "
print(text.lower())         # "  hello world  "
print(text.replace("World", "Python"))  # "  Hello Python  "

# 문자열 분할
csv_line = "W001,95.5,PASS"
parts = csv_line.split(",")  # ['W001', '95.5', 'PASS']

# 문자열 포맷팅
wafer = "W001"
yield_val = 95.5
result = f"Wafer {wafer}: {yield_val:.2f}%"
```

### 리스트 (List)

```python
# 리스트 생성
wafers = ["W001", "W002", "W003"]
yields = [95.5, 96.2, 94.8]
mixed = [1, "text", 3.14, True]

# 인덱싱 (0부터 시작)
first = wafers[0]    # "W001"
last = wafers[-1]    # "W003"

# 슬라이싱
subset = wafers[0:2]  # ["W001", "W002"]
subset = wafers[:2]   # ["W001", "W002"]
subset = wafers[1:]   # ["W002", "W003"]

# 리스트 수정
wafers.append("W004")           # 끝에 추가
wafers.insert(0, "W000")        # 특정 위치에 삽입
wafers.remove("W002")           # 값으로 제거
popped = wafers.pop()           # 마지막 요소 제거 및 반환
wafers.pop(0)                   # 특정 인덱스 제거

# 리스트 연산
combined = wafers + yields      # 연결
sorted_yields = sorted(yields)  # 정렬 (새 리스트)
yields.sort()                   # 제자리 정렬
yields.reverse()                # 역순

# 리스트 메서드
length = len(wafers)            # 길이
count = wafers.count("W001")    # 특정 값 개수
index = wafers.index("W002")    # 특정 값의 인덱스
```

### 튜플 (Tuple)

```python
# 튜플 생성 (불변)
coordinates = (10, 20)
wafer_info = ("W001", 95.5, True)

# 인덱싱
x = coordinates[0]   # 10
y = coordinates[1]   # 20

# 언패킹
wafer_id, yield_val, passed = wafer_info
```

### 딕셔너리 (Dictionary)

```python
# 딕셔너리 생성
wafer_data = {
    "wafer_id": "W001",
    "yield": 95.5,
    "lot_id": "LOT001",
    "status": "PASS"
}

# 접근
wafer_id = wafer_data["wafer_id"]
yield_val = wafer_data.get("yield", 0)  # 기본값 지정

# 수정
wafer_data["yield"] = 96.0
wafer_data["equipment"] = "EQ001"

# 제거
del wafer_data["status"]
removed = wafer_data.pop("equipment")

# 딕셔너리 메서드
keys = wafer_data.keys()        # 키 목록
values = wafer_data.values()    # 값 목록
items = wafer_data.items()      # (키, 값) 쌍

# 순회
for key, value in wafer_data.items():
    print(f"{key}: {value}")
```

### 세트 (Set)

```python
# 세트 생성 (중복 없음)
equipment_ids = {"EQ001", "EQ002", "EQ003"}
lot_ids = set(["LOT001", "LOT002", "LOT001"])  # {"LOT001", "LOT002"}

# 세트 연산
set1 = {1, 2, 3}
set2 = {3, 4, 5}

union = set1 | set2         # {1, 2, 3, 4, 5} (합집합)
intersection = set1 & set2  # {3} (교집합)
difference = set1 - set2    # {1, 2} (차집합)

# 세트 메서드
equipment_ids.add("EQ004")
equipment_ids.remove("EQ001")
```

---

## 제어 구조

### if 문

```python
# 기본 if
yield_value = 95.5

if yield_value >= 95:
    print("PASS")

# if-else
if yield_value >= 95:
    print("PASS")
else:
    print("FAIL")

# if-elif-else
if yield_value >= 98:
    grade = "A"
elif yield_value >= 95:
    grade = "B"
elif yield_value >= 90:
    grade = "C"
else:
    grade = "D"

# 조건 연산자
status = "PASS" if yield_value >= 95 else "FAIL"
```

### for 문

```python
# 리스트 순회
wafers = ["W001", "W002", "W003"]
for wafer in wafers:
    print(wafer)

# 인덱스와 함께
for i, wafer in enumerate(wafers):
    print(f"{i}: {wafer}")

# 범위
for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):       # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 10, 2):   # 0, 2, 4, 6, 8
    print(i)

# 딕셔너리 순회
wafer_data = {"wafer_id": "W001", "yield": 95.5}
for key, value in wafer_data.items():
    print(f"{key}: {value}")

# 리스트 컴프리헨션
squares = [x**2 for x in range(10)]
even_nums = [x for x in range(10) if x % 2 == 0]
```

### while 문

```python
# 기본 while
count = 0
while count < 5:
    print(count)
    count += 1

# break와 continue
i = 0
while True:
    if i >= 10:
        break  # 루프 종료
    if i % 2 == 0:
        i += 1
        continue  # 다음 반복으로
    print(i)
    i += 1
```

---

## 함수

### 기본 함수

```python
# 함수 정의
def greet(name):
    print(f"Hello, {name}!")

# 함수 호출
greet("World")

# 반환값
def add(a, b):
    return a + b

result = add(3, 5)  # 8

# 여러 반환값
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers)

min_val, max_val, total = get_stats([1, 2, 3, 4, 5])
```

### 매개변수

```python
# 기본값
def calculate_yield(good, total=25):
    return (good / total) * 100

yield1 = calculate_yield(24)      # total=25
yield2 = calculate_yield(24, 30)  # total=30

# 키워드 인자
def create_wafer(wafer_id, lot_id, yield_value):
    return {
        "wafer_id": wafer_id,
        "lot_id": lot_id,
        "yield": yield_value
    }

wafer = create_wafer(wafer_id="W001", yield_value=95.5, lot_id="LOT001")

# 가변 인자
def calculate_average(*numbers):
    return sum(numbers) / len(numbers)

avg = calculate_average(1, 2, 3, 4, 5)

# 가변 키워드 인자
def create_report(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

create_report(wafer_id="W001", yield=95.5, status="PASS")
```

### Lambda 함수

```python
# Lambda (익명 함수)
square = lambda x: x ** 2
print(square(5))  # 25

add = lambda x, y: x + y
print(add(3, 5))  # 8

# map 함수와 함께
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, numbers))

# filter 함수와 함께
even_nums = list(filter(lambda x: x % 2 == 0, numbers))

# sorted와 함께
wafers = [
    {"id": "W001", "yield": 95.5},
    {"id": "W002", "yield": 96.2},
    {"id": "W003", "yield": 94.8}
]
sorted_wafers = sorted(wafers, key=lambda w: w["yield"], reverse=True)
```

---

## 파일 처리

### 텍스트 파일

```python
# 파일 쓰기
with open("output.txt", "w") as f:
    f.write("Hello World\n")
    f.write("Line 2\n")

# 파일 읽기
with open("data.txt", "r") as f:
    content = f.read()  # 전체 읽기

# 줄 단위 읽기
with open("data.txt", "r") as f:
    for line in f:
        print(line.strip())

# 모든 줄을 리스트로
with open("data.txt", "r") as f:
    lines = f.readlines()
```

### CSV 파일

```python
import csv

# CSV 쓰기
data = [
    ["wafer_id", "yield", "status"],
    ["W001", "95.5", "PASS"],
    ["W002", "96.2", "PASS"],
    ["W003", "94.8", "FAIL"]
]

with open("wafers.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerows(data)

# CSV 읽기
with open("wafers.csv", "r") as f:
    reader = csv.reader(f)
    header = next(reader)  # 헤더
    for row in reader:
        print(row)

# 딕셔너리로 읽기
with open("wafers.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(f"Wafer {row['wafer_id']}: {row['yield']}%")
```

---

## 모듈과 패키지

### 모듈 import

```python
# 전체 모듈 import
import math
print(math.sqrt(16))  # 4.0

# 특정 함수 import
from math import sqrt, pi
print(sqrt(16))  # 4.0
print(pi)        # 3.141592...

# 별칭
import numpy as np
import pandas as pd

# 모든 것 import (권장하지 않음)
from math import *
```

### 자주 쓰는 내장 모듈

```python
# os - 운영체제 관련
import os
print(os.getcwd())              # 현재 디렉토리
os.listdir(".")                 # 파일 목록
os.path.exists("file.txt")      # 파일 존재 확인

# datetime - 날짜/시간
from datetime import datetime, timedelta
now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M:%S"))
tomorrow = now + timedelta(days=1)

# random - 난수
import random
print(random.random())          # 0~1 실수
print(random.randint(1, 10))    # 1~10 정수
print(random.choice([1,2,3]))   # 리스트에서 선택

# json - JSON 처리
import json
data = {"wafer_id": "W001", "yield": 95.5}
json_str = json.dumps(data)     # 딕셔너리 → JSON
data_back = json.loads(json_str)  # JSON → 딕셔너리
```

---

## 반도체 업무 예제

### 예제 1: 수율 계산

```python
def calculate_yield(good_dies, total_dies):
    """수율 계산"""
    if total_dies == 0:
        return 0
    return (good_dies / total_dies) * 100

# 사용
good = 240
total = 250
yield_value = calculate_yield(good, total)
print(f"수율: {yield_value:.2f}%")
```

### 예제 2: 웨이퍼 데이터 처리

```python
# 웨이퍼 데이터
wafers = [
    {"id": "W001", "yield": 95.5, "defects": 5},
    {"id": "W002", "yield": 96.2, "defects": 3},
    {"id": "W003", "yield": 94.8, "defects": 8},
    {"id": "W004", "yield": 97.1, "defects": 2}
]

# 평균 수율
total_yield = sum(w["yield"] for w in wafers)
avg_yield = total_yield / len(wafers)
print(f"평균 수율: {avg_yield:.2f}%")

# 수율이 95% 이상인 웨이퍼
good_wafers = [w for w in wafers if w["yield"] >= 95]
print(f"우수 웨이퍼: {len(good_wafers)}개")

# 불량 개수별 정렬
sorted_wafers = sorted(wafers, key=lambda w: w["defects"])
```

### 예제 3: CSV 데이터 분석

```python
import csv

# CSV 파일 읽기 및 분석
def analyze_wafer_data(filename):
    yields = []
    
    with open(filename, "r") as f:
        reader = csv.DictReader(f)
        for row in reader:
            yields.append(float(row["yield"]))
    
    # 통계 계산
    avg = sum(yields) / len(yields)
    min_val = min(yields)
    max_val = max(yields)
    
    return {
        "average": avg,
        "min": min_val,
        "max": max_val,
        "count": len(yields)
    }

# 사용
stats = analyze_wafer_data("wafer_data.csv")
print(f"평균: {stats['average']:.2f}%")
print(f"최소: {stats['min']:.2f}%")
print(f"최대: {stats['max']:.2f}%")
```

### 예제 4: 데이터 필터링

```python
# 측정 데이터
measurements = [
    {"date": "2025-10-01", "value": 850.5},
    {"date": "2025-10-02", "value": 851.2},
    {"date": "2025-10-03", "value": 849.8},
    {"date": "2025-10-04", "value": 852.1},
    {"date": "2025-10-05", "value": 850.0}
]

# 범위 필터링
def filter_by_range(data, min_val, max_val):
    return [d for d in data if min_val <= d["value"] <= max_val]

# 사용
filtered = filter_by_range(measurements, 850, 851)
print(f"범위 내 데이터: {len(filtered)}개")
```

### 예제 5: 보고서 생성

```python
def generate_report(wafer_data):
    """웨이퍼 데이터 보고서 생성"""
    
    # 통계 계산
    yields = [w["yield"] for w in wafer_data]
    avg_yield = sum(yields) / len(yields)
    max_yield = max(yields)
    min_yield = min(yields)
    
    # 등급 분류
    grade_a = len([w for w in wafer_data if w["yield"] >= 98])
    grade_b = len([w for w in wafer_data if 95 <= w["yield"] < 98])
    grade_c = len([w for w in wafer_data if w["yield"] < 95])
    
    # 보고서 작성
    report = f"""
=== 웨이퍼 분석 보고서 ===
총 웨이퍼 수: {len(wafer_data)}개
평균 수율: {avg_yield:.2f}%
최대 수율: {max_yield:.2f}%
최소 수율: {min_yield:.2f}%

등급 분류:
  Grade A (≥98%): {grade_a}개
  Grade B (95-98%): {grade_b}개
  Grade C (<95%): {grade_c}개
========================
"""
    return report

# 사용
wafers = [
    {"id": "W001", "yield": 95.5},
    {"id": "W002", "yield": 98.2},
    {"id": "W003", "yield": 94.8},
    {"id": "W004", "yield": 97.1}
]

print(generate_report(wafers))
```

---

## 예외 처리

```python
# try-except
try:
    result = 10 / 0
except ZeroDivisionError:
    print("0으로 나눌 수 없습니다")

# 여러 예외 처리
try:
    with open("data.txt", "r") as f:
        data = f.read()
        value = int(data)
except FileNotFoundError:
    print("파일을 찾을 수 없습니다")
except ValueError:
    print("숫자로 변환할 수 없습니다")

# finally
try:
    f = open("data.txt", "r")
    data = f.read()
except FileNotFoundError:
    print("파일 없음")
finally:
    f.close()  # 항상 실행
```

---

## 유용한 팁

### 문자열 포맷팅

```python
wafer_id = "W001"
yield_val = 95.567

# f-string (Python 3.6+, 권장)
print(f"{wafer_id}: {yield_val:.2f}%")

# format 메서드
print("{}: {:.2f}%".format(wafer_id, yield_val))

# % 포맷팅 (구식)
print("%s: %.2f%%" % (wafer_id, yield_val))
```

### 리스트 다루기

```python
# 리스트 합치기
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2

# 리스트 펼치기
nested = [[1, 2], [3, 4], [5, 6]]
flat = [item for sublist in nested for item in sublist]

# zip으로 묶기
ids = ["W001", "W002", "W003"]
yields = [95.5, 96.2, 94.8]
paired = list(zip(ids, yields))
```

### 파일 경로 처리

```python
import os

# 경로 조합
path = os.path.join("data", "wafers", "2025.csv")

# 경로 분리
directory = os.path.dirname(path)
filename = os.path.basename(path)

# 확장자 분리
name, ext = os.path.splitext(filename)
```

---

## 관련 문서

- [[Python 데이터 분석 가이드]] - Pandas, NumPy
- [[Python + SQL 연동 가이드]] - 데이터베이스 연동
- [[SciPy 가이드]] - 과학 계산

---

**Python으로 반도체 업무를 자동화하세요!** 🐍
