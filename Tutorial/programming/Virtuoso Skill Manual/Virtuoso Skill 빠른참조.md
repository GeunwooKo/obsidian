# Virtuoso SKILL 빠른 참조 (Cheat Sheet)

## 기본 문법

### 변수
```skill
; 전역 변수
myVar = 10

; 지역 변수
let((x y)
    x = 10
    y = 20
)
```

### 데이터 타입
```skill
int = 100
float = 3.14
string = "Hello"
bool = t              ; true
bool = nil            ; false
list = '(1 2 3)
point = 10:20         ; 좌표
```

### 연산자
```skill
; 산술
x + y, x - y, x * y, x / y, x % y

; 비교
(equal x y), (lessp x y), (greaterp x y)

; 논리
(and x y), (or x y), (not x)
```

---

## 제어 구조

### 조건문
```skill
; if-then-else
if(condition then
    action1
else
    action2
)

; when (if without else)
when(condition
    action
)

; case
case(value
    (1 action1)
    (2 action2)
    (t defaultAction)
)
```

### 반복문
```skill
; for
for(i 0 10
    println(i)
)

; while
while(condition
    action
)

; foreach
foreach(item list
    println(item)
)
```

---

## 리스트 함수

```skill
car(list)              ; 첫 번째 요소
cdr(list)              ; 나머지
cons(item list)        ; 앞에 추가
append(list1 list2)    ; 연결
length(list)           ; 길이
nth(n list)            ; n번째 요소 (0부터)
reverse(list)          ; 역순
member(item list)      ; 포함 여부
mapcar(func list)      ; 각 요소에 함수 적용
```

---

## 문자열 함수

```skill
strcat(str1 str2)           ; 연결
strlen(str)                 ; 길이
substring(str start end)    ; 부분 문자열
parseString(str)            ; 공백으로 분리
sprintf(nil "fmt" args)     ; 포맷팅
atoi(str)                   ; 문자열 → 정수
atof(str)                   ; 문자열 → 실수
upperCase(str)              ; 대문자
lowerCase(str)              ; 소문자
```

---

## 수학 함수

```skill
abs(x)                 ; 절대값
sqrt(x)                ; 제곱근
pow(x y)               ; x^y
exp(x)                 ; e^x
log(x)                 ; ln(x)
log10(x)               ; log10(x)
sin(x), cos(x), tan(x) ; 삼각함수 (라디안)
asin(x), acos(x), atan(x)
ceil(x), floor(x), round(x)
min(x y), max(x y)
pi                     ; 3.14159...
```

---

## 파일 I/O

```skill
; 읽기
port = infile("file.txt")
line = gets(buffer port)
close(port)

; 쓰기
port = outfile("file.txt" "w")
fprintf(port "text\n")
close(port)

; 파일 존재 확인
isFile("filename")
```

---

## 데이터베이스 함수

### 셀뷰 열기/닫기
```skill
cvId = dbOpenCellViewByType(
    "libName"          ; 라이브러리
    "cellName"         ; 셀
    "layout"           ; 뷰
    nil                ; 모드
    "a"                ; 접근 (r/w/a)
)

dbSave(cvId)
dbClose(cvId)
dbRevert(cvId)         ; 변경사항 취소
```

### 도형 생성
```skill
; 사각형
rectId = dbCreateRect(
    cvId
    list(layerNum "drawing")
    list(x1:y1 x2:y2)
)

; 다각형
polyId = dbCreatePolygon(
    cvId
    list(layerNum "drawing")
    list(pt1 pt2 pt3 ...)
)

; 경로
pathId = dbCreatePath(
    cvId
    list(layerNum "drawing")
    list(pt1 pt2 pt3)
    width
)

; 라벨
labelId = dbCreateLabel(
    cvId
    list(layerNum "drawing")
    x:y
    "text"
    "centerCenter"     ; 정렬
    "R0"               ; 회전
    "roman"            ; 폰트
    1.0                ; 높이
)
```

