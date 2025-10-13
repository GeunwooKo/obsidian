# 반도체 Dry Etch 공정 개요

## 목차
1. [[#개요]]
2. [[#주요 공정 파라미터]]
3. [[#주요 공정 가스]]
4. [[#물질별 Dry Etch 가이드]]
5. [[#공정 모니터링]]
6. [[#고급 Etch 기술]]
7. [[#주요 이슈 및 해결방안]]
8. [[#안전 및 환경]]
9. [[#관련 문서]]

---

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

## 주요 이슈 및 해결방안

### 1. Microloading Effect

#### 현상 설명
패턴 밀도에 따라 식각 속도가 달라지는 현상입니다. Dense 패턴 영역은 Open 영역보다 식각 속도가 느립니다.

**발생 메커니즘**
- **Reactant depletion**: Dense 영역에서 반응성 라디칼 고갈
- **Ion shadowing**: 좌우 구조물에 의한 이온 차폐
- **Neutral transport limitation**: 패턴 내부로 중성종 확산 제한
- **Product removal limitation**: 부산물 배출 제한

#### 정량적 평가
```
Microloading = (ER_open - ER_dense) / ER_open × 100%

목표: <5% within die
심각 수준: >10%
```

#### 해결 방안

**1. 가스 조건 최적화**
```python
# 라디칼 공급 증가
- 주 반응 가스 유량 15-30% 증가
- 예: CF4 150 sccm → 180 sccm

# 압력 조정
- 압력 증가: 5 mTorr → 15 mTorr
- 효과: 라디칼 수명 증가, 확산 향상
```

**2. Overetch Time 증가**
- Main etch 100% 후 20-40% overetch
- Dense 영역 완전 개방 보장

**3. 다단계 에칭 전략**
```
Step 1: Breakthrough (20-30%)
  - 높은 bias, 빠른 개방
  
Step 2: Main Etch (60-70%)
  - 균일 에칭, 적정 선택비
  
Step 3: Soft Landing (10-20%)
  - 낮은 bias, 선택비 향상
```

**4. 플라즈마 조건 변경**
- Source power 증가: 플라즈마 밀도 향상
- 높은 온도: 부산물 탈착 촉진

---

### 2. ARDE (Aspect Ratio Dependent Etching)

#### 현상 설명
홀 또는 트렌치의 종횡비(AR)가 증가할수록 식각 속도가 감소하는 현상입니다.

**발생 메커니즘**
- **Ion angular distribution**: 좌우 벽면 충돌로 이온 에너지 감소
- **Neutral shadowing**: 중성종의 직진성으로 홀 바닥 도달 제한
- **Knudsen transport**: AR >5:1에서 분자 충돌 효과 지배적
- **Charging effect**: 절연체 침투 때 전하 축적

#### 정량적 평가
```
Lag = (Depth_AR3 - Depth_AR10) / Depth_AR3 × 100%

AR 3:1 대비 AR 10:1 식각 깊이 차이
목표: <10%
심각: >30%
```

#### 해결 방안

**1. 이온 에너지 증가**
```python
# Bias power 증가
Bias: 200W → 400W
이온 에너지: 150eV → 300eV
효과: 바닥 깊이 도달성 향상

주의: 선택비 감소 및 벽면 손상 가능
```

**2. Pulsed Plasma 기법**
```
조건:
- Duty cycle: 20-50%
- Frequency: 100Hz - 1kHz
- Peak power: 2-3x 연속 모드

장점:
- 높은 순간 이온 에너지
- 전하 축적 감소
- 선택비 유지
```

**3. 압력 감소**
- 15 mTorr → 5 mTorr
- 중성종 평균 자유 경로 증가
- 직진성 향상

**4. 온도 최적화**
- 낮은 온도 (-20°C): Polymer passivation 향상
- 높은 온도 (60°C): 부산물 탈착 촉진

**5. 가스 화학 조정**
```
# Passivation gas 추가
C4F8 또는 CHF3 5-15% 추가
효과: Sidewall 보호, 수직 프로파일
```

---

### 3. Sidewall Profile 문제

#### 3-1. Sidewall Bowing (볼록 프로파일)

**현상**
- Sidewall 중간 부분이 바깥쪽으로 볼록하게 돌출
- Critical dimension (CD) 제어 어려움

**원인**
```
1. 과도한 화학적 에칭
   - 라디칼 공격이 sidewall에 집중
   
2. Polymer 부족
   - Sidewall passivation layer 형성 불충분
   
3. 온도 불균일
   - 상부/하부 온도 차이
```

**해결 방법**

1. **Polymerizing Gas 추가**
```python
# C4F8/Ar 추가
C4F8: 5-20 sccm
Ar: 50-150 sccm
효과: CF2 polymer 형성, sidewall 보호
```

2. **온도 제어**
```
Chuck 온도: 10-20°C
He backside: 10-20 Torr
효과: Polymer 분포 균일화
```

3. **Bias Power 감소**
- 400W → 250W
- 화학적 에칭 비율 증가

4. **다단계 공정**
```
Step 1 (0-70%): 높은 polymer, 수직 profile
Step 2 (70-100%): 낮은 polymer, 선택비 향상
```

#### 3-2. Sidewall Roughness

**현상**
- Sidewall 표면의 미세 거칠기
- LER (Line Edge Roughness) 증가
- 전기적 특성 저하

**원인**
```
1. 과도한 물리적 스퍼터링
   - 높은 이온 에너지
   
2. 불균일 Polymer 형성
   - 불안정 Passivation
   
3. Micro-masking
   - 부산물 재증착
```

**해결 방법**

1. **Bias Power 감소**
```
Bias: 400W → 200W
이온 에너지 감소
```

2. **안정적 Passivation**
```
온도: 0-10°C (저온)
C4F8 비율 증가
```

3. **Overetch 최소화**
- Endpoint detection 정확도 향상
- Overetch 10-20%로 제한

#### 3-3. Footing (바닥 잔여)

**현상**
- 패턴 바닥에 잔여물 남음
- CD 증가 및 패턴 변형

**원인**
```
1. Overetch 부족
2. Polymer 과다 형성
3. 낮은 이온 에너지
4. Micro-loading effect
```

**해결 방법**

1. **Overetch Time 증가**
```
Main etch 100% + Overetch 30-50%
```

2. **오버에칭 가스 변경**
```
# Cl2/O2 오버에칭
Cl2: 80 sccm
O2: 20 sccm
효과: Polymer 제거, 잔여물 클리닝
```

3. **높은 Bias Power**
- Overetch 단계에서 Bias 증가
- 물리적 에칭 기여도 증가

---

### 4. Notching (Charging Damage)

#### 현상 설명
절연체 위의 도체 패턴 식각 시, 패턴 하부에 수평 방향으로 파인 notch 형성

**발생 메커니즘**
```
1. Electron shading
   - 전자와 이온의 모빌리티 차이
   
2. Differential charging
   - 절연체 표면 양전하 축적
   - 도체 패턴 음전하 축적
   
3. Ion deflection
   - 전기장에 의한 이온 경로 굴절
   - Sidewall 수평 공격
```

#### 해결 방안

**1. Pulsed Plasma**
```python
Duty cycle: 10-30%
Off-time: 전하 중화 시간 확보
효과: 전하 축적 방지
```

**2. 저주파 RF 추가**
```
Dual frequency bias:
- HF: 13.56 MHz (주 에너지)
- LF: 400 kHz (전하 중화)
```

**3. Conducting Layer 사용**
```
# Bottom Anti-Reflective Coating (BARC)
- 도전성 BARC 사용
- 전하 분산 경로 제공
```

**4. 공정 순서 변경**
```
# Insulator-first → Metal-first
절연체 식각 후 도체 증착/식각
```

---

### 5. Black Silicon / Grass Formation

#### 현상
- Silicon 표면에 미세 돌기 형성
- 검은 외관
- HAR Contact/Via 에칭에서 발생

**원인**
```
1. Micro-masking
   - 불순물 입자가 mask 역할
   - Ni, Fe, Cu 등 금속 오염
   
2. Polymer re-deposition
   - CF polymer 재증착
   
3. 불균일 에칭
   - 국부 식각 속도 편차
```

#### 해결 방안

**1. Pre-cleaning**
```python
# HF dip
- 0.5% HF, 30초
- 효과: 자연 산화막 제거

# Piranha cleaning
- H2SO4:H2O2 = 3:1, 10분
- 효과: 유기물 및 금속 제거
```

**2. 공정 조건 최적화**
```
# Polymer 감소
O2 비율 증가: 5% → 15%
온도 증가: 20°C → 40°C

# 이온 에너지 증가
Bias power: 200W → 350W
```

**3. Post-etch Treatment**
```
# O2 plasma ashing
O2: 200 sccm
Power: 500W
시간: 60초
```

---

### 6. Metal Corrosion (Al Etch)

#### 현상
- 식각 후 시간 경과에 따라 Al 부식 발생
- White residue 형성 (AlCl3·xH2O)
- 전기적 open/short 발생

**원인**
```
1. 잔여 Cl 가스
   - Cl2 분자 표면 흡착
   
2. 습기 반응
   - AlCl3 + 3H2O → Al(OH)3 + 3HCl
   - HCl이 추가 부식 가속
   
3. Galvanic corrosion
   - Al/Cu, Al/Ti 전위차 부식
```

#### 해결 방안

**1. 저습도 유지**
```python
# 장비 내 습도 제어
목표: <1% RH
방법:
- N2 purge 지속
- 습도 센서 모니터링
- Load-lock 습도 <5% RH
```

**2. Post-etch Cleaning**
```
# In-situ plasma treatment
Step 1: O2 plasma (60초)
  - 유기물 제거
  
Step 2: H2O vapor + Ar plasma (30초)
  - AlClx 수화물 형성 및 증발
  - AlCl3 + H2O → Al(OH)3 + HCl (↑)
```

**3. Ex-situ Wet Cleaning**
```
# DI water rinse
- 60°C DI water, 5분
- AlCl3 용해 제거

# Passivation treatment
- 0.1% H3PO4, 2분
- Al 표면 산화막 형성
```

**4. Breakthrough Step 최적화**
```
# Al2O3 native oxide 제거
BCl3 plasma:
- BCl3: 100 sccm
- Power: 300W
- Time: 10-15초

효과: Cl2 과다 노출 방지
```

**5. 공정 후 신속 처리**
```
Etch → Cleaning 시간: <30분
이상적: <10분
```

---

### 7. RIE Lag

#### 현상
패턴 크기(CD)에 따라 식각 속도가 달라지는 현상. 큰 홀이 작은 홀보다 빠르게 식각됨.

**원인**
```
1. Ion current density 차이
   - 큰 홀: 높은 이온 플럭스
   - 작은 홀: 낮은 이온 플럭스
   
2. Charging effect
   - 작은 홀에서 전하 축적 심화
   
3. Neutral depletion
   - 작은 홀 내부 라디칼 고갈
```

#### 해결 방안

**1. 압력 증가**
```
5 mTorr → 20 mTorr
효과: 이온 각도 분포 완화, 균일성 향상
```

**2. Source Power 증가**
```
플라즈마 밀도 증가
→ 라디칼 공급 증가
```

**3. Overetch 연장**
```
작은 홀 완전 개방 보장
```

---

### 8. Etch Stop 문제

#### 현상
식각이 의도치 않게 멈추거나 극도로 느려지는 현상

**원인**
```
1. Etch byproduct 재증착
   - 비휘발성 부산물 축적
   
2. Polymer 과다 형성
   - 표면 passivation layer 두께 증가
   
3. 온도 이상
   - 과냉각으로 인한 증발 억제
   
4. Endpoint overshot
   - EPD 오작동
```

#### 해결 방안

**1. 온도 조절**
```
과냉각 확인 및 조정
적정 온도 범위 유지
```

**2. O2 추가**
```
O2: 10-20 sccm 추가
효과: Polymer 제거, 부산물 휘발
```

**3. Power 증가**
```
Bias/Source power 단계적 증가
물리적 에칭 기여도 증가
```

**4. Breakthrough Plasma**
```
# 고에너지 짧은 pulse
Ar plasma
Bias: 500W
Time: 5-10초
```

---

### 9. Striations (Sidewall 줄무늬)

#### 현상
Sidewall에 수평 방향 주기적 줄무늬 패턴 형성

**원인**
```
1. Plasma instability
   - 플라즈마 파워 변동
   - Impedance mismatch
   
2. 주기적 polymer 형성/제거
   - Temperature fluctuation
   - Gas flow pulsation
   
3. 장비 공진
   - RF matching 불안정
```

#### 해결 방안

**1. 플라즈마 안정화**
```
# Impedance matching 최적화
Auto-matching 활성화
Matching network 점검
```

**2. 온도 안정화**
```
He backside pressure 안정화
Chiller 온도 변동 <±0.5°C
```

**3. Gas Flow 안정화**
```
MFC calibration
Pressure control 정밀도 향상
```

**4. 공정 조건 조정**
```
압력 감소: 변동 영향 최소화
Polymer 감소: 안정적 profile
```

---

### 10. Non-uniformity (균일도 문제)

#### 현상
웨이퍼 내 식각 속도 또는 CD 편차 발생

**원인 분석**
```
1. Center-fast
   - 플라즈마 밀도 중심 집중
   - Gas depletion at edge
   
2. Edge-fast
   - RF edge effect
   - 온도 edge 상승
   
3. Azimuthal non-uniformity
   - Gas flow 방향성
   - Slot valve effect
```

#### 해결 방안

**1. Hardware 조정**
```
# Showerhead 조정
- Gas hole 분포 최적화
- Center/Edge flow ratio

# Focus ring 조정
- RF uniformity 개선
- Edge plasma 제어
```

**2. 공정 조건 최적화**
```
# Center-fast 개선
- 압력 증가
- Source power 감소
- Gas flow 증가

# Edge-fast 개선
- Bias power 증가 (center)
- 온도 edge 감소
```

**3. Multi-zone 제어**
```
Center/Edge independent:
- Temperature control
- Gas flow control
- Power distribution
```

---

## 이슈 진단 체크리스트

### 식각 속도 관련
- [ ] Microloading 평가 (Open vs Dense)
- [ ] ARDE 평가 (AR 변화)
- [ ] RIE Lag 평가 (CD 변화)
- [ ] Uniformity 측정 (Wafer map)

### Profile 관련
- [ ] Sidewall angle 측정
- [ ] Bowing 유무 확인
- [ ] Footing 유무 확인
- [ ] LER/LWR 측정

### 선택비 관련
- [ ] Mask 손실률
- [ ] Underlying layer 손상
- [ ] Overetch margin

### 결함 관련
- [ ] Notching 유무
- [ ] Black silicon 확인
- [ ] Residue 검사
- [ ] Corrosion 테스트 (Al)

### 공정 안정성
- [ ] Run-to-run variation
- [ ] Chamber seasoning 상태
- [ ] Plasma stability (Vpp, Vdc)
- [ ] Temperature uniformity

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
