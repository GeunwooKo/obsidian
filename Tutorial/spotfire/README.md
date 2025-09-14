# Spotfire 종합 매뉴얼 모음

이 폴더에는 반도체 업무에 특화된 **8개의 완전한 Spotfire 매뉴얼**이 포함되어 있습니다.

## 📚 완성된 매뉴얼 목록

### 🎯 기본 시각화 차트 (4개)

#### 1. [[Spotfire Scatter Plot Manual]]
- **용도**: 두 변수 간 상관관계 분석, 웨이퍼 맵 분석
- **반도체 활용**: 
  - 웨이퍼 맵 분석 (X, Y 좌표별 품질 분포)
  - 공정 파라미터 상관관계 분석
  - 품질 지표 상관성 분석
- **주요 기능**: 추세선, 색상/크기 매핑, 다중 축
- **연관 매뉴얼**: [[Spotfire Cross Table Manual]], [[Spotfire Custom Expression & Document Property Manual]]

#### 2. [[Spotfire Line Chart Manual]]
- **용도**: 시간별 트렌드 분석, 연속 데이터 모니터링
- **반도체 활용**:
  - 시간별 수율 모니터링
  - 장비 파라미터 트렌드 분석
  - 공정 안정성 추적
- **주요 기능**: 시계열 분석, 예측, 관리 한계선
- **연관 매뉴얼**: [[Spotfire Box Plot Manual]], [[Spotfire Custom Expression & Document Property Manual]]

#### 3. [[Spotfire Bar Chart Manual]]
- **용도**: 범주형 데이터 비교, 순위 분석
- **반도체 활용**:
  - 제품별/라인별 수율 비교
  - 불량 유형별 분포 분석
  - 장비별 성능 비교
- **주요 기능**: 스택/그룹 차트, 파레토 분석, 참조선
- **연관 매뉴얼**: [[Spotfire Cross Table Manual]], [[Spotfire Custom Expression & Document Property Manual]]

#### 4. [[Spotfire Box Plot Manual]]
- **용도**: 데이터 분포 분석, 이상값 탐지, 그룹 간 비교
- **반도체 활용**:
  - 공정 안정성 분석
  - 제품별 품질 분포 비교
  - 배치별 균일성 평가
- **주요 기능**: 통계 분석, 바이올린 플롯, SPC
- **연관 매뉴얼**: [[Spotfire Line Chart Manual]], [[Spotfire Custom Expression & Document Property Manual]]

### 📊 고급 분석 도구 (2개)

#### 6. [[Spotfire Cross Table Manual]]
- **용도**: 다차원 교차 분석, 피벗 테이블
- **반도체 활용**:
  - 제품별/라인별 성과 매트릭스
  - 장비별/시간별 가동률 분석
  - 공정별/파라미터별 품질 지표
- **주요 기능**: 계층 구조, 조건부 서식, 스파크라인
- **연관 매뉴얼**: [[Spotfire Bar Chart Manual]], [[Spotfire Heatmap Manual]], [[Spotfire Custom Expression & Document Property Manual]]

#### 7. [[Spotfire Custom Expression & Document Property Manual]]
- **용도**: 고급 계산, 동적 대시보드 제어
- **반도체 활용**:
  - 수율/품질 지표 자동 계산 (Cpk, OEE, MTBF)
  - 동적 필터링 및 파라미터 제어
  - SPC 관리도 및 알림 시스템
- **주요 기능**: Expression 문법, Property Control, 조건부 로직
- **연관 매뉴얼**: 모든 시각화 매뉴얼과 연계

### 🔗 데이터 연결 및 활용 (2개)

#### 8. [[Spotfire Information Link+ Impala SQL Guide]]
- **용도**: 동적 데이터 연결, SQL 쿼리 최적화
- **반도체 활용**:
  - 실시간 데이터 연결
  - 파라미터화된 쿼리
  - 대용량 데이터 처리
- **주요 기능**: Impala SQL 문법, 성능 최적화
- **연관 매뉴얼**: [[Spotfire Custom Expression & Document Property Manual]]

#### 9. [[Screenshot Guide]]
- **용도**: 매뉴얼 완성도 향상을 위한 스크린샷 가이드
- **포함 내용**: 11개 핵심 스크린샷 촬영 가이드
- **연관 매뉴얼**: 모든 매뉴얼의 시각적 완성도 향상

## 🎯 업무별 추천 차트 매트릭스

### 품질 관리 (QC)
1. **[[Spotfire Box Plot Manual]]**: 공정 안정성, 분포 분석
2. **[[Spotfire Line Chart Manual]]**: SPC 차트, 트렌드 모니터링
3. **[[Spotfire Heatmap Manual]]**: 품질 매트릭스, 상관관계 분석
4. **[[Spotfire Custom Expression & Document Property Manual]]**: Cpk, SPC 규칙 자동 계산

