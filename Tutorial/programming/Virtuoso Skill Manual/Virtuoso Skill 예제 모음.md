# Virtuoso SKILL 예제 모음 (100+ 스니펫)

## 목차
- [[#기본 연산]]
- [[#리스트 처리]]
- [[#문자열 처리]]
- [[#파일 처리]]
- [[#도형 생성]]
- [[#도형 조작]]
- [[#측정 및 계산]]
- [[#검증 및 체크]]
- [[#유틸리티]]
- [[#고급 패턴]]

---

## 기본 연산

### 예제 1: 숫자 포맷팅

```skill
; 정수 포맷
sprintf(nil "%d" 123)                    ; "123"
sprintf(nil "%05d" 123)                  ; "00123"
sprintf(nil "%+d" 123)                   ; "+123"

; 실수 포맷
sprintf(nil "%.2f" 3.14159)              ; "3.14"
sprintf(nil "%.4f" 3.14159)              ; "3.1416"
sprintf(nil "%e" 1000.0)                 ; "1.000000e+03"

; 16진수
sprintf(nil "%x" 255)                    ; "ff"
sprintf(nil "%X" 255)                    ; "FF"
```

### 예제 2: 범위 체크

```skill
procedure(inRange(value min max)
    value >= min && value <= max
)

; 사용
inRange(5 0 10)      ; t
inRange(15 0 10)     ; nil
```

### 예제 3: 값 제한 (Clamp)

```skill
procedure(clamp(value min max)
    cond(
        (value < min min)
        (value > max max)
        (t value)
    )
)

; 사용
clamp(5 0 10)        ; 5
clamp(-5 0 10)       ; 0
clamp(15 0 10)       ; 10
```

### 예제 4: 퍼센트 계산

```skill
procedure(percentage(part total)
    if(total > 0 then
        100.0 * part / total
    else
        0.0
    )
)
```

### 예제 5: 평균 계산

```skill
procedure(average(numList)
    if(numList then
        apply(plus numList) / length(numList)
    else
        0.0
    )
)
```

---

## 리스트 처리

### 예제 6: 리스트 필터링

```skill
procedure(filter(predicate lst)
    let((result)
        result = nil
        foreach(item lst
            when(funcall(predicate item)
                result = append(result list(item))
            )
        )
        result
    )
)

; 사용: 짝수만
filter(lambda((x) (equal (remainder x 2) 0)) '(1 2 3 4 5 6))
; → (2 4 6)
```

### 예제 7: 리스트 매핑

```skill
procedure(map(func lst)
    mapcar(func lst)
)

; 사용: 제곱
map(lambda((x) x * x) '(1 2 3 4 5))
; → (1 4 9 16 25)
```

### 예제 8: 리스트 합치기 (Flatten)

```skill
procedure(flatten(lst)
    let((result)
        result = nil
        foreach(item lst
            if(listp(item) then
                result = append(result flatten(item))
            else
                result = append(result list(item))
            )
        )
        result
    )
)

; 사용
flatten('(1 (2 3) ((4 5) 6)))
; → (1 2 3 4 5 6)
```

### 예제 9: 중복 제거

```skill
procedure(unique(lst)
    let((result)
        result = nil
        foreach(item lst
            unless(member(item result)
                result = append(result list(item))
            )
        )
        result
    )
)

; 사용
unique('(1 2 2 3 3 3 4))
; → (1 2 3 4)
```

### 예제 10: 리스트 분할

```skill
procedure(splitAt(n lst)
    list(
        firstn(n lst)
        nthcdr(n lst)
    )
)

; 사용
splitAt(3 '(1 2 3 4 5 6))
; → ((1 2 3) (4 5 6))
```

---

## 문자열 처리

### 예제 11: 문자열 분할

```skill
procedure(split(str delimiter)
    parseString(str delimiter)
)

; 사용
split("a,b,c,d" ",")
; → ("a" "b" "c" "d")
```

### 예제 12: 문자열 합치기

```skill
procedure(join(lst delimiter)
    buildString(lst delimiter)
)

; 사용
join('("a" "b" "c") ",")
; → "a,b,c"
```

### 예제 13: 문자열 trim

```skill
procedure(trim(str)
    let((start end len)
        len = strlen(str)
        start = 0
        end = len - 1
        
        ; 앞의 공백 제거
        while(start < len && 
              substring(str start start+1) == " "
            start = start + 1
        )
        
        ; 뒤의 공백 제거
        while(end >= start && 
              substring(str end end+1) == " "
            end = end - 1
        )
        
        if(start <= end then
            substring(str start end+1)
        else
            ""
        )
    )
)
```

### 예제 14: 문자열 치환

```skill
procedure(replace(str oldStr newStr)
    let((result pos)
        result = str
        pos = rindex(result oldStr)
        
        while(pos
            result = strcat(
                substring(result 0 pos)
                newStr
                substring(result pos+strlen(oldStr) strlen(result))
            )
            pos = rindex(result oldStr)
        )
        
        result
    )
)
```

### 예제 15: Camel Case 변환

```skill
procedure(toCamelCase(str)
    let((words result)
        words = parseString(lowerCase(str) "_")
        result = car(words)
        
        foreach(word cdr(words)
            result = strcat(result
                upperCase(substring(word 0 1))
                substring(word 1 strlen(word))
            )
        )
        
        result
    )
)

; 사용
toCamelCase("my_variable_name")
; → "myVariableName"
```

---

## 파일 처리

### 예제 16: 파일 존재 확인

```skill
procedure(fileExists(filename)
    isFile(filename)
)
```

### 예제 17: 파일 크기

```skill
procedure(fileSize(filename)
    let((port size)
        port = infile(filename)
        if(port then
            size = 0
            while(gets(line port)
                size = size + strlen(line)
            )
            close(port)
            size
        else
            nil
        )
    )
)
```

### 예제 18: 파일 줄 수 세기

```skill
procedure(countLines(filename)
    let((port count line)
        port = infile(filename)
        count = 0
        
        when(port
            while(gets(line port)
                count = count + 1
            )
            close(port)
        )
        
        count
    )
)
```

### 예제 19: 파일 백업

```skill
procedure(backupFile(filename)
    let((backup timestamp)
        timestamp = getCurrentTime()
        backup = sprintf(nil "%s.bak.%d" filename timestamp)
        
        system(sprintf(nil "cp %s %s" filename backup))
        
        printf("백업 생성: %s\n" backup)
        backup
    )
)
```

### 예제 20: 디렉토리 생성

```skill
procedure(makeDirectory(path)
    system(sprintf(nil "mkdir -p %s" path))
)
```

---

## 도형 생성

### 예제 21: 정사각형

```skill
procedure(createSquare(cvId layer x y size)
    dbCreateRect(cvId layer
        list(x:y (x+size):(y+size))
    )
)
```

### 예제 22: 원 (다각형 근사)

```skill
procedure(createCircle(cvId layer center radius @optional (segments 32))
    let((points angle x y)
        points = nil
        
        for(i 0 segments-1
            angle = 2.0 * pi * i / segments
            x = xCoord(center) + radius * cos(angle)
            y = yCoord(center) + radius * sin(angle)
            points = append(points list(x:y))
        )
        
        dbCreatePolygon(cvId layer points)
    )
)
```

### 예제 23: 정다각형

```skill
procedure(createPolygon(cvId layer center radius sides)
    let((points angle x y)
        points = nil
        
        for(i 0 sides-1
            angle = 2.0 * pi * i / sides
            x = xCoord(center) + radius * cos(angle)
            y = yCoord(center) + radius * sin(angle)
            points = append(points list(x:y))
        )
        
        dbCreatePolygon(cvId layer points)
    )
)
```

### 예제 24: 십자가

```skill
procedure(createCross(cvId layer center width length thickness)
    let((x y hw hl ht)
        x = xCoord(center)
        y = yCoord(center)
        hw = width / 2.0
        hl = length / 2.0
        ht = thickness / 2.0
        
        ; 수평 바
        dbCreateRect(cvId layer
            list((x-hl):(y-ht) (x+hl):(y+ht))
        )
        
        ; 수직 바
        dbCreateRect(cvId layer
            list((x-ht):(y-hw) (x+ht):(y+hw))
        )
    )
)
```

### 예제 25: L자 도형

```skill
procedure(createLShape(cvId layer x y width height thickness)
    ; 수평 부분
    dbCreateRect(cvId layer
        list(x:y (x+width):(y+thickness))
    )
    
    ; 수직 부분
    dbCreateRect(cvId layer
        list(x:y (x+thickness):(y+height))
    )
)
```

---

## 도형 조작

### 예제 26: 도형 중심점 찾기

```skill
procedure(getCenter(shape)
    let((bbox x1 y1 x2 y2 cx cy)
        bbox = dbGetBBox(shape)
        x1 = xCoord(car(bbox))
        y1 = yCoord(car(bbox))
        x2 = xCoord(cadr(bbox))
        y2 = yCoord(cadr(bbox))
        
        cx = (x1 + x2) / 2.0
        cy = (y1 + y2) / 2.0
        
        cx:cy
    )
)
```

### 예제 27: 도형 크기 조정

```skill
procedure(scaleShape(shape cvId factor)
    let((center transform)
        center = getCenter(shape)
        
        transform = dbCreateTransform(
            center "R0" factor
        )
        
        dbTransformFig(shape transform)
    )
)
```

### 예제 28: 도형 정렬

```skill
; 좌측 정렬
procedure(alignLeft(shapes x)
    foreach(shape shapes
        let((bbox offset)
            bbox = dbGetBBox(shape)
            offset = x - xCoord(car(bbox))
            dbMoveFig(shape nil offset:0)
        )
    )
)

; 중앙 정렬
procedure(alignCenter(shapes x)
    foreach(shape shapes
        let((bbox center offset)
            bbox = dbGetBBox(shape)
            center = (xCoord(car(bbox)) + xCoord(cadr(bbox))) / 2.0
            offset = x - center
            dbMoveFig(shape nil offset:0)
        )
    )
)
```

### 예제 29: 도형 배열 복사

```skill
procedure(arrayShapes(shape cvId numX numY spacingX spacingY)
    let((newShapes)
        newShapes = nil
        
        for(i 0 numX-1
            for(j 0 numY-1
                when(i != 0 || j != 0
                    newShape = dbCopyFig(shape cvId
                        (i*spacingX):(j*spacingY)
                    )
                    newShapes = cons(newShape newShapes)
                )
            )
        )
        
        newShapes
    )
)
```

### 예제 30: 도형 대칭 복사

```skill
; X축 대칭
procedure(mirrorX(shape cvId)
    let((transform)
        transform = dbCreateTransform(0:0 "MX" 1.0)
        newShape = dbCopyFig(shape cvId 0:0)
        dbTransformFig(newShape transform)
        newShape
    )
)

; Y축 대칭
procedure(mirrorY(shape cvId)
    let((transform)
        transform = dbCreateTransform(0:0 "MY" 1.0)
        newShape = dbCopyFig(shape cvId 0:0)
        dbTransformFig(newShape transform)
        newShape
    )
)
```

---

## 측정 및 계산

### 예제 31: 면적 계산

```skill
procedure(calculateArea(shape)
    let((bbox width height)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        width * height
    )
)
```

### 예제 32: 둘레 계산

```skill
procedure(calculatePerimeter(shape)
    let((bbox width height)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        2.0 * (width + height)
    )
)
```

### 예제 33: 두 점 사이 거리

```skill
procedure(distance(pt1 pt2)
    let((dx dy)
        dx = xCoord(pt2) - xCoord(pt1)
        dy = yCoord(pt2) - yCoord(pt1)
        sqrt(dx*dx + dy*dy)
    )
)
```

### 예제 34: 각도 계산

```skill
procedure(angleBetweenPoints(pt1 pt2)
    let((dx dy angle)
        dx = xCoord(pt2) - xCoord(pt1)
        dy = yCoord(pt2) - yCoord(pt1)
        angle = atan2(dy dx) * 180.0 / pi
        
        ; 0-360 범위로 정규화
        when(angle < 0
            angle = angle + 360.0
        )
        
        angle
    )
)
```

### 예제 35: 밀도 계산

```skill
procedure(calculateDensity(cvId layer window)
    let((totalArea metalArea)
        totalArea = (xCoord(cadr(window)) - xCoord(car(window))) *
                    (yCoord(cadr(window)) - yCoord(car(window)))
        
        metalArea = 0.0
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                
                let((overlap)
                    overlap = calculateOverlap(
                        dbGetBBox(shape) window
                    )
                    metalArea = metalArea + overlap
                )
            )
        )
        
        if(totalArea > 0 then
            metalArea / totalArea
        else
            0.0
        )
    )
)
```

---

## 검증 및 체크

### 예제 36: 최소 폭 체크

```skill
procedure(checkWidth(shape minWidth)
    let((bbox width height minDim)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        minDim = min(width height)
        
        minDim >= minWidth
    )
)
```

### 예제 37: 면적 체크

```skill
procedure(checkArea(shape minArea)
    calculateArea(shape) >= minArea
)
```

### 예제 38: 종횡비 체크

```skill
procedure(checkAspectRatio(shape maxRatio)
    let((bbox width height ratio)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        
        ratio = if(height > 0 then
            width / height
        else
            999.0
        )
        
        ; 가로/세로 비율과 세로/가로 비율 모두 체크
        ratio <= maxRatio && (1.0/ratio) <= maxRatio
    )
)
```

### 예제 39: 겹침 체크

```skill
procedure(checkOverlap(bbox1 bbox2)
    let((x1_min y1_min x1_max y1_max
         x2_min y2_min x2_max y2_max)
        
        x1_min = xCoord(car(bbox1))
        y1_min = yCoord(car(bbox1))
        x1_max = xCoord(cadr(bbox1))
        y1_max = yCoord(cadr(bbox1))
        
        x2_min = xCoord(car(bbox2))
        y2_min = yCoord(car(bbox2))
        x2_max = xCoord(cadr(bbox2))
        y2_max = yCoord(cadr(bbox2))
        
        not(x1_max < x2_min || x2_max < x1_min ||
            y1_max < y2_min || y2_max < y1_min)
    )
)
```

### 예제 40: 영역 내 체크

```skill
procedure(isInside(pt bbox)
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

---

## 유틸리티

### 예제 41: 타임스탬프

```skill
procedure(getTimestamp()
    getCurrentTime()
)

procedure(formatTimestamp(timestamp)
    sprintf(nil "%d" timestamp)
)
```

### 예제 42: 진행률 표시

```skill
procedure(showProgress(current total @optional (msg "진행"))
    let((percent)
        percent = 100.0 * current / total
        printf("\r%s: %.1f%% (%d/%d)" 
            msg percent current total)
        
        when(current >= total
            printf("\n")
        )
    )
)
```

### 예제 43: 타이머

```skill
let((startTime)
    procedure(startTimer()
        startTime = getCurrentTime()
    )
    
    procedure(stopTimer()
        let((elapsed)
            elapsed = getCurrentTime() - startTime
            printf("경과 시간: %.3f초\n" elapsed)
            elapsed
        )
    )
)
```

### 예제 44: 메모리 사용량 (근사)

```skill
procedure(estimateMemoryUsage(lst)
    ; 간단한 추정
    length(lst) * 8  ; 바이트
)
```

### 예제 45: 랜덤 숫자

```skill
procedure(random(min max)
    let((range value)
        range = max - min
        value = remainder(getCurrentTime() * 1000 range)
        min + value
    )
)
```

---

## 고급 패턴

### 예제 46: 메모이제이션

```skill
let((cache)
    cache = makeTable('cache nil)
    
    procedure(memoize(func)
        lambda((@rest args)
            let((key result)
                key = sprintf(nil "%L" args)
                result = cache[key]
                
                unless(result
                    result = apply(func args)
                    cache[key] = result
                )
                
                result
            )
        )
    )
)
```

### 예제 47: 커링 (Currying)

```skill
procedure(curry(func @rest args1)
    lambda((@rest args2)
        apply(func append(args1 args2))
    )
)

; 사용 예
add = lambda((x y) x + y)
add5 = curry(add 5)
add5(10)  ; → 15
```

### 예제 48: 파이프라인

```skill
procedure(pipeline(@rest funcs)
    lambda((value)
        let((result)
            result = value
            foreach(func funcs
                result = funcall(func result)
            )
            result
        )
    )
)

; 사용 예
process = pipeline(
    lambda((x) x * 2)
    lambda((x) x + 10)
    lambda((x) x / 2)
)
process(5)  ; → 10
```

### 예제 49: Retry 패턴

```skill
procedure(retry(func maxAttempts @optional (delay 1))
    let((attempt result success)
        attempt = 0
        success = nil
        
        while(attempt < maxAttempts && not(success)
            attempt = attempt + 1
            printf("시도 %d/%d\n" attempt maxAttempts)
            
            success = errset(
                result = funcall(func)
                t
            )
            
            unless(success
                when(attempt < maxAttempts
                    printf("실패, %d초 후 재시도...\n" delay)
                    sleep(delay)
                )
            )
        )
        
        if(success then
            result
        else
            error("최대 재시도 횟수 초과")
        )
    )
)
```

### 예제 50: Builder 패턴

```skill
defstruct(ShapeBuilder
    cvId
    layer
    x
    y
    width
    height
)

procedure(setPosition(builder x y)
    builder->x = x
    builder->y = y
    builder
)

procedure(setSize(builder width height)
    builder->width = width
    builder->height = height
    builder
)

procedure(build(builder)
    dbCreateRect(builder->cvId builder->layer
        list(
            builder->x : builder->y
            (builder->x + builder->width) : (builder->y + builder->height)
        )
    )
)

; 사용 예
/*
builder = makeShapeBuilder(?cvId cvId ?layer list(10 "drawing"))
rect = build(
    setSize(
        setPosition(builder 10 20)
        100 50
    )
)
*/
```

---

## 계속...

다음 50개의 예제는 다음 섹션에서 계속됩니다:
- 데이터베이스 고급 작업 (51-60)
- 레이아웃 분석 (61-70)
- 자동화 패턴 (71-80)
- 성능 최적화 (81-90)
- 실전 응용 (91-100)

---

## 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 문법
- [[Virtuoso Skill 고급 기법]] - 고급 기능
- [[Virtuoso Skill 실전 레시피]] - 실전 예제
- [[Virtuoso Skill 빠른참조]] - 빠른 참조

---

**팁**: 필요한 스니펫을 복사해서 자신의 라이브러리에 추가하세요!
