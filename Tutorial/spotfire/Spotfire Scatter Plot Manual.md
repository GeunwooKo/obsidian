# Spotfire Scatter Plot 매뉴얼

> 📝 **참고**: 이 매뉴얼에는 스크린샷 위치가 표시되어 있습니다. 실제 Spotfire 화면을 캡처하여 해당 위치에 이미지를 추가하시면 더욱 완성도 높은 매뉴얼이 됩니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Heatmap Manual]] | [[Spotfire Cross Table Manual]] | [[Spotfire Custom Expression & Document Property Manual]] | [[README]]

## 개요
Scatter Plot(산점도)는 두 개 이상의 변수 간의 관계를 시각화하는 가장 기본적이면서도 강력한 차트 유형입니다. Spotfire에서 Scatter Plot을 활용하여 데이터의 패턴, 상관관계, 이상값을 효과적으로 분석할 수 있습니다.

*🖼️ [이미지 추가 위치: Spotfire Scatter Plot 예시 화면]*

## 기본 Scatter Plot 생성

### 1. 새 Scatter Plot 생성
- **Insert** 메뉴 → **Visual** → **Scatter Plot** 선택
- 또는 툴바에서 Scatter Plot 아이콘 클릭

*🖼️ [이미지 추가 위치: Insert 메뉴에서 Scatter Plot 선택하는 화면]*

### 2. 축 설정
#### X축 설정
- **X-Axis** 드롭다운에서 원하는 열 선택
- 연속형 변수(수치형) 또는 범주형 변수 모두 사용 가능

#### Y축 설정
- **Y-Axis** 드롭다운에서 원하는 열 선택
- 일반적으로 연속형 변수 사용

*🖼️ [이미지 추가 위치: 축 설정 패널 및 드롭다운 메뉴]*

### 3. 기본 시각화 확인
- 데이터 포인트가 플롯에 표시됨
- 각 점은 하나의 데이터 행을 나타냄

*🖼️ [이미지 추가 위치: 기본 Scatter Plot 완성 화면]*

## 고급 설정 및 커스터마이징

### 1. 색상(Color By) 설정
```
Properties → Coloring → Color By
```
- 범주형 변수: 각 카테고리별로 다른 색상
- 연속형 변수: 색상 그라디언트로 값의 크기 표현
- **Scheme**: 색상 팔레트 선택
- **Reverse**: 색상 순서 반전

*🖼️ [이미지 추가 위치: Properties 패널의 Coloring 설정 화면]*
*🖼️ [이미지 추가 위치: Color By가 적용된 Scatter Plot 예시]*

### 2. 크기(Size By) 설정
```
Properties → Appearance → Size By
```
- 데이터 값에 따라 점의 크기 조절
- **Min Size / Max Size**: 최소/최대 크기 설정
- 버블 차트 효과 생성 가능

*🖼️ [이미지 추가 위치: Size By 설정과 버블 차트 결과]*

### 3. 모양(Shape By) 설정
```
Properties → Appearance → Shape By
```
- 범주형 변수에 따라 다른 모양 사용
- 원, 사각형, 삼각형, 다이아몬드 등

### 4. 투명도 설정
```
Properties → Appearance → Transparency
```
- 데이터 포인트가 많을 때 겹침 현상 완화
- 0-100% 범위에서 조절

## 축 옵션 설정

### 1. 축 범위 설정
```
Properties → Axes → [X-Axis/Y-Axis] → Scale
```
- **Auto**: 자동 범위 설정
- **Fixed**: 수동 범위 설정 (Min/Max 값 지정)
- **Zoom to filtered**: 필터된 데이터에 맞춰 조정

### 2. 축 변환
```
Properties → Axes → [X-Axis/Y-Axis] → Transform
```
- **Linear**: 선형 스케일 (기본값)
- **Log**: 로그 스케일
- **Square root**: 제곱근 변환

### 3. 축 레이블 및 제목
```
Properties → Axes → [X-Axis/Y-Axis]
```
- **Title**: 축 제목 설정
- **Show labels**: 축 레이블 표시/숨김
- **Label format**: 숫자 형식 설정

## 추세선 및 통계 분석

### 1. 추세선 추가
```
Properties → Lines & Curves → Curve
```
- **Linear**: 선형 회귀선
- **Polynomial**: 다항식 회귀선
- **Exponential**: 지수 회귀선
- **Logarithmic**: 로그 회귀선
- **Power**: 거듭제곱 회귀선
- **Smooth**: 스무딩 곡선

