# Photolithography 공정 설명

## 목차
1. [[#개요]]
2. [[#공정 원리]]
3. [[#상세 공정 단계]]
4. [[#주요 공정 변수 및 제어]]
5. [[#공정 모니터링 및 측정]]
6. [[#주요 결함 및 해결 방법]]
7. [[#고급 기술]]
8. [[#공정 최적화 전략]]
9. [[#품질 관리]]
10. [[#안전 및 환경]]

---

## 개요
Photolithography(포토리소그래피)는 반도체 제조에서 가장 핵심적인 공정 중 하나로, 빛을 이용하여 웨이퍼 표면에 원하는 패턴을 형성하는 기술입니다. 이 공정을 통해 회로의 미세한 패턴을 정확하게 웨이퍼에 전사할 수 있습니다.

## 공정 원리

### 기본 원리
1. **감광성 물질(Photoresist) 도포**: 웨이퍼 표면에 빛에 민감한 포토레지스트를 도포
2. **마스크를 통한 노광**: UV 광을 마스크(Mask)를 통해 선택적으로 조사
3. **현상(Develop)**: 노광된 부분 또는 노광되지 않은 부분을 선택적으로 제거
4. **패턴 형성**: 원하는 회로 패턴이 웨이퍼 표면에 형성

### 포토레지스트 종류
- **Positive Resist**: 노광된 부분이 현상액에 용해되어 제거
- **Negative Resist**: 노광되지 않은 부분이 현상액에 용해되어 제거

## 상세 공정 단계

### 1. 전처리 (Pre-treatment)
```
목적: 웨이퍼 표면 정리 및 접착력 향상
```
- **표면 청소**: 유기물 및 입자 제거
- **탈수 베이킹 (Dehydration Bake)**: 표면 수분 제거 (100-120°C)
- **HMDS 처리**: 접착 촉진제 도포 (Hexamethyldisilazane)
- **프라이머 코팅**: 포토레지스트와 웨이퍼 간 접착력 향상

### 2. 포토레지스트 코팅 (Resist Coating)
```
장비: Spin Coater
목적: 균일한 두께의 포토레지스트 막 형성
```

#### 주요 파라미터
- **Dispense 속도**: 500-1000 rpm
- **Spread 속도**: 1000-2000 rpm  
- **Final 속도**: 2000-6000 rpm
- **코팅 시간**: 30-60초
- **코팅 두께**: 0.5-3.0 μm (회전속도에 반비례)

#### 두께 제어 공식

$$ \begin{aligned}
&Thickness ∝ \frac{1}{\sqrt{\phi}}\\
&Thickness = K × {\mu}^{\alpha} × {\phi}^{\beta}\\
\\
&여기서\;K는\;상수,\;\alpha\;≈\;0.5,\;\beta\;≈\;-0.5,\;\mu:\;viscocity\;of\;resist, \phi:\;RPM\\
\end{aligned} $$


### 3. 소프트 베이킹 (Soft Bake/Pre-bake)
```
목적: 용매 제거 및 포토레지스트 안정화
```
- **온도**: 90-110°C
- **시간**: 60-120초
- **방법**: Hot plate 또는 Oven 사용
- **효과**: 
  - 용매 제거 (residual solvent < 1%)
  - 막 밀도 증가
  - 접착력 향상
  - 현상 특성 개선

### 4. 노광 (Exposure)
```
장비: Stepper, Scanner
목적: 마스크 패턴을 포토레지스트에 전사
```

#### 노광 시스템 종류 및 주요 장비

**1. Contact/Proximity Printer**
   - 마스크와 웨이퍼가 접촉 또는 근접
   - 해상도: ~1μm
   - 용도: 단순 패턴, 연구용

**2. Nikon i11d Scanner**
```
장비 타입: i-line Stepper/Scanner
파장: 365nm (i-line)
해상도: 0.35μm ~ 0.5μm
```

**주요 사양**
- **Numerical Aperture (NA)**: 0.57 ~ 0.68
- **Field Size**: 17.5mm × 17.5mm (single exposure)
- **Overlay Accuracy**: ±80nm (3σ)
- **CD Uniformity**: ±30nm (3σ, within field)
- **Throughput**: 120-150 wafers/hour
- **Chuck Size**: 150mm, 200mm wafer 지원

**기술적 특징**
- **Reduction Ratio**: 4:1 또는 5:1
- **Focus Control**: ±0.2μm accuracy
- **Illumination**: Conventional, Annular, Quadrupole
- **Reticle Size**: 6 inch × 6 inch
- **Auto Focus**: TTL (Through The Lens) 방식

**공정 적용 분야**
- Non-critical layer lithography
- Packaging/Assembly 공정
- MEMS 및 Sensor 제조
- 연구개발 및 프로토타입
- Legacy node 생산 (>0.35μm)

**운영 조건**
```
노광량 (Dose): 150-400 mJ/cm²
포커스 범위: ±1.5μm
환경 조건:
- 온도: 22±0.1°C
- 습도: 45±2% RH
- 진동: <0.1μm (1-200Hz)
```

**3. ASML KrF Scanner**
```
장비 타입: Deep UV Scanner
파장: 248nm (KrF Excimer Laser)
해상도: 0.13μm ~ 0.25μm
```

**주요 사양**
- **Numerical Aperture (NA)**: 0.63 ~ 0.93
- **Field Size**: 26mm × 33mm (scanner field)
- **Overlay Accuracy**: ±25nm (3σ)
- **CD Uniformity**: ±10nm (3σ, within field)
- **Throughput**: 180-220 wafers/hour
- **Chuck Size**: 200mm, 300mm wafer 지원

**기술적 특징**
- **Reduction Ratio**: 4:1
- **Scanning Speed**: 200-600 mm/s
- **Focus Control**: ±0.05μm accuracy
- **Illumination**: 
  - Conventional
  - Annular (σ: 0.5-0.8)
  - Quadrupole (σ: 0.3-0.9)
  - QUASAR (Advanced illumination)
- **Reticle Size**: 6 inch × 6 inch × 0.25 inch
- **Leveling System**: Multi-point leveling sensor

**고급 기능**
- **AERIAL (Aberration Recognition and Lithography Enhancement)**
  - 렌즈 수차 실시간 보정
  - 온도/압력 변화 보상
- **ATHENA (Advanced Thermal and Environment Enhancement)**
  - Reticle 온도 제어
  - 환경 안정화
- **TWINSCANTM Technology**
  - Dual-stage 구조
  - 높은 throughput 달성

**공정 적용 분야**
- Logic 0.13μm ~ 0.25μm node
- Memory (DRAM, Flash) 생산
- Mixed-signal 및 Analog IC
- Power device 제조

**운영 조건**
```
노광량 (Dose): 20-50 mJ/cm²
포커스 범위: ±0.4μm
환경 조건:
- 온도: 22±0.01°C
- 습도: 45±0.5% RH
- 진동: <0.05μm (1-200Hz)
- Clean Room: Class 1
```

**4. Scanner (EUV)**
   - 슬릿을 이용한 스캐닝 노광
   - 해상도: <0.1μm (최신 EUV: ~3nm)
   - Step & Scan 방식
   - 용도: 최첨단 공정

#### 장비 비교 분석

| 항목         | Nikon i11d          | ASML KrF Scanner      |
| ---------- | ------------------- | --------------------- |
| 파장         | 365nm (i-line)      | 248nm (KrF)           |
| 해상도        | 0.35-0.5μm          | 0.13-0.25μm           |
| Overlay    | ±200nm              | ±100nm                |
| Throughput | 120-150 wph         | 180-220 wph           |
| 적용 분야      | Non-critical/Legacy | Advanced Logic/Memory |
| 비용         | 상대적 저비용             | 고비용                   |

#### 장비별 공정 최적화

**Nikon i11d 최적화 포인트**
```
1. 노광량 최적화
   - i-line resist 특성 고려
   - 두께운 resist 허용 (높은 DOF)
   - Soft bake 온도 제어

2. 포커스 제어
   - 웨이퍼 평탄도 관리
   - Chuck temperature 안정화
   - Auto-focus offset 조정
   - 넓은 DOF 활용 (i-line 장점)

3. Overlay 개선
   - 웨이퍼 mark 품질 관리
   - Alignment 정확도 향상
   - Process-induced shift 보정
   - Thermal expansion 고려
```

**ASML KrF Scanner 최적화 포인트**
```
1. 해상도 향상 기법
   - Off-axis illumination 최적화
   - Phase shift mask 활용
   - OPC (Optical Proximity Correction)
   - **BARC 사용 필수** (Standing wave 방지)

2. 공정 윈도우 확장
   - Dose/Focus matrix 최적화
   - MEEF (Mask Error Enhancement Factor) 최소화
   - Process latitude 극대화
   - **Standing wave 효과 감소**
   - **얘은 resist 두께 제어** (좋은 DOF)

3. Overlay 정밀 제어
   - Grid matching 최적화
   - Higher order correction
   - Intra-field correction
   - 고정밀 온도 제어
```

#### 주요 노광 파라미터
- **파장 (Wavelength)**
  - g-line: 436nm
  - i-line: 365nm  
  - KrF: 248nm
  - ArF: 193nm
  - EUV: 13.5nm

- **NA (Numerical Aperture)**: 0.3-1.35
- **DOF (Depth of Focus)**: λ/(2×NA²)
- **해상도**: 0.61×λ/NA
- **노광량 (Dose)**: 10-50 mJ/cm²

### 5. 노광 후 베이킹 (Post Exposure Bake, PEB)
```
목적: 화학 반응 완성 및 패턴 안정화
```
- **온도**: 100-130°C
- **시간**: 60-120초
- **효과**:
  - 산 확산 제어 (Chemically Amplified Resist)
  - 선폭 제어
  - 프로파일 개선
  - Standing wave 효과 감소

### 6. 현상 (Development)
```
목적: 노광된/노광되지 않은 부분 선택적 제거
```

#### 현상액 종류
- **TMAH (Tetramethylammonium Hydroxide)**: 2.38% 수용액
- **NaOH**: 수산화나트륨 (구형)
- **KOH**: 수산화칼륨 (구형)

#### 현상 방법
1. **Puddle Development**
   - 웨이퍼에 현상액을 담아두고 현상
   - 시간: 30-90초
   - 균일성 우수

2. **Spray Development**
   - 현상액을 분사하여 현상
   - 연속 공정 가능
   - 처리량 높음

#### 현상 파라미터
- **현상액 농도**: 2.38% TMAH
- **현상 시간**: 30-90초
- **현상액 온도**: 20-25°C
- **Rinse**: DI water로 세정
- **건조**: Spin dry 또는 N₂ blow

### 7. 하드 베이킹 (Hard Bake)
```
목적: 후공정 내성 향상 (선택적 단계)
```
- **온도**: 120-150°C
- **시간**: 5-30분
- **용도**: 
  - 이온 주입 내성
  - 에칭 내성 향상
  - 열적 안정성 증가

## 주요 공정 변수 및 제어

### 1. 선폭 제어 (CD Control)
```
Critical Dimension (CD) = 설계 선폭
```

#### 영향 인자
- **노광량 (Dose)**: ±2% 변화 시 CD ±5% 변화
- **포커스 (Focus)**: ±0.1μm 변화 시 CD ±2% 변화
- **PEB 온도**: ±1°C 변화 시 CD ±1% 변화
- **현상 시간**: ±10% 변화 시 CD ±3% 변화

#### 제어 전략
```
CD = f(Dose, Focus, PEB_temp, Dev_time, Resist_thickness)
```

### 2. 오버레이 정확도 (Overlay Accuracy)
```
목적: 이전 층과의 정렬 정확도 확보
```
- **측정 방법**: Overlay mark 측정
- **목표 사양**: ±10-30nm (공정에 따라)
- **제어 인자**: 
  - Stepper alignment 정확도
  - 웨이퍼 변형 (thermal expansion)
  - 공정 유도 변형

### 3. 균일성 (Uniformity)
```
Within-wafer uniformity, Wafer-to-wafer uniformity
```

#### CD 균일성
- **Within wafer**: ±2-5% (3σ)
- **Wafer to wafer**: ±1-3% (3σ)

#### 두께 균일성
- **포토레지스트 두께**: ±2-5%
- **측정 방법**: Ellipsometer, Reflectometer

## 공정 모니터링 및 측정

### 1. 인라인 측정
```
실시간 공정 모니터링
```
- **CD 측정**: SEM, Scatterometry (OCD)
- **두께 측정**: Ellipsometer, Reflectometer  
- **오버레이 측정**: Overlay measurement tool
- **결함 검사**: Defect inspection system

### 2. 공정 제어 차트
```
Statistical Process Control (SPC)
```
- **CD bias**: Target ± 5nm
- **두께 variation**: Target ± 2%
- **오버레이**: Target ± 15nm
- **결함 밀도**: <0.1 defects/cm²

### 3. 샘플링 계획
```
측정 빈도 및 위치
```
- **CD 측정**: 매 웨이퍼, 9 point 측정
- **두께 측정**: 매 5 웨이퍼, 49 point 측정
- **오버레이**: 매 웨이퍼, 대표 site 측정

## 주요 결함 및 해결 방법

### 1. CD 관련 결함
```
Critical Dimension 문제
```

#### CD 불균일성
- **원인**: 노광량 불균일, 포커스 변동, 현상 불균일
- **해결**: 
  - 노광기 calibration
  - 현상 조건 최적화
  - 베이킹 온도 균일성 개선

#### CD bias (설계 대비 편차)
- **원인**: 광근접효과, 마스크 bias, 공정 조건
- **해결**:
  - OPC (Optical Proximity Correction)
  - 마스크 bias 조정
  - 노광 조건 최적화

### 2. 프로파일 결함
```
포토레지스트 단면 형상 문제
```

#### Sloped sidewall
- **원인**: 노광량 부족, PEB 과다
- **해결**: 노광량 증가, PEB 조건 최적화

#### Undercut
- **원인**: 과현상, 노광량 과다
- **해결**: 현상 시간 단축, 노광량 조정

#### T-topping
- **원인**: 표면 에너지 흡수, 확산 불균일, 과도한 PEB
- **해결**: 
  - PEB 온도/시간 최적화
  - 노광량 조정
  - 표면 처리 개선
  
**참고**: Standing wave 및 BARC는 주로 Deep UV(KrF 248nm, ArF 193nm)에서 중요한 이슈입니다. i-line(365nm)은 파장이 길어 standing wave 효과가 상대적으로 작고, 두께운 resist를 사용할 수 있어 DOF(Depth of Focus) 확보가 유리합니다.

### 3. 접착 관련 결함
```
포토레지스트 박리 문제
```

#### Poor adhesion
- **원인**: 표면 오염, 수분, HMDS 부족
- **해결**: 
  - 표면 청소 강화
  - 탈수 베이킹 강화
  - HMDS 처리 개선

### 4. 입자 결함
```
Particle contamination
```
- **원인**: 환경 입자, 장비 contamination
- **해결**: 
  - Clean room 관리 강화
  - 장비 청소 주기 단축
  - 정전기 제어

## 고급 기술

### 1. 해상도 향상 기술
```
Resolution Enhancement Techniques (RET)
```

#### OPC (Optical Proximity Correction)
- **목적**: 광근접효과 보정
- **방법**: 마스크 패턴 수정, Sub-resolution assist feature

#### PSM (Phase Shift Mask)
- **Alternating PSM**: 인접 개구부의 위상을 180° 변화
- **Attenuated PSM**: 부분 투과로 위상 조절

#### Off-Axis Illumination
- **Annular**: 도넛형 조명
- **Quadrupole**: 4극 조명
- **Dipole**: 2극 조명

### 2. 다중 패터닝 기술
```
Multiple Patterning Techniques
```

#### LELE (Litho-Etch-Litho-Etch)
- 2회 리소그래피로 밀도 2배 증가
- 오버레이 정확도 매우 중요

#### SADP (Self-Aligned Double Patterning)
- Spacer를 이용한 자기정렬 패터닝
- 오버레이 에러 없음

#### SAQP (Self-Aligned Quadruple Patterning)
- SADP의 확장, 밀도 4배 증가

### 3. EUV 리소그래피
```
Extreme Ultra Violet Lithography
```
- **파장**: 13.5nm
- **해상도**: 3-5nm
- **장점**: 단일 노광으로 미세 패턴 가능
- **과제**: 
  - 광원 출력 부족
  - 마스크 결함 민감도
  - 포토레지스트 성능

## 공정 최적화 전략

### 1. DOE (Design of Experiments)
```
체계적 실험 설계
```
- **주요 인자**: Dose, Focus, PEB temperature, Develop time
- **반응 변수**: CD, Profile, Defect density
- **최적화 목표**: 
  - CD target 달성
  - Process window 극대화
  - 결함 최소화

### 2. Process Window 분석
```
공정 마진 확보
```
- **Dose margin**: ±5-10%
- **Focus margin**: ±0.3-0.5μm
- **Overlay margin**: ±30% of design rule

### 3. 통계적 공정 관리
```
Statistical Process Control
```
- **Control limit**: ±3σ
- **Specification limit**: ±6σ
- **Cpk target**: >1.33

## 품질 관리

### 1. 검사 항목
```
Quality Control Parameters
```
- **CD measurement**: Target ± specification
- **Overlay measurement**: ±15-30nm
- **Defect inspection**: <0.1/cm²
- **Profile check**: SEM cross-section
- **Adhesion test**: Tape test

### 2. 샘플링 계획
```
Sampling Strategy
```
- **AQL (Acceptable Quality Level)**: 0.1-1.0%
- **측정 빈도**: Risk-based sampling
- **측정 위치**: Representative sites

### 3. 데이터 분석
```
Data Analytics
```
- **Trend analysis**: 시간별 변화 추이
- **Correlation analysis**: 공정 변수 간 상관관계
- **Predictive modeling**: 수율 예측 모델

## 안전 및 환경

### 1. 화학물질 안전
```
Chemical Safety
```
- **포토레지스트**: 유기용매 포함, 환기 필요
- **현상액**: 알칼리성, 피부 접촉 주의  
- **HMDS**: 독성, 밀폐 공간 작업 금지

### 2. 장비 안전
```
Equipment Safety
```
- **UV 노광**: 자외선 차단, 보호장비 착용
- **고온 공정**: 화상 위험, 내열 장갑 사용

### 3. 폐기물 처리
```
Waste Management
```
- **용매 폐액**: 지정 폐기물 처리
- **포토레지스트**: 유기폐기물 분리수거
- **현상액**: 중화 후 처리

---

*Photolithography는 반도체 제조의 핵심 공정으로, 정밀한 제어와 지속적인 개선이 필요합니다. 각 단계별 파라미터 최적화와 체계적인 품질 관리를 통해 안정적인 공정 운영이 가능합니다.*