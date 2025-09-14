# 반도체 Dry Etch 공정 개요

반도체 dry etch(건식 식각) 공정은 플라즈마를 이용하여 웨이퍼 표면의 물질을 선택적으로 제거하는 핵심 공정입니다. 습식 식각과 달리 화학 용액 대신 반응성 가스와 플라즈마를 사용하여 미세 패턴을 형성합니다.

## Dry Etch의 기본 원리

Dry etch는 물리적 스퍼터링(sputtering)과 화학적 반응(chemical reaction)이 결합된 방식으로 진행됩니다. 플라즈마 내에서 생성된 이온과 라디칼이 웨이퍼 표면과 반응하여 휘발성 부산물을 형성하고, 이를 진공 펌프로 배출시켜 식각이 이루어집니다.

## 주요 공정 파라미터

### 1. 압력 (Chamber Pressure)
압력은 일반적으로 1~500 mTorr 범위에서 제어됩니다. 압력이 낮을수록 이온의 평균 자유 행로(mean free path)가 길어져 방향성(anisotropic) 식각이 향상됩니다. 반대로 압력이 높으면 라디칼 밀도가 증가하여 화학적 식각이 우세해지고 등방성(isotropic) 식각 특성을 보입니다.

### 2. Gas Flow Rate
가스 유량은 sccm(standard cubic centimeter per minute) 단위로 제어되며, 일반적으로 10~500 sccm 범위에서 조절됩니다. 적절한 유량 제어는 식각 속도와 균일도에 직접적인 영향을 미칩니다. 과도한 유량은 압력 상승과 플라즈마 불안정을 초래할 수 있습니다.

### 3. Source Power (ICP/TCP Power)
Source power는 플라즈마 밀도를 결정하는 핵심 파라미터로, 일반적으로 200~3000W 범위에서 운용됩니다. 높은 source power는 플라즈마 밀도를 증가시켜 식각 속도를 향상시키지만, 과도한 power는 마스크 선택비 저하와 damage를 유발할 수 있습니다.

### 4. Bias Power (RF Power)
Bias power는 이온의 가속 에너지를 제어하여 식각의 방향성을 결정합니다. 일반적으로 13.56 MHz의 RF를 사용하며, 50~500W 범위에서 조절됩니다. 높은 bias power는 수직 프로파일 형성에 유리하지만, 기판 손상과 마스크 침식을 증가시킬 수 있습니다.

### 5. 공정 온도 (Process Temperature)
웨이퍼 온도는 일반적으로 -20°C에서 80°C 범위에서 제어됩니다. 낮은 온도는 polymer 형성을 촉진하여 sidewall passivation에 유리하며, 높은 온도는 반응 부산물의 탈착을 촉진하여 식각 속도를 향상시킵니다.

### 6. Helium Cooling (Backside Cooling)
헬륨은 웨이퍼 뒷면과 ESC(Electrostatic Chuck) 사이의 열전달 매개체로 사용됩니다. 일반적으로 5~50 Torr의 압력으로 공급되며, 웨이퍼 온도의 균일한 제어를 가능하게 합니다. 헬륨 압력이 부족하면 중심부와 가장자리의 온도 편차가 발생하여 식각 균일도가 저하됩니다.

## 주요 공정 가스

### 불소계 가스 (Fluorine-based)
**CF4, C4F8, CHF3**: Silicon oxide와 silicon nitride 식각에 주로 사용됩니다. CF4는 F 라디칼 생성이 많아 식각 속도가 빠르며, C4F8은 C/F 비율이 높아 polymer 형성이 많고 선택비가 우수합니다. CHF3는 중간 특성을 가지며 contact hole 식각에 널리 사용됩니다.

**SF6**: Silicon과 poly-silicon 식각에 매우 효과적입니다. F 라디칼 생성이 많아 높은 식각 속도를 보이지만, 등방성 식각 경향이 강해 추가적인 passivation 가스와 함께 사용됩니다.

