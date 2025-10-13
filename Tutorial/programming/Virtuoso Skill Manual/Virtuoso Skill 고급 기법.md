# Virtuoso SKILL 고급 기법 및 실전 예제

## 목차
- [[#고급 데이터 구조]]
- [[#파일 입출력]]
- [[#GUI 프로그래밍]]
- [[#성능 최적화]]
- [[#실전 프로젝트 예제]]
- [[#트러블슈팅]]
- [[#베스트 프랙티스]]

---

## 고급 데이터 구조

### 해시 테이블 (Association List)

```skill
; 해시 테이블 생성
procedure(createHashTable()
    let((table)
        table = makeTable('myTable nil)
        table
    )
)

; 해시 테이블 사용 예제
procedure(hashTableExample()
    let((deviceTable device)
        ; 테이블 생성
        deviceTable = makeTable('deviceTable nil)
        
        ; 데이터 추가
        deviceTable["NMOS"] = list(
            'width 1.0
            'length 0.18
            'fingers 4
        )
        deviceTable["PMOS"] = list(
            'width 2.0
            'length 0.18
            'fingers 4
        )
        
        ; 데이터 조회
        device = deviceTable["NMOS"]
        printf("NMOS Width: %.2f\n" cadr(device))
        
        ; 모든 키 순회
        foreach(key deviceTable
            printf("Device: %s\n" key)
        )
        
        deviceTable
    )
)
```

### 구조체 (Defstruct)

```skill
; 구조체 정의
defstruct(Device
    name
    width
    length
    fingers
    x
    y
)

; 구조체 사용
procedure(structExample()
    let((nmos pmos)
        ; 인스턴스 생성
        nmos = makeDevice(
            ?name "NMOS_1"
            ?width 1.0
            ?length 0.18
            ?fingers 4
            ?x 0
            ?y 0
        )
        
        ; 필드 접근
        printf("Name: %s\n" nmos->name)
        printf("Width: %.2f\n" nmos->width)
        
        ; 필드 수정
        nmos->x = 10.0
        nmos->y = 20.0
        
        nmos
    )
)
```

---

## 파일 입출력

### 텍스트 파일 읽기/쓰기

```skill
procedure(readTextFile(filename)
    let((port line lineNum content)
        content = nil
        lineNum = 0
        port = infile(filename)
        
        if(port then
            while(gets(line port)
                lineNum = lineNum + 1
                content = append(content list(line))
            )
            close(port)
            printf("총 %d줄 읽음\n" lineNum)
        else
            error("파일을 열 수 없습니다: %s" filename)
        )
        content
    )
)

procedure(writeTextFile(filename content)
    let((port)
        port = outfile(filename "w")
        if(port then
            foreach(line content
                fprintf(port "%s\n" line)
            )
            close(port)
            printf("파일 작성 완료: %s\n" filename)
            t
        else
            error("파일을 생성할 수 없습니다: %s" filename)
            nil
        )
    )
)
```

### CSV 파일 처리

```skill
procedure(readCSV(filename @optional (delimiter ","))
    let((port line fields data)
        data = nil
        port = infile(filename)
        
        when(port
            while(gets(line port)
                fields = parseString(line delimiter)
                data = append(data list(fields))
            )
            close(port)
        )
        data
    )
)

procedure(writeCSV(filename data @optional (delimiter ","))
    let((port)
        port = outfile(filename "w")
        when(port
            foreach(row data
                fprintf(port "%s\n" 
                    buildString(row delimiter)
                )
            )
            close(port)
        )
    )
)
```

---

## GUI 프로그래밍

### 기본 대화상자

```skill
procedure(showInfo(message)
    hiDisplayAppDBox(
        ?name 'infoBox
        ?dboxBanner "정보"
        ?dboxText message
        ?buttonLayout 'OKCancel
    )
)

procedure(confirmAction(message)
    let((result)
        result = hiDisplayAppDBox(
            ?name 'confirmBox
            ?dboxBanner "확인"
            ?dboxText message
            ?buttonLayout 'YesNo
        )
        result
    )
)
```

### 입력 폼 생성

```skill
procedure(createInputForm()
    let((form fields)
        fields = list(
            hiCreateStringField(
                ?name 'deviceName
                ?prompt "Device Name"
                ?value "M1"
            )
            hiCreateFloatField(
                ?name 'width
                ?prompt "Width (um)"
                ?value 1.0
            )
            hiCreateFloatField(
                ?name 'length
                ?prompt "Length (um)"
                ?value 0.18
            )
        )
        
        form = hiCreateAppForm(
            ?name 'inputForm
            ?formTitle "Device Parameters"
            ?fields fields
            ?callback list("applyCallback()")
        )
        
        hiDisplayForm(form)
    )
)
```

---

## 성능 최적화

### 벡터 연산 활용

```skill
; 느린 방법: 반복문
procedure(sumListSlow(numList)
    let((total)
        total = 0
        foreach(num numList
            total = total + num
        )
        total
    )
)

; 빠른 방법: apply 함수
procedure(sumListFast(numList)
    apply(plus numList)
)
```

### 캐싱 활용

```skill
let((cache)
    cache = makeTable('cache nil)
    
    procedure(expensiveOperation(input)
        let((result)
            result = cache[input]
            unless(result
                printf("계산 중: %s\n" input)
                result = input * input
                cache[input] = result
            )
            result
        )
    )
)
```

### 데이터베이스 접근 최적화

```skill
; 효율적: 한 번만 열고 닫기
procedure(efficientAccess()
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout")
        
        for(i 1 100
            ; 작업 수행
        )
        
        dbClose(cvId)
    )
)

; 더 효율적: 배치 처리
procedure(batchProcessing(operations)
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout" nil "a")
        dbBeginTransaction(cvId)
        
        foreach(op operations
            funcall(op cvId)
        )
        
        dbEndTransaction(cvId)
        dbSave(cvId)
        dbClose(cvId)
    )
)
```

---

## 실전 프로젝트 예제

### 프로젝트 1: 파라메트릭 셀 생성기

```skill
procedure(createParametricTransistor(
    cvId 
    name
    @key
    (width 1.0)
    (length 0.18)
    (fingers 1)
    (type "NMOS")
    (x 0)
    (y 0)
)
    let((gateLayer diffLayer contactLayer metalLayer
         fingerWidth spacing totalWidth)
        
        ; 레이어 정의
        gateLayer = list(10 "drawing")
        diffLayer = list(20 "drawing")
        contactLayer = list(30 "drawing")
        metalLayer = list(40 "drawing")
        
        ; 계산
        fingerWidth = width / fingers
        spacing = 0.2
        totalWidth = width + (fingers - 1) * spacing
        
        ; Diffusion 영역
        dbCreateRect(cvId diffLayer
            list(
                (x-0.1):(y-0.2)
                (x+totalWidth+0.1):(y+fingerWidth+0.2)
            )
        )
        
        ; Gate 생성
        for(i 0 fingers-1
            let((gateX)
                gateX = x + i * (width/fingers + spacing)
                dbCreateRect(cvId gateLayer
                    list(
                        gateX:(y-0.5)
                        (gateX+length):(y+fingerWidth+0.5)
                    )
                )
            )
        )
        
        ; 라벨 추가
        dbCreateLabel(cvId metalLayer 
            (x+totalWidth/2):(y+fingerWidth+1.0)
            name "centerCenter" "R0" "roman" 0.5
        )
        
        printf("트랜지스터 생성: %s (W=%.2f L=%.2f F=%d)\n"
            name width length fingers
        )
    )
)
```

### 프로젝트 2: 레이아웃 분석 도구

```skill
; 레이어별 통계 수집
procedure(analyzeLayout(cvId)
    let((stats layerStats layer shapeType area totalArea)
        stats = makeTable('stats nil)
        
        printf("\n=== 레이아웃 분석 ===\n")
        printf("Cell: %s\n" cvId~>cellName)
        
        ; 모든 도형 순회
        foreach(shape cvId~>shapes
            layer = sprintf(nil "%d:%s" 
                shape~>layerNum 
                shape~>purpose
            )
            shapeType = dbGetObjectType(shape)
            
            ; 레이어별 통계 초기화
            unless(stats[layer]
                stats[layer] = makeTable('layerStats nil)
                stats[layer]["count"] = 0
                stats[layer]["area"] = 0.0
            )
            
            ; 카운트 증가
            stats[layer]["count"] = stats[layer]["count"] + 1
            
            ; 면적 계산
            when(member(shapeType list("rect" "polygon"))
                area = calculateArea(shape)
                stats[layer]["area"] = stats[layer]["area"] + area
            )
        )
        
        ; 결과 출력
        printf("\n레이어별 통계:\n")
        printf("%-15s %10s %15s\n" "Layer" "Count" "Total Area")
        printf("%-15s %10s %15s\n" "-----" "-----" "----------")
        
        foreach(layer stats
            printf("%-15s %10d %15.2f\n"
                layer
                stats[layer]["count"]
                stats[layer]["area"]
            )
        )
        
        stats
    )
)

; 면적 계산 함수
procedure(calculateArea(shape)
    let((bbox width height)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        width * height
    )
)
```

### 프로젝트 3: 자동 배선 도구

```skill
; Manhattan 스타일 라우팅
procedure(routeManhattan(cvId layer startPt endPt width)
    let((x1 y1 x2 y2 midX pathPoints)
        x1 = xCoord(startPt)
        y1 = yCoord(startPt)
        x2 = xCoord(endPt)
        y2 = yCoord(endPt)
        
        ; L자 경로 생성
        midX = (x1 + x2) / 2.0
        
        pathPoints = list(
            startPt
            midX:y1
            midX:y2
            endPt
        )
        
        ; Path 생성
        dbCreatePath(cvId layer pathPoints width)
        
        printf("라우팅: (%g,%g) → (%g,%g)\n" x1 y1 x2 y2)
    )
)

; 버스 라우팅
procedure(routeBus(cvId layer startPts endPts width pitch)
    let((numWires i)
        numWires = length(startPts)
        
        for(i 0 numWires-1
            let((start end offset)
                start = nth(i startPts)
                end = nth(i endPts)
                offset = i * pitch
                
                ; 오프셋 적용
                start = xCoord(start):(yCoord(start) + offset)
                end = xCoord(end):(yCoord(end) + offset)
                
                routeManhattan(cvId layer start end width)
            )
        )
        
        printf("버스 라우팅 완료: %d개 와이어\n" numWires)
    )
)
```

### 프로젝트 4: 레이아웃 비교 도구

```skill
; 두 레이아웃 비교
procedure(compareLayouts(cvId1 cvId2)
    let((shapes1 shapes2 diff)
        printf("\n=== 레이아웃 비교 ===\n")
        printf("Layout 1: %s/%s\n" 
            cvId1~>libName cvId1~>cellName)
        printf("Layout 2: %s/%s\n"
            cvId2~>libName cvId2~>cellName)
        
        ; 도형 개수 비교
        shapes1 = length(cvId1~>shapes)
        shapes2 = length(cvId2~>shapes)
        
        printf("\n도형 개수:\n")
        printf("  Layout 1: %d\n" shapes1)
        printf("  Layout 2: %d\n" shapes2)
        printf("  차이: %d\n" abs(shapes1 - shapes2))
        
        ; 레이어별 비교
        diff = compareByLayer(cvId1 cvId2)
        
        ; 바운딩 박스 비교
        compareBBox(cvId1 cvId2)
        
        diff
    )
)

procedure(compareByLayer(cvId1 cvId2)
    let((layers1 layers2 layer)
        layers1 = getLayerCounts(cvId1)
        layers2 = getLayerCounts(cvId2)
        
        printf("\n레이어별 비교:\n")
        printf("%-15s %10s %10s %10s\n" 
            "Layer" "Layout1" "Layout2" "Diff")
        printf("%-15s %10s %10s %10s\n" 
            "-----" "-------" "-------" "----")
        
        ; 모든 레이어 순회
        foreach(layer layers1
            let((count1 count2 diff)
                count1 = layers1[layer]
                count2 = if(layers2[layer] then layers2[layer] else 0)
                diff = count1 - count2
                
                printf("%-15s %10d %10d %10d\n"
                    layer count1 count2 diff
                )
            )
        )
    )
)

procedure(getLayerCounts(cvId)
    let((counts layer)
        counts = makeTable('counts nil)
        
        foreach(shape cvId~>shapes
            layer = sprintf(nil "%d:%s"
                shape~>layerNum
                shape~>purpose
            )
            
            unless(counts[layer]
                counts[layer] = 0
            )
            counts[layer] = counts[layer] + 1
        )
        
        counts
    )
)
```

---

## 트러블슈팅

### 일반적인 에러와 해결법

**에러 1: "Attempt to apply non-procedure"**
```skill
; 잘못된 코드
myVar = 10
result = myVar(5)  ; 에러!

; 올바른 코드
procedure(myFunc(x)
    x * 2
)
result = myFunc(5)  ; OK
```

**에러 2: "unbound variable"**
```skill
; 잘못된 코드
println(undefinedVar)  ; 에러!

; 올바른 코드
let((myVar)
    myVar = 10
    println(myVar)  ; OK
)
```

**에러 3: "bad argument type"**
```skill
; 잘못된 코드
x = "10"
y = x + 5  ; 에러!

; 올바른 코드
x = "10"
y = atoi(x) + 5  ; OK
```

### 디버깅 팁

```skill
; 함수 실행 추적
procedure(traceFunction(funcName)
    let((origFunc wrapper)
        origFunc = get(funcName 'expr)
        
        wrapper = procedure((@rest args)
            printf("→ %s 호출: %L\n" funcName args)
            let((result)
                result = apply(origFunc args)
                printf("← %s 반환: %L\n" funcName result)
                result
            )
        )
        
        put(funcName 'origExpr origFunc)
        put(funcName 'expr wrapper)
    )
)

; 추적 해제
procedure(untraceFunction(funcName)
    let((origFunc)
        origFunc = get(funcName 'origExpr)
        when(origFunc
            put(funcName 'expr origFunc)
            remprop(funcName 'origExpr)
        )
    )
)
```

### 성능 프로파일링

```skill
; 함수 실행 시간 측정
procedure(profileFunction(func @rest args)
    let((startTime endTime result elapsed)
        startTime = getCurrentTime()
        result = apply(func args)
        endTime = getCurrentTime()
        elapsed = endTime - startTime
        
        printf("함수: %s\n" func)
        printf("실행 시간: %.3f초\n" elapsed)
        printf("인자: %L\n" args)
        
        result
    )
)

; 벤치마크
procedure(benchmarkCode(name iterations codeFunc)
    let((times totalTime avgTime minTime maxTime)
        times = nil
        
        printf("\n벤치마크: %s (%d iterations)\n" 
            name iterations)
        
        for(i 1 iterations
            let((startTime endTime elapsed)
                startTime = getCurrentTime()
                funcall(codeFunc)
                endTime = getCurrentTime()
                elapsed = endTime - startTime
                times = cons(elapsed times)
            )
        )
        
        totalTime = apply(plus times)
        avgTime = totalTime / iterations
        minTime = apply(min times)
        maxTime = apply(max times)
        
        printf("총 시간: %.3f초\n" totalTime)
        printf("평균: %.3f초\n" avgTime)
        printf("최소: %.3f초\n" minTime)
        printf("최대: %.3f초\n" maxTime)
    )
)
```

---

## 베스트 프랙티스

### 1. 코드 구조화

```skill
; ===== 파일 헤더 =====
; File: myLibrary.il
; Author: Your Name
; Date: 2025-10-14
; Description: 트랜지스터 생성 라이브러리

; ===== 상수 정의 =====
let((METAL1_LAYER METAL2_LAYER MIN_SPACING)
    METAL1_LAYER = list(40 "drawing")
    METAL2_LAYER = list(41 "drawing")
    MIN_SPACING = 0.14
    
    ; ===== 유틸리티 함수 =====
    procedure(checkSpacing(value)
        value >= MIN_SPACING
    )
    
    ; ===== 메인 함수 =====
    procedure(createLayout(params)
        ; 구현
    )
)
```

### 2. 에러 처리

```skill
procedure(safeOperation(cvId)
    let((result success)
        success = errset(
            ; 위험한 작업
            result = performOperation(cvId)
            t
        )
        
        if(success then
            result
        else
            printf("에러 발생!\n")
            nil
        )
    )
)
```

### 3. 문서화

```skill
/*
 * Function: createTransistor
 * Description: 파라메트릭 트랜지스터 생성
 * 
 * Parameters:
 *   cvId   - 셀뷰 ID
 *   name   - 트랜지스터 이름
 *   width  - 폭 (um)
 *   length - 길이 (um)
 * 
 * Returns:
 *   생성된 트랜지스터 객체 ID
 * 
 * Example:
 *   createTransistor(cvId "M1" 1.0 0.18)
 */
procedure(createTransistor(cvId name width length)
    ; 구현
)
```

### 4. 테스트 코드

```skill
; 단위 테스트
procedure(testCreateTransistor()
    let((cvId result passed)
        printf("\n=== Test: createTransistor ===\n")
        passed = t
        
        ; 테스트 케이스 1
        cvId = dbOpenCellViewByType(
            "testLib" "testCell" "layout" 
            "maskLayout" "w"
        )
        
        result = createTransistor(cvId "M1" 1.0 0.18)
        
        if(result then
            printf("✓ Test 1 통과\n")
        else
            printf("✗ Test 1 실패\n")
            passed = nil
        )
        
        dbClose(cvId)
        
        if(passed then
            printf("\n모든 테스트 통과!\n")
        else
            printf("\n일부 테스트 실패\n")
        )
        
        passed
    )
)
```

### 5. 설정 관리

```skill
; 설정 파일: config.il
let((CONFIG)
    CONFIG = makeTable('CONFIG nil)
    
    ; 기본 설정
    CONFIG["minWidth"] = 0.14
    CONFIG["minSpacing"] = 0.14
    CONFIG["gridSize"] = 0.005
    
    ; 레이어 정의
    CONFIG["layers"] = makeTable('layers nil)
    CONFIG["layers"]["metal1"] = list(40 "drawing")
    CONFIG["layers"]["metal2"] = list(41 "drawing")
    CONFIG["layers"]["via1"] = list(50 "drawing")
    
    ; 설정 접근 함수
    procedure(getConfig(key)
        CONFIG[key]
    )
    
    procedure(setConfig(key value)
        CONFIG[key] = value
    )
)
```

---

## 유용한 스니펫

### 1. 그리드 스냅

```skill
procedure(snapToGrid(coord gridSize)
    round(coord / gridSize) * gridSize
)

procedure(snapPointToGrid(pt gridSize)
    let((x y)
        x = snapToGrid(xCoord(pt) gridSize)
        y = snapToGrid(yCoord(pt) gridSize)
        x:y
    )
)
```

### 2. 레이어 가시성 제어

```skill
procedure(hideAllLayers(cvId)
    let((lswId)
        lswId = getCurrentWindow()~>layerSelWindow
        lswSetLayerVisible(lswId "all" nil)
    )
)

procedure(showLayer(cvId layerNum purpose)
    let((lswId)
        lswId = getCurrentWindow()~>layerSelWindow
        lswSetLayerVisible(lswId 
            sprintf(nil "%d:%s" layerNum purpose) 
            t
        )
    )
)
```

### 3. 좌표 변환 유틸리티

```skill
; 극좌표 → 직교좌표
procedure(polarToCartesian(r theta)
    let((x y)
        x = r * cos(theta * pi / 180.0)
        y = r * sin(theta * pi / 180.0)
        x:y
    )
)

; 직교좌표 → 극좌표
procedure(cartesianToPolar(pt)
    let((x y r theta)
        x = xCoord(pt)
        y = yCoord(pt)
        r = sqrt(x*x + y*y)
        theta = atan2(y x) * 180.0 / pi
        list(r theta)
    )
)
```

### 4. 단위 변환

```skill
; 마이크론 → 데이터베이스 단위
procedure(umToDBU(value)
    value * 1000
)

; 데이터베이스 단위 → 마이크론
procedure(dbuToUM(value)
    value / 1000.0
)
```

---

## 참고 자료

### 추천 학습 순서

1. **기초 문법** - [[Virtuoso Skill 가이드]]
2. **고급 기법** - 이 문서
3. **실전 프로젝트** - 직접 구현해보기

### 유용한 리소스

- Cadence SKILL Language Reference
- Cadence SKILL Language User Guide
- Database Access SKILL Functions Reference
- OpenAccess Programmer's Guide

### 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 가이드
- [[programming/KLayout_XSection_Manual/]] - KLayout 참고
- [[programming/Python 가이드]] - Python 연동

---

## 실전 팁

1. **시작은 작게**: 간단한 스크립트부터 시작
2. **재사용**: 자주 쓰는 함수는 라이브러리로 저장
3. **테스트**: 작은 테스트 셀에서 먼저 검증
4. **문서화**: 코드에 주석 작성 습관화
5. **버전 관리**: Git으로 코드 버전 관리
6. **커뮤니티**: 온라인 포럼에서 질문하고 답변하기

---

**마지막 업데이트:** 2025-10-14
