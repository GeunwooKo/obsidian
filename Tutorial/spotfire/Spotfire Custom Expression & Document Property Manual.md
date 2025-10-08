# Spotfire Custom Expression & Document Property 매뉴얼

> 📝 **참고**: 이 매뉴얼에는 스크린샷 위치가 표시되어 있습니다. 실제 Spotfire 화면을 캡처하여 해당 위치에 이미지를 추가하시면 더욱 완성도 높은 매뉴얼이 됩니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Scatter Plot Manual]] | [[Spotfire Line Chart Manual]] | [[Spotfire Bar Chart Manual]] | [[Spotfire Heatmap Manual]] | [[Spotfire Box Plot Manual]] | [[Spotfire Cross Table Manual]] | [[Spotfire Information Link+ Impala SQL Guide]] | [[spotfire/README]]

## 개요
Custom Expression과 Document Property는 Spotfire의 고급 기능으로, 복잡한 계산과 동적 분석을 가능하게 합니다. Custom Expression을 통해 기존 데이터를 변환하고 새로운 인사이트를 도출할 수 있으며, Document Property를 통해 대시보드 전체의 동적 제어가 가능합니다.

*🖼️ [이미지 추가 위치: Custom Expression 및 Document Property 개요 화면]*

# Part 1: Custom Expression

## Custom Expression 기본 개념

### 1. Custom Expression이란?
```
정의: 기존 데이터 컬럼을 기반으로 새로운 계산된 컬럼을 생성
용도: 데이터 변환, 조건부 로직, 집계 계산, 분석 지표 생성
위치: Data → Column Properties → Insert Calculated Column
```

### 2. Expression 생성 방법
- **Insert** 메뉴 → **Calculated Column** 선택
- **Data** 패널에서 우클릭 → **Insert Calculated Column**
- **Column Properties** → **Insert Calculated Column**

*🖼️ [이미지 추가 위치: Calculated Column 생성 화면]*

### 3. Expression Editor 구성
```
Components:
- Functions: 사용 가능한 함수 목록
- Columns: 데이터 테이블의 컬럼
- Parameters: Document Property 및 변수
- Expression Field: 수식 입력 영역
- Validation: 문법 검사 및 오류 표시
```

## 기본 Expression 문법

### 1. 기본 연산자
```
산술 연산자:
+ (더하기)    - (빼기)
* (곱하기)    / (나누기)
% (나머지)    ^ (거듭제곱)

비교 연산자:
= (같음)      != <> (다름)
< (작음)      > (큼)
<= (작거나 같음) >= (크거나 같음)

논리 연산자:
AND (그리고)  OR (또는)
NOT (부정)    

문자열 연산자:
+ (문자열 결합)
```

### 2. 기본 문법 구조
```
조건부 표현:
If([조건], [True일때 값], [False일때 값])

Case 문:
Case 
  When [조건1] Then [값1]
  When [조건2] Then [값2]
  Else [기본값]
End

함수 호출:
FunctionName([매개변수1], [매개변수2], ...)
```

## 주요 함수 카테고리

### 1. 수학 함수
```
기본 수학:
Abs([값])                  - 절대값
Round([값], [소수점자리])   - 반올림
Ceiling([값])              - 올림
Floor([값])                - 내림
Sqrt([값])                 - 제곱근
Power([밑], [지수])        - 거듭제곱

통계 함수:
Min([값1], [값2])          - 최소값
Max([값1], [값2])          - 최대값
Average([값1], [값2])      - 평균

예시:
Round([Yield_Rate], 2)     - 수율을 소수점 2자리로 반올림
Abs([Target] - [Actual])   - 목표와 실제값의 절대 차이
```

### 2. 문자열 함수
```
기본 문자열:
Left([문자열], [길이])     - 왼쪽에서 N글자
Right([문자열], [길이])    - 오른쪽에서 N글자
Mid([문자열], [시작], [길이]) - 중간 부분 추출
Len([문자열])              - 문자열 길이
Upper([문자열])            - 대문자 변환
Lower([문자열])            - 소문자 변환
Trim([문자열])             - 공백 제거

검색 및 치환:
Find([찾을문자], [대상문자열]) - 문자 위치 찾기
Replace([문자열], [찾을것], [바꿀것]) - 문자 치환
Concatenate([문자1], [문자2]) - 문자열 결합

예시:
Left([Lot_ID], 8)          - Lot ID 앞 8자리만 추출
[Product_Type] + "_" + [Line_Name] - 제품타입과 라인명 결합
```