### 인스턴스
```skill
; 인스턴스 배치
instId = dbCreateInst(
    cvId               ; 현재 셀뷰
    masterCvId         ; 마스터 셀
    nil                ; 이름
    x:y                ; 위치
    "R0"               ; 회전/반전
)

; 회전/반전 옵션
; R0, R90, R180, R270
; MX (X축 대칭), MY (Y축 대칭)
; MXR90, MYR90
```

### 도형 조작
```skill
; 이동
dbMoveFig(figId cvId dx:dy)

; 회전
dbRotateFig(figId cvId origin angle)

; 복사
newFig = dbCopyFig(figId cvId offset)

; 삭제
dbDeleteObject(figId)

; 변환
transform = dbCreateTransform(
    offset             ; x:y
    orient             ; "R0", "R90", etc.
    scale              ; 1.0
)
dbTransformFig(figId transform)
```

### 객체 정보
```skill
; 바운딩 박스
bbox = dbGetBBox(figId)
lowerLeft = car(bbox)
upperRight = cadr(bbox)

; 전체 바운딩 박스
bbox = dbComputeBBox(cvId)

; 객체 타입
type = dbGetObjectType(figId)
; "rect", "polygon", "path", "label", etc.

; 레이어 정보
layerNum = figId~>layerNum
purpose = figId~>purpose
```

### 순회
```skill
; 모든 도형
foreach(shape cvId~>shapes
    ; 처리
)

; 모든 인스턴스
foreach(inst cvId~>instances
    ; 처리
)

; 특정 레이어의 도형
foreach(shape cvId~>shapes
    when(shape~>layerNum == 10
        ; 처리
    )
)
```

---

## 좌표 함수

```skill
; 좌표 생성
pt = x:y
pt = 10.5:20.3

; 좌표 추출
x = xCoord(pt)
y = yCoord(pt)

; 좌표 연산
pt3 = pt1 + pt2
pt3 = pt1 - pt2

; 거리
dist = dbDistance(pt1 pt2)

; 좌표 변환
newPt = dbTransformPoint(pt transform)
```

---

## 함수 정의

### 기본 함수
```skill
procedure(functionName(arg1 arg2)
    ; 함수 본문
    result
)
```

### 선택적 인자
```skill
procedure(func(@optional (arg 기본값))
    ; 본문
)
```

### 키워드 인자
```skill
procedure(func(@key (width 1.0) (height 2.0))
    ; 본문
)

; 호출
func(?width 3.0 ?height 4.0)
```

### 가변 인자
```skill
procedure(func(@rest args)
    ; args는 리스트
)
```

---

## 유용한 패턴

### 안전한 파일 작업
```skill
let((cvId success)
    cvId = dbOpenCellViewByType(...)
    
    success = errset(
        ; 작업 수행
        ; ...
        dbSave(cvId)
        t
    )
    
    unless(success
        dbRevert(cvId)
    )
    
    dbClose(cvId)
)
```

### 진행 상황 표시
```skill
let((total current)
    total = length(items)
    current = 0
    
    foreach(item items
        current = current + 1
        printf("\r진행: %d/%d" current total)
        ; 처리
    )
    printf("\n")
)
```

### 해시 테이블
```skill
table = makeTable('myTable nil)
table["key"] = value
value = table["key"]

; 순회
foreach(key table
    printf("%s: %L\n" key table[key])
)
```

### 람다 함수
```skill
; 익명 함수
lambda((x) x * 2)

; 사용 예
result = mapcar(
    lambda((x) x * x)
    list(1 2 3 4 5)
)
; result = (1 4 9 16 25)
```

---

## 디버깅

### 출력
```skill
println(value)
printf("format %d\n" value)
printf("List: %L\n" myList)
```

### 타입 확인
```skill
type(value)
; "fixnum", "flonum", "string", "list", etc.
```

### 에러 처리
```skill
errset(
    ; 위험한 코드
    t
)

; 에러 발생
error("메시지")
```

### 스택 트레이스
```skill
errTrace()
```

---

## 자주 사용하는 스니펫