### 염소계 가스 (Chlorine-based)
**Cl2, BCl3**: 알루미늄과 같은 금속 배선 식각에 주로 사용됩니다. BCl3는 자연 산화막 제거에 효과적이며, Cl2와 함께 사용하여 식각 프로파일을 제어합니다. BCl3는 또한 scavenger로 작용하여 챔버 내 수분과 산소를 제거하는 역할도 합니다.

**HBr**: Poly-silicon과 silicon 식각에 사용되며, 특히 gate 식각에서 우수한 프로파일과 선택비를 제공합니다. 낮은 온도에서 HBr은 sidewall passivation 효과가 뛰어나 수직 프로파일 형성에 유리합니다.

### 첨가 가스
**O2**: Photoresist 제거와 polymer 제어에 사용됩니다. 소량 첨가 시 식각 속도를 향상시키지만, 과도한 첨가는 마스크 침식을 가속화합니다.

**Ar**: 불활성 가스로 물리적 스퍼터링을 증가시키고 플라즈마 안정화에 기여합니다. 특히 금속 식각에서 재증착 물질 제거에 효과적입니다.

**N2, H2**: Passivation layer 형성과 프로파일 제어에 사용됩니다. N2는 silicon nitride 형성을, H2는 polymer 제거와 선택비 향상에 기여합니다.

## 공정 최적화 고려사항

Dry etch 공정 최적화는 식각 속도, 선택비, 프로파일, 균일도, 그리고 damage 최소화 사이의 균형을 찾는 과정입니다. 각 파라미터는 서로 연관되어 있어, 하나의 변경이 전체 공정에 영향을 미칩니다. 예를 들어, 식각 속도를 높이기 위해 power를 증가시키면 선택비가 저하되고 substrate damage가 증가할 수 있습니다.

최신 반도체 공정에서는 HAR(High Aspect Ratio) 구조와 3D 구조의 식각이 증가하면서, 더욱 정밀한 파라미터 제어와 새로운 가스 조합의 개발이 요구되고 있습니다. 특히 ALE(Atomic Layer Etching)와 같은 원자 단위 제어 기술이 차세대 공정의 핵심으로 부상하고 있습니다.

## 물질별 Dry Etch 가스 및 조건

### 1. Silicon Dioxide (SiO2) Etch

#### 주요 에치 가스
**CF4 기반 화학**
```
주 반응: SiO2 + 4CF4 → SiF4 + 2COF2 + 2CF2
          SiO2 + 4F → SiF4 + O2
```

**가스 조합 및 비율**
- **CF4/O2**: CF4 (80-200 sccm) + O2 (5-20 sccm)
- **CHF3/CF4**: CHF3 (50-150 sccm) + CF4 (20-80 sccm)
- **C4F8/O2**: C4F8 (20-60 sccm) + O2 (10-30 sccm)
- **C4F8/Ar**: C4F8 (40-80 sccm) + Ar (100-300 sccm)

#### 공정 조건
```
압력 (Pressure): 5-50 mTorr
Source Power: 800-2000W
Bias Power: 100-500W
온도 (Temperature): 0-40°C
He Backside Pressure: 10-20 Torr
```

#### 선택비 및 특성
- **SiO2/Si 선택비**: 5:1 ~ 15:1
- **SiO2/SiN 선택비**: 3:1 ~ 8:1
- **SiO2/Photoresist 선택비**: 1:1 ~ 3:1
- **식각 속도**: 1000-3000 Å/min

#### 응용 분야
- STI (Shallow Trench Isolation) 식각
- Contact/Via hole 식각
- ILD (Inter Layer Dielectric) 패터닝
- TEOS, PSG, BSG 식각

### 2. Silicon Nitride (SiN/Si3N4) Etch

#### 주요 에치 가스
**불소계 + 탄소계 조합**
```
주 반응: Si3N4 + 12F → 3SiF4 + 2N2
          Si3N4 + 12CF4 → 3SiF4 + 2N2 + 12CF2
```

