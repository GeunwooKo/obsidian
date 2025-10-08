# Regex (정규표현식) 가이드

## 1. 정규표현식이란?

**정규표현식(Regular Expression, Regex)**는 문자열에서 특정 패턴을 찾거나 치환하기 위한 강력한 도구입니다.

### 주요 용도
- 텍스트 검색 및 추출
- 데이터 유효성 검증 (이메일, 전화번호 등)
- 문자열 치환 및 정제
- 로그 파일 분석
- 코드 리팩토링

---

## 2. 기본 문법

### 2.1 리터럴 문자

일반 문자는 그대로 매칭됩니다.

| 패턴 | 설명 | 매칭 예시 |
|------|------|-----------|
| `abc` | "abc" 문자열 매칭 | "abc", "abcdef" |
| `123` | "123" 숫자 매칭 | "123", "abc123" |

### 2.2 메타 문자 (특수 문자)

특별한 의미를 가진 문자들:

| 문자 | 의미 |
|------|------|
| `.` | 임의의 한 문자 (개행 제외) |
| `^` | 문자열의 시작 |
| `$` | 문자열의 끝 |
| `*` | 0회 이상 반복 |
| `+` | 1회 이상 반복 |
| `?` | 0회 또는 1회 |
| `\` | 이스케이프 문자 |
| `|` | OR 연산자 |
| `()` | 그룹화 |
| `[]` | 문자 클래스 |
| `{}` | 반복 횟수 지정 |

---

## 3. 문자 클래스 (Character Classes)

### 3.1 기본 문자 클래스

| 패턴 | 설명 | 예시 |
|------|------|------|
| `[abc]` | a, b, c 중 하나 | "a", "b", "c" |
| `[^abc]` | a, b, c가 아닌 문자 | "d", "1", "@" |
| `[a-z]` | 소문자 알파벳 | "a"~"z" |
| `[A-Z]` | 대문자 알파벳 | "A"~"Z" |
| `[0-9]` | 숫자 | "0"~"9" |
| `[a-zA-Z]` | 모든 알파벳 | "a"~"z", "A"~"Z" |
| `[a-zA-Z0-9]` | 알파벳+숫자 | "a", "Z", "5" |

### 3.2 미리 정의된 문자 클래스

| 패턴 | 설명 | 동등한 표현 |
|------|------|-------------|
| `\d` | 숫자 | `[0-9]` |
| `\D` | 숫자가 아닌 것 | `[^0-9]` |
| `\w` | 단어 문자 (알파벳, 숫자, _) | `[a-zA-Z0-9_]` |
| `\W` | 단어 문자가 아닌 것 | `[^a-zA-Z0-9_]` |
| `\s` | 공백 문자 (스페이스, 탭, 개행 등) | `[ \t\n\r\f\v]` |
| `\S` | 공백이 아닌 문자 | `[^ \t\n\r\f\v]` |

---

## 4. 수량자 (Quantifiers)

### 4.1 기본 수량자

| 패턴 | 설명 | 예시 |
|------|------|------|
| `*` | 0회 이상 | `a*` → "", "a", "aa", "aaa" |
| `+` | 1회 이상 | `a+` → "a", "aa", "aaa" |
| `?` | 0회 또는 1회 | `a?` → "", "a" |
| `{n}` | 정확히 n회 | `a{3}` → "aaa" |
| `{n,}` | n회 이상 | `a{2,}` → "aa", "aaa", "aaaa" |
| `{n,m}` | n회 이상 m회 이하 | `a{2,4}` → "aa", "aaa", "aaaa" |

### 4.2 탐욕적 vs 게으른 매칭

| 타입 | 패턴 | 설명 |
|------|------|------|
| **탐욕적** | `.*` | 가능한 한 많이 매칭 |
| **게으른** | `.*?` | 가능한 한 적게 매칭 |

**예시:**
```
문자열: <div>Hello</div><div>World</div>