### 3. 날짜/시간 함수
```
날짜 추출:
Year([날짜])               - 연도 추출
Month([날짜])              - 월 추출
Day([날짜])                - 일 추출
DayOfWeek([날짜])          - 요일 (1=일요일)
Hour([시간])               - 시간 추출
Minute([시간])             - 분 추출

날짜 계산:
DateAdd([간격], [수량], [날짜]) - 날짜 더하기
DateDiff([간격], [시작날짜], [끝날짜]) - 날짜 차이
Today()                    - 오늘 날짜
Now()                      - 현재 시간

예시:
Year([Test_Date])          - 테스트 연도
DateDiff("day", [Start_Date], [End_Date]) - 소요 일수
```

### 4. 집계 함수
```
기본 집계:
Sum([값])                  - 합계
Avg([값])                  - 평균
Count([값])                - 개수
CountDistinct([값])        - 고유값 개수
StdDev([값])               - 표준편차
Var([값])                  - 분산

조건부 집계:
Sum([값]) OVER ([그룹핑])  - 그룹별 합계
Avg([값]) OVER (AllPrevious([정렬])) - 누적 평균

예시:
Sum([Production_Qty]) OVER ([Product_Type]) - 제품별 총 생산량
Avg([Yield]) OVER (AllPrevious([Date])) - 누적 평균 수율
```

## 반도체 업무 실용 Expression

### 1. 수율 및 품질 계산
```
수율 계산:
If([Total_Units] > 0, 
   [Pass_Units] / [Total_Units] * 100, 
   0)

Cpk 계산 (단측):
Min(([USL] - [Mean]) / (3 * [StdDev]), 
    ([Mean] - [LSL]) / (3 * [StdDev]))

불량률 계산:
If([Total_Tested] > 0,
   ([Total_Tested] - [Pass_Count]) / [Total_Tested] * 100,
   0)

품질 등급:
Case
  When [Cpk] >= 1.67 Then "Excellent"
  When [Cpk] >= 1.33 Then "Good"
  When [Cpk] >= 1.0 Then "Marginal"
  Else "Poor"
End
```

### 2. 시간 기반 분석
```
Shift 구분:
Case
  When Hour([Timestamp]) >= 8 And Hour([Timestamp]) < 20 Then "Day"
  Else "Night"
End

주차 계산:
"W" + String(Week([Date])) + "_" + String(Year([Date]))

가동 시간 계산:
DateDiff("hour", [Start_Time], [End_Time])

월별 그룹화:
String(Year([Date])) + "-" + 
Right("0" + String(Month([Date])), 2)
```

### 3. 장비 및 공정 분석
```
가동률 계산:
If([Total_Time] > 0,
   [Run_Time] / [Total_Time] * 100,
   0)

MTBF (Mean Time Between Failures):
[Operating_Hours] / [Failure_Count]

OEE (Overall Equipment Effectiveness):
[Availability] * [Performance] * [Quality] / 10000

장비 상태:
Case
  When [Utilization] >= 85 Then "High"
  When [Utilization] >= 70 Then "Medium"
  Else "Low"
End
```

# Part 2: Document Property

## Document Property 기본 개념

### 1. Document Property란?
```
정의: 문서 전체에서 사용할 수 있는 변수/파라미터
용도: 동적 필터링, 파라미터 제어, 사용자 입력, 계산 결과 저장
접근: Edit → Document Properties
```

### 2. Property 생성 방법
- **Edit** 메뉴 → **Document Properties** 선택
- **Property Control** 삽입 시 자동 생성
- **Custom Expression**에서 파라미터로 생성

*🖼️ [이미지 추가 위치: Document Properties 설정 화면]*

### 3. Property 타입
```
Input Properties (입력 속성):
- String: 문자열 입력
- Integer: 정수 입력  
- Real: 실수 입력
- Boolean: True/False
- Date: 날짜 선택

Output Properties (출력 속성):
- Expression 결과 저장
- 계산된 값 공유
- 상태 정보 표시
```

## Property Control 활용

### 1. 사용자 입력 컨트롤
#### Text Input
```
용도: 자유 텍스트 입력
예시: Lot ID, 제품명, 코멘트
설정: 기본값, 입력 형식, 유효성 검사
```

