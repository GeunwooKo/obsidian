# Spotfire Bar Chart 매뉴얼

> 📝 **참고**: 이 매뉴얼에는 스크린샷 위치가 표시되어 있습니다. 실제 Spotfire 화면을 캡처하여 해당 위치에 이미지를 추가하시면 더욱 완성도 높은 매뉴얼이 됩니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Cross Table Manual]] | [[Spotfire Line Chart Manual]] | [[Spotfire Custom Expression & Document Property Manual]] | [[spotfire/README]]

## 개요
Bar Chart(막대 차트)는 범주형 데이터를 비교하는 데 가장 효과적인 시각화 도구입니다. 반도체 업무에서 제품별 수율 비교, 장비별 성능 분석, 불량 유형별 분포 등을 분석할 때 필수적으로 사용됩니다.

*🖼️ [이미지 추가 위치: Spotfire Bar Chart 예시 화면]*

## 기본 Bar Chart 생성

### 1. 새 Bar Chart 생성
- **Insert** 메뉴 → **Visual** → **Bar Chart** 선택
- 또는 툴바에서 Bar Chart 아이콘 클릭

*🖼️ [이미지 추가 위치: Insert 메뉴에서 Bar Chart 선택하는 화면]*

### 2. 축 설정
#### Category Axis (범주 축)
- **X-Axis**: 범주형 변수 (제품, 장비, 라인 등)
- **정렬**: 알파벳순, 값 크기순, 사용자 정의

#### Value Axis (값 축)
- **Y-Axis**: 수치형 변수 (수율, 수량, 비율 등)
- **집계 함수**: Sum, Average, Count, Max, Min

*🖼️ [이미지 추가 위치: 축 설정 패널 및 드롭다운 메뉴]*

### 3. 그룹화 설정
- **Split By**: 하위 범주로 막대 분할
- **Stack By**: 막대 내부 스택 생성

*🖼️ [이미지 추가 위치: 기본 Bar Chart 완성 화면]*

## 차트 유형 선택

### 1. Vertical Bar Chart (세로 막대)
- 일반적인 막대 차트 형태
- 범주가 많지 않을 때 적합
- X축에 범주, Y축에 값

### 2. Horizontal Bar Chart (가로 막대)
- 범주명이 길거나 범주가 많을 때 유용
- Y축에 범주, X축에 값
- 순위 비교에 효과적

*🖼️ [이미지 추가 위치: 세로/가로 막대 차트 비교]*

### 3. Stacked Bar Chart (누적 막대)
- 전체 대비 부분의 비율 표시
- 여러 하위 범주의 구성 비교
- 100% Stacked로 비율 표시 가능

### 4. Grouped Bar Chart (그룹 막대)
- 여러 시리즈의 직접 비교
- 각 범주 내 하위 그룹 비교

*🖼️ [이미지 추가 위치: Stacked와 Grouped Bar Chart 예시]*

## 고급 설정 및 커스터마이징

### 1. 막대 스타일 설정
```
Properties → Appearance → Bar Settings
```
- **Bar Width**: 막대 너비 (0.1 - 1.0)
- **Gap Width**: 막대 간 간격
- **3D Effect**: 3차원 효과 (권장하지 않음)

*🖼️ [이미지 추가 위치: Bar Settings 패널]*

### 2. 색상 설정
```
Properties → Coloring
```
- **Automatic**: 자동 색상 할당
- **By Category**: 범주별 색상
- **By Value**: 값 크기에 따른 색상 그라디언트
- **Custom Scheme**: 사용자 정의 색상

*🖼️ [이미지 추가 위치: 다양한 색상 설정이 적용된 막대 차트]*

### 3. 정렬 옵션
```
Properties → Category Axis → Sort
```
- **Alphabetical**: 알파벳순
- **By Value**: 값 크기순 (ascending/descending)
- **Custom**: 사용자 정의 순서
- **By Expression**: 표현식 기반 정렬

