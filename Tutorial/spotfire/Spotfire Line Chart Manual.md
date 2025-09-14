# Spotfire Line Chart 매뉴얼

> 📝 **참고**: 이 매뉴얼에는 스크린샷 위치가 표시되어 있습니다. 실제 Spotfire 화면을 캡처하여 해당 위치에 이미지를 추가하시면 더욱 완성도 높은 매뉴얼이 됩니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Box Plot Manual]] | [[Spotfire Scatter Plot Manual]] | [[Spotfire Custom Expression & Document Property Manual]] | [[README]]

## 개요
Line Chart(선 차트)는 시간에 따른 데이터 변화를 시각화하는 데 가장 효과적인 차트입니다. 반도체 공정에서 시간별 수율 변화, 장비 파라미터 트렌드, 품질 지표 모니터링 등에 필수적으로 사용됩니다.

*🖼️ [이미지 추가 위치: Spotfire Line Chart 예시 화면]*

## 기본 Line Chart 생성

### 1. 새 Line Chart 생성
- **Insert** 메뉴 → **Visual** → **Line Chart** 선택
- 또는 툴바에서 Line Chart 아이콘 클릭

*🖼️ [이미지 추가 위치: Insert 메뉴에서 Line Chart 선택하는 화면]*

### 2. 축 설정
#### X축 설정 (일반적으로 시간)
- **X-Axis**: 날짜/시간 또는 순서형 변수
- **데이터 타입**: DateTime, Date, 또는 연속형 숫자

#### Y축 설정 (측정값)
- **Y-Axis**: 추적하고자 하는 수치형 변수
- 여러 Y축 변수 동시 표시 가능

*🖼️ [이미지 추가 위치: 축 설정 패널 및 드롭다운 메뉴]*

### 3. 선 분할 설정
- **Split By**: 범주형 변수로 여러 선 생성
- 예: 장비별, 제품별, 라인별 구분

*🖼️ [이미지 추가 위치: 기본 Line Chart 완성 화면]*

## 고급 설정 및 커스터마이징

### 1. 선 스타일 설정
```
Properties → Lines → Line Style
```
- **Line Type**: Solid, Dashed, Dotted
- **Line Width**: 선 두께 조절 (1-10)
- **Connection**: 데이터 포인트 연결 방식

*🖼️ [이미지 추가 위치: Line Style 설정 패널]*

### 2. 마커 설정
```
Properties → Lines → Markers
```
- **Show Markers**: 데이터 포인트 표시/숨김
- **Marker Shape**: 원, 사각형, 삼각형 등
- **Marker Size**: 크기 조절

*🖼️ [이미지 추가 위치: 마커가 적용된 Line Chart 예시]*

### 3. 색상 설정
```
Properties → Coloring
```
- **Automatic**: 자동 색상 할당
- **By Category**: 범주별 색상 구분
- **Custom**: 사용자 정의 색상

### 4. 다중 Y축 설정
```
Properties → Axes → Y-Axis → Add
```
- 서로 다른 단위의 변수들을 동시 표시
- **Primary/Secondary Axis**: 좌측/우측 축 구분

*🖼️ [이미지 추가 위치: 다중 Y축 설정 및 결과]*

## 시간축 특별 설정

### 1. 시간 범위 설정
```
Properties → Axes → X-Axis → Scale
```
- **Auto**: 전체 데이터 범위
- **Fixed**: 특정 시간 구간 설정
- **Sliding Window**: 최근 N일/시간만 표시

### 2. 시간 간격 설정
```
Properties → Axes → X-Axis → Label Format
```
- **Hour/Day/Week/Month**: 시간 단위 선택
- **Format**: 날짜/시간 표시 형식
- **Rotation**: 레이블 회전각

*🖼️ [이미지 추가 위치: 시간축 설정 패널]*

### 3. 시간대 설정
```
Properties → Axes → X-Axis → Time Zone
```
- 글로벌 공장 운영 시 필수
- **Local/UTC/Custom**: 시간대 선택

## 통계 분석 기능

### 1. 추세선 추가
```
Properties → Lines & Curves → Curve
```
- **Linear Trend**: 선형 추세
- **Moving Average**: 이동평균
- **Exponential Smoothing**: 지수 평활

*🖼️ [이미지 추가 위치: 추세선이 적용된 Line Chart]*

### 2. 예측선 생성
```
Properties → Lines & Curves → Forecasting
```
- **Method**: ARIMA, Exponential Smoothing
- **Periods**: 예측 기간 설정
- **Confidence Interval**: 신뢰구간 표시

### 3. 관리 한계선
```
Properties → Lines & Curves → Reference Lines
```
- **Upper/Lower Control Limit**: 상한/하한 관리선
- **Target Line**: 목표값 선
- **Specification Limits**: 규격 한계선

*🖼️ [이미지 추가 위치: 관리 한계선이 있는 품질 관리 차트]*

## 반도체 업무 활용 예시

### 1. 시간별 수율 모니터링
```
X축: 날짜/시간
Y축: 수율(%)
Split By: 제품 라인
추가 기능: 목표 수율선, 관리 한계선
```

