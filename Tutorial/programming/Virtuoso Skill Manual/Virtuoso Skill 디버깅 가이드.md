# Virtuoso SKILL 디버깅 완전 가이드

## 목차
- [[#디버깅 기초]]
- [[#일반적인 에러]]
- [[#디버깅 도구]]
- [[#고급 디버깅 기법]]
- [[#성능 프로파일링]]
- [[#트러블슈팅 체크리스트]]

---

## 디버깅 기초

### 기본 출력 함수

```skill
; 간단한 출력
println("Hello")
println(myVariable)

; 포맷 출력
printf("값: %d\n" 123)
printf("실수: %.2f\n" 3.14159)
printf("문자열: %s\n" "test")
printf("리스트: %L\n" '(1 2 3))

; 여러 값 출력
printf("x=%d, y=%d\n" x y)
```

### 변수 타입 확인

```skill
; 타입 확인
type(value)

; 가능한 타입들
; "fixnum"    - 정수
; "flonum"    - 실수
; "string"    - 문자열
; "symbol"    - 심볼
; "list"      - 리스트
; "dbobject"  - 데이터베이스 객체

; 타입 체크 함수
procedure(checkType(value)
    let((t)
        t = type(value)
        printf("Type: %s, Value: %L\n" t value)
        t
    )
)
```

### 조건부 디버그 출력

```skill
let((DEBUG_MODE)
    DEBUG_MODE = t  ; 디버그 모드 활성화
    
    procedure(debug(msg @rest args)
        when(DEBUG_MODE
            apply(printf append(list(msg) args))
        )
    )
    
    procedure(setDebugMode(enable)
        DEBUG_MODE = enable
    )
)

; 사용
debug("변수 값: %d\n" myVar)
```

---

## 일반적인 에러

### 에러 1: "unbound variable"

**원인**: 변수가 선언되지 않음

```skill
; 잘못된 코드
println(myVar)  ; myVar가 정의되지 않음

; 올바른 코드
let((myVar)
    myVar = 10
    println(myVar)
)

; 또는 전역 변수로
myVar = 10
println(myVar)
```

**디버깅 팁**:
```skill
; 변수가 정의되었는지 확인
procedure(isDefined(varName)
    errset(
        eval(varName)
        t
    )
)
```

### 에러 2: "bad argument type"

**원인**: 함수에 잘못된 타입 전달

```skill
; 잘못된 코드
x = "10"
y = x + 5  ; 문자열 + 숫자

; 올바른 코드
x = "10"
y = atoi(x) + 5  ; 문자열을 숫자로 변환

; 또는 타입 체크
procedure(safeAdd(a b)
    cond(
        ((and (numberp a) (numberp b))
            a + b
        )
        (t
            error("숫자가 아닙니다: %L, %L" a b)
        )
    )
)
```

**타입 체크 함수들**:
```skill
numberp(value)    ; 숫자인지
stringp(value)    ; 문자열인지
listp(value)      ; 리스트인지
symbolp(value)    ; 심볼인지
```

### 에러 3: "attempt to apply non-procedure"

**원인**: 함수가 아닌 것을 함수처럼 호출

```skill
; 잘못된 코드
myVar = 10
result = myVar(5)  ; myVar는 숫자, 함수가 아님

; 올바른 코드
procedure(myFunc(x)
    x * 2
)
result = myFunc(5)
```

### 에러 4: "nil isn't a valid cellview"

**원인**: 셀뷰를 열지 못함

```skill
; 잘못된 코드
cvId = dbOpenCellViewByType("lib" "cell" "layout")
dbSave(cvId)  ; cvId가 nil이면 에러

; 올바른 코드
cvId = dbOpenCellViewByType("lib" "cell" "layout")
if(cvId then
    ; 작업 수행
    dbSave(cvId)
    dbClose(cvId)
else
    error("셀뷰를 열 수 없습니다")
)
```

**셀뷰 열기 디버깅**:
```skill
procedure(debugOpenCellView(lib cell view)
    let((cvId)
        printf("셀뷰 열기 시도:\n")
        printf("  라이브러리: %s\n" lib)
        printf("  셀: %s\n" cell)
        printf("  뷰: %s\n" view)
        
        cvId = dbOpenCellViewByType(lib cell view)
        
        if(cvId then
            printf("  성공!\n")
        else
            printf("  실패!\n")
            printf("가능한 원인:\n")
            printf("  1. 라이브러리가 없음\n")
            printf("  2. 셀이 없음\n")
            printf("  3. 뷰가 없음\n")
            printf("  4. 권한 문제\n")
        )
        
        cvId
    )
)
```

### 에러 5: "divide by zero"

```skill
; 잘못된 코드
result = x / y  ; y가 0이면 에러

; 올바른 코드
procedure(safeDivide(x y)
    if(y != 0 then
        x / y
    else
        printf("경고: 0으로 나누기\n")
        nil
    )
)
```

### 에러 6: "index out of range"

```skill
; 잘못된 코드
list = '(1 2 3)
value = nth(10 list)  ; 인덱스 초과

; 올바른 코드
procedure(safeNth(n list)
    if(n >= 0 && n < length(list) then
        nth(n list)
    else
        printf("인덱스 범위 초과: %d (0-%d)\n" 
            n length(list)-1)
        nil
    )
)
```

---

## 디버깅 도구

### 1. 스택 트레이스

```skill
; 에러 발생 시 스택 확인
errTrace()

; 사용 예
procedure(problematicFunc()
    error("의도적 에러")
)

procedure(caller()
    problematicFunc()
)

; 호출하면 스택 트레이스 출력
errset(
    caller()
)
errTrace()
```

### 2. 함수 추적 (Tracing)

```skill
; 함수 호출 추적
procedure(traceFunction(funcName)
    let((origFunc wrapper)
        origFunc = get(funcName 'expr)
        
        wrapper = procedure((@rest args)
            printf("→ 호출: %s(%L)\n" funcName args)
            let((result startTime endTime)
                startTime = getCurrentTime()
                result = apply(origFunc args)
                endTime = getCurrentTime()
                printf("← 반환: %s → %L (%.3fs)\n" 
                    funcName result endTime-startTime)
                result
            )
        )
        
        put(funcName 'origExpr origFunc)
        put(funcName 'expr wrapper)
        
        printf("추적 시작: %s\n" funcName)
    )
)

; 추적 해제
procedure(untraceFunction(funcName)
    let((origFunc)
        origFunc = get(funcName 'origExpr)
        when(origFunc
            put(funcName 'expr origFunc)
            remprop(funcName 'origExpr)
            printf("추적 해제: %s\n" funcName)
        )
    )
)
```

### 3. 브레이크포인트

```skill
; 브레이크포인트 설정
procedure(breakpoint(msg)
    printf("\n=== BREAKPOINT ===\n")
    printf("%s\n" msg)
    printf("계속하려면 Enter를 누르세요...\n")
    gets(input)
)

; 사용 예
procedure(myFunction(x)
    printf("시작: x=%d\n" x)
    x = x * 2
    breakpoint("x를 2배로 만듦")
    x = x + 10
    breakpoint("x에 10을 더함")
    x
)
```

### 4. 로그 파일

```skill
let((logPort logFile)
    logFile = "debug.log"
    
    procedure(openLog()
        logPort = outfile(logFile "a")
        when(logPort
            fprintf(logPort "\n=== Log Start: %s ===\n" 
                getCurrentTime())
        )
    )
    
    procedure(log(msg @rest args)
        unless(logPort
            openLog()
        )
        
        when(logPort
            apply(fprintf append(list(logPort msg) args))
        )
        
        ; 콘솔에도 출력
        apply(printf append(list(msg) args))
    )
    
    procedure(closeLog()
        when(logPort
            fprintf(logPort "=== Log End ===\n")
            close(logPort)
            logPort = nil
        )
    )
)

; 사용
openLog()
log("작업 시작\n")
log("변수: %d\n" myVar)
closeLog()
```

### 5. 어설션 (Assertion)

```skill
procedure(assert(condition msg @rest args)
    unless(condition
        printf("어설션 실패: ")
        apply(printf append(list(msg) args))
        printf("\n")
        errTrace()
        error("어설션 실패")
    )
)

; 사용 예
procedure(divide(x y)
    assert(y != 0 "0으로 나눌 수 없습니다")
    x / y
)
```

---

## 고급 디버깅 기법

### 변수 감시 (Watch)

```skill
let((watchList)
    watchList = makeTable('watchList nil)
    
    procedure(watch(varName)
        watchList[varName] = eval(varName)
        printf("감시 시작: %s = %L\n" varName eval(varName))
    )
    
    procedure(checkWatch(varName)
        let((oldValue newValue)
            oldValue = watchList[varName]
            newValue = eval(varName)
            
            unless(equal(oldValue newValue)
                printf("변수 변경: %s\n" varName)
                printf("  이전: %L\n" oldValue)
                printf("  현재: %L\n" newValue)
                watchList[varName] = newValue
            )
        )
    )
    
    procedure(checkAllWatch()
        foreach(varName watchList
            checkWatch(varName)
        )
    )
)
```

### 메모리 누수 감지

```skill
let((allocatedObjects)
    allocatedObjects = nil
    
    procedure(trackAlloc(objType objId)
        allocatedObjects = cons(
            list('type objType 'id objId 'time getCurrentTime())
            allocatedObjects
        )
        printf("할당: %s (%L)\n" objType objId)
    )
    
    procedure(trackFree(objId)
        allocatedObjects = filter(
            lambda((obj) 
                not(equal(cadr(assoc('id obj)) objId))
            )
            allocatedObjects
        )
        printf("해제: %L\n" objId)
    )
    
    procedure(checkLeaks()
        printf("\n=== 메모리 누수 체크 ===\n")
        printf("미해제 객체: %d\n" length(allocatedObjects))
        
        foreach(obj allocatedObjects
            printf("  타입: %s, ID: %L, 시간: %d\n"
                cadr(assoc('type obj))
                cadr(assoc('id obj))
                cadr(assoc('time obj))
            )
        )
    )
)
```

### 조건부 디버깅

```skill
let((debugConditions)
    debugConditions = makeTable('debugConditions nil)
    
    procedure(setDebugCondition(name condition)
        debugConditions[name] = condition
    )
    
    procedure(debugIf(name msg @rest args)
        when(debugConditions[name] && 
             funcall(debugConditions[name]))
            apply(printf append(list(msg) args))
        )
    )
)

; 사용 예
setDebugCondition("largeValues" lambda(() x > 100))
debugIf("largeValues" "큰 값: %d\n" x)
```

### 코드 커버리지

```skill
let((coverage)
    coverage = makeTable('coverage nil)
    
    procedure(markExecuted(funcName line)
        let((key)
            key = sprintf(nil "%s:%d" funcName line)
            coverage[key] = t
        )
    )
    
    procedure(showCoverage()
        let((count)
            count = 0
            printf("\n=== 코드 커버리지 ===\n")
            
            foreach(key coverage
                count = count + 1
                printf("%s\n" key)
            )
            
            printf("실행된 라인: %d\n" count)
        )
    )
)
```

---

## 성능 프로파일링

### 실행 시간 측정

```skill
procedure(measureTime(func @rest args)
    let((startTime endTime result elapsed)
        startTime = getCurrentTime()
        result = apply(func args)
        endTime = getCurrentTime()
        elapsed = endTime - startTime
        
        printf("실행 시간: %.3f초\n" elapsed)
        
        list('result result 'time elapsed)
    )
)

; 사용
measureTime(myFunction 10 20)
```

### 함수별 프로파일링

```skill
let((profileData)
    profileData = makeTable('profileData nil)
    
    procedure(profileFunction(funcName)
        let((origFunc wrapper)
            origFunc = get(funcName 'expr)
            
            ; 프로파일 데이터 초기화
            profileData[funcName] = list(
                'calls 0
                'totalTime 0.0
                'minTime 999999.0
                'maxTime 0.0
            )
            
            wrapper = procedure((@rest args)
                let((startTime endTime elapsed data)
                    startTime = getCurrentTime()
                    result = apply(origFunc args)
                    endTime = getCurrentTime()
                    elapsed = endTime - startTime
                    
                    ; 통계 업데이트
                    data = profileData[funcName]
                    data->calls = data->calls + 1
                    data->totalTime = data->totalTime + elapsed
                    data->minTime = min(data->minTime elapsed)
                    data->maxTime = max(data->maxTime elapsed)
                    
                    result
                )
            )
            
            put(funcName 'origExpr origFunc)
            put(funcName 'expr wrapper)
        )
    )
    
    procedure(showProfile(@optional funcName)
        if(funcName then
            ; 특정 함수 프로파일
            let((data)
                data = profileData[funcName]
                printf("\n=== Profile: %s ===\n" funcName)
                printf("호출 횟수: %d\n" data->calls)
                printf("총 시간: %.3f초\n" data->totalTime)
                printf("평균 시간: %.3f초\n" 
                    data->totalTime / data->calls)
                printf("최소 시간: %.3f초\n" data->minTime)
                printf("최대 시간: %.3f초\n" data->maxTime)
            )
        else
            ; 전체 프로파일
            printf("\n=== Profile Summary ===\n")
            printf("%-20s %10s %15s %15s\n" 
                "Function" "Calls" "Total(s)" "Avg(s)")
            printf("%-20s %10s %15s %15s\n"
                "--------" "-----" "--------" "------")
            
            foreach(funcName profileData
                let((data)
                    data = profileData[funcName]
                    printf("%-20s %10d %15.3f %15.6f\n"
                        funcName
                        data->calls
                        data->totalTime
                        data->totalTime / data->calls
                    )
                )
            )
        )
    )
    
    procedure(resetProfile()
        profileData = makeTable('profileData nil)
        printf("프로파일 데이터 초기화\n")
    )
)
```

### 메모리 사용량 프로파일링

```skill
procedure(profileMemory(func @rest args)
    let((before after delta result)
        ; 가비지 컬렉션 실행
        gc()
        
        ; 메모리 사용량 측정 (근사값)
        before = getCurrentTime()
        result = apply(func args)
        after = getCurrentTime()
        
        printf("메모리 프로파일:\n")
        printf("  실행 시간: %.3f초\n" after-before)
        printf("  (정확한 메모리 측정은 Virtuoso 툴 사용)\n")
        
        result
    )
)
```

### 병목 지점 찾기

```skill
procedure(findBottleneck(func @rest args)
    let((samples sampleCount interval)
        samples = makeTable('samples nil)
        sampleCount = 100
        interval = 0.01  ; 10ms
        
        ; 주기적으로 샘플링
        for(i 1 sampleCount
            let((location)
                ; 현재 실행 위치 (간소화)
                location = "function"
                
                unless(samples[location]
                    samples[location] = 0
                )
                samples[location] = samples[location] + 1
            )
        )
        
        ; 결과 출력
        printf("\n=== 병목 분석 ===\n")
        foreach(location samples
            printf("%s: %.1f%%\n" 
                location 
                100.0 * samples[location] / sampleCount)
        )
    )
)
```

---

## 트러블슈팅 체크리스트

### 스크립트가 실행되지 않을 때

```
□ 파일 경로가 정확한가?
□ 문법 오류가 없는가?
□ 괄호가 제대로 닫혔는가?
□ 필요한 라이브러리가 로드되었는가?
□ 권한 문제는 없는가?
```

### 결과가 예상과 다를 때

```
□ 변수 타입이 올바른가?
□ 변수 범위(scope)가 올바른가?
□ 연산 순서가 올바른가?
□ 부동소수점 오차는 없는가?
□ nil 체크를 했는가?
```

### 성능이 느릴 때

```
□ 불필요한 반복문은 없는가?
□ 파일을 여러 번 열고 닫는가?
□ 불필요한 출력문은 없는가?
□ 큰 리스트를 비효율적으로 처리하는가?
□ 캐싱을 활용할 수 있는가?
```

### 메모리 문제

```
□ 셀뷰를 닫았는가?
□ 파일을 닫았는가?
□ 큰 데이터를 계속 쌓고 있는가?
□ 순환 참조는 없는가?
□ 가비지 컬렉션이 필요한가?
```

---

## 디버깅 워크플로우

### 단계별 디버깅

```skill
procedure(debugWorkflow()
    printf("=== 디버깅 워크플로우 ===\n")
    printf("1. 문제 재현\n")
    printf("2. 최소 재현 케이스 작성\n")
    printf("3. 가설 수립\n")
    printf("4. 디버그 출력 추가\n")
    printf("5. 테스트 및 검증\n")
    printf("6. 수정\n")
    printf("7. 재테스트\n")
)
```

### 체계적 디버깅 템플릿

```skill
/*
 * 디버깅 템플릿
 * 
 * 문제 설명:
 *   [문제가 무엇인지 설명]
 * 
 * 재현 방법:
 *   1. [단계 1]
 *   2. [단계 2]
 *   3. [단계 3]
 * 
 * 예상 결과:
 *   [어떻게 동작해야 하는지]
 * 
 * 실제 결과:
 *   [실제로 어떻게 동작하는지]
 * 
 * 시도한 해결책:
 *   - [시도 1]
 *   - [시도 2]
 * 
 * 해결 방법:
 *   [최종 해결 방법]
 */
```

---

## 유용한 디버깅 함수 모음

```skill
; 전체 디버그 유틸리티
let((DEBUG_ENABLED)
    DEBUG_ENABLED = t
    
    ; 디버그 출력
    procedure(dbg(msg @rest args)
        when(DEBUG_ENABLED
            printf("[DEBUG] ")
            apply(printf append(list(msg) args))
        )
    )
    
    ; 경고
    procedure(warn(msg @rest args)
        printf("[WARN] ")
        apply(printf append(list(msg) args))
    )
    
    ; 에러
    procedure(err(msg @rest args)
        printf("[ERROR] ")
        apply(printf append(list(msg) args))
    )
    
    ; 변수 덤프
    procedure(dump(name value)
        printf("[DUMP] %s = %L (type: %s)\n" 
            name value type(value))
    )
    
    ; 함수 진입/종료
    procedure(enter(funcName @rest args)
        dbg("→ %s(%L)\n" funcName args)
    )
    
    procedure(exit(funcName result)
        dbg("← %s = %L\n" funcName result)
    )
)
```

---

## 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 문법
- [[Virtuoso Skill 예제 모음]] - 예제 코드
- [[Virtuoso Skill 빠른참조]] - 빠른 참조

---

**디버깅은 프로그래밍의 50%입니다. 체계적으로 접근하세요!**