### 4. 레이블 설정
```
Properties → Labels
```
- **Show Labels**: 막대 위 값 표시
- **Position**: 막대 내부/외부/중앙
- **Format**: 소수점, 단위, 천 단위 구분자
- **Rotation**: 레이블 회전각

*🖼️ [이미지 추가 위치: 레이블이 표시된 막대 차트]*

## 통계 분석 기능

### 1. 참조선 추가
```
Properties → Lines → Reference Lines
```
- **Average Line**: 평균값 선
- **Target Line**: 목표값 선
- **Median Line**: 중앙값 선
- **Custom Value**: 사용자 정의 값

*🖼️ [이미지 추가 위치: 참조선이 있는 막대 차트]*

### 2. 오차 막대
```
Properties → Error Bars
```
- **Standard Deviation**: 표준편차
- **Standard Error**: 표준오차
- **Custom Expression**: 사용자 정의 오차
- **Confidence Interval**: 신뢰구간

### 3. 임계값 표시
```
Properties → Coloring → Color Mode → By Rules
```
- 특정 값 이상/이하에 따른 색상 구분
- 품질 기준선 기반 색상 표시
- 경고/위험 수준 시각화

*🖼️ [이미지 추가 위치: 임계값 기반 색상 구분 막대 차트]*

## 반도체 업무 활용 예시

### 1. 제품별 수율 비교
```
Category Axis: 제품명
Value Axis: 수율(%) - Average
Color By: 목표 달성 여부
참조선: 목표 수율선
```

*🖼️ [이미지 추가 위치: 제품별 수율 비교 막대 차트]*

### 2. 장비별 가동률 분석
```
Category Axis: 장비 ID
Value Axis: 가동률(%) - Average
Sort By: 가동률 내림차순
Color By: Value (그라디언트)
```

### 3. 불량 유형별 분포
```
Chart Type: Horizontal Bar (불량 유형명이 길 경우)
Category Axis: 불량 유형
Value Axis: 발생 건수 - Sum
Sort By: 발생 건수 내림차순
```

*🖼️ [이미지 추가 위치: 불량 유형별 분포 가로 막대 차트]*

### 4. 라인별 생산량 현황
```
Chart Type: Stacked Bar
Category Axis: 생산 라인
Value Axis: 생산량 - Sum
Stack By: 제품 타입
Color By: 제품 타입
```

### 5. 월별 수율 추이 (범주형 시간)
```
Category Axis: 월 (범주형)
Value Axis: 수율(%) - Average
Split By: 제품군
참조선: 목표 수율, 평균 수율
```

*🖼️ [이미지 추가 위치: 월별 수율 추이 그룹 막대 차트]*

## 고급 분석 기능

### 1. 파레토 차트
```
Properties → Analysis → Pareto
```
- **Primary Bars**: 발생 빈도
- **Secondary Line**: 누적 비율
- 80-20 법칙 시각화

*🖼️ [이미지 추가 위치: 파레토 차트 예시]*

### 2. Small Multiples (Trellis)
```
Properties → Trellis
```
- 범주별로 별도 차트 생성
- **Columns/Rows**: 배치 방식
- **Individual Scales**: 독립적인 축 스케일

### 3. 통계적 비교
```
Properties → Analysis → Statistical Tests
```
- **T-test**: 평균값 비교
- **ANOVA**: 다중 그룹 비교
- **Chi-square**: 범주형 데이터 독립성 검정

## 데이터 처리 및 집계

### 1. 데이터 집계
```
Data → Column Properties → Aggregation Method
```
- **Sum**: 합계 (생산량, 불량 수 등)
- **Average**: 평균 (수율, 품질 지표 등)
- **Count**: 개수 (제품 종류, 이벤트 수 등)
- **Distinct Count**: 고유값 개수

### 2. 필터링
```
Properties → Data → Limit data using expression
```
- 특정 조건의 데이터만 표시
- 날짜 범위, 제품 타입 등으로 필터링

### 3. 계산된 값
```
Insert → Calculated Column
```
- 비율 계산 (불량률, 달성률 등)
- 차이 계산 (목표 대비 실적 등)
- 조건부 값 (Pass/Fail 판정 등)