**가스 조합 및 비율**
- **CF4/O2**: CF4 (100-250 sccm) + O2 (10-40 sccm)
- **CHF3/O2**: CHF3 (80-200 sccm) + O2 (15-50 sccm)
- **C4F8/CH4**: C4F8 (30-80 sccm) + CH4 (10-40 sccm)
- **SF6/O2**: SF6 (50-150 sccm) + O2 (5-25 sccm)

#### 공정 조건
```
압력 (Pressure): 10-80 mTorr
Source Power: 1000-2500W
Bias Power: 150-600W
온도 (Temperature): 20-60°C
He Backside Pressure: 8-15 Torr
```

#### 선택비 및 특성
- **SiN/SiO2 선택비**: 8:1 ~ 20:1
- **SiN/Si 선택비**: 10:1 ~ 25:1
- **SiN/Photoresist 선택비**: 0.8:1 ~ 2:1
- **식각 속도**: 800-2500 Å/min

#### 응용 분야
- Spacer 형성을 위한 SiN 식각
- Hard mask SiN 제거
- LPCVD/PECVD SiN 패터닝
- Passivation layer 식각

### 3. Aluminum (Al) Etch

#### 주요 에치 가스
**염소계 가스 조합**
```
주 반응: Al + 3Cl → AlCl3
          2Al + 3Cl2 → 2AlCl3
```

**가스 조합 및 비율**
- **Cl2/BCl3**: Cl2 (80-200 sccm) + BCl3 (20-80 sccm)
- **Cl2/BCl3/CHCl3**: Cl2 (100-180 sccm) + BCl3 (30-60 sccm) + CHCl3 (5-20 sccm)
- **BCl3/Cl2/Ar**: BCl3 (60-120 sccm) + Cl2 (40-100 sccm) + Ar (50-150 sccm)
- **Cl2/N2**: Cl2 (120-250 sccm) + N2 (10-50 sccm)

#### 공정 조건
```
압력 (Pressure): 2-20 mTorr
Source Power: 500-1500W
Bias Power: 80-300W
온도 (Temperature): 40-80°C
He Backside Pressure: 15-25 Torr
```

#### 선택비 및 특성
- **Al/SiO2 선택비**: 15:1 ~ 50:1
- **Al/Photoresist 선택비**: 2:1 ~ 5:1
- **Al/TiN 선택비**: 8:1 ~ 20:1
- **식각 속도**: 3000-8000 Å/min

#### 특별 고려사항
- **자연 산화막**: Al2O3 제거를 위한 breakthrough step 필요
- **Corrosion 방지**: 낮은 습도 (<1% RH) 유지 필수
- **재증착 방지**: 충분한 ion bombardment와 Ar 첨가

#### 응용 분야
- Metal 1, 2, 3 interconnect 식각
- Al pad 식각
- Al/Cu alloy 식각
- Bondpad opening

### 4. Tungsten (W) Etch

#### 주요 에치 가스
**불소계 가스 조합**
```
주 반응: W + 6F → WF6
          W + 3F2 → WF6
```

**가스 조합 및 비율**
- **SF6/O2**: SF6 (100-300 sccm) + O2 (10-50 sccm)
- **CF4/O2**: CF4 (150-400 sccm) + O2 (20-80 sccm)
- **NF3/Ar**: NF3 (50-150 sccm) + Ar (100-250 sccm)
- **SF6/Cl2**: SF6 (80-200 sccm) + Cl2 (20-60 sccm)

#### 공정 조건
```
압력 (Pressure): 5-50 mTorr
Source Power: 800-2000W
Bias Power: 100-400W
온도 (Temperature): 20-60°C
He Backside Pressure: 10-20 Torr
```

#### 선택비 및 특성
- **W/SiO2 선택비**: 20:1 ~ 80:1
- **W/SiN 선택비**: 15:1 ~ 40:1
- **W/Ti 선택비**: 5:1 ~ 15:1
- **W/TiN 선택비**: 8:1 ~ 25:1
- **식각 속도**: 2000-6000 Å/min