*🖼️ [이미지 추가 위치: 수율 모니터링 Line Chart 예시]*

### 2. 장비 파라미터 트렌드
```
X축: 시간
Y축 (Primary): 온도
Y축 (Secondary): 압력
Split By: 장비 ID
추가 기능: 이동평균선
```

*🖼️ [이미지 추가 위치: 장비 파라미터 트렌드 차트]*

### 3. 웨이퍼별 품질 추적
```
X축: 웨이퍼 순서 (시간순)
Y축: 품질 지표 (Cpk, 불량률 등)
Split By: 제품 타입
추가 기능: 관리 한계선, 예측선
```

### 4. 공정 안정성 모니터링
```
X축: 시간
Y축: 공정 변수 (두께, 저항 등)
Color By: 규격 적합성
추가 기능: 규격 한계선, 추세선
```

*🖼️ [이미지 추가 위치: 공정 안정성 모니터링 차트]*

## 고급 분석 기능

### 1. 시계열 분해
```
Properties → Analysis → Time Series Decomposition
```
- **Trend**: 장기 추세
- **Seasonal**: 계절성 패턴
- **Residual**: 잔차 분석

### 2. 이상값 탐지
```
Properties → Analysis → Outlier Detection
```
- **Statistical Methods**: Z-score, IQR
- **Time Series Methods**: ARIMA residuals
- **Highlighting**: 이상값 자동 표시

### 3. 상관관계 분석
- 여러 변수의 시간별 상관관계 추적
- **Correlation coefficient**: 상관계수 표시
- **Lag Analysis**: 시차 상관관계

## 성능 최적화

### 1. 데이터 집계
```
Properties → Data → Aggregation
```
- **Bin by Time**: 시간 단위별 집계
- **Average/Sum/Max/Min**: 집계 함수 선택

### 2. 샘플링
```
Properties → Data → Limit data using expression
```
- 대용량 데이터의 경우 샘플링 적용
- **Random/Systematic**: 샘플링 방법

### 3. 캐싱
- 자주 사용하는 시간 범위의 데이터 캐싱
- **Real-time refresh**: 실시간 업데이트 설정

## 상호작용 및 필터링

### 1. 줌 및 팬
- **X축 줌**: 특정 시간 구간 확대
- **Y축 줌**: 값 범위 조절
- **Reset Zoom**: 전체 보기 복원

### 2. 크로스필터링
- 다른 시각화와 연동
- **Time Range Selection**: 시간 구간 선택
- **Value Range Selection**: 값 범위 선택

### 3. 툴팁 커스터마이징
```
Properties → Tooltip
```
- **Additional Columns**: 추가 정보 표시
- **Format**: 툴팁 형식 설정

*🖼️ [이미지 추가 위치: 커스터마이징된 툴팁 예시]*

## 내보내기 및 알림

### 1. 자동 보고서 생성
```
Tools → Automation Services
```
- **Scheduled Reports**: 정기 보고서
- **Email Alerts**: 임계값 초과 시 알림

### 2. 이미지 내보내기
```
File → Export → Export Images
```
- **High Resolution**: 고해상도 이미지
- **Multiple Formats**: PNG, PDF, SVG

### 3. 데이터 내보내기
```
File → Export → Export Data
```
- **Filtered Data**: 필터된 데이터만
- **Time Range**: 특정 시간 구간

## 팁 및 모범 사례

### 1. 색상 선택
- 색맹 친화적 색상 팔레트 사용
- 최대 7-8개 선까지 권장
- 중요한 선은 굵게 표시

### 2. 축 설정
- Y축 0점 포함 여부 신중히 결정
- 로그 스케일 필요 시 적극 활용
- 축 제목과 단위 명확히 표시

### 3. 성능 고려사항
- 대용량 데이터는 적절한 집계 수준 선택
- 실시간 차트는 업데이트 주기 최적화
- 불필요한 시각적 효과 제거

## 자주 사용하는 단축키

| 기능 | 단축키 |
|------|--------|
| 줌 인 (X축) | Ctrl + 마우스 드래그 |
| 줌 아웃 | Ctrl + 더블클릭 |
| 팬 (이동) | Shift + 마우스 드래그 |
| 전체 보기 | Ctrl + 0 |
| 데이터 포인트 선택 | 클릭 |

## 문제 해결

### 1. 선이 연결되지 않는 경우
- X축 데이터 타입 확인
- 정렬 순서 확인
- 누락 데이터 처리 방식 확인

### 2. 성능이 느린 경우
- 데이터 양 확인 및 샘플링 적용
- 불필요한 계산 컬럼 제거
- 캐싱 설정 확인

### 3. 시간축 표시 문제
- 시간대 설정 확인
- 날짜 형식 호환성 확인
- 데이터 범위 확인

---

*이 매뉴얼은 Spotfire의 Line Chart 기능을 효율적으로 활용하기 위한 가이드입니다. 시간별 데이터 분석과 모니터링에 최적화된 설정 방법을 제공합니다.*