#### List Box / Drop-down
```
용도: 미리 정의된 옵션 선택
데이터 소스: 고정 리스트, 데이터 컬럼, Expression
예시: 제품 타입, 장비 ID, 공정 단계

설정 예시:
- Fixed List: "Line1;Line2;Line3;All"
- From Column: Distinct values from [Equipment_ID]
- From Expression: 
  Concatenate("All;", 
    Concatenate(String(UniqueValue([Product_Type])), ";"))
```

#### Slider
```
용도: 숫자 범위 선택
예시: 날짜 범위, 임계값, 퍼센타일
설정: 최소/최대값, 단계, 기본값
```

#### Date Range
```
용도: 날짜 범위 선택
예시: 분석 기간, 보고 범위
설정: 시작일, 종료일, 디폴트 범위
```

#### Check Box
```
용도: 온/오프 옵션
예시: 필터 옵션, 표시 옵션, 기능 온/오프
```

*🖼️ [이미지 추가 위치: 다양한 Property Control 예시]*

### 2. 동적 필터링 구현
#### 기본 필터 Expression
```
단일 값 필터:
[Product_Type] = "${ProductSelection}"

다중 값 필터:
[Equipment_ID] in ("${EquipmentList}")

날짜 범위 필터:
[Test_Date] >= Date("${StartDate}") AND 
[Test_Date] <= Date("${EndDate}")

수치 범위 필터:
[Yield_Rate] >= ${MinYield} AND [Yield_Rate] <= ${MaxYield}
```

#### 조건부 필터링
```
옵션에 따른 필터:
If("${FilterOption}" = "All", 
   true, 
   [Category] = "${FilterOption}")

체크박스 기반 필터:
If(${ShowOnlyPass}, 
   [Test_Result] = "Pass", 
   true)

복합 조건 필터:
(
  If("${ProductFilter}" = "All", true, [Product] = "${ProductFilter}") AND
  If("${LineFilter}" = "All", true, [Line] = "${LineFilter}") AND
  [Date] >= Date("${StartDate}")
)
```

## 반도체 업무 활용 예시

### 1. 웨이퍼 맵 분석 대시보드
```
Property 설정:
- WaferID (String): 웨이퍼 선택
- TestType (List): "All;Parametric;Functional;Burn-in"
- BinFilter (List): "All;Pass;Fail;Specific"
- ShowStatistics (Boolean): 통계 표시 여부

필터 Expression:
[Wafer_ID] = "${WaferID}" AND
If("${TestType}" = "All", true, [Test_Type] = "${TestType}") AND
If("${BinFilter}" = "All", true, 
   If("${BinFilter}" = "Pass", [Bin_Number] = 1,
      If("${BinFilter}" = "Fail", [Bin_Number] != 1, true)))

동적 제목:
"Wafer Map Analysis - " + "${WaferID}" + 
" (" + "${TestType}" + " Test)"
```

### 2. 생산 실적 모니터링
```
Property 설정:
- ProductLine (List): Line1;Line2;Line3;All
- TimeRange (Date Range): 분석 기간
- ShowTarget (Boolean): 목표 라인 표시
- AlertThreshold (Real): 알림 임계값

성과 지표 계산:
Production_Rate = 
Sum([Production_Qty]) / 
DateDiff("day", Date("${TimeRange_Start}"), Date("${TimeRange_End}"))

목표 달성률:
If(${ShowTarget},
   [Actual_Production] / [Target_Production] * 100,
   null)

알림 로직:
Case
  When [Production_Rate] < ${AlertThreshold} Then "Alert"
  When [Production_Rate] < ${AlertThreshold} * 1.1 Then "Warning"
  Else "Normal"
End
```

### 3. 품질 관리 대시보드
```
Property 설정:
- QualityMetric (List): "Yield;Cpk;Defect_Rate;First_Pass"
- ControlLimits (Boolean): 관리한계 표시
- SigmaLevel (Integer): 시그마 수준 (1-6)
- TrendPeriod (Integer): 트렌드 분석 기간

동적 분석:
Selected_Metric = 
Case
  When "${QualityMetric}" = "Yield" Then [Yield_Rate]
  When "${QualityMetric}" = "Cpk" Then [Cpk_Value]
  When "${QualityMetric}" = "Defect_Rate" Then [Defect_PPM]
  Else [First_Pass_Yield]
End

관리한계 계산:
If(${ControlLimits},
   [Mean] + ${SigmaLevel} * [StdDev],
   null)

트렌드 분석:
Avg([Selected_Metric]) OVER (
  LastPeriods(${TrendPeriod}, [Date])
)
```

