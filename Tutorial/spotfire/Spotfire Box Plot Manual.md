# Spotfire Box Plot 매뉴얼

> 📝 **참고**: 이 매뉴얼에는 스크린샷 위치가 표시되어 있습니다. 실제 Spotfire 화면을 캡처하여 해당 위치에 이미지를 추가하시면 더욱 완성도 높은 매뉴얼이 됩니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Line Chart Manual]] | [[Spotfire Scatter Plot Manual]] | [[Spotfire Custom Expression & Document Property Manual]] | [[spotfire/README]]

## 개요
Box Plot(상자 그림)은 데이터의 분포와 통계적 특성을 한눈에 파악할 수 있는 강력한 시각화 도구입니다. 반도체 업무에서 공정 안정성 분석, 품질 데이터 분포 확인, 이상값 탐지, 그룹 간 성능 비교 등에 필수적으로 사용됩니다.

*🖼️ [이미지 추가 위치: Spotfire Box Plot 예시 화면]*

## 기본 Box Plot 생성

### 1. 새 Box Plot 생성
- **Insert** 메뉴 → **Visual** → **Box Plot** 선택
- 또는 툴바에서 Box Plot 아이콘 클릭

*🖼️ [이미지 추가 위치: Insert 메뉴에서 Box Plot 선택하는 화면]*

### 2. 축 설정
#### Category Axis (범주 축)
- **X-Axis**: 범주형 변수 (제품, 장비, 라인, 시간 구간 등)
- 각 범주별로 별도의 Box Plot 생성

#### Value Axis (값 축)
- **Y-Axis**: 분석할 수치형 변수 (수율, 처리시간, 측정값 등)
- 분포를 확인하고자 하는 연속형 데이터

*🖼️ [이미지 추가 위치: 축 설정 패널 및 드롭다운 메뉴]*

### 3. Box Plot 구성요소 이해
- **중앙값 (Median)**: 박스 내부의 가운데 선
- **1사분위수 (Q1)**: 박스의 하단
- **3사분위수 (Q3)**: 박스의 상단
- **사분위수 범위 (IQR)**: Q3 - Q1
- **위스커 (Whiskers)**: 박스에서 연장된 선
- **이상값 (Outliers)**: 위스커 밖의 점들

*🖼️ [이미지 추가 위치: Box Plot 구성요소가 라벨된 기본 완성 화면]*

## Box Plot 설정 및 커스터마이징

### 1. 위스커 설정
```
Properties → Box Plot → Whisker Type
```
- **Standard (1.5 × IQR)**: 표준 설정 (기본값)
- **Min/Max**: 최솟값/최댓값까지 연장
- **95th Percentile**: 5-95 백분위수
- **99th Percentile**: 1-99 백분위수
- **Custom Percentile**: 사용자 정의 백분위수

*🖼️ [이미지 추가 위치: 다양한 위스커 설정이 적용된 Box Plot 비교]*

### 2. 이상값 표시 설정
```
Properties → Box Plot → Show Outliers
```
- **Show All Outliers**: 모든 이상값 표시
- **Hide Outliers**: 이상값 숨김
- **Limit Outliers**: 표시할 이상값 개수 제한
- **Outlier Shape/Size**: 이상값 모양과 크기 설정

### 3. 박스 스타일 설정
```
Properties → Appearance → Box Style
```
- **Box Width**: 박스 너비 조정
- **Fill Color**: 박스 내부 색상
- **Border Color**: 박스 테두리 색상
- **Border Width**: 테두리 두께

*🖼️ [이미지 추가 위치: 다양한 박스 스타일이 적용된 Box Plot]*

### 4. 통계 정보 표시
```
Properties → Labels → Statistical Labels
```
- **Mean**: 평균값 표시 (다이아몬드 또는 X 표시)
- **Sample Size**: 샘플 크기 표시
- **Standard Deviation**: 표준편차 표시
- **Confidence Interval**: 신뢰구간 표시

*🖼️ [이미지 추가 위치: 통계 정보가 표시된 Box Plot]*

## 색상 및 그룹화

### 1. 색상 설정
```
Properties → Coloring
```
- **By Category**: 범주별 색상 구분
- **By Value**: 통계값에 따른 색상 그라디언트
- **Custom Colors**: 사용자 정의 색상

### 2. 다중 그룹화
```
Properties → Category Axis → Grouping
```
- **Primary Grouping**: 1차 그룹화 (주 범주)
- **Secondary Grouping**: 2차 그룹화 (하위 범주)
- **Split By**: 별도 패널로 분할