#### 공정 유형
**1. W Plug Etchback**
```
목적: Via/Contact 내 W 잉여분 제거
가스: SF6/O2 (높은 선택비)
조건: 낮은 bias power, 높은 선택비 중심
```

**2. W Via/Contact Fill**
```
목적: Via 충진을 위한 W 선택적 증착
전처리: TiN barrier 위 W 증착
후처리: CMP 또는 Etchback
```

#### 응용 분야
- Via/Contact plug 형성
- Local interconnect
- Gate electrode (W gate)
- X-ray mask 식각

### 5. Titanium Nitride (TiN) Etch

#### 주요 에치 가스
**염소계 + 불소계 조합**
```
주 반응: TiN + 4Cl → TiCl4 + 1/2N2
          TiN + 3F2 → TiF4 + 1/2N2 + F2
```

**가스 조합 및 비율**
- **Cl2/BCl3/Ar**: Cl2 (60-150 sccm) + BCl3 (30-80 sccm) + Ar (50-200 sccm)
- **SF6/Cl2**: SF6 (80-200 sccm) + Cl2 (40-100 sccm)
- **CF4/Cl2/O2**: CF4 (100-250 sccm) + Cl2 (30-80 sccm) + O2 (10-30 sccm)
- **BCl3/Ar**: BCl3 (100-200 sccm) + Ar (100-300 sccm)

#### 공정 조건
```
압력 (Pressure): 3-30 mTorr
Source Power: 600-1800W
Bias Power: 100-500W
온도 (Temperature): 40-80°C
He Backside Pressure: 12-25 Torr
```

#### 선택비 및 특성
- **TiN/SiO2 선택비**: 10:1 ~ 30:1
- **TiN/Si 선택비**: 8:1 ~ 20:1
- **TiN/W 선택비**: 15:1 ~ 40:1
- **TiN/Photoresist 선택비**: 1:1 ~ 3:1
- **식각 속도**: 1500-4000 Å/min

#### 특별 고려사항
- **산화 방지**: 낮은 O2 농도 유지
- **Sidewall passivation**: 적절한 polymerization 제어
- **Microloading 효과**: Via 크기에 따른 식각 속도 변화

#### 응용 분야
- Barrier layer 패터닝
- Hard mask 제거
- Gate electrode TiN 식각
- Capacitor electrode 형성

## 공정별 최적화 전략

### 1. Contact/Via Etch 최적화
```
Breakthrough → Main Etch → Over Etch → Soft Landing
```

**Breakthrough Step**
- 목적: 자연 산화막 및 residue 제거
- 가스: 높은 F/C 비율 (CF4/O2)
- 시간: 5-15초

**Main Etch Step**
- 목적: 빠른 식각 속도로 bulk 제거
- 가스: 중간 F/C 비율 (CHF3/CF4)
- 제어: 높은 이온 에너지

**Over Etch Step**
- 목적: 완전한 개구부 형성
- 가스: 낮은 F/C 비율 (C4F8/O2)
- 특징: 높은 선택비

### 2. Metal Line Etch 최적화
```
Main Etch → Residue Clean → Passivation
```

**Main Etch**
- 고속 식각으로 bulk 제거
- 수직 프로파일 확보

**Residue Clean**
- Redeposition 물질 제거
- 산화물 잔여물 제거

### 3. HAR (High Aspect Ratio) Etch
```
특별 고려사항:
- RIE lag 최소화
- ARDE (Aspect Ratio Dependent Etching) 제어
- Sidewall bowing 방지
- Bottom residue 제거
```

#### HAR 최적화 기법
- **Pulsed Plasma**: On/Off cycle로 ion flux 제어
- **Multi-step Process**: 단계별 조건 변경
- **Temperature Control**: Sidewall passivation 최적화

## 공정 모니터링 및 제어

