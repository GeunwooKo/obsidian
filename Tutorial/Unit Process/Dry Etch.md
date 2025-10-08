# 반도체 Dry Etch 공정 개요

## 개요

반도체 dry etch(건식 식각) 공정은 플라즈마를 이용하여 웨이퍼 표면의 물질을 선택적으로 제거하는 핵심 공정입니다. 습식 식각과 달리 화학 용액 대신 반응성 가스와 플라즈마를 사용하여 미세 패턴을 형성합니다.

---

## 주요 공정 파라미터

### 1. 압력 (Chamber Pressure)
- **범위**: 1~500 mTorr
- **저압**: 방향성(anisotropic) 식각 향상
- **고압**: 화학적 식각 우세, 등방성(isotropic) 특성

### 2. Gas Flow Rate
- **범위**: 10~500 sccm
- **영향**: 식각 속도 및 균일도에 직접 영향
- **주의**: 과도한 유량은 압력 상승 및 플라즈마 불안정

### 3. Source Power (ICP/TCP Power)
- **범위**: 200~3000W
- **역할**: 플라즈마 밀도 결정
- **주의**: 과도한 power는 마스크 선택비 저하

### 4. Bias Power (RF Power)
- **범위**: 50~500W (13.56 MHz)
- **역할**: 이온 가속 에너지, 식각 방향성 제어
- **주의**: 높은 bias는 기판 손상 증가

### 5. 공정 온도
- **범위**: -20°C ~ 80°C
- **저온**: Polymer 형성 촉진, sidewall passivation
- **고온**: 부산물 탈착 촉진, 식각 속도 향상

### 6. Helium Backside Cooling
- **압력**: 5~50 Torr
- **역할**: 웨이퍼 온도 균일 제어
- **중요성**: 온도 편차 방지, 균일도 확보

---

## 주요 공정 가스

### 불소계 가스 (Fluorine-based)

**CF4, C4F8, CHF3**
- **용도**: Silicon oxide, Silicon nitride 식각
- **CF4**: 높은 F 라디칼, 빠른 식각
- **C4F8**: 높은 C/F 비율, 우수한 선택비
- **CHF3**: 중간 특성, contact hole 식각

**SF6**
- **용도**: Silicon, Poly-silicon, Tungsten 식각
- **특징**: 높은 식각 속도, 등방성 경향

### 염소계 가스 (Chlorine-based)

**Cl2, BCl3**
- **용도**: 알루미늄 금속 배선 식각
- **BCl3**: 산화막 제거, Scavenger 역할
- **Cl2**: 높은 식각 속도

**HBr**
- **용도**: Poly-silicon, Gate 식각
- **특징**: 우수한 프로파일, 낮은 온도에서 sidewall passivation

### 첨가 가스

**O2**: Photoresist 제거, Polymer 제어
**Ar**: 물리적 스퍼터링, 플라즈마 안정화
**N2, H2**: Passivation layer 형성, 프로파일 제어

---

## 물질별 Dry Etch 가이드

### 1. Silicon Dioxide (SiO2)

#### 주요 가스 조합
- CF4/O2: CF4 (80-200 sccm) + O2 (5-20 sccm)
- CHF3/CF4: CHF3 (50-150 sccm) + CF4 (20-80 sccm)
- C4F8/O2: C4F8 (20-60 sccm) + O2 (10-30 sccm)

#### 공정 조건
```
압력: 5-50 mTorr
Source Power: 800-2000W
Bias Power: 100-500W
온도: 0-40°C
```

#### 선택비
- SiO2/Si: 5:1 ~ 15:1
- SiO2/SiN: 3:1 ~ 8:1
- 식각 속도: 1000-3000 Å/min

### 2. Silicon Nitride (SiN)

#### 주요 가스 조합
- CF4/O2: CF4 (100-250 sccm) + O2 (10-40 sccm)
- CHF3/O2: CHF3 (80-200 sccm) + O2 (15-50 sccm)
- SF6/O2: SF6 (50-150 sccm) + O2 (5-25 sccm)

#### 공정 조건
```
압력: 10-80 mTorr
Source Power: 1000-2500W
Bias Power: 150-600W
온도: 20-60°C
```

#### 선택비
- SiN/SiO2: 8:1 ~ 20:1
- SiN/Si: 10:1 ~ 25:1
- 식각 속도: 800-2500 Å/min

### 3. Aluminum (Al)

#### 주요 가스 조합
- Cl2/BCl3: Cl2 (80-200 sccm) + BCl3 (20-80 sccm)
- Cl2/BCl3/CHCl3: 복합 조합
- BCl3/Cl2/Ar: Ar 추가로 재증착 방지