*🖼️ [이미지 추가 위치: 다중 그룹화된 Box Plot 예시]*

### 3. 트렐리스 (Small Multiples)
```
Properties → Trellis
```
- 범주별로 별도 패널 생성
- **Columns/Rows**: 패널 배치 설정
- **Individual Scales**: 각 패널별 독립 축

## 반도체 업무 활용 예시

### 1. 공정 안정성 분석
```
Category Axis: 공정 단계 또는 장비 ID
Value Axis: 공정 파라미터 (온도, 압력, 시간)
목적: 공정별 변동성 비교, 이상 공정 탐지
```

*🖼️ [이미지 추가 위치: 공정 안정성 분석 Box Plot 예시]*

### 2. 제품별 수율 분포 비교
```
Category Axis: 제품 타입
Value Axis: 수율(%)
Secondary Grouping: 생산 라인
목적: 제품별/라인별 수율 분포 차이 분석
```

### 3. 웨이퍼별 품질 지표 분석
```
Category Axis: 웨이퍼 배치 (Lot)
Value Axis: 품질 측정값 (저항, 임계전압 등)
색상: 규격 적합성 (Pass/Fail)
목적: 배치별 품질 일관성 확인
```

*🖼️ [이미지 추가 위치: 웨이퍼별 품질 지표 Box Plot]*

### 4. 시간대별 장비 성능 분석
```
Category Axis: 시간대 (8시간 단위)
Value Axis: 처리량 또는 가동률
Split By: 장비 타입
목적: 시간대별 성능 패턴 분석
```

### 5. 측정 위치별 균일성 분석
```
Category Axis: 측정 위치 (웨이퍼 내 위치)
Value Axis: 측정값
Grouping: 공정 조건
목적: 공정 균일성 평가
```

*🖼️ [이미지 추가 위치: 측정 위치별 균일성 분석 Box Plot]*

## 고급 분석 기능

### 1. 통계적 비교
```
Properties → Analysis → Statistical Tests
```
- **ANOVA**: 다중 그룹 평균 비교
- **Kruskal-Wallis**: 비모수 다중 그룹 비교
- **T-test**: 두 그룹 평균 비교
- **Mann-Whitney**: 비모수 두 그룹 비교

*🖼️ [이미지 추가 위치: 통계 검정 결과가 표시된 Box Plot]*

### 2. 참조선 추가
```
Properties → Lines → Reference Lines
```
- **Target Line**: 목표값 선
- **Specification Limits**: 규격 상한/하한선
- **Average Line**: 전체 평균선
- **Control Limits**: 관리 한계선

### 3. 노치 (Notch) 표시
```
Properties → Box Plot → Show Notches
```
- 중앙값의 신뢰구간 표시
- 노치가 겹치지 않으면 통계적으로 유의한 차이
- 그룹 간 중앙값 비교에 유용

*🖼️ [이미지 추가 위치: 노치가 표시된 Box Plot]*

## 바이올린 플롯 (Violin Plot) 옵션

### 1. 바이올린 플롯 활성화
```
Properties → Box Plot → Show Distribution
```
- 박스 플롯에 확률밀도 곡선 추가
- 데이터의 분포 형태를 더 상세히 표현
- 다봉분포, 비대칭 분포 탐지에 유용

### 2. 바이올린 스타일 설정
```
Properties → Distribution → Style
```
- **Full Violin**: 양쪽 대칭 분포 표시
- **Half Violin**: 한쪽 분포만 표시
- **Transparency**: 투명도 조정

*🖼️ [이미지 추가 위치: 바이올린 플롯과 박스 플롯 비교]*

## 데이터 포인트 오버레이

### 1. 개별 데이터 포인트 표시
```
Properties → Box Plot → Show Data Points
```
- **All Points**: 모든 데이터 포인트 표시
- **Outliers Only**: 이상값만 표시
- **Sample Points**: 샘플 포인트만 표시

### 2. 지터 (Jitter) 설정
```
Properties → Data Points → Jitter
```
- 겹치는 포인트들을 수평으로 분산
- **Jitter Amount**: 분산 정도 조정
- 데이터 밀도 시각화에 도움

*🖼️ [이미지 추가 위치: 지터가 적용된 데이터 포인트 오버레이]*

## 상호작용 및 필터링

### 1. 브러싱 및 필터링
- **박스 클릭**: 해당 범주 데이터 선택
- **이상값 클릭**: 개별 이상값 선택
- **드래그**: 값 범위 선택
- 선택된 데이터로 다른 시각화 필터링

### 2. 줌 및 팬
- **Y축 줌**: 값 범위 확대/축소
- **X축 팬**: 범주 간 이동 (범주가 많은 경우)