### 수율 분석
1. **[[Spotfire Line Chart Manual]]**: 시간별 수율 트렌드
2. **[[Spotfire Bar Chart Manual]]**: 제품/라인별 수율 비교
3. **[[Spotfire Scatter Plot Manual]]**: 수율 영향 인자 분석
4. **[[Spotfire Cross Table Manual]]**: 다차원 수율 매트릭스

### 웨이퍼 분석
1. **[[Spotfire Heatmap Manual]]**: 웨이퍼 맵 패턴 분석
2. **[[Spotfire Scatter Plot Manual]]**: Die별 특성 분포
3. **[[Spotfire Box Plot Manual]]**: 웨이퍼별 균일성 평가
4. **[[Spotfire Custom Expression & Document Property Manual]]**: Bin 분류, 수율 계산

### 장비 모니터링
1. **[[Spotfire Line Chart Manual]]**: 장비 파라미터 트렌드
2. **[[Spotfire Bar Chart Manual]]**: 장비별 성능 비교
3. **[[Spotfire Heatmap Manual]]**: 시간대별 장비 상태
4. **[[Spotfire Cross Table Manual]]**: 장비/시간 교차 분석
5. **[[Spotfire Custom Expression & Document Property Manual]]**: OEE, MTBF 계산

### 공정 최적화
1. **[[Spotfire Scatter Plot Manual]]**: 파라미터 상관관계
2. **[[Spotfire Box Plot Manual]]**: 공정 조건별 결과 분포
3. **[[Spotfire Heatmap Manual]]**: 다변수 공정 매트릭스
4. **[[Spotfire Custom Expression & Document Property Manual]]**: 동적 공정 제어

## 📝 차트 선택 가이드

### 데이터 타입별 추천

| 데이터 타입 | 1순위 | 2순위 | 3순위 |
|------------|-------|-------|-------|
| **시계열 데이터** | [[Spotfire Line Chart Manual]] | [[Spotfire Bar Chart Manual]] | [[Spotfire Heatmap Manual]] |
| **범주형 비교** | [[Spotfire Bar Chart Manual]] | [[Spotfire Cross Table Manual]] | [[Spotfire Box Plot Manual]] |
| **연속형 상관관계** | [[Spotfire Scatter Plot Manual]] | [[Spotfire Heatmap Manual]] | [[Spotfire Line Chart Manual]] |
| **2차원 매트릭스** | [[Spotfire Heatmap Manual]] | [[Spotfire Cross Table Manual]] | [[Spotfire Scatter Plot Manual]] |
| **분포 분석** | [[Spotfire Box Plot Manual]] | [[Spotfire Line Chart Manual]] | [[Spotfire Bar Chart Manual]] |

### 분석 목적별 추천

| 분석 목적 | 1순위 | 2순위 | 3순위 |
|-----------|-------|-------|-------|
| **트렌드 분석** | [[Spotfire Line Chart Manual]] | [[Spotfire Bar Chart Manual]] | [[Spotfire Heatmap Manual]] |
| **이상값 탐지** | [[Spotfire Box Plot Manual]] | [[Spotfire Scatter Plot Manual]] | [[Spotfire Heatmap Manual]] |
| **상관관계 분석** | [[Spotfire Scatter Plot Manual]] | [[Spotfire Heatmap Manual]] | [[Spotfire Line Chart Manual]] |
| **그룹 비교** | [[Spotfire Bar Chart Manual]] | [[Spotfire Box Plot Manual]] | [[Spotfire Cross Table Manual]] |
| **패턴 발견** | [[Spotfire Heatmap Manual]] | [[Spotfire Scatter Plot Manual]] | [[Spotfire Box Plot Manual]] |
| **다차원 분석** | [[Spotfire Cross Table Manual]] | [[Spotfire Heatmap Manual]] | [[Spotfire Custom Expression & Document Property Manual]] |

## 🚀 고급 활용 팁

### 1. 다중 차트 조합 및 연계
- **대시보드 구성**: 여러 차트를 조합하여 종합 분석
- **크로스 필터링**: 차트 간 상호작용으로 동적 분석
- **드릴다운**: 상위 레벨에서 세부 분석으로 이동
- **참조**: [[Spotfire Cross Table Manual]], [[Spotfire Custom Expression & Document Property Manual]]

### 2. 실시간 모니터링 및 데이터 연결
- **자동 새로고침**: 실시간 데이터 업데이트
- **알림 설정**: 임계값 초과 시 자동 알림
- **대시보드 스케줄링**: 정기 보고서 자동 생성
- **참조**: [[Spotfire Information Link+ Impala SQL Guide]], [[Spotfire Custom Expression & Document Property Manual]]