## 고급 기능 및 팁

### 1. 복잡한 비즈니스 로직
```
다단계 승인 프로세스:
Case
  When [Status] = "Submitted" AND [Approval_Level] = 1 Then "Pending_L1"
  When [Status] = "L1_Approved" AND [Value] > ${L2_Threshold} Then "Pending_L2"
  When [Status] = "L1_Approved" AND [Value] <= ${L2_Threshold} Then "Approved"
  When [Status] = "L2_Approved" Then "Final_Approved"
  Else "In_Progress"
End

동적 색상 스킴:
Case
  When "${ColorScheme}" = "Traffic_Light" Then
    Case
      When [Performance] >= 95 Then "Green"
      When [Performance] >= 85 Then "Yellow"
      Else "Red"
    End
  When "${ColorScheme}" = "Heat_Map" Then
    Case
      When [Performance] >= 90 Then "Dark_Blue"
      When [Performance] >= 80 Then "Blue"
      When [Performance] >= 70 Then "Light_Blue"
      Else "White"
    End
  Else "Default"
End
```

### 2. 성능 최적화
```
효율적인 조건문:
// 비효율적
If([A] = 1, "One", 
   If([A] = 2, "Two", 
      If([A] = 3, "Three", "Other")))

// 효율적
Case
  When [A] = 1 Then "One"
  When [A] = 2 Then "Two"
  When [A] = 3 Then "Three"
  Else "Other"
End

인덱스 활용:
// 비효율적
Left([Code], 3) = "ABC"

// 효율적
[Code] LIKE "ABC%"

계산 결과 캐싱:
// Property에 계산 결과 저장
Expensive_Calculation = Complex_Expression_Result
// 다른 곳에서 ${Expensive_Calculation} 참조
```

### 3. 디버깅 및 검증
```
단계별 검증:
Step1_Result = [Base_Value] * [Factor]
Step2_Result = If([Step1_Result] > 0, [Step1_Result], 0)
Final_Result = [Step2_Result] + [Adjustment]

NULL 값 처리:
Coalesce([Primary_Value], [Backup_Value], 0)

데이터 타입 검증:
If(IsNumeric([String_Value]), 
   Real([String_Value]), 
   0)

범위 검증:
If([Value] BETWEEN [Min_Allowed] AND [Max_Allowed],
   [Value],
   [Default_Value])
```

## 모범 사례

### 1. 명명 규칙
```
Property 명명:
- 의미 있는 이름 사용
- PascalCase 또는 snake_case 일관성
- 접두사 활용 (Filter_, Calc_, Display_)

Expression 명명:
- 계산 목적 명시
- 단위 포함 (Rate_Percent, Time_Hours)
- 버전 관리 (Yield_V2, Quality_Updated)

예시:
Filter_ProductType
Calc_YieldRate_Percent
Display_AlertStatus
Trend_MovingAverage_30Days
```

### 2. 문서화 및 주석
```
Expression 주석:
// 목적: 라인별 OEE 계산
// 입력: Availability, Performance, Quality (각각 %)
// 출력: OEE 값 (0-100%)
// 수정일: 2024-01-15
[Availability] * [Performance] * [Quality] / 10000

Property 설명:
Property Description 필드 활용:
"분석 기간 시작일. 기본값은 30일 전"
"제품 타입 필터. All 선택시 전체 제품 표시"
```

### 3. 재사용성 고려
```
모듈화된 계산:
// 기본 계산을 Property로 저장
Base_Yield = [Pass_Count] / [Total_Count] * 100

// 다양한 곳에서 재사용
Adjusted_Yield = ${Base_Yield} * [Adjustment_Factor]
Target_Gap = [Target_Yield] - ${Base_Yield}

템플릿 설계:
// 범용적인 Property 구조
Generic_Filter_Value
Generic_Threshold_Upper
Generic_Threshold_Lower
Generic_Time_Period
```

---

*Custom Expression과 Document Property는 Spotfire의 매우 강력한 기능으로, 적절한 활용을 통해 정적인 데이터를 동적이고 인터랙티브한 분석 도구로 변환할 수 있습니다. 반도체 업무에서 복잡한 계산과 다차원 분석을 효율적으로 수행하는 핵심 도구입니다.*