<div>.*</div>   → <div>Hello</div><div>World</div>  (전체 매칭)
<div>.*?</div>  → <div>Hello</div>  (첫 번째만 매칭)
```

---

## 5. 앵커 (Anchors)

| 패턴 | 설명 | 예시 |
|------|------|------|
| `^` | 줄의 시작 | `^Hello` → "Hello world" (O), "Say Hello" (X) |
| `$` | 줄의 끝 | `end$` → "The end" (O), "end game" (X) |
| `\b` | 단어 경계 | `\bcat\b` → "cat" (O), "category" (X) |
| `\B` | 단어 경계 아님 | `\Bcat\B` → "concatenate" 중 "cat" |

---

## 6. 그룹화와 캡처

### 6.1 그룹 (Groups)

| 패턴 | 설명 | 예시 |
|------|------|------|
| `(abc)` | 캡처 그룹 | `(ab)+` → "ab", "abab" |
| `(?:abc)` | 비캡처 그룹 | `(?:ab)+` → "ab", "abab" (캡처 안 함) |
| `(a|b)` | OR 연산 | `(cat|dog)` → "cat" 또는 "dog" |

### 6.2 후방 참조 (Backreferences)

```regex
(\w+)\s+\1
```
- `(\w+)`: 첫 번째 단어 캡처
- `\s+`: 공백
- `\1`: 첫 번째 그룹과 동일한 단어

**매칭:** "hello hello", "test test"

---

## 7. 전방탐색과 후방탐색 (Lookahead & Lookbehind)

### 7.1 전방탐색 (Lookahead)

| 패턴 | 설명 | 예시 |
|------|------|------|
| `(?=...)` | 긍정 전방탐색 | `\d+(?=px)` → "100px"에서 "100" |
| `(?!...)` | 부정 전방탐색 | `\d+(?!px)` → "100em"에서 "100" |

### 7.2 후방탐색 (Lookbehind)

| 패턴 | 설명 | 예시 |
|------|------|------|
| `(?<=...)` | 긍정 후방탐색 | `(?<=\$)\d+` → "$100"에서 "100" |
| `(?<!...)` | 부정 후방탐색 | `(?<!\$)\d+` → "€100"에서 "100" |

---

## 8. 이스케이프 시퀀스

메타 문자를 리터럴로 사용하려면 `\`를 앞에 붙입니다.

| 패턴 | 설명 |
|------|------|
| `\.` | 마침표 자체 |
| `\*` | 별표 자체 |
| `\+` | 더하기 자체 |
| `\?` | 물음표 자체 |
| `\[` | 대괄호 자체 |
| `\(` | 괄호 자체 |
| `\\` | 백슬래시 자체 |

---

## 9. 플래그 (Flags)

### Python에서의 플래그

| 플래그 | 설명 | 사용법 |
|--------|------|--------|
| `re.IGNORECASE` 또는 `re.I` | 대소문자 무시 | `re.search(r'hello', text, re.I)` |
| `re.MULTILINE` 또는 `re.M` | 다중 행 모드 | `re.search(r'^start', text, re.M)` |
| `re.DOTALL` 또는 `re.S` | `.`이 개행 포함 | `re.search(r'a.b', 'a\nb', re.S)` |
| `re.VERBOSE` 또는 `re.X` | 가독성 모드 (주석, 공백 허용) | `re.compile(r'pattern', re.X)` |

---

## 10. Python에서 Regex 사용

### 10.1 기본 함수

```python
import re

# 검색 (첫 번째 매칭)
match = re.search(r'pattern', text)

# 검색 (시작부터 매칭)
match = re.match(r'pattern', text)

# 모든 매칭 찾기
matches = re.findall(r'pattern', text)

# 매칭된 객체 반환
matches = re.finditer(r'pattern', text)

# 치환
new_text = re.sub(r'pattern', 'replacement', text)

# 분할
parts = re.split(r'pattern', text)
```

### 10.2 예제 코드

```python
import re

# 1. 이메일 찾기
text = "Contact: john@example.com or support@test.org"
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
print(emails)  # ['john@example.com', 'support@test.org']

# 2. 전화번호 찾기
text = "Call 010-1234-5678 or 02-123-4567"
phones = re.findall(r'\d{2,3}-\d{3,4}-\d{4}', text)
print(phones)  # ['010-1234-5678', '02-123-4567']

# 3. 날짜 추출 (YYYY-MM-DD)
text = "Meeting on 2025-10-15 and 2025-11-20"
dates = re.findall(r'\d{4}-\d{2}-\d{2}', text)
print(dates)  # ['2025-10-15', '2025-11-20']

# 4. HTML 태그 제거
html = "<p>Hello <b>World</b></p>"
clean_text = re.sub(r'<[^>]+>', '', html)
print(clean_text)  # "Hello World"

# 5. 그룹 캡처
text = "Name: John Doe, Age: 30"
match = re.search(r'Name:\s*(\w+\s+\w+),\s*Age:\s*(\d+)', text)
if match:
    print(f"Name: {match.group(1)}")  # John Doe
    print(f"Age: {match.group(2)}")   # 30
```

---

## 11. 실전 패턴 모음

### 11.1 데이터 검증

```python
# 이메일
r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'

# URL
r'^https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&/=]*)$'

# IP 주소 (간단한 버전)
r'^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$'

# 한국 전화번호
r'^01[0-9]-\d{3,4}-\d{4}$'

# 비밀번호 (8자 이상, 영문+숫자+특수문자)
r'^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$'
```

### 11.2 텍스트 처리

```python
# 단어 추출
r'\b\w+\b'

# 숫자 추출 (정수)
r'-?\d+'

# 숫자 추출 (소수점 포함)
r'-?\d+\.?\d*'

# 공백 제거 (양쪽)
r'^\s+|\s+$'

# 연속된 공백을 하나로
r'\s+'

# 한글만 추출
r'[가-힣]+'
```

### 11.3 파일명/경로

```python
# 파일 확장자 추출
r'\.([a-zA-Z0-9]+)$'

# 파일명 (확장자 제외)
r'^(.+)\.([a-zA-Z0-9]+)$'

# Windows 경로
r'^[a-zA-Z]:\\(?:[^\\/:*?"<>|\r\n]+\\)*[^\\/:*?"<>|\r\n]*$'
```

---

## 12. 반도체/Klayout 관련 예제

### 12.1 좌표 추출

```python
import re

# GDS 좌표 패턴
gds_text = "BOUNDARY (100, 200) (300, 400) (500, 600)"
coords = re.findall(r'\((\d+),\s*(\d+)\)', gds_text)
print(coords)  # [('100', '200'), ('300', '400'), ('500', '600')]

# 좌표를 숫자로 변환
coords_int = [(int(x), int(y)) for x, y in coords]
```

### 12.2 레이어 정보 추출

```python
# Layer/Datatype 패턴
layer_text = "Layer: 10/0, Layer: 20/1, Layer: 30/2"
layers = re.findall(r'Layer:\s*(\d+)/(\d+)', layer_text)
print(layers)  # [('10', '0'), ('20', '1'), ('30', '2')]
```

### 12.3 측정 데이터 파싱

```python
# 두께/저항 측정값
data = """
Point1: Thickness=150.5nm, Rs=0.234ohm/sq
Point2: Thickness=148.3nm, Rs=0.241ohm/sq
Point3: Thickness=152.1nm, Rs=0.228ohm/sq
"""

pattern = r'Point\d+:\s*Thickness=([0-9.]+)nm,\s*Rs=([0-9.]+)ohm/sq'
measurements = re.findall(pattern, data)

for thickness, rs in measurements:
    print(f"Thickness: {thickness} nm, Rs: {rs} Ω/sq")
```

---

## 13. 주의사항 및 팁

### 13.1 성능 최적화

1. **컴파일된 패턴 재사용**
```python
pattern = re.compile(r'\d+')
# 여러 번 사용 시 효율적
for text in texts:
    matches = pattern.findall(text)
```

2. **불필요한 캡처 그룹 제거**
```python
# 나쁜 예: (abc)+
# 좋은 예: (?:abc)+  (캡처 불필요 시)
```

3. **백트래킹 최소화**
```python
# 나쁜 예: .*<div>
# 좋은 예: [^<]*<div>
```

### 13.2 디버깅

- **온라인 테스트 도구**: 
  - [regex101.com](https://regex101.com) (추천)
  - [regexr.com](https://regexr.com)
  - [regexpal.com](https://regexpal.com)

- **Python 디버깅**:
```python
import re

pattern = re.compile(r'\d+', re.DEBUG)
# 패턴 구조 출력
```

### 13.3 가독성

복잡한 패턴은 주석과 함께 작성:

```python
pattern = re.compile(r'''
    ^                   # 문자열 시작
    (?P<name>\w+)       # 이름 그룹
    \s*:\s*             # 콜론과 공백
    (?P<value>\d+)      # 값 그룹
    $                   # 문자열 끝
''', re.VERBOSE)
```

---

## 14. 연습 문제

### 문제 1: 이메일 추출
```python
text = "Contact us at info@example.com or support@test.co.kr"
# 모든 이메일 주소를 추출하세요
```

### 문제 2: 날짜 형식 변환
```python
text = "2025-10-06"
# YYYY-MM-DD → DD/MM/YYYY 형식으로 변환
```

### 문제 3: 로그 파싱
```python
log = "[2025-10-06 14:30:22] ERROR: Connection failed"
# 날짜, 시간, 레벨, 메시지를 각각 추출
```

<details>
<summary>정답 보기</summary>

```python
# 문제 1
import re
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)

# 문제 2
result = re.sub(r'(\d{4})-(\d{2})-(\d{2})', r'\3/\2/\1', text)

# 문제 3
pattern = r'\[(\d{4}-\d{2}-\d{2})\s+(\d{2}:\d{2}:\d{2})\]\s+(\w+):\s+(.+)'
match = re.search(pattern, log)
date, time, level, message = match.groups()
```
</details>

---

## 15. 참고 자료

### 온라인 도구
- [regex101.com](https://regex101.com) - 최고의 정규표현식 테스트 도구
- [regexr.com](https://regexr.com) - 시각적 설명 제공
- [regexper.com](https://regexper.com) - 정규표현식 다이어그램

### 문서
- [Python re 모듈 공식 문서](https://docs.python.org/3/library/re.html)
- [Regular-Expressions.info](https://www.regular-expressions.info/)

### 치트시트
- [Regex Cheat Sheet (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet)

---

**작성일**: 2025-10-06  
**버전**: 1.0  
**태그**: #regex #정규표현식 #python #텍스트처리