### 레이어 정의
```skill
METAL1 = list(40 "drawing")
METAL2 = list(41 "drawing")
VIA1 = list(50 "drawing")
```

### 그리드 스냅
```skill
procedure(snapToGrid(value grid)
    round(value / grid) * grid
)
```

### 영역 체크
```skill
procedure(pointInBox(pt bbox)
    let((x y x1 y1 x2 y2)
        x = xCoord(pt)
        y = yCoord(pt)
        x1 = xCoord(car(bbox))
        y1 = yCoord(car(bbox))
        x2 = xCoord(cadr(bbox))
        y2 = yCoord(cadr(bbox))
        
        x >= x1 && x <= x2 && y >= y1 && y <= y2
    )
)
```

### 도형 면적
```skill
procedure(getRectArea(rectId)
    let((bbox w h)
        bbox = dbGetBBox(rectId)
        w = xCoord(cadr(bbox)) - xCoord(car(bbox))
        h = yCoord(cadr(bbox)) - yCoord(car(bbox))
        w * h
    )
)
```

---

## 성능 팁

### DO
```skill
; 한 번만 열고 닫기
cvId = dbOpenCellViewByType(...)
; 여러 작업
dbSave(cvId)
dbClose(cvId)

; apply 사용
total = apply(plus numList)

; 배치 처리
dbBeginTransaction(cvId)
; 여러 작업
dbEndTransaction(cvId)
```

### DON'T
```skill
; 반복적으로 열고 닫기 (느림)
for(i 1 100
    cvId = dbOpenCellViewByType(...)
    ; 작업
    dbClose(cvId)
)

; 반복문으로 합계 (느림)
total = 0
foreach(num numList
    total = total + num
)
```

---

## 단축키 (CIW)

```
Ctrl+L      : 화면 지우기
Up/Down     : 명령 히스토리
Tab         : 자동 완성
```

---

## 자주 발생하는 에러

**"unbound variable"**
→ 변수가 정의되지 않음. let으로 선언 필요.

**"bad argument type"**
→ 함수에 잘못된 타입 전달. 타입 변환 필요.

**"attempt to apply non-procedure"**
→ 함수가 아닌 것을 함수처럼 호출.

**"nil isn't a valid cellview"**
→ 셀뷰를 열지 못함. 라이브러리/셀/뷰 이름 확인.

---

## 유용한 링크

- [[Virtuoso Skill 가이드]] - 기본 문법 상세
- [[Virtuoso Skill 고급 기법]] - 고급 기능
- [[Virtuoso Skill 실전 레시피]] - 실용 예제

---

## 빠른 시작 템플릿

```skill
/* ==========================================
 * 파일명: myScript.il
 * 작성자: Your Name
 * 날짜: 2025-10-14
 * 설명: 스크립트 설명
 * ==========================================
 */

; ===== 상수 정의 =====
let((LAYER1 LAYER2)
    LAYER1 = list(10 "drawing")
    LAYER2 = list(20 "drawing")
    
    ; ===== 유틸리티 함수 =====
    procedure(myUtil(param)
        ; 구현
    )
    
    ; ===== 메인 함수 =====
    procedure(main()
        let((cvId)
            ; 셀뷰 열기
            cvId = dbOpenCellViewByType(
                "myLib"
                "myCell"
                "layout"
                nil
                "a"
            )
            
            ; 작업 수행
            when(cvId
                ; 여기에 코드 작성
                
                ; 저장 및 닫기
                dbSave(cvId)
                dbClose(cvId)
                
                printf("완료!\n")
            )
        )
    )
    
    ; ===== 테스트 =====
    procedure(test()
        printf("테스트 시작...\n")
        ; 테스트 코드
        printf("테스트 완료\n")
    )
)

; 스크립트 로드 시 메시지
printf("myScript.il 로드됨\n")
printf("사용법: main() 또는 test()\n")
```

---

**인쇄용 팁**: 이 문서를 PDF로 저장하여 책상에 두고 참고하세요!