### 1. End Point Detection (EPD)
**Optical Emission Spectroscopy (OES)**
- SiO2 etch: CO (483nm), SiF (440nm)
- Si etch: SiF (440nm), SiCl (287nm)
- Al etch: AlCl (261nm, 308nm)
- W etch: WF (429nm)
- TiN etch: TiCl (519nm)

**Mass Spectrometry**
- 반응 부산물 실시간 모니터링
- SiF4 (m/z=104), AlCl3 (m/z=133), WF6 (m/z=298)

### 2. In-situ 측정
**Interferometry**
- 실시간 두께 측정
- 식각 속도 모니터링

**Impedance Monitoring**
- 플라즈마 상태 변화 감지
- Chamber condition 모니터링

### 3. 공정 제어 파라미터
```
CD (Critical Dimension) Control
- Target: ±5% within wafer
- Control: Gas ratio, bias power

Profile Control
- Target: 88-92° sidewall angle
- Control: Temperature, pressure

Selectivity Control
- Target: >10:1 (material dependent)
- Control: Chemistry selection

Uniformity Control
- Target: ±3% (1σ) across wafer
- Control: Gas distribution, temperature
```

## 고급 Etch 기술

### 1. Atomic Layer Etching (ALE)
```
원리: 흡착 → 반응 → 탈착의 자기제한 반응
장점: 원자 수준 정밀 제어
응용: 3D NAND, FinFET, Gate-all-around
```

### 2. Directional ALE
```
방법: Ion bombardment + Chemical reaction
특징: 수직 방향 선택적 식각
응용: Spacer 형성, HAR contact
```

### 3. Cryogenic Etch
```
온도: -120°C ~ -80°C
장점: 극도로 높은 선택비
응용: Deep silicon etch, MEMS
```

## 공정 결함 및 해결방안

### 1. 공통 결함
**Microloading**
- 현상: 패턴 밀도에 따른 식각 속도 차이
- 원인: 반응 부산물 배출 제한
- 해결: 가스 유량 증가, 압력 최적화

**ARDE (Aspect Ratio Dependent Etching)**
- 현상: Aspect ratio에 따른 식각 속도 감소
- 원인: 이온 flux 감소, neutral 공급 제한
- 해결: 높은 이온 에너지, pulsed plasma

**Sidewall Bowing**
- 현상: Sidewall이 볼록하게 식각
- 원인: 불충분한 passivation
- 해결: Polymerizing gas 추가, 온도 조절

### 2. 물질별 특별 결함
**SiO2 Etch**
- Black silicon: 높은 aspect ratio에서 발생
- Trenching: Over etch 시 기판 손상

**Metal Etch**
- Corrosion: 습도 및 잔여 Cl 영향
- Redeposition: 식각된 금속의 재증착

**TiN Etch**
- Faceting: 결정 방향에 따른 식각 속도 차이
- Oxidation: 산소 노출에 의한 산화막 형성

## 안전 및 환경 고려사항

### 1. 독성 가스 관리
**고독성 가스 (PH3, AsH3, B2H6)**
- 농도 모니터링 시스템 필수
- 비상 정지 시스템 구비
- 개인 보호 장비 착용

**부식성 가스 (HF, HCl, HBr)**
- 재질 호환성 확인
- 스크러버 시스템 운영
- 정기적 장비 점검

### 2. 폐가스 처리
**Scrubber System**
- Wet scrubber: 수용성 가스 제거
- Dry scrubber: 고체 흡착제 사용
- Thermal oxidizer: 유기물 완전 연소

**Global Warming Gas**
- CF4, CHF3 등 온실가스 배출 최소화
- 대체 가스 개발 및 적용
- 분해 효율 향상

---

*Dry Etch 공정은 반도체 제조에서 가장 복잡하고 중요한 공정 중 하나입니다. 각 물질별 최적 조건을 이해하고 지속적인 공정 개선을 통해 안정적인 생산이 가능합니다.*