*🖼️ [이미지 추가 위치: Lines & Curves 설정 패널]*
*🖼️ [이미지 추가 위치: 다양한 추세선이 적용된 Scatter Plot 예시]*

### 2. 신뢰구간 표시
```
Properties → Lines & Curves → Show confidence bands
```
- 추세선의 신뢰구간 표시
- 신뢰도 수준 설정 가능 (보통 95%)

### 3. 참조선 추가
```
Properties → Lines & Curves → Reference lines
```
- **Horizontal/Vertical lines**: 특정 값에서 참조선
- **Diagonal line**: 대각선 참조선 (y=x 등)

## 필터링 및 상호작용

### 1. 브러싱(Brushing)
- 마우스로 데이터 포인트 선택
- 선택된 포인트는 다른 시각화에서도 하이라이트
- **Ctrl + 클릭**: 다중 선택
- **Shift + 드래그**: 영역 선택

### 2. 필터 패널 활용
```
View → Panels → Filters
```
- 데이터 필터링으로 관심 있는 데이터만 표시
- 동적 필터링으로 실시간 분석

### 3. 줌 및 팬
- **마우스 휠**: 줌 인/아웃
- **드래그**: 팬(이동)
- **더블클릭**: 전체 보기로 복원

## 다중 시리즈 설정

### 1. Trellis 차트
```
Properties → Trellis
```
- 범주형 변수별로 별도 패널 생성
- **Columns/Rows**: 패널 배치 설정
- **Individual axis scales**: 각 패널별 독립적인 축 스케일

### 2. 여러 Y축 설정
- Y축에서 **Add** 버튼 클릭하여 추가 Y축 생성
- 서로 다른 스케일의 변수들을 동시 비교

## 반도체 데이터 분석 활용 예시

### 1. 웨이퍼 맵 분석
```
X축: X 좌표
Y축: Y 좌표
Color By: 테스트 결과 (Pass/Fail)
Size By: 측정값 (예: 저항, 임계전압)
```

*🖼️ [이미지 추가 위치: 웨이퍼 맵 Scatter Plot 예시]*

### 2. 공정 파라미터 상관관계 분석
```
X축: 온도
Y축: 수율
Color By: 장비 ID
추세선: Linear regression
```

*🖼️ [이미지 추가 위치: 공정 파라미터 상관관계 분석 Scatter Plot]*

### 3. 품질 관리 차트
```
X축: 시간 (날짜/시간)
Y축: 측정값
Color By: 규격 범위 (In spec/Out of spec)
참조선: 규격 상한/하한선
```

## 팁 및 모범 사례

### 1. 성능 최적화
- 대용량 데이터의 경우 **Show max** 설정으로 표시할 포인트 수 제한
- 필요시 데이터 샘플링 고려

### 2. 가독성 향상
- 적절한 색상 대비 사용
- 범례 위치 최적화
- 축 제목과 차트 제목 명확하게 작성

### 3. 상호작용성 활용
- 마킹과 필터링을 활용한 동적 분석
- 여러 시각화 간의 연동 활용

### 4. 내보내기 및 공유
```
File → Export → Export Images
```
- 고해상도 이미지로 내보내기
- 보고서나 프레젠테이션용 형식 선택

## 자주 사용하는 단축키

| 기능 | 단축키 |
|------|--------|
| 줌 인 | Ctrl + 마우스 휠 위 |
| 줌 아웃 | Ctrl + 마우스 휠 아래 |
| 전체 보기 | Ctrl + 0 |
| 선택 해제 | Esc |
| 다중 선택 | Ctrl + 클릭 |

## 문제 해결

### 1. 데이터가 표시되지 않는 경우
- 축 설정 확인
- 필터 상태 확인
- 데이터 타입 호환성 확인

### 2. 성능이 느린 경우
- 표시할 데이터 포인트 수 제한
- 불필요한 시각적 효과 제거
- 메모리 사용량 모니터링

### 3. 색상이 제대로 표시되지 않는 경우
- Color By 설정 확인
- 색상 스킴 변경
- 데이터 범위 확인

---

*이 매뉴얼은 Spotfire의 Scatter Plot 기능을 효율적으로 활용하기 위한 가이드입니다. 실제 사용 시 데이터의 특성에 맞게 설정을 조정하시기 바랍니다.*