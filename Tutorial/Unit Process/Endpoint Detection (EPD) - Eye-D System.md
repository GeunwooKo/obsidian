# Endpoint Detection (EPD) in Dry Etch - Eye-D 시스템

## 개요

**EPD (Endpoint Detection)**는 드라이 에칭 공정에서 에칭이 완료되는 정확한 시점을 실시간으로 감지하는 기술입니다. Eye-D는 이러한 EPD 시스템의 상용 제품 중 하나로, 광학 기반 엔드포인트 검출 시스템입니다.

**중요성**:
- Over-etch 방지 (하부막 손상 방지)
- Under-etch 방지 (잔류물 제거 부족)
- 공정 재현성 향상
- 수율(Yield) 향상
- 공정 시간 최적화

---

## 목차

- [[#Endpoint Detection 기본 개념]]
- [[#EPD 필요성]]
- [[#EPD 방법 종류]]
- [[#OES (Optical Emission Spectroscopy)]]
- [[#Laser Interferometry]]
- [[#Eye-D 시스템 특징]]
- [[#EPD 신호 분석]]
- [[#응용 사례]]
- [[#문제 해결 가이드]]
- [[#Best Practices]]
- [[#관련 문서]]

---

## Endpoint Detection 기본 개념

### 정의

**Endpoint**는 다음을 의미합니다:
1. **Main Etch 완료 시점**: 타겟 막이 완전히 제거된 시점
2. **특정 깊이 도달**: 원하는 에칭 깊이에 도달한 시점
3. **막 전환 시점**: 한 막에서 다른 막으로 전환되는 시점

### 일반적인 에칭 프로세스

```
Start Etch
    ↓
Breakthrough (막 관통 시작)
    ↓
Main Etch (주 에칭)
    ↓
→ Endpoint Detection ← (여기서 감지!)
    ↓
Over-etch (추가 에칭, 균일성 확보)
    ↓
End Etch
```

### EPD의 장점

1. **실시간 모니터링**
   - 각 웨이퍼마다 정확한 시점 감지
   - 공정 변동 자동 보상

2. **수율 향상**
   - Under-etch 방지 (잔류물)
   - Over-etch 방지 (하부막 손상)

3. **재현성**
   - Run-to-run 일관성
   - Wafer-to-wafer 균일성

4. **공정 최적화**
   - Over-etch 시간 최소화
   - Throughput 향상

---

## EPD 필요성

### Under-etch vs Over-etch

#### Under-etch (부족 에칭)
**결과**:
- 잔류물 (residue) 남음
- 전기적 단락 (short) 가능
- 콘택 저항 증가
- **Device failure**

#### Over-etch (과다 에칭)
**결과**:
- 하부막 손상
- Sidewall 손상
- Pattern 변형/열화
- CD 증가
- **Device 성능 저하**

### 에칭 속도 변동 요인

1. **챔버 상태**: 벽면 코팅, 사용 횟수
2. **공정 조건**: 온도, 압력, Gas flow
3. **웨이퍼 상태**: 막 두께, Pattern density
4. **재료 특성**: 막 품질, Doping

---

## EPD 방법 종류

### 비교표

| 방법 | 감도 | 속도 | 비용 | 적용 | 주요 사용 |
|------|------|------|------|------|----------|
| **OES** | 높음 | 빠름 | 중간 | 넓음 | **생산** |
| **Laser** | 매우높음 | 빠름 | 중간 | 제한적 | 생산/R&D |
| **Mass Spec** | 매우높음 | 중간 | 높음 | 넓음 | R&D |
| **RF Monitor** | 낮음 | 빠름 | 낮음 | 제한적 | 보조 |

---

## OES (Optical Emission Spectroscopy)

### 기본 원리

**플라즈마 발광 메커니즘**:
```
Target Layer 에칭 → By-product 생성
    ↓
Plasma에서 여기 (Excitation)
    ↓
특정 파장 빛 방출
    ↓
OES 감지 → Intensity 측정
    ↓
Endpoint: 강도 급변
```

### 시스템 구성

```
Plasma Chamber (View port)
    ↓
Fiber Optic Cable
    ↓
Spectrometer (분광기)
    ↓
Detector (CCD/PMT)
    ↓
PC/Software
```

### 주요 모니터링 파장

| 원소/분자 | 파장 (nm) | 적용 |
|----------|-----------|------|
| **F** | 704, 686 | SiO₂, Si₃N₄ 에칭 |
| **CO** | 484, 520 | Carbon 기반 |
| **Cl** | 725, 837 | Poly-Si, Metal |
| **Si** | 288, 252 | Si 에칭 |
| **O** | 777, 845 | Oxide |
| **Ar** | 750, 811 | Reference |
| **Al** | 394, 396 | Al 에칭 |
| **W** | 400, 430 | W 에칭 |

### OES Endpoint 감지 방법

#### 1. Single Wavelength
**예시 - SiO₂ 에칭**:
```
F emission (704nm) 모니터링
SiO₂ 에칭 중: F 소모 → 강도 낮음
SiO₂ 제거: F 소모 안됨 → 강도 급증
```

#### 2. Wavelength Ratio
**예시 - Poly-Si on SiO₂**:
```
Ratio = I(Si) / I(F)

Poly-Si: Si 신호 강함 → 비율 높음
SiO₂ 노출: Si 감소, F 증가 → 비율 급감
```

#### 3. Multivariate Analysis
- PCA (Principal Component Analysis)
- K-means Clustering
- Machine Learning

### OES 신호 트레이스

#### SiO₂ 에칭 (F 모니터링)
```
Intensity
    ↑
    |          ┌────────← Endpoint
    |         /
    |  ──────/
    |
    └────────────────────→ Time
```

#### Poly-Si on SiO₂ (Ratio)
```
I(Si)/I(F)
    ↑
    |  ─────────\
    |            \← Endpoint
    |             ───────
    |
    └────────────────────→ Time
```

### OES 제약사항

**Open Area 요구**:
- 최소 수 cm² 필요
- Small die → 약한 신호

**해결책**:
- 고감도 검출기
- Multivariate 분석
- 여러 파장 조합

---

## Laser Interferometry

### 기본 원리

**간섭 현상**:
```
광학 경로차 = 2 × n × d
(n: 굴절률, d: 막 두께)

보강 간섭: 2nd = mλ
상쇄 간섭: 2nd = (m + 0.5)λ
```

### 시스템 구성

```
Laser Source (670/905/980nm)
    ↓
Focusing Lens
    ↓
Wafer (spot: 20-60μm)
    ↓
Reflected Light
    ↓
Detector + Camera
```

### Laser EPD 감지 방법

#### 1. Reflectance Change
**Metal 에칭**:
```
Metal: 높은 반사율
    ↓ Endpoint
Oxide/Si: 낮은 반사율
```

#### 2. Interference Fringes
**투명막 에칭**:
```
Ripple 개수 → 에칭 깊이

Δd = λ / (2n)
```

### 장단점

**장점**:
- 직접 측정, 고정밀
- Small sample 가능
- Depth control

**단점**:
- Manual positioning (생산 부적합)
- 제한적 응용
- Local measurement

---

## Eye-D 시스템 특징

### 주요 기능

1. **Real-time Monitoring**
   - ms 단위 데이터
   - 즉각 감지

2. **Automatic Detection**
   - 자동 endpoint call
   - 장비 제어 신호

3. **Multi-wavelength**
   - 여러 파장 동시
   - Ratio 계산

4. **Data Logging**
   - 트레이스 저장
   - Trend 분석

### Eye-D 구성

```
Etch Chamber ← Eye-D Sensor
    ↓
Fiber/Laser Module
    ↓
Control Unit
    ↓
Software (PC)
    ↓
Tool Control
```

### 설정 파라미터

#### OES 모드

1. **Wavelength Selection**
   - Primary wavelength
   - Reference wavelength

2. **Threshold**
   - Trigger level
   - Noise filtering

3. **Analysis Method**
   - Single/Ratio/Derivative
   - PCA/ML

4. **Timing**
   - Detection window
   - Confirmation time

### 사용 절차

#### 초기 설정

1. **Baseline 측정**
   - Dummy wafer
   - Plasma on, no etch

2. **Test Run**
   - 전체 트레이스 관찰
   - 수동 endpoint 확인

3. **Parameter 최적화**
   - 파장 선택
   - Threshold 설정

4. **Verification**
   - 여러 wafer 검증
   - 정확도 확인

---

## EPD 신호 분석

### 좋은 신호 특성

1. **High S/N Ratio**
```
S/N = Signal Change / Noise

Good: S/N > 5
Marginal: S/N = 2-5
Poor: S/N < 2
```

2. **Clear Transition**
   - Sharp change
   - Distinct signal

3. **Reproducibility**
   - Run-to-run 일관성

### 신호 처리 기법

#### 1. Moving Average
- Noise 감소
- 실시간 가능

#### 2. Derivative
```
1차 미분: dI/dt
2차 미분: d²I/dt²
```
- 변화율 강조

#### 3. Wavelet Transform
- Multi-resolution
- Noise 분리

#### 4. PCA
- 고차원→저차원
- Pattern recognition

---

## 응용 사례

### 1. Contact/Via Etch

**공정**: SiO₂ etch

**EPD**:
- F (704nm) 모니터
- SiO₂ → Si 전환
- Over-etch: 30-50%

### 2. Poly-Si Gate

**공정**: Poly-Si on gate oxide

**EPD**:
- Si/F ratio
- Over-etch: 20-30%

### 3. Metal Etch (Al, Cu)

**공정**: Al/Cu interconnect

**EPD**:
- Al (394nm) 또는 Cu (521nm)
- Cl (837nm)

### 4. STI

**공정**: Si trench

**EPD**:
- Laser interferometry (preferred)
- Precise depth

### 5. HARC

**도전과제**:
- 작은 open area
- 약한 신호

**전략**:
- Multiple wavelengths
- PCA/ML
- Cumulative analysis

### 6. Oxide Strip (Descum)

**공정**: PR ash 후 잔류 oxide

**EPD**:
- CO (484nm) 감소
- Plasma cleaning

---

## 문제 해결 가이드

### 1. Weak Signal

**원인**:
- Small open area
- HARC
- Low etch rate

**해결**:
- 고감도 detector
- Multiple wavelengths
- Multivariate analysis
- Chamber seasoning

### 2. High Noise

**원인**:
- Plasma 불안정
- Arc, sputtering
- RF interference

**해결**:
- 시간 평균
- Filtering (DWT)
- Plasma 최적화

### 3. Gradual Change

**원인**:
- Non-uniform etch
- Slow transition

**해결**:
- Derivative 사용
- Inflection point
- 2차 미분

### 4. False Endpoint

**False Positive**:
- Breakthrough spike
- Plasma transient

**해결**:
- Detection window
- Confirmation time

**False Negative**:
- Weak signal
- Threshold 높음

**해결**:
- Threshold 조정
- 대체 파장

### 5. Baseline Drift

**원인**:
- Chamber coating 변화
- PM 후

**해결**:
- Chamber seasoning
- Dynamic baseline
- Ratio method

### 6. Pattern Dependency

**원인**:
- 다양한 pattern density
- Loading effect

**해결**:
- 각 제품별 설정
- Adaptive threshold
- ML model

---

## Best Practices

### 설정 최적화

1. **파장 선택**
   - 재료별 최적 파장
   - High S/N 확보
   - Reference 파장 활용

2. **Threshold 설정**
   - 여러 wafer 테스트
   - False detection 최소화
   - Safety margin

3. **Detection Window**
   - 예상 시간 ±20%
   - Breakthrough 제외
   - 충분한 margin

4. **Confirmation Time**
   - 0.5-2초 권장
   - Noise spike 무시
   - 안정적 신호 확인

### 모니터링

1. **Daily Check**
   - Baseline 확인
   - Signal quality
   - Detection accuracy

2. **Trend Analysis**
   - Endpoint time drift
   - Signal intensity 변화
   - Chamber 상태 추적

3. **Data Review**
   - 저장된 트레이스
   - False detection
   - 이상 신호

### 유지보수

1. **View Port 청소**
   - 정기적 점검
   - Coating 제거
   - Optical path 확보

2. **Fiber Optic**
   - 연결 확인
   - 파손 점검
   - 정렬 유지

3. **Calibration**
   - Wavelength 보정
   - Intensity 보정
   - 정기 교정

4. **Chamber Seasoning**
   - PM 후 conditioning
   - Baseline 안정화
   - 일관된 조건

### Recipe 개발

1. **Test Matrix**
```
- Baseline measurement
- 3-5 wafer test
- 파장 스캔
- Threshold 최적화
- Verification (10+ wafer)
```

2. **Documentation**
   - 설정값 기록
   - 신호 특성
   - 문제점/해결책

3. **Backup Strategy**
   - EPD fail 시 대책
   - Time-based backup
   - Alarm 설정

---

## 실전 Tips

### 신호 품질 향상

1. **Open Area 최적화**
   - Test structure 배치
   - Scribe lane 활용
   - Dense pattern 회피

2. **공정 조건**
   - 안정적 plasma
   - 적절한 pressure
   - Uniform temperature

3. **신호 처리**
   - Smoothing
   - Filtering
   - Baseline correction

### Troubleshooting 순서

```
1. 신호 확인
   - Trace review
   - S/N ratio
   - Baseline

2. 설정 점검
   - Wavelength
   - Threshold
   - Window

3. 하드웨어 확인
   - View port
   - Fiber
   - Detector

4. 공정 확인
   - Chamber 상태
   - Gas flow
   - Plasma stability

5. 대안 방법
   - 다른 파장
   - Ratio method
   - ML approach
```

### 고급 기술

1. **Multivariate EPD**
   - 전체 스펙트럼 사용
   - PCA score
   - ML classification

2. **Adaptive Threshold**
   - Real-time 조정
   - Historical data
   - Self-learning

3. **Virtual Metrology**
   - EPD + Process data
   - Etch depth 예측
   - 통합 모델

---

## 핵심 요약

### EPD 방법 선택

| 응용 | 권장 방법 | 이유 |
|------|----------|------|
| Contact/Via | OES | 넓은 open area |
| Gate | OES | 얇은 막, 정밀 제어 |
| Metal | OES/Laser | 반사율 변화 |
| STI | Laser | Depth control |
| HARC | OES+ML | 작은 signal |

### 주요 고려사항

1. **Open Area**: OES는 수 cm² 필요
2. **Pattern Density**: Loading effect 고려
3. **Signal Quality**: S/N > 5 목표
4. **Reproducibility**: ±5% 이내

### 성공 요소

✅ 적절한 파장 선택
✅ 최적 threshold
✅ Chamber seasoning
✅ 정기 유지보수
✅ Data 분석
✅ Backup 전략

---

## 관련 문서

### Internal Links
- [[Dry Etch]]
- [[Dry Etch Mechanisms]]
- [[Tungsten Etching Comprehensive Guide]]
- [[Aluminum-Cl-Etch-ACT945-Cleaning]]

### 추가 참고

**기술 논문**:
- OES for plasma monitoring
- Machine learning EPD
- HARC endpoint detection

**업체 자료**:
- Oxford Instruments EPD
- Applied Materials EPD
- Lam Research EPD

---

## Tags

#endpoint-detection #EPD #Eye-D #OES #laser-interferometry #dry-etch #plasma-monitoring #optical-emission #process-control #semiconductor

---

**작성일**: 2025-10-06
**최종 수정**: 2025-10-06
**참고**: General EPD technology overview