### 3. 툴팁 정보
```
Properties → Tooltip
```
- **Statistical Summary**: 통계 요약 정보
- **Data Count**: 데이터 개수
- **Custom Information**: 추가 컬럼 정보

*🖼️ [이미지 추가 위치: 상세한 툴팁이 표시된 Box Plot]*

## 성능 최적화

### 1. 대용량 데이터 처리
- **Sampling**: 대표 샘플링으로 성능 향상
- **Binning**: 연속형 데이터를 구간으로 나누기
- **Aggregation**: 적절한 집계 수준 선택

### 2. 시각화 최적화
- **범주 수 제한**: 너무 많은 범주 피하기
- **데이터 포인트 제한**: 대용량 데이터 시 표시 포인트 수 조절
- **불필요한 통계 제거**: 필요한 통계 정보만 표시

## 해석 가이드

### 1. 분포 형태 분석
- **대칭 분포**: 박스가 위스커 중앙에 위치
- **우편향**: 박스가 위스커 하단에 치우침
- **좌편향**: 박스가 위스커 상단에 치우침
- **이봉분포**: 바이올린 플롯에서 두 개의 봉우리

### 2. 변동성 평가
- **박스 크기**: IQR, 중간 50% 데이터의 범위
- **위스커 길이**: 전체 데이터 범위 (이상값 제외)
- **이상값 개수**: 프로세스 안정성 지표

### 3. 그룹 간 비교
- **중앙값 비교**: 박스 내 선의 위치
- **분산 비교**: 박스와 위스커의 크기
- **이상값 패턴**: 그룹별 이상값 발생 경향

*🖼️ [이미지 추가 위치: 다양한 분포 형태를 보여주는 Box Plot 예시]*

## 품질 관리 응용

### 1. 공정 능력 평가
- **Cp/Cpk 계산**: 규격 대비 공정 능력
- **관리 한계**: 3σ 한계선 표시
- **목표값 대비 중심화**: 중앙값과 목표값 비교

### 2. SPC (통계적 공정 관리)
- **이상 신호 탐지**: 이상값 패턴 분석
- **경향성 분석**: 시간순 박스 플롯
- **주기성 확인**: 규칙적인 변동 패턴

### 3. 6시그마 분석
- **DMAIC 단계**: 각 단계별 개선 효과 확인
- **Before/After 비교**: 개선 전후 분포 변화
- **변동 요인 분석**: 그룹별 분포 차이

*🖼️ [이미지 추가 위치: 품질 관리에 활용된 Box Plot 예시]*

## 팁 및 모범 사례

### 1. 데이터 준비
- 충분한 샘플 크기 확보 (그룹당 최소 20개 이상)
- 이상값 사전 검토 및 원인 분석
- 적절한 범주화 수준 선택

### 2. 시각화 설계
- 의미있는 범주 순서 (시간순, 크기순 등)
- 색상 구분으로 중요 그룹 강조
- 참조선으로 기준값 표시

### 3. 해석 지원
- 명확한 축 제목과 단위
- 통계적 유의성 정보 제공
- 범례와 주석으로 이해도 향상

*🖼️ [이미지 추가 위치: 모범 사례가 적용된 완성도 높은 Box Plot]*

## 자주 사용하는 단축키

| 기능 | 단축키 |
|------|--------|
| 범주 선택 | 클릭 |
| 다중 범주 선택 | Ctrl + 클릭 |
| 값 범위 선택 | Y축에서 드래그 |
| 줌 리셋 | Ctrl + 0 |
| 이상값 선택 | 이상값 포인트 클릭 |

## 문제 해결

### 1. 박스가 표시되지 않는 경우
- 데이터 필터 상태 확인
- 범주별 충분한 데이터 존재 확인
- 축 설정 및 데이터 타입 확인

### 2. 이상값이 너무 많은 경우
- 위스커 타입 변경 (95th percentile 등)
- 데이터 스케일 변환 (로그 변환 등)
- 이상값 표시 제한 설정

### 3. 범주가 겹쳐 보이는 경우
- 차트 크기 조정
- 범주 수 제한 또는 그룹화
- 가로 박스 플롯으로 변경

### 4. 성능이 느린 경우
- 데이터 샘플링 적용
- 불필요한 통계 계산 제거
- 데이터 포인트 표시 제한

---

*이 매뉴얼은 Spotfire의 Box Plot 기능을 효율적으로 활용하기 위한 가이드입니다. 데이터 분포 분석과 통계적 품질 관리에 최적화된 설정 방법을 제공합니다.*