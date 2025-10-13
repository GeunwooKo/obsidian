# Virtuoso SKILL 프로그래밍 가이드

## 목차
- [[#SKILL 언어 소개]]
- [[#기본 문법]]
- [[#데이터 타입]]
- [[#제어 구조]]
- [[#함수 정의]]
- [[#레이아웃 관련 함수]]
- [[#실용 예제]]
- [[#디버깅]]

---

## SKILL 언어 소개

SKILL은 Cadence Virtuoso에서 사용하는 LISP 기반의 프로그래밍 언어입니다.

### 특징
- LISP 계열 언어 (괄호 사용)
- 인터프리터 방식
- Virtuoso의 모든 기능에 접근 가능
- 레이아웃 자동화, 검증, 분석에 활용

### SKILL 코드 실행 방법

**1. CIW (Command Interpreter Window)**
```skill
; CIW에서 직접 입력 후 Enter
println("Hello SKILL")
```

**2. SKILL 파일 로드**
```skill
; 파일 로드
load("myScript.il")

; 또는 Virtuoso 메뉴에서
; Tools → SKILL Development → Load...
```

**3. Virtuoso 시작 시 자동 로드**
```bash
# .cdsinit 파일에 추가
load("~/skill/myLibrary.il")
```

---

## 기본 문법

### 주석

```skill
; 한 줄 주석

/* 
 * 여러 줄 주석
 * 블록 주석
 */
```

### 변수 선언 및 할당

```skill
; 전역 변수
myVar = 10

; 지역 변수 (let 사용)
let((x y z)
    x = 10
    y = 20
    z = x + y
    println(z)  ; 30 출력
)

; 변수 타입
intVar = 100              ; 정수
floatVar = 3.14           ; 실수
stringVar = "Hello"       ; 문자열
boolVar = t               ; 불리언 (t = true, nil = false)
```

### 산술 연산

```skill
; 기본 연산
sum = 10 + 5              ; 15
diff = 10 - 5             ; 5
prod = 10 * 5             ; 50
quot = 10 / 5             ; 2
remainder = 10 % 3        ; 1

; 함수 형태 (LISP 스타일)
(plus 10 5)               ; 15
(difference 10 5)         ; 5
(times 10 5)              ; 50
(quotient 10 5)           ; 2
(remainder 10 3)          ; 1

; 비교 연산
(equal 10 10)             ; t (true)
(lessp 5 10)              ; t
(greaterp 10 5)           ; t
```

---

## 데이터 타입

### 리스트

```skill
; 리스트 생성
myList = list(1 2 3 4 5)
myList2 = '(a b c d e)

; 리스트 접근
car(myList)               ; 첫 번째 요소: 1
cdr(myList)               ; 나머지: (2 3 4 5)
nth(2 myList)             ; 세 번째 요소: 3 (0부터 시작)

; 리스트 조작
append(list(1 2) list(3 4))    ; (1 2 3 4)
cons(0 myList)                  ; (0 1 2 3 4 5)
length(myList)                  ; 5

; 리스트 순회
foreach(item myList
    println(item)
)
```

### 문자열

```skill
; 문자열 생성
str1 = "Hello"
str2 = "World"

; 문자열 연결
strcat(str1 " " str2)     ; "Hello World"

; 문자열 길이
strlen(str1)              ; 5

; 문자열 비교
(equal str1 "Hello")      ; t

; 문자열 → 숫자 변환
atoi("123")               ; 123
atof("3.14")              ; 3.14

; 숫자 → 문자열 변환
sprintf(nil "%d" 123)     ; "123"
sprintf(nil "%.2f" 3.14159)  ; "3.14"
```

### 심볼과 프로퍼티 리스트

```skill
; 심볼
mySymbol = 'test

; 프로퍼티 리스트 (연관 배열)
obj = makeTable('myTable nil)
obj["key1"] = "value1"
obj["key2"] = 100

; 접근
obj["key1"]               ; "value1"

; 또는
putprop(mySymbol 100 'age)
putprop(mySymbol "John" 'name)
get(mySymbol 'age)        ; 100
get(mySymbol 'name)       ; "John"
```

---

## 제어 구조

### 조건문 (if)

```skill
; 기본 if
if(x > 10 then
    println("x는 10보다 큽니다")
else
    println("x는 10 이하입니다")
)

; if without else
when(x > 10
    println("x는 10보다 큽니다")
)

; unless (when의 반대)
unless(x <= 10
    println("x는 10보다 큽니다")
)
```

### 조건문 (case)

```skill
case(value
    (1 println("One"))
    (2 println("Two"))
    (3 println("Three"))
    (t println("Other"))  ; default
)
```

### 반복문 (for)

```skill
; for 루프
for(i 1 10
    println(i)
)

; 증가값 지정
for(i 0 100 10
    println(i)  ; 0, 10, 20, ..., 100
)
```

### 반복문 (while)

```skill
; while 루프
let((i)
    i = 0
    while(i < 10
        println(i)
        i = i + 1
    )
)
```

### 반복문 (foreach)

```skill
; 리스트 순회
myList = '(1 2 3 4 5)
foreach(item myList
    println(item)
)

; 조건부 처리
foreach(item myList
    when(item > 3
        println(item)
    )
)
```

---

## 함수 정의

### 기본 함수

```skill
; 함수 정의 (procedure)
procedure(myFunction(x y)
    x + y
)

; 호출
myFunction(10 20)  ; 30

; 여러 문장이 있는 함수
procedure(complexFunction(a b)
    let((result)
        result = a * b
        printf("결과: %d\n" result)
        result  ; 반환값
    )
)
```

### 기본값이 있는 함수

```skill
procedure(greet(@optional (name "World"))
    printf("Hello, %s!\n" name)
)

greet()           ; "Hello, World!"
greet("John")     ; "Hello, John!"
```

### 키워드 인자

```skill
procedure(createRect(@key (x 0) (y 0) (width 100) (height 50))
    printf("사각형: (%d,%d) %dx%d\n" x y width height)
)

createRect(?x 10 ?y 20 ?width 200)
```

### 가변 인자

```skill
procedure(sumAll(@rest numbers)
    let((total)
        total = 0
        foreach(num numbers
            total = total + num
        )
        total
    )
)

sumAll(1 2 3 4 5)  ; 15
```

---

## 레이아웃 관련 함수

### 셀뷰 열기/닫기

```skill
; 셀뷰 열기
cvId = dbOpenCellViewByType(
    "myLibrary"      ; 라이브러리
    "myCell"         ; 셀
    "layout"         ; 뷰
    nil              ; 모드
    "a"              ; 접근 모드 (r=읽기, a=추가, w=쓰기)
)

; 셀뷰 닫기
dbSave(cvId)
dbClose(cvId)
```

### 레이어 정보

```skill
; 레이어 번호와 용도 가져오기
layerNum = 10
purpose = "drawing"

; 레이어 확인
techId = techGetTechFile(cvId)
layerExists = techFindLayer(techId layerNum purpose)
```

### 도형 생성

```skill
; 사각형 생성
rectId = dbCreateRect(
    cvId                    ; 셀뷰 ID
    list(layerNum purpose)  ; 레이어
    list(0:0 100:50)        ; 좌하단, 우상단 좌표
)

; 다각형 생성
polyPoints = list(
    0:0
    100:0
    100:100
    50:150
    0:100
)
polyId = dbCreatePolygon(
    cvId
    list(layerNum purpose)
    polyPoints
)

; 경로 (Path) 생성
pathPoints = list(0:0 100:0 100:100)
pathId = dbCreatePath(
    cvId
    list(layerNum purpose)
    pathPoints
    5.0    ; 너비
)

; 원 생성 (실제로는 다각형으로 근사)
; Virtuoso에는 직접적인 원 생성 함수가 없음
; 사용자 정의 함수 필요
```

### 도형 속성 조회/수정

```skill
; 바운딩 박스
bbox = dbGetBBox(rectId)
lowerLeft = car(bbox)      ; 좌하단
upperRight = cadr(bbox)    ; 우상단

; 도형 이동
dbMoveFig(rectId cvId 10:20)

; 도형 회전
dbRotateFig(rectId cvId 0:0 90.0)  ; 원점 기준 90도 회전

; 도형 삭제
dbDeleteObject(rectId)
```

### 인스턴스 생성

```skill
; 셀 인스턴스 배치
masterCvId = dbOpenCellViewByType("myLib" "masterCell" "layout")

instId = dbCreateInst(
    cvId              ; 배치할 셀뷰
    masterCvId        ; 마스터 셀
    nil               ; 인스턴스 이름
    0:0               ; 위치
    "R0"              ; 회전/반전 (R0, R90, R180, R270, MX, MY, MXR90, MYR90)
)

; 인스턴스 배열
dbCreateInstArray(
    cvId
    masterCvId
    nil
    0:0               ; 시작 위치
    "R0"
    5                 ; X 방향 개수
    3                 ; Y 방향 개수
    100.0             ; X 간격
    80.0              ; Y 간격
)
```

### 객체 순회

```skill
; 모든 도형 순회
foreach(shape cvId~>shapes
    ; shape의 타입 확인
    shapeType = dbGetObjectType(shape)
    
    case(shapeType
        ("rect" 
            printf("사각형 발견\n")
        )
        ("polygon"
            printf("다각형 발견\n")
        )
        ("path"
            printf("경로 발견\n")
        )
    )
)

; 특정 레이어의 도형만
foreach(shape cvId~>shapes
    when(shape~>layerNum == 10
        printf("레이어 10의 도형\n")
    )
)
```

### 좌표 변환

```skill
; 포인트 생성
pt1 = 10:20
pt2 = 30:40

; 좌표 값 추출
xCoord(pt1)           ; 10.0
yCoord(pt1)           ; 20.0

; 포인트 연산
ptSum = pt1 + pt2     ; 40:60
ptDiff = pt2 - pt1    ; 20:20

; 거리 계산
dist = dbDistance(pt1 pt2)

; 변환 행렬
trans = dbCreateTransform(
    10:20       ; 이동
    "R90"       ; 회전
    1.0         ; 스케일
)

; 좌표 변환
newPt = dbTransformPoint(pt1 trans)
```

---

## 실용 예제

### 예제 1: 사각형 배열 생성

```skill
procedure(createRectArray(cvId layer startX startY width height numX numY spaceX spaceY)
    let((x y rectId)
        for(i 0 numX-1
            for(j 0 numY-1
                x = startX + i * (width + spaceX)
                y = startY + j * (height + spaceY)
                
                rectId = dbCreateRect(
                    cvId
                    layer
                    list(x:y (x+width):(y+height))
                )
            )
        )
        printf("배열 생성 완료: %dx%d\n" numX numY)
    )
)

; 사용 예
cvId = dbOpenCellViewByType("myLib" "myCell" "layout" nil "a")
createRectArray(cvId list(10 "drawing") 0 0 50 30 5 3 10 10)
dbSave(cvId)
dbClose(cvId)
```

### 예제 2: 레이어별 도형 개수 세기

```skill
procedure(countShapesByLayer(cvId)
    let((layerCount layer key)
        ; 레이어별 카운트 테이블 생성
        layerCount = makeTable('layerCount nil)
        
        ; 모든 도형 순회
        foreach(shape cvId~>shapes
            layer = sprintf(nil "%d:%s" 
                shape~>layerNum 
                shape~>purpose
            )
            
            ; 카운트 증가
            when(layerCount[layer] == nil
                layerCount[layer] = 0
            )
            layerCount[layer] = layerCount[layer] + 1
        )
        
        ; 결과 출력
        printf("\n=== 레이어별 도형 개수 ===\n")
        foreach(key layerCount
            printf("%s: %d\n" key layerCount[key])
        )
        
        layerCount
    )
)

; 사용 예
cvId = dbOpenCellViewByType("myLib" "myCell" "layout")
countShapesByLayer(cvId)
dbClose(cvId)
```

### 예제 3: 바운딩 박스 계산

```skill
procedure(getTotalBBox(cvId)
    let((minX minY maxX maxY bbox shape)
        minX = nil
        minY = nil
        maxX = nil
        maxY = nil
        
        ; 모든 도형의 바운딩 박스 확인
        foreach(shape cvId~>shapes
            bbox = dbGetBBox(shape)
            when(bbox
                ; 최소/최대값 업데이트
                when(minX == nil || xCoord(car(bbox)) < minX
                    minX = xCoord(car(bbox))
                )
                when(minY == nil || yCoord(car(bbox)) < minY
                    minY = yCoord(car(bbox))
                )
                when(maxX == nil || xCoord(cadr(bbox)) > maxX
                    maxX = xCoord(cadr(bbox))
                )
                when(maxY == nil || yCoord(cadr(bbox)) > maxY
                    maxY = yCoord(cadr(bbox))
                )
            )
        )
        
        ; 결과 반환
        if(minX then
            list(minX:minY maxX:maxY)
        else
            nil
        )
    )
)

; 사용 예
cvId = dbOpenCellViewByType("myLib" "myCell" "layout")
totalBBox = getTotalBBox(cvId)
printf("전체 바운딩 박스: %L\n" totalBBox)
dbClose(cvId)
```

### 예제 4: DRC 간격 체크

```skill
procedure(checkSpacing(cvId layer minSpace)
    let((shapes rectList violations)
        violations = 0
        rectList = nil
        
        ; 특정 레이어의 모든 사각형 수집
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>objType == "rect"
                rectList = cons(shape rectList)
            )
        )
        
        ; 모든 쌍에 대해 간격 체크
        foreach(rect1 rectList
            foreach(rect2 rectList
                when(rect1 != rect2
                    bbox1 = dbGetBBox(rect1)
                    bbox2 = dbGetBBox(rect2)
                    
                    ; 간단한 간격 체크 (개선 가능)
                    spacing = calculateSpacing(bbox1 bbox2)
                    
                    when(spacing < minSpace && spacing >= 0
                        violations = violations + 1
                        printf("간격 위반: %.3f < %.3f\n" 
                            spacing minSpace)
                    )
                )
            )
        )
        
        printf("총 %d개의 간격 위반 발견\n" violations)
        violations
    )
)

; 보조 함수: 간격 계산
procedure(calculateSpacing(bbox1 bbox2)
    let((x1_min y1_min x1_max y1_max x2_min y2_min x2_max y2_max
         dx dy)
        x1_min = xCoord(car(bbox1))
        y1_min = yCoord(car(bbox1))
        x1_max = xCoord(cadr(bbox1))
        y1_max = yCoord(cadr(bbox1))
        
        x2_min = xCoord(car(bbox2))
        y2_min = yCoord(car(bbox2))
        x2_max = xCoord(cadr(bbox2))
        y2_max = yCoord(cadr(bbox2))
        
        ; X 방향 간격
        if(x1_max < x2_min then
            dx = x2_min - x1_max
        else if(x2_max < x1_min then
            dx = x1_min - x2_max
        else
            dx = 0
        ))
        
        ; Y 방향 간격
        if(y1_max < y2_min then
            dy = y2_min - y1_max
        else if(y2_max < y1_min then
            dy = y1_min - y2_max
        else
            dy = 0
        ))
        
        ; 최소 간격 반환
        if(dx > 0 && dy > 0 then
            sqrt(dx*dx + dy*dy)  ; 대각선 거리
        else if(dx > 0 then
            dx
        else if(dy > 0 then
            dy
        else
            -1  ; 겹침
        )))
    )
)
```

### 예제 5: 텍스트 레이블 추가

```skill
procedure(addLabel(cvId layer text x y @optional (height 1.0) (justify "left"))
    let((labelId)
        labelId = dbCreateLabel(
            cvId
            layer
            x:y
            text
            justify
            "R0"        ; 회전
            "roman"     ; 폰트
            height      ; 높이
        )
        labelId
    )
)

; 사용 예
cvId = dbOpenCellViewByType("myLib" "myCell" "layout" nil "a")
addLabel(cvId list(10 "drawing") "VDD" 100 200 2.0 "center")
addLabel(cvId list(10 "drawing") "GND" 100 0 2.0 "center")
dbSave(cvId)
dbClose(cvId)
```

### 예제 6: 파일에서 좌표 읽어 도형 생성

```skill
procedure(createShapesFromFile(cvId layer filename)
    let((port line coords x y width height)
        ; 파일 열기
        port = infile(filename)
        
        when(port
            ; 각 줄 읽기
            while(gets(line port)
                ; 좌표 파싱 (예: "10 20 100 50" 형식)
                coords = parseString(line)
                
                when(length(coords) >= 4
                    x = atof(car(coords))
                    y = atof(cadr(coords))
                    width = atof(caddr(coords))
                    height = atof(cadddr(coords))
                    
                    ; 사각형 생성
                    dbCreateRect(
                        cvId
                        layer
                        list(x:y (x+width):(y+height))
                    )
                )
            )
            
            close(port)
            printf("파일에서 도형 생성 완료\n")
        else
            error("파일을 열 수 없습니다: %s" filename)
        )
    )
)
```

---

## 디버깅

### 디버그 출력

```skill
; 기본 출력
println("디버그 메시지")
printf("변수 값: %d\n" myVar)

; 변수 타입 확인
type(myVar)

; 객체 정보 출력
dbShowObject(rectId)

; 리스트 출력
printf("리스트: %L\n" myList)
```

### 에러 처리

```skill
; 에러 핸들링
errset(
    ; 에러가 발생할 수 있는 코드
    cvId = dbOpenCellViewByType("lib" "cell" "layout")
    ; ...
    t  ; 성공 시 반환값
    ; 에러 발생 시 nil 반환
)

; 조건부 에러
when(cvId == nil
    error("셀뷰를 열 수 없습니다")
)
```

### 실행 시간 측정

```skill
procedure(timeIt(func)
    let((startTime endTime)
        startTime = getCurrentTime()
        
        ; 함수 실행
        funcall(func)
        
        endTime = getCurrentTime()
        printf("실행 시간: %.3f초\n" endTime - startTime)
    )
)
```

### SKILL 디버거 사용

```skill
; 브레이크포인트 (실행 중지)
break()

; 스택 트레이스
errTrace()

; 현재 환경 변수 확인
envList()
```

---

## 유용한 팁

### 1. 셀뷰 존재 확인

```skill
procedure(cellViewExists(lib cell view)
    let((cvId exists)
        cvId = dbOpenCellViewByType(lib cell view nil "r")
        exists = (cvId != nil)
        when(cvId
            dbClose(cvId)
        )
        exists
    )
)
```

### 2. 안전한 파일 작업

```skill
procedure(safeFileOperation(cvId @rest operations)
    let((success)
        success = t
        
        errset(
            ; 작업 수행
            foreach(op operations
                funcall(op cvId)
            )
            
            ; 저장
            dbSave(cvId)
            t
        )
        
        unless(success
            printf("작업 실패, 변경사항 취소\n")
            dbRevert(cvId)  ; 되돌리기
        )
        
        success
    )
)
```

### 3. 진행 상황 표시

```skill
procedure(processWithProgress(itemList processFunc)
    let((total current percent)
        total = length(itemList)
        current = 0
        
        foreach(item itemList
            current = current + 1
            percent = 100.0 * current / total
            
            printf("\r진행: %.1f%% (%d/%d)" percent current total)
            
            funcall(processFunc item)
        )
        
        printf("\n완료!\n")
    )
)
```

### 4. 설정 파일 사용

```skill
; 설정 파일 읽기 (myconfig.il)
procedure(loadConfig(filename)
    let((config)
        config = makeTable('config nil)
        
        ; 파일이 존재하면 로드
        when(isFile(filename)
            load(filename)
            
            ; 설정값 저장
            config["minSpacing"] = minSpacing
            config["layerNum"] = layerNum
            ; ...
        )
        
        config
    )
)
```

---

## 참고 자료

### 공식 문서
- Cadence SKILL Language Reference
- Cadence SKILL Language User Guide
- Database Access SKILL Functions Reference

### 유용한 함수 카테고리

**데이터베이스 접근:**
- `dbOpenCellViewByType()` - 셀뷰 열기
- `dbSave()` - 저장
- `dbClose()` - 닫기
- `dbCreateRect()`, `dbCreatePolygon()` - 도형 생성

**도형 조작:**
- `dbMoveFig()` - 이동
- `dbRotateFig()` - 회전
- `dbCopyFig()` - 복사
- `dbDeleteObject()` - 삭제

**좌표 및 변환:**
- `dbCreateTransform()` - 변환 행렬
- `dbTransformPoint()` - 좌표 변환
- `dbDistance()` - 거리 계산

**조회:**
- `dbGetBBox()` - 바운딩 박스
- `dbGetObjectType()` - 객체 타입
- `dbFindAnyInst()` - 인스턴스 찾기

---

## 실전 팁

1. **코드 재사용**: 자주 사용하는 함수는 별도 라이브러리 파일로 저장
2. **에러 처리**: 항상 `errset`으로 감싸서 안전하게 실행
3. **성능**: 큰 데이터는 반복문보다 벡터 연산 활용
4. **문서화**: 함수 시작 부분에 주석으로 설명 추가
5. **테스트**: 작은 테스트 셀에서 먼저 검증 후 실제 적용

---

## 관련 문서

- [[KLayout_XSection_Manual/]] - KLayout 사용법
- [[programming/]] - 프로그래밍 관련 문서