*🖼️ [이미지 추가 위치: 계산된 컬럼이 적용된 막대 차트]*

## 상호작용 및 필터링

### 1. 드릴다운
```
Properties → Hierarchy
```
- 상위 카테고리에서 하위로 드릴다운
- 예: 제품군 → 제품 → 모델

### 2. 브러싱 및 링킹
- 막대 선택 시 다른 시각화 연동
- **Ctrl + 클릭**: 다중 선택
- **범위 선택**: 연속된 막대 선택

### 3. 툴팁 설정
```
Properties → Tooltip
```
- **Additional Columns**: 추가 정보 표시
- **Custom Format**: 툴팁 형식 커스터마이징

*🖼️ [이미지 추가 위치: 상호작용 기능 시연]*

## 성능 최적화

### 1. 데이터 제한
- **Top N**: 상위 N개 범주만 표시
- **Bottom N**: 하위 N개 범주만 표시
- **Others 그룹화**: 기타 항목으로 그룹화

### 2. 집계 수준 조정
- 적절한 집계 단위 선택
- 불필요한 세부 데이터 제거

### 3. 메모리 관리
- 대용량 데이터셋의 경우 샘플링
- 캐싱 활용

## 내보내기 및 공유

### 1. 이미지 내보내기
```
File → Export → Export Images
```
- **Format**: PNG, JPEG, PDF, SVG
- **Resolution**: 프레젠테이션용 고해상도
- **Size**: 용도에 맞는 크기 설정

### 2. 데이터 내보내기
```
File → Export → Export Data
```
- **Visible Data**: 화면에 표시된 데이터
- **All Data**: 전체 데이터
- **Format**: Excel, CSV, Text

### 3. 템플릿 저장
```
File → Save As Template
```
- 차트 설정을 템플릿으로 저장
- 일관된 보고서 형식 유지

## 팁 및 모범 사례

### 1. 시각적 디자인
- 막대 수는 7-12개가 적당
- 중요한 막대는 다른 색상으로 강조
- 0에서 시작하는 Y축 사용 (비율 비교 시)

### 2. 색상 선택
- 색맹 친화적 색상 팔레트
- 의미있는 색상 사용 (빨강=위험, 초록=양호)
- 과도한 색상 변화 피하기

### 3. 레이블 관리
- 범주명이 길면 가로 막대 차트 고려
- 값 레이블은 필요한 경우만 표시
- 축 제목과 단위 명확히 표시

### 4. 데이터 정렬
- 의미있는 순서로 정렬 (크기순, 시간순 등)
- 파레토 분석 시 내림차순 정렬
- 범주형 시간 데이터는 시간순 정렬

*🖼️ [이미지 추가 위치: 모범 사례가 적용된 막대 차트 예시]*

## 자주 사용하는 단축키

| 기능 | 단축키 |
|------|--------|
| 막대 선택 | 클릭 |
| 다중 선택 | Ctrl + 클릭 |
| 범위 선택 | Shift + 클릭 |
| 선택 해제 | Esc |
| 줌 | Ctrl + 마우스 휠 |

## 문제 해결

### 1. 막대가 표시되지 않는 경우
- 데이터 필터 상태 확인
- 집계 함수 설정 확인
- 축 범위 설정 확인

### 2. 범주가 잘려서 보이는 경우
- 차트 크기 조정
- 가로 막대 차트로 변경
- 레이블 회전 또는 축 설정 조정

### 3. 색상이 의도와 다른 경우
- 색상 스킴 설정 확인
- Color By 변수 확인
- 범주 순서 확인

### 4. 성능이 느린 경우
- 표시할 범주 수 제한
- 데이터 집계 수준 조정
- 불필요한 시각적 효과 제거

---

*이 매뉴얼은 Spotfire의 Bar Chart 기능을 효율적으로 활용하기 위한 가이드입니다. 범주형 데이터 비교와 분석에 최적화된 설정 방법을 제공합니다.*