#### 공정 조건
```
압력: 2-20 mTorr
Source Power: 500-1500W
Bias Power: 80-300W
온도: 40-80°C
```

#### 선택비
- Al/SiO2: 15:1 ~ 50:1
- 식각 속도: 3000-8000 Å/min

#### 특별 고려사항
- **자연 산화막** (Al2O3) 제거 breakthrough step 필요
- **Corrosion 방지**: 낮은 습도 (<1% RH) 필수
- **재증착 방지**: 충분한 ion bombardment

### 4. Tungsten (W)

**상세 내용은 [[Tungsten Etching Comprehensive Guide]] 참조**

#### 주요 가스
- SF6/O2, SF6/N2, NF3/Ar

#### 선택비
- W/SiO2: 20:1 ~ 80:1
- W/TiN: 8:1 ~ 25:1
- 식각 속도: 2000-6000 Å/min

### 5. Titanium Nitride (TiN)

#### 주요 가스 조합
- Cl2/BCl3/Ar: Cl2 (60-150 sccm) + BCl3 (30-80 sccm) + Ar (50-200 sccm)
- SF6/Cl2: SF6 (80-200 sccm) + Cl2 (40-100 sccm)

#### 공정 조건
```
압력: 3-30 mTorr
Source Power: 600-1800W
Bias Power: 100-500W
온도: 40-80°C
```

#### 선택비
- TiN/SiO2: 10:1 ~ 30:1
- TiN/W: 15:1 ~ 40:1
- 식각 속도: 1500-4000 Å/min

---

## 공정 모니터링

### End Point Detection (EPD)

**Optical Emission Spectroscopy (OES)**
- SiO2: CO (483nm), SiF (440nm)
- Si: SiF (440nm), SiCl (287nm)
- Al: AlCl (261nm, 308nm)
- W: WF (429nm)
- TiN: TiCl (519nm)

**Mass Spectrometry**
- 반응 부산물 실시간 모니터링
- SiF4 (m/z=104), AlCl3 (m/z=133), WF6 (m/z=298)

### 공정 제어 목표

```
CD Control: ±5% within wafer
Profile: 88-92° sidewall angle
Selectivity: >10:1 (물질별)
Uniformity: ±3% (1σ) across wafer
```

---

## 고급 Etch 기술

### 1. Atomic Layer Etching (ALE)
- **원리**: 흡착 → 반응 → 탈착의 자기제한 반응
- **장점**: 원자 수준 정밀 제어
- **응용**: 3D NAND, FinFET, Gate-all-around

### 2. Directional ALE
- **방법**: Ion bombardment + Chemical reaction
- **특징**: 수직 방향 선택적 식각
- **응용**: Spacer 형성, HAR contact

### 3. Cryogenic Etch
- **온도**: -120°C ~ -80°C
- **장점**: 극도로 높은 선택비
- **응용**: Deep silicon etch, MEMS

---

## 주요 결함 및 해결방안

### 1. Microloading
- **현상**: 패턴 밀도에 따른 식각 속도 차이
- **해결**: 가스 유량 증가, 압력 최적화

### 2. ARDE (Aspect Ratio Dependent Etching)
- **현상**: AR에 따른 식각 속도 감소
- **해결**: 높은 이온 에너지, Pulsed plasma

### 3. Sidewall Bowing
- **현상**: Sidewall이 볼록하게 식각
- **해결**: Polymerizing gas 추가, 온도 조절

### 4. Black Silicon (SiO2 Etch)
- **현상**: 높은 AR에서 발생
- **해결**: 공정 조건 최적화

### 5. Corrosion (Metal Etch)
- **현상**: 습도 및 잔여 Cl 영향
- **해결**: 낮은 습도 유지, Post-etch cleaning

---

## 안전 및 환경

### 1. 독성 가스 관리
- 농도 모니터링 시스템 필수
- 비상 정지 시스템 구비
- 개인 보호 장비 착용

### 2. 폐가스 처리
- **Wet scrubber**: 수용성 가스 제거
- **Dry scrubber**: 고체 흡착제 사용
- **Thermal oxidizer**: 유기물 완전 연소

### 3. 온실가스
- CF4, CHF3 등 배출 최소화
- 대체 가스 개발 및 적용

---

## 관련 문서

- [[Tungsten Etching Comprehensive Guide]] - Tungsten 에칭 상세 가이드
- [[Dry Etch Mechanisms]] - 에칭 메커니즘 원리
- [[Aluminum-Cl-Etch-ACT945-Cleaning]] - Al 에칭 후처리
- [[Temperature Effect on Reaction Rate]] - 온도 영향

---

**Tags**: #dry-etch #semiconductor #plasma #process-control

**작성일**: 2025-10-06
**최종 수정**: 2025-10-06
