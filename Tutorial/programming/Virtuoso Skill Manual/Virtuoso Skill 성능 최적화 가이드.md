# Virtuoso SKILL 성능 최적화 가이드

## 목차
- [[#성능 측정]]
- [[#데이터 구조 최적화]]
- [[#알고리즘 최적화]]
- [[#데이터베이스 접근 최적화]]
- [[#메모리 관리]]
- [[#병렬 처리]]
- [[#캐싱 전략]]
- [[#최적화 체크리스트]]

---

## 성능 측정

### 기본 측정

```skill
procedure(benchmark(name func @rest args)
    let((startTime endTime elapsed result)
        printf("\n=== Benchmark: %s ===\n" name)
        
        startTime = getCurrentTime()
        result = apply(func args)
        endTime = getCurrentTime()
        elapsed = endTime - startTime
        
        printf("실행 시간: %.3f초\n" elapsed)
        printf("결과: %L\n" result)
        
        list('time elapsed 'result result)
    )
)
```

### 비교 벤치마크

```skill
procedure(compare(name1 func1 name2 func2 @rest args)
    let((time1 time2 ratio)
        printf("\n=== 성능 비교 ===\n")
        
        time1 = cadr(apply(benchmark cons(name1 cons(func1 args))))
        time2 = cadr(apply(benchmark cons(name2 cons(func2 args))))
        
        ratio = if(time2 > 0 then time1/time2 else 0)
        
        printf("\n비교 결과:\n")
        if(ratio > 1 then
            printf("  %s가 %.2fx 빠름\n" name2 ratio)
        else if(ratio < 1 then
            printf("  %s가 %.2fx 빠름\n" name1 1.0/ratio)
        else
            printf("  비슷한 성능\n")
        ))
    )
)
```

---

## 데이터 구조 최적화

### 리스트 vs 배열

```skill
; 느림: 리스트 - O(n) 접근
procedure(slowAccess(n)
    let((lst)
        lst = nil
        for(i 0 n-1
            lst = cons(i lst)
        )
        nth(n/2 lst)
    )
)

; 빠름: 배열 - O(1) 접근
procedure(fastAccess(n)
    let((arr)
        arr = makeVector(n 0)
        for(i 0 n-1
            arr[i] = i
        )
        arr[n/2]
    )
)
```

### 리스트 연결 최적화

```skill
; 느림: append 반복 - O(n²)
procedure(slowBuild(n)
    let((result)
        result = nil
        for(i 0 n-1
            result = append(result list(i))  ; 매번 전체 복사
        )
        result
    )
)

; 빠름: cons + reverse - O(n)
procedure(fastBuild(n)
    let((result)
        result = nil
        for(i 0 n-1
            result = cons(i result)  ; O(1) 연산
        )
        reverse(result)
    )
)
```

### 해시 테이블 활용

```skill
; 느림: 리스트 검색 - O(n)
procedure(slowFind(key data)
    let((found)
        found = nil
        foreach(item data
            when(equal(car(item) key)
                found = cadr(item)
            )
        )
        found
    )
)

; 빠름: 해시 테이블 - O(1)
let((cache)
    cache = makeTable('cache nil)
    
    procedure(fastFind(key)
        cache[key]
    )
)
```

---

## 알고리즘 최적화

### 불필요한 계산 제거

```skill
; 느림: 반복 계산
procedure(slowCalc(n)
    let((sum)
        sum = 0
        for(i 1 n
            for(j 1 n
                sum = sum + sqrt(i*i + j*j)  ; 매번 계산
            )
        )
        sum
    )
)

; 빠름: 결과 재사용
procedure(fastCalc(n)
    let((sum sqrtCache)
        sqrtCache = makeVector(n*n 0)
        sum = 0
        
        ; 제곱근 미리 계산
        for(i 1 n
            for(j 1 n
                let((idx val)
                    idx = i*n + j
                    val = sqrt(i*i + j*j)
                    sqrtCache[idx] = val
                )
            )
        )
        
        ; 캐시 사용
        for(i 1 n
            for(j 1 n
                sum = sum + sqrtCache[i*n + j]
            )
        )
        sum
    )
)
```

### 조기 종료

```skill
; 느림: 모든 요소 검사
procedure(slowSearch(target list)
    let((found result)
        found = nil
        result = nil
        
        foreach(item list
            when(equal(item target)
                found = t
                result = item
            )
        )
        result
    )
)

; 빠름: 찾으면 즉시 반환
procedure(fastSearch(target list)
    let((result found)
        result = nil
        found = nil
        
        foreach(item list
            unless(found
                when(equal(item target)
                    result = item
                    found = t
                )
            )
        )
        result
    )
)
```

### 벡터 연산 활용

```skill
; 느림: 반복문으로 합계
procedure(slowSum(numList)
    let((sum)
        sum = 0
        foreach(num numList
            sum = sum + num
        )
        sum
    )
)

; 빠름: apply + plus
procedure(fastSum(numList)
    apply(plus numList)
)

; 더 많은 벡터 연산
procedure(vectorOps(list)
    list(
        'sum (apply plus list)
        'min (apply min list)
        'max (apply max list)
        'product (apply times list)
    )
)
```

---

## 데이터베이스 접근 최적화

### 파일 열기/닫기 최적화

```skill
; 느림: 매번 열고 닫기
procedure(slowDBAccess(n)
    for(i 1 n
        let((cvId)
            cvId = dbOpenCellViewByType("lib" "cell" "layout")
            ; 작업
            dbClose(cvId)
        )
    )
)

; 빠름: 한 번만 열고 닫기
procedure(fastDBAccess(n)
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout")
        
        for(i 1 n
            ; 작업
        )
        
        dbClose(cvId)
    )
)
```

### 트랜잭션 활용

```skill
; 느림: 매번 저장
procedure(slowTransaction(cvId operations)
    foreach(op operations
        funcall(op cvId)
        dbSave(cvId)  ; 매번 디스크 쓰기
    )
)

; 빠름: 배치 트랜잭션
procedure(fastTransaction(cvId operations)
    dbBeginTransaction(cvId)
    
    foreach(op operations
        funcall(op cvId)
    )
    
    dbEndTransaction(cvId)
    dbSave(cvId)  ; 한 번만 저장
)
```

### 선택적 로딩

```skill
; 느림: 모든 데이터 로드
procedure(slowLoad(cvId)
    let((allShapes)
        allShapes = nil
        
        foreach(shape cvId~>shapes
            allShapes = cons(shape allShapes)
        )
        
        allShapes
    )
)

; 빠름: 필요한 것만 로드
procedure(fastLoad(cvId layer)
    let((shapes)
        shapes = nil
        
        foreach(shape cvId~>shapes
            when(shape~>layerNum == car(layer)
                shapes = cons(shape shapes)
            )
        )
        
        shapes
    )
)
```

### 공간 인덱싱

```skill
; 느림: 선형 검색
procedure(slowFindNear(cvId targetPt radius)
    let((result)
        result = nil
        
        foreach(shape cvId~>shapes
            let((center dist)
                center = getCenter(shape)
                dist = distance(center targetPt)
                
                when(dist <= radius
                    result = cons(shape result)
                )
            )
        )
        
        result
    )
)

; 빠름: 그리드 기반 검색
let((spatialGrid)
    spatialGrid = makeTable('spatialGrid nil)
    
    procedure(buildSpatialIndex(cvId gridSize)
        spatialGrid = makeTable('spatialGrid nil)
        
        foreach(shape cvId~>shapes
            let((center gridX gridY key)
                center = getCenter(shape)
                gridX = floor(xCoord(center) / gridSize)
                gridY = floor(yCoord(center) / gridSize)
                key = sprintf(nil "%d,%d" gridX gridY)
                
                unless(spatialGrid[key]
                    spatialGrid[key] = nil
                )
                
                spatialGrid[key] = cons(shape spatialGrid[key])
            )
        )
    )
    
    procedure(fastFindNear(targetPt radius gridSize)
        let((result gridX gridY)
            result = nil
            gridX = floor(xCoord(targetPt) / gridSize)
            gridY = floor(yCoord(targetPt) / gridSize)
            
            ; 주변 그리드만 검색
            for(dx -1 1
                for(dy -1 1
                    let((key shapes)
                        key = sprintf(nil "%d,%d" gridX+dx gridY+dy)
                        shapes = spatialGrid[key]
                        
                        when(shapes
                            foreach(shape shapes
                                let((center dist)
                                    center = getCenter(shape)
                                    dist = distance(center targetPt)
                                    
                                    when(dist <= radius
                                        result = cons(shape result)
                                    )
                                )
                            )
                        )
                    )
                )
            )
            
            result
        )
    )
)
```

---

## 메모리 관리

### 메모리 누수 방지

```skill
; 나쁨: 셀뷰를 닫지 않음
procedure(badMemory()
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout")
        ; 작업
        ; cvId를 닫지 않음 - 메모리 누수!
    )
)

; 좋음: 항상 닫기
procedure(goodMemory()
    let((cvId)
        cvId = dbOpenCellViewByType("lib" "cell" "layout")
        
        errset(
            ; 작업
            t
        )
        
        ; 에러가 나도 반드시 닫기
        when(cvId
            dbClose(cvId)
        )
    )
)
```

### 큰 데이터 처리

```skill
; 나쁨: 모든 데이터를 메모리에
procedure(badLargeData(filename)
    let((allLines)
        allLines = nil
        port = infile(filename)
        
        ; 전체 파일을 메모리에 로드
        while(gets(line port)
            allLines = append(allLines list(line))
        )
        
        close(port)
        ; 메모리 부족 위험!
        
        allLines
    )
)

; 좋음: 스트리밍 처리
procedure(goodLargeData(filename processFunc)
    let((port line count)
        port = infile(filename)
        count = 0
        
        when(port
            ; 한 줄씩 처리
            while(gets(line port)
                funcall(processFunc line)
                count = count + 1
                
                ; 진행 상황 출력
                when(remainder(count 1000) == 0
                    printf("처리: %d줄\n" count)
                )
            )
            close(port)
        )
        
        count
    )
)
```

### 가비지 컬렉션

```skill
; 명시적 GC 호출
procedure(processWithGC(operations)
    let((count)
        count = 0
        
        foreach(op operations
            funcall(op)
            count = count + 1
            
            ; 주기적으로 GC 실행
            when(remainder(count 100) == 0
                gc()
                printf("GC 실행 (%d operations)\n" count)
            )
        )
    )
)
```

---

## 캐싱 전략

### 단순 캐시

```skill
let((simpleCache)
    simpleCache = makeTable('simpleCache nil)
    
    procedure(cachedCompute(input)
        let((result)
            result = simpleCache[input]
            
            unless(result
                ; 복잡한 계산
                result = expensiveCalculation(input)
                simpleCache[input] = result
            )
            
            result
        )
    )
    
    procedure(clearCache()
        simpleCache = makeTable('simpleCache nil)
    )
)
```

### LRU 캐시

```skill
let((lruCache lruList maxSize)
    maxSize = 100
    lruCache = makeTable('lruCache nil)
    lruList = nil
    
    procedure(lruGet(key)
        let((value)
            value = lruCache[key]
            
            when(value
                ; LRU 리스트 업데이트
                lruList = cons(key 
                    filter(lambda((k) not(equal(k key))) lruList)
                )
            )
            
            value
        )
    )
    
    procedure(lruPut(key value)
        ; 캐시 크기 체크
        when(length(lruList) >= maxSize
            let((oldest)
                oldest = car(reverse(lruList))
                lruCache[oldest] = nil
                lruList = filter(lambda((k) not(equal(k oldest))) lruList)
            )
        )
        
        lruCache[key] = value
        lruList = cons(key lruList)
    )
)
```

### 계층적 캐시

```skill
let((l1Cache l2Cache)
    l1Cache = makeTable('l1Cache nil)  ; 빠르지만 작음
    l2Cache = makeTable('l2Cache nil)  ; 느리지만 큼
    
    procedure(hierarchicalGet(key)
        let((value)
            ; L1 캐시 확인
            value = l1Cache[key]
            
            unless(value
                ; L2 캐시 확인
                value = l2Cache[key]
                
                when(value
                    ; L1으로 승격
                    l1Cache[key] = value
                )
            )
            
            value
        )
    )
)
```

---

## 최적화 체크리스트

### 코드 레벨

```
□ 불필요한 계산 제거
□ 반복문 최소화
□ 벡터 연산 활용
□ 조기 종료 구현
□ 적절한 데이터 구조 선택
```

### 데이터베이스

```
□ 파일을 한 번만 열고 닫기
□ 트랜잭션 활용
□ 필요한 데이터만 로드
□ 공간 인덱싱 활용
□ 배치 처리
```

### 메모리

```
□ 리소스 해제 확인
□ 큰 데이터 스트리밍 처리
□ 주기적 GC
□ 메모리 누수 체크
□ 불필요한 복사 제거
```

### 캐싱

```
□ 반복 계산 캐싱
□ 적절한 캐시 크기
□ 캐시 무효화 전략
□ 다단계 캐싱 고려
```

---

## 최적화 워크플로우

### 1단계: 측정

```skill
; 현재 성능 측정
procedure(measureBaseline()
    benchmark("baseline" 'myFunction args)
)
```

### 2단계: 병목 지점 찾기

```skill
; 각 부분 프로파일링
profileFunction('functionA)
profileFunction('functionB)
showProfile()
```

### 3단계: 최적화

```skill
; 가장 느린 부분부터 최적화
procedure(optimizeSlowest()
    ; 최적화 적용
)
```

### 4단계: 재측정

```skill
; 개선 효과 확인
compare("이전" 'oldVersion "최적화" 'newVersion args)
```

### 5단계: 반복

```
더 이상 개선이 필요 없을 때까지 반복
```

---

## 실전 최적화 예제

### 예제 1: 레이어 통계

```skill
; 최적화 전
procedure(slowLayerStats(cvId)
    let((stats)
        stats = makeTable('stats nil)
        
        foreach(shape cvId~>shapes
            let((layer)
                layer = sprintf(nil "%d:%s" 
                    shape~>layerNum shape~>purpose)
                
                unless(stats[layer]
                    stats[layer] = 0
                )
                
                stats[layer] = stats[layer] + 1
            )
        )
        
        stats
    )
)

; 최적화 후
procedure(fastLayerStats(cvId)
    let((stats layerShapes)
        stats = makeTable('stats nil)
        layerShapes = makeTable('layerShapes nil)
        
        ; 레이어별로 그룹화
        foreach(shape cvId~>shapes
            let((layerNum)
                layerNum = shape~>layerNum
                
                unless(layerShapes[layerNum]
                    layerShapes[layerNum] = nil
                )
                
                layerShapes[layerNum] = cons(shape layerShapes[layerNum])
            )
        )
        
        ; 카운트
        foreach(layerNum layerShapes
            stats[layerNum] = length(layerShapes[layerNum])
        )
        
        stats
    )
)
```

### 예제 2: 대량 도형 생성

```skill
; 최적화 전
procedure(slowCreate(cvId layer n)
    for(i 0 n-1
        dbCreateRect(cvId layer
            list(i:0 (i+1):1)
        )
        dbSave(cvId)  ; 매번 저장
    )
)

; 최적화 후
procedure(fastCreate(cvId layer n)
    dbBeginTransaction(cvId)
    
    for(i 0 n-1
        dbCreateRect(cvId layer
            list(i:0 (i+1):1)
        )
    )
    
    dbEndTransaction(cvId)
    dbSave(cvId)  ; 한 번만 저장
)
```

---

## 성능 목표 설정

### 응답 시간 가이드라인

```
즉시 (< 0.1초): 사용자 입력 응답
빠름 (< 1초): 간단한 계산
보통 (< 10초): 복잡한 분석
느림 (< 60초): 대량 데이터 처리
매우 느림 (> 60초): 배치 작업
```

### 처리량 목표

```
작은 파일 (< 1MB): 초당 수백 개
중간 파일 (1-10MB): 초당 수십 개
큰 파일 (> 10MB): 초당 몇 개
```

---

## 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 문법
- [[Virtuoso Skill 디버깅 가이드]] - 디버깅 방법
- [[Virtuoso Skill 예제 모음]] - 최적화 예제

---

**"조기 최적화는 모든 악의 근원이다" - 하지만 성능 고려는 설계 단계부터!**
