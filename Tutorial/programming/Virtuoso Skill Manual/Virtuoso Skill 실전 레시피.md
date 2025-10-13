# Virtuoso SKILL 실전 레시피 모음

## 목차
- [[#레이아웃 자동화]]
- [[#디자인 룰 체크]]
- [[#보고서 생성]]
- [[#데이터 추출]]
- [[#유틸리티 함수]]

---

## 레이아웃 자동화

### 레시피 1: 컨택 배열 생성

```skill
/*
 * 컨택 배열 자동 생성
 * 주어진 영역에 최대한 많은 컨택을 배치
 */
procedure(fillContactArray(
    cvId 
    layer 
    bbox 
    contactSize 
    spacing
)
    let((x1 y1 x2 y2 numX numY i j x y)
        ; 영역 추출
        x1 = xCoord(car(bbox))
        y1 = yCoord(car(bbox))
        x2 = xCoord(cadr(bbox))
        y2 = yCoord(cadr(bbox))
        
        ; 배치 가능한 개수 계산
        numX = floor((x2 - x1 - contactSize) / 
                     (contactSize + spacing)) + 1
        numY = floor((y2 - y1 - contactSize) / 
                     (contactSize + spacing)) + 1
        
        ; 중앙 정렬을 위한 시작점 계산
        x1 = x1 + (x2 - x1 - 
                   (numX * contactSize + (numX-1) * spacing)) / 2.0
        y1 = y1 + (y2 - y1 - 
                   (numY * contactSize + (numY-1) * spacing)) / 2.0
        
        ; 컨택 배열 생성
        for(i 0 numX-1
            for(j 0 numY-1
                x = x1 + i * (contactSize + spacing)
                y = y1 + j * (contactSize + spacing)
                
                dbCreateRect(cvId layer
                    list(
                        x:y
                        (x+contactSize):(y+contactSize)
                    )
                )
            )
        )
        
        printf("컨택 배열 생성: %dx%d = %d개\n" 
            numX numY numX*numY)
        
        list(numX numY)
    )
)

; 사용 예
procedure(exampleContactArray()
    let((cvId)
        cvId = dbOpenCellViewByType(
            "myLib" "testCell" "layout" nil "a"
        )
        
        ; 10x10 영역에 0.5x0.5 컨택을 0.5 간격으로 배치
        fillContactArray(
            cvId
            list(30 "drawing")  ; 컨택 레이어
            list(0:0 10:10)     ; 영역
            0.5                  ; 컨택 크기
            0.5                  ; 간격
        )
        
        dbSave(cvId)
        dbClose(cvId)
    )
)
```

### 레시피 2: 가드링 생성

```skill
/*
 * 셀 주변에 가드링 생성
 * 셀의 전체 영역을 둘러싸는 사각형 링
 */
procedure(createGuardRing(
    cvId 
    layer 
    @key
    (margin 5.0)
    (width 2.0)
)
    let((bbox innerBox outerBox x1 y1 x2 y2)
        ; 전체 바운딩 박스 가져오기
        bbox = dbComputeBBox(cvId)
        
        x1 = xCoord(car(bbox)) - margin
        y1 = yCoord(car(bbox)) - margin
        x2 = xCoord(cadr(bbox)) + margin
        y2 = yCoord(cadr(bbox)) + margin
        
        ; 외곽 사각형
        outerBox = list(x1:y1 x2:y2)
        
        ; 내부 사각형 (구멍)
        innerBox = list(
            (x1+width):(y1+width)
            (x2-width):(y2-width)
        )
        
        ; 4개의 사각형으로 링 생성
        ; 상단
        dbCreateRect(cvId layer
            list(x1:y1 x2:(y1+width))
        )
        ; 하단
        dbCreateRect(cvId layer
            list(x1:(y2-width) x2:y2)
        )
        ; 좌측
        dbCreateRect(cvId layer
            list(x1:(y1+width) (x1+width):(y2-width))
        )
        ; 우측
        dbCreateRect(cvId layer
            list((x2-width):(y1+width) x2:(y2-width))
        )
        
        printf("가드링 생성 완료\n")
    )
)
```

### 레시피 3: 대칭 레이아웃 생성

```skill
/*
 * 도형을 대칭으로 복사
 */
procedure(createSymmetricLayout(cvId shapes axis)
    let((newShapes transform)
        newShapes = nil
        
        foreach(shape shapes
            ; 대칭 변환 생성
            case(axis
                ("x" 
                    ; X축 대칭 (상하 반전)
                    transform = dbCreateTransform(
                        0:0 "MX" 1.0
                    )
                )
                ("y"
                    ; Y축 대칭 (좌우 반전)
                    transform = dbCreateTransform(
                        0:0 "MY" 1.0
                    )
                )
                (t
                    error("잘못된 축: %s (x 또는 y)" axis)
                )
            )
            
            ; 도형 복사 및 변환
            newShape = dbCopyFig(shape cvId 0:0)
            dbTransformFig(newShape transform)
            
            newShapes = cons(newShape newShapes)
        )
        
        printf("대칭 레이아웃 생성: %d개 도형\n" 
            length(newShapes))
        
        newShapes
    )
)
```

### 레시피 4: 패턴 채우기

```skill
/*
 * 영역을 패턴으로 채우기 (더미 메탈 등)
 */
procedure(fillPattern(
    cvId 
    layer 
    bbox 
    patternWidth 
    patternHeight 
    spaceX 
    spaceY
)
    let((x1 y1 x2 y2 x y count)
        x1 = xCoord(car(bbox))
        y1 = yCoord(car(bbox))
        x2 = xCoord(cadr(bbox))
        y2 = yCoord(cadr(bbox))
        
        count = 0
        x = x1
        
        while(x + patternWidth <= x2
            y = y1
            while(y + patternHeight <= y2
                dbCreateRect(cvId layer
                    list(
                        x:y
                        (x+patternWidth):(y+patternHeight)
                    )
                )
                count = count + 1
                y = y + patternHeight + spaceY
            )
            x = x + patternWidth + spaceX
        )
        
        printf("패턴 채우기: %d개 생성\n" count)
        count
    )
)
```

---

## 디자인 룰 체크

### 레시피 5: 간격 검증

```skill
/*
 * 같은 레이어 내 도형 간 최소 간격 체크
 */
procedure(checkMinSpacing(cvId layer minSpacing)
    let((shapes violations)
        violations = nil
        
        ; 레이어의 모든 도형 수집
        shapes = nil
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                shapes = cons(shape shapes)
            )
        )
        
        printf("간격 체크: %d개 도형\n" length(shapes))
        
        ; 모든 쌍 검사 (O(n²) - 큰 레이아웃에서는 최적화 필요)
        foreach(shape1 shapes
            foreach(shape2 shapes
                when(shape1 != shape2
                    let((bbox1 bbox2 spacing)
                        bbox1 = dbGetBBox(shape1)
                        bbox2 = dbGetBBox(shape2)
                        spacing = calculateMinSpacing(bbox1 bbox2)
                        
                        when(spacing >= 0 && spacing < minSpacing
                            violations = cons(
                                list(
                                    'shape1 shape1
                                    'shape2 shape2
                                    'spacing spacing
                                    'required minSpacing
                                )
                                violations
                            )
                        )
                    )
                )
            )
        )
        
        printf("간격 위반: %d개\n" length(violations))
        violations
    )
)

; 두 바운딩 박스 간 최소 거리 계산
procedure(calculateMinSpacing(bbox1 bbox2)
    let((x1_min y1_min x1_max y1_max
         x2_min y2_min x2_max y2_max
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
        cond(
            (x1_max < x2_min
                dx = x2_min - x1_max
            )
            (x2_max < x1_min
                dx = x1_min - x2_max
            )
            (t
                dx = 0  ; 겹침
            )
        )
        
        ; Y 방향 간격
        cond(
            (y1_max < y2_min
                dy = y2_min - y1_max
            )
            (y2_max < y1_min
                dy = y1_min - y2_max
            )
            (t
                dy = 0  ; 겹침
            )
        )
        
        ; 최소 거리 반환
        cond(
            (dx > 0 && dy > 0
                sqrt(dx*dx + dy*dy)  ; 대각선 거리
            )
            (dx > 0
                dx
            )
            (dy > 0
                dy
            )
            (t
                -1  ; 겹침
            )
        )
    )
)
```

### 레시피 6: 최소 폭 검증

```skill
procedure(checkMinWidth(cvId layer minWidth)
    let((violations)
        violations = nil
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                
                let((bbox width height minDim)
                    bbox = dbGetBBox(shape)
                    width = xCoord(cadr(bbox)) - xCoord(car(bbox))
                    height = yCoord(cadr(bbox)) - yCoord(car(bbox))
                    minDim = min(width height)
                    
                    when(minDim < minWidth
                        violations = cons(
                            list(
                                'shape shape
                                'width minDim
                                'required minWidth
                                'location car(bbox)
                            )
                            violations
                        )
                    )
                )
            )
        )
        
        printf("폭 위반: %d개\n" length(violations))
        violations
    )
)
```

### 레시피 7: 밀도 검증

```skill
/*
 * 지정된 윈도우 내 메탈 밀도 계산
 */
procedure(calculateDensity(cvId layer windowSize step)
    let((bbox x1 y1 x2 y2 densityMap)
        bbox = dbComputeBBox(cvId)
        x1 = xCoord(car(bbox))
        y1 = yCoord(car(bbox))
        x2 = xCoord(cadr(bbox))
        y2 = yCoord(cadr(bbox))
        
        densityMap = nil
        
        ; 윈도우를 슬라이딩하며 밀도 계산
        for(x x1 x2 step
            for(y y1 y2 step
                let((window metalArea density)
                    window = list(
                        x:y
                        (x+windowSize):(y+windowSize)
                    )
                    
                    metalArea = calculateMetalArea(
                        cvId layer window
                    )
                    
                    density = metalArea / 
                              (windowSize * windowSize)
                    
                    densityMap = cons(
                        list(
                            'position x:y
                            'density density
                        )
                        densityMap
                    )
                )
            )
        )
        
        densityMap
    )
)

procedure(calculateMetalArea(cvId layer window)
    let((area)
        area = 0.0
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                
                let((bbox overlap)
                    bbox = dbGetBBox(shape)
                    overlap = calculateOverlap(bbox window)
                    area = area + overlap
                )
            )
        )
        
        area
    )
)
```

---

## 보고서 생성

### 레시피 8: 레이아웃 통계 보고서

```skill
procedure(generateLayoutReport(cvId outputFile)
    let((port stats)
        port = outfile(outputFile "w")
        
        fprintf(port "====================================\n")
        fprintf(port "    레이아웃 통계 보고서\n")
        fprintf(port "====================================\n\n")
        
        fprintf(port "셀 정보:\n")
        fprintf(port "  라이브러리: %s\n" cvId~>libName)
        fprintf(port "  셀: %s\n" cvId~>cellName)
        fprintf(port "  뷰: %s\n" cvId~>viewName)
        fprintf(port "\n")
        
        ; 바운딩 박스
        let((bbox width height)
            bbox = dbComputeBBox(cvId)
            width = xCoord(cadr(bbox)) - xCoord(car(bbox))
            height = yCoord(cadr(bbox)) - yCoord(car(bbox))
            
            fprintf(port "바운딩 박스:\n")
            fprintf(port "  크기: %.2f x %.2f um\n" width height)
            fprintf(port "  면적: %.2f um²\n" width * height)
            fprintf(port "\n")
        )
        
        ; 레이어별 통계
        stats = collectLayerStats(cvId)
        fprintf(port "레이어별 통계:\n")
        fprintf(port "%-20s %10s %15s\n" 
            "Layer" "Count" "Total Area")
        fprintf(port "%-20s %10s %15s\n"
            "----" "-----" "----------")
        
        foreach(layer stats
            fprintf(port "%-20s %10d %15.2f\n"
                layer
                stats[layer]["count"]
                stats[layer]["area"]
            )
        )
        
        ; 인스턴스 통계
        fprintf(port "\n인스턴스 통계:\n")
        fprintf(port "  총 인스턴스: %d\n" 
            length(cvId~>instances))
        
        close(port)
        printf("보고서 생성: %s\n" outputFile)
    )
)

procedure(collectLayerStats(cvId)
    let((stats layer area)
        stats = makeTable('stats nil)
        
        foreach(shape cvId~>shapes
            layer = sprintf(nil "%d:%s"
                shape~>layerNum
                shape~>purpose
            )
            
            unless(stats[layer]
                stats[layer] = makeTable('layerStats nil)
                stats[layer]["count"] = 0
                stats[layer]["area"] = 0.0
            )
            
            stats[layer]["count"] = stats[layer]["count"] + 1
            
            area = calculateShapeArea(shape)
            stats[layer]["area"] = stats[layer]["area"] + area
        )
        
        stats
    )
)

procedure(calculateShapeArea(shape)
    let((bbox width height)
        bbox = dbGetBBox(shape)
        width = xCoord(cadr(bbox)) - xCoord(car(bbox))
        height = yCoord(cadr(bbox)) - yCoord(car(bbox))
        width * height
    )
)
```

### 레시피 9: HTML 보고서 생성

```skill
procedure(generateHTMLReport(cvId outputFile)
    let((port)
        port = outfile(outputFile "w")
        
        ; HTML 헤더
        fprintf(port "<!DOCTYPE html>\n")
        fprintf(port "<html>\n<head>\n")
        fprintf(port "<title>Layout Report</title>\n")
        fprintf(port "<style>\n")
        fprintf(port "body { font-family: Arial; margin: 20px; }\n")
        fprintf(port "table { border-collapse: collapse; width: 100%%; }\n")
        fprintf(port "th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }\n")
        fprintf(port "th { background-color: #4CAF50; color: white; }\n")
        fprintf(port "</style>\n")
        fprintf(port "</head>\n<body>\n")
        
        fprintf(port "<h1>레이아웃 보고서</h1>\n")
        fprintf(port "<h2>%s/%s/%s</h2>\n"
            cvId~>libName cvId~>cellName cvId~>viewName)
        
        ; 통계 테이블
        fprintf(port "<h3>레이어 통계</h3>\n")
        fprintf(port "<table>\n")
        fprintf(port "<tr><th>Layer</th><th>Count</th><th>Area</th></tr>\n")
        
        let((stats)
            stats = collectLayerStats(cvId)
            foreach(layer stats
                fprintf(port "<tr><td>%s</td><td>%d</td><td>%.2f</td></tr>\n"
                    layer
                    stats[layer]["count"]
                    stats[layer]["area"]
                )
            )
        )
        
        fprintf(port "</table>\n")
        
        ; HTML 푸터
        fprintf(port "</body>\n</html>\n")
        
        close(port)
        printf("HTML 보고서 생성: %s\n" outputFile)
    )
)
```

---

## 데이터 추출

### 레시피 10: 네트리스트 추출

```skill
procedure(extractNetlist(cvId outputFile)
    let((port nets)
        port = outfile(outputFile "w")
        
        fprintf(port "* Netlist extracted from %s/%s\n"
            cvId~>libName cvId~>cellName)
        fprintf(port "* Date: %s\n\n" getCurrentTime())
        
        ; 인스턴스 추출
        foreach(inst cvId~>instances
            let((instName masterName)
                instName = inst~>name
                masterName = inst~>master~>cellName
                
                fprintf(port "X%s " instName)
                
                ; 핀 연결 (간단화된 버전)
                fprintf(port "... %s\n" masterName)
            )
        )
        
        fprintf(port "\n.END\n")
        close(port)
        
        printf("네트리스트 추출: %s\n" outputFile)
    )
)
```

### 레시피 11: 좌표 데이터 추출

```skill
procedure(extractCoordinates(cvId layer outputFile)
    let((port)
        port = outfile(outputFile "w")
        
        fprintf(port "# Layer: %d:%s\n" 
            car(layer) cadr(layer))
        fprintf(port "# Format: x1,y1,x2,y2\n\n")
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                
                let((bbox)
                    bbox = dbGetBBox(shape)
                    fprintf(port "%.3f,%.3f,%.3f,%.3f\n"
                        xCoord(car(bbox))
                        yCoord(car(bbox))
                        xCoord(cadr(bbox))
                        yCoord(cadr(bbox))
                    )
                )
            )
        )
        
        close(port)
        printf("좌표 데이터 추출: %s\n" outputFile)
    )
)
```

---

## 유틸리티 함수

### 레시피 12: 레이어 복사

```skill
procedure(copyLayer(cvId fromLayer toLayer)
    let((count)
        count = 0
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(fromLayer) &&
                 shape~>purpose == cadr(fromLayer)
                
                let((newShape)
                    newShape = dbCopyFig(shape cvId 0:0)
                    newShape~>layerNum = car(toLayer)
                    newShape~>purpose = cadr(toLayer)
                    count = count + 1
                )
            )
        )
        
        printf("레이어 복사: %d개 도형\n" count)
        count
    )
)
```

### 레시피 13: 도형 필터링

```skill
; 조건에 맞는 도형만 선택
procedure(filterShapes(cvId filterFunc)
    let((result)
        result = nil
        
        foreach(shape cvId~>shapes
            when(funcall(filterFunc shape)
                result = cons(shape result)
            )
        )
        
        result
    )
)

; 사용 예: 면적이 1.0 이상인 도형만
procedure(exampleFilter()
    let((cvId largeShapes)
        cvId = dbOpenCellViewByType("lib" "cell" "layout")
        
        largeShapes = filterShapes(cvId
            lambda((shape)
                calculateShapeArea(shape) >= 1.0
            )
        )
        
        printf("큰 도형: %d개\n" length(largeShapes))
        
        dbClose(cvId)
    )
)
```

### 레시피 14: 일괄 변환

```skill
; 모든 도형에 함수 적용
procedure(transformAllShapes(cvId layer transformFunc)
    let((count)
        count = 0
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer) &&
                 shape~>purpose == cadr(layer)
                
                funcall(transformFunc shape)
                count = count + 1
            )
        )
        
        printf("변환 완료: %d개 도형\n" count)
        count
    )
)

; 사용 예: 모든 도형을 10% 축소
procedure(exampleTransform()
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout" nil "a")
        
        transformAllShapes(cvId list(40 "drawing")
            lambda((shape)
                let((transform)
                    transform = dbCreateTransform(
                        0:0 "R0" 0.9  ; 90% 스케일
                    )
                    dbTransformFig(shape transform)
                )
            )
        )
        
        dbSave(cvId)
        dbClose(cvId)
    )
)
```

---

## 통합 예제

### 완전한 레이아웃 생성 스크립트

```skill
/*
 * 완전한 레이아웃 자동 생성 예제
 * 트랜지스터 배열 + 가드링 + 컨택
 */
procedure(createCompleteLayout()
    let((cvId libName cellName)
        libName = "myLib"
        cellName = "autoLayout"
        
        ; 새 셀 생성
        cvId = dbOpenCellViewByType(
            libName cellName "layout" 
            "maskLayout" "w"
        )
        
        printf("\n=== 레이아웃 자동 생성 시작 ===\n")
        
        ; 1. 트랜지스터 배열 생성
        printf("1. 트랜지스터 생성...\n")
        for(i 0 4
            createParametricTransistor(cvId
                sprintf(nil "M%d" i+1)
                ?width 2.0
                ?length 0.18
                ?fingers 4
                ?x i*15
                ?y 0
            )
        )
        
        ; 2. 메탈 라우팅
        printf("2. 라우팅...\n")
        routeManhattan(cvId
            list(40 "drawing")
            0:10
            60:10
            0.5
        )
        
        ; 3. 컨택 추가
        printf("3. 컨택 추가...\n")
        fillContactArray(cvId
            list(30 "drawing")
            list(20:5 40:8)
            0.5
            0.5
        )
        
        ; 4. 가드링
        printf("4. 가드링 생성...\n")
        createGuardRing(cvId
            list(20 "drawing")
            ?margin 5.0
            ?width 2.0
        )
        
        ; 5. 저장
        dbSave(cvId)
        
        ; 6. 보고서 생성
        printf("5. 보고서 생성...\n")
        generateLayoutReport(cvId "layout_report.txt")
        generateHTMLReport(cvId "layout_report.html")
        
        dbClose(cvId)
        
        printf("\n=== 완료! ===\n")
        printf("셀: %s/%s/layout\n" libName cellName)
    )
)
```

---

## 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 문법
- [[Virtuoso Skill 고급 기법]] - 고급 기능
- [[KLayout_XSection_Manual/]] - KLayout 참고

---

**팁**: 이 레시피들을 개인 라이브러리 파일(.il)로 저장해서 재사용하세요!