### 3. 고급 분석 기능
- **통계 분석**: 회귀분석, 분산분석, 가설검정
- **예측 모델링**: 시계열 예측, 머신러닝 통합
- **클러스터링**: 데이터 세분화 및 패턴 발견
- **참조**: [[Spotfire Box Plot Manual]], [[Spotfire Custom Expression & Document Property Manual]]

## 📊 통합 워크플로우

### 반도체 데이터 분석 워크플로우
```
1. 데이터 연결: [[Spotfire Information Link+ Impala SQL Guide]]
   ↓
2. 기본 분석: [[Spotfire Line Chart Manual]] or [[Spotfire Bar Chart Manual]]
   ↓
3. 상세 분석: [[Spotfire Scatter Plot Manual]] or [[Spotfire Box Plot Manual]]
   ↓
4. 패턴 분석: [[Spotfire Heatmap Manual]] or [[Spotfire Cross Table Manual]]
   ↓
5. 고급 계산: [[Spotfire Custom Expression & Document Property Manual]]
   ↓
6. 대시보드 완성: [[Screenshot Guide]]
```

## 📱 파일 구조 및 Obsidian 그래프 링크

```
spotfire/
├── README.md (이 인덱스 파일)
├── Spotfire Scatter Plot Manual.md → 연결: Heatmap, Custom Expression
├── Spotfire Line Chart Manual.md → 연결: Box Plot, Custom Expression
├── Spotfire Bar Chart Manual.md → 연결: Cross Table, Custom Expression
├── Spotfire Heatmap Manual.md → 연결: Scatter Plot, Cross Table
├── Spotfire Box Plot Manual.md → 연결: Line Chart, Custom Expression
├── Spotfire Cross Table Manual.md → 연결: Bar Chart, Heatmap, Custom Expression
├── Spotfire Custom Expression & Document Property Manual.md → 모든 매뉴얼과 연결
├── Spotfire Information Link+ Impala SQL Guide.md → 연결: Custom Expression
├── Screenshot Guide.md → 모든 매뉴얼 시각적 완성도 향상
└── images/ (스크린샷 저장 폴더)
    ├── scatter_plot/
    ├── line_chart/
    ├── bar_chart/
    ├── heatmap/
    ├── box_plot/
    ├── cross_table/
    ├── custom_expression/
    └── sql_guide/
```

## 🔧 설정 및 사용법

### 1. 매뉴얼 활용 방법
1. **분석 목적 파악**: 위 가이드를 참조하여 적절한 차트 선택
2. **기본 생성**: 해당 매뉴얼의 "기본 생성" 섹션부터 시작
3. **실제 적용**: "반도체 업무 활용 예시" 참조하여 실제 적용
4. **고급 기능**: 필요시 "고급 설정" 및 "문제 해결" 참조
5. **매뉴얼 연결**: 관련 매뉴얼 링크를 통해 확장 분석

### 2. 스크린샷 추가 방법
1. **[[Screenshot Guide]]** 참조
2. 실제 Spotfire에서 화면 캡처
3. `images/` 폴더에 저장
4. 매뉴얼의 🖼️ 표시를 실제 이미지 링크로 교체

### 3. 커스터마이징 및 확장
1. **기존 매뉴얼 수정**: 회사 특성에 맞게 예시 데이터 및 시나리오 수정
2. **새로운 매뉴얼 추가**: 기존 매뉴얼 구조를 참조하여 새 차트 매뉴얼 작성
3. **링크 업데이트**: 새 매뉴얼 추가 시 README.md 업데이트

## 📞 지원 및 기여

### 문제 또는 제안이 있으신가요?
- 각 매뉴얼의 "문제 해결" 섹션 참조
- [[Screenshot Guide]]를 통한 시각적 개선
- 실제 사용 사례 공유

### 매뉴얼 개선 및 확장
- 반도체 업무 특성에 맞는 예시 추가
- 새로운 시각화 기법 매뉴얼 작성
- 고급 분석 시나리오 추가

---

**✨ 이 매뉴얼 모음을 통해 Spotfire를 활용한 전문적인 반도체 데이터 분석이 가능합니다!**

📊 **총 8개 매뉴얼** | 🎯 **실무 예시 포함** | 🔗 **Obsidian 그래프 링크 지원** | 🖼️ **스크린샷 가이드 제공**

*이 매뉴얼 모음을 통해 Spotfire를 활용한 전문적인 반도체 데이터 분석을 수행하실 수 있습니다. 각 매뉴얼에는 스크린샷 추가 위치가 표시되어 있어 완전한 가이드로 활용할 수 있습니다.*