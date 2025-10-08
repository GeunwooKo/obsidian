# Wet Etch Rates for Micromachining Processing

## 개요

이 문서는 K.R. Williams와 R.S. Muller의 논문 "Etch Rates for Micromachining Processing"을 기반으로 반도체 및 MEMS 공정에서 사용되는 다양한 재료의 습식 식각 속도를 정리한 참고 자료입니다.

**원문 출처**: 
- Part I: IEEE Journal, Williams & Muller (1996) - 317개 조합
- Part II: JMEMS vol. 12 No 6 (2003) - 620개 조합

**측정 대상**: 53가지 재료, 28가지 이상 에천트

---

## 목차

- [[#주요 측정 재료]]
- [[#실리콘 에칭]]
- [[#실리콘 산화막 에칭]]
- [[#실리콘 질화막 에칭]]
- [[#금속 에칭]]
- [[#유기물 에칭]]
- [[#에칭 속도 변동 요인]]
- [[#습식 vs 건식 에칭]]
- [[#이방성 에칭]]
- [[#실전 가이드]]
- [[#참고자료]]

---

## 주요 측정 재료

### 반도체
- Single-crystal Si (2 doping levels)
- Polysilicon (2 doping levels)
- Polycrystalline Ge
- Polycrystalline SiGe
- Graphite

### 절연막
**Silicon Dioxide (9종)**:
- Thermal oxide
- TEOS (CVD)
- PECVD oxide
- Fused quartz
- Pyrex 7740
- LTO, PSG 등

**Silicon Nitride (4종)**:
- Stoichiometric Si₃N₄
- Silicon-rich nitride
- LPCVD, PECVD

**기타**: Al₂O₃, Sapphire

### 금속
Al, Al/2%Si, Ti, V, Nb, Ta, Cr, Mo, W, Ni, Pd, Pt, Cu, Ag, Au, TiW(10/90), Nichrome(80/20), TiN

### 유기물
Photoresist (4종), Parylene-C, Polyimide

---

## 실리콘 에칭

### 이방성 에천트

#### KOH (80°C)
**조성**: 1kg KOH pellets : 2L H₂O (~29wt%)

**선택비**: <100>/<111> = **400:1**

**에칭 속도**:
- <100>: ~1 μm/min
- <111>: ~0.0025 μm/min

**특징**:
- 자체 발열 반응
- V-groove 형성 (54.74°)
- SiO₂도 에칭 (~1-2 nm/min)

**장점**: 높은 이방성, 낮은 비용
**단점**: K⁺ 오염, CMOS 제한

#### TMAH
**선택비**: {100}/{111} = **37:1**

**특징**:
- IC-compatible (이동성 이온 없음)
- EDP보다 안전
- Non-toxic

#### EDP
**선택비**: <100>/<111> = **17:1**

**특징**:
- SiO₂ 거의 에칭 안함
- Boron etch stop

**단점**: 발암물질, 독성

**비교표**:

| 에천트 | 선택비 | SiO₂ 에칭 | CMOS | 안전성 |
|--------|-------|-----------|------|--------|
| KOH | 400:1 | 예 | 제한 | 보통 |
| TMAH | 37:1 | 매우느림 | 우수 | 높음 |
| EDP | 17:1 | 거의없음 | 제한 | 낮음 |

### 등방성 에천트

#### HNO₃ + HF + H₂O
**Berkeley 배합**: HNO₃(70%) : H₂O : NH₄F(40%) = 126:60:5

**반응**:
```
3Si + 4HNO₃ → 3SiO₂ + 4NO + 2H₂O  (산화)
SiO₂ + 6HF → H₂SiF₆ + 2H₂O        (에칭)
```

**용도**: Polysilicon 에칭
**특징**: 완전 등방성, 속도 조절 가능

---

## 실리콘 산화막 에칭

### HF (Hydrofluoric Acid)

**반응**:
```
SiO₂ + 6HF → H₂SiF₆ + 2H₂O
```

**농도별 에칭 속도** (Thermal oxide, RT):

| HF 농도 | 에칭 속도 |
|---------|----------|
| 1% | ~10 nm/min |
| 5% | ~50 nm/min |
| 10% | ~100 nm/min |
| 49% | >1000 nm/min |

**재료별 상대 속도**:

| 재료 | 상대 속도 |
|------|----------|
| Thermal oxide | 1× |
| LTO | 2-2.5× |
| TEOS | 3× |
| PECVD | 7× |
| Si₃N₄ | <0.01× |
| Si | <0.001× |

**선택비**:
- SiO₂ vs Si: >1000:1
- SiO₂ vs Si₃N₄: >100:1

**특징**:
- 에칭 후 소수성 표면
- Al 부식 주의

### BHF (Buffered HF)

**조성**: 40% NH₄F : 49% HF = 10:1

**Thermal oxide**: 50 nm/min

**재료별 속도**:

| 재료 | 속도 |
|------|------|
| Thermal oxide | 50 nm/min |
| TEOS | 150 nm/min |
| PECVD | 350 nm/min |

**반응** (Kikuyama):
```
SiO₂ + 6HF₂⁻ → SiF₆²⁻ + 2H₂O + 4F⁻
```

**장점**:
- pH 버퍼링 → 안정적
- CVD oxide 선택적 제거 가능
- 재현성 우수

**HF vs BHF**:

| 항목 | HF | BHF |
|------|----|----|
| 속도 | 빠름 | 느림 |
| 안정성 | 낮음 | 높음 |
| 제어 | 어려움 | 쉬움 |

### Oxide 밀도와 에칭 속도

**속도 순서** (빠름→느림):
1. PECVD (낮은 밀도, porous)
2. TEOS (중간)
3. Thermal (높은 밀도, dense)

**원리**: 밀도↑ → HF 침투↓ → 속도↓

---

## 실리콘 질화막 에칭

### Hot H₃PO₄ (Phosphoric Acid)

**조건**:
- 온도: 160-180°C (일반적 165°C)
- 농도: 85% H₃PO₄

**에칭 속도**:
- LPCVD Si₃N₄: 5-10 nm/min
- PECVD SiNₓ: 더 빠름

**반응**:
```
Si₃N₄ + 12H₃PO₄ → 3Si(HPO₄)₂ + 4(NH₄)H₂PO₄
```

**선택비**:
- Si₃N₄ vs SiO₂: ~10:1 (낮음!)
- Si₃N₄ vs Si: >100:1

**특징**:
- Nitride 습식 에칭 표준
- 고온 (안전 주의)

**온도 의존성**: 10°C↑ → 2×속도

**장점**: 균일한 에칭
**단점**: 고온, 낮은 SiO₂ 선택비

⚠️ **안전**: 매우 고온, 화상 위험, 흄후드 필수

---

## 금속 에칭

### 1. Aluminum Etchant (Type A)

**조성**: H₃PO₄ : HNO₃ : CH₃COOH : H₂O = 16:1:1:2 (예시)

**조건**: 40-60°C

**속도**: ~100-500 nm/min

**반응**:
```
2Al + 6HNO₃ → Al₂O₃ + 6NO₂ + 3H₂O
Al₂O₃ + 2H₃PO₄ → 2AlPO₄ + 3H₂O
```

⚠️ 온도 제어 중요 (±2°C)
⚠️ Undercut 발생

### 2. Titanium Etchant

**조성**: NH₄OH : H₂O₂ : H₂O = 1:1-3:5-20

**조건**: 60-80°C

**특징**:
- Ti 선택적 에칭
- ⚠️ Si도 에칭 가능
- 용액 수명 짧음

### 3. Tungsten Etchant

**에천트**:
- H₂O₂ 기반
- KOH + K₃Fe(CN)₆

**특징**:
- 습식 에칭 어려움
- **건식 에칭 선호** (SF₆)

### 4. Copper Etchant

#### CE-200
- FeCl₃ 기반
- 빠른 에칭

#### APS-100
- (NH₄)₂S₂O₈ 기반

#### Dilute Aqua Regia
- HCl : HNO₃ : H₂O

⚠️ **극도로 위험**: NO₂ 발생, 독성

### 5. Chromium Etchant (CR-7, CR-14)

**조성**: Ceric ammonium nitrate + Perchloric acid

**용도**: Cr mask 제거

### 6. Gold Etchant (AU-5)

**조성**: I₂/KI 또는 aqua regia

### 7. 기타

- **Molybdenum**: H₃PO₄ + HNO₃
- **Nichrome (TFN)**: 80Ni/20Cr 전용
- **Palladium, Platinum**: Aqua regia

### 금속 에칭 일반 원칙

**고려사항**:
- 마스크 재료 선택비
- 하부 재료 보호
- 온도 제어 (40-80°C)
- Undercut 계산

---

## 유기물 에칭

### 1. Piranha (SPM)
**조성**: H₂SO₄ : H₂O₂
**용도**: PR 완전 제거
**참조**: [[Semiconductor Wet Cleaning Solutions#SPM]]

### 2. Microstrip 2001
- 상용 PR stripper

### 3. Solvent
- **Acetone**: PR 용해
- **Methanol**: 세정
- **IPA**: 린스/건조

---

## 에칭 속도 변동 요인

Williams는 변동 요인을 **3그룹**으로 분류:

### 1. Etch Setup (장비 조건)

#### 온도
- **가장 중요한 변수**
- Arrhenius: Rate = A×exp(-Ea/kT)
- 10°C↑ → 1.5-3× 속도 증가
- ±1°C 제어 권장

#### 화학 약품 농도
- 에칭 속도에 강한 영향
- 사용에 따라 농도 변화
- 정기 교체/보충 필요

#### 교반 (Agitation)
- 포화 에천트 제거
- 신선한 에천트 공급
- 균일성 향상

**효과**:
- 교반 무: 확산 제한
- 교반 유: 반응 제한

#### 노출 면적
- 총 에칭 면적이 속도 영향
- 용액 부피/면적 비율 중요

### 2. Material (재료 특성)

#### 재료 품질
**Oxide 종류**:
- Thermal: Dense → 느림
- CVD: Less dense → 빠름
- PECVD: Least dense → 가장 빠름

**도핑 농도**:
- Heavy B doping → KOH 느림
- Etch stop으로 활용

**결정 방향**:
- 이방성 에천트에서 중요
- KOH: <100> vs <111> = 400:1

#### 막 두께
- 얇은 막: 짧은 시간
- 두꺼운 막: 에천트 소모

#### 표면 상태
- Roughness
- 잔류물
- Native oxide
- Contamination

### 3. Layout & Structure

#### Pattern Density
- 개방 영역 비율
- Loading effect

#### Feature Size
- 작은 feature: wetting 문제
- 깊은 feature: 물질 전달 제한

#### Mask Quality
- 선택비
- 접착력
- Peeling

#### Undercut/Bias
**계산**:
```
Bias = Etch Rate × Time / 2
```

---

## 습식 vs 건식 에칭

### 습식 에칭

#### 장점
1. **높은 선택비** (>100:1)
2. **높은 에칭 속도** (수백~수천 Å/min)
3. **간단한 장비** (저비용)
4. **Batch 처리** (높은 생산성)
5. **균일한 에칭**

#### 단점
1. **등방성** (lateral undercut)
2. **화학물질 대량** (폐기물)
3. **제어 어려움**
4. **Mask 제약**

### 건식 에칭

#### 장점
1. **이방성** (수직 에칭)
2. **정밀 제어**
3. **화학물질 감소**
4. **자동화 용이**

#### 단점
1. **낮은 선택비** (10-50:1)
2. **낮은 속도**
3. **비싼 장비**
4. **Plasma damage**

### 비교표

| 특성 | 습식 | 건식 |
|------|------|------|
| 이방성 | 등방성 | 이방성 |
| 선택비 | >100:1 | 10-50:1 |
| 속도 | 빠름 | 느림 |
| Feature | >1μm | <100nm |
| 비용 | 낮음 | 높음 |
| 용도 | 세정, 두꺼운막 | VLSI |

### 현대 반도체 적용

**습식 사용**:
- Wafer cleaning
- PR strip 후 세정
- Sacrificial oxide
- Non-critical etch

**건식 사용**:
- Gate patterning
- Contact/Via
- Trench
- Critical dimension

**하이브리드**:
- Main: 건식 (이방성)
- Post-clean: 습식 (잔류물)

---

## 이방성 에칭

### 결정학적 이방성

#### 기본 원리
단결정에서 결정면별 에칭 속도 차이 이용

**Si 선택비**:
```
KOH:  R<100>/R<111> = 400:1
EDP:  R<100>/R<111> = 17:1
TMAH: R<100>/R<111> = 37:1
```

#### 에칭 계산

**깊이**:
```
D = R_xxx × T
```

**이방성**:
```
S = (R_fast - R_slow) / R_slow
```

#### (100) Wafer 구조

**V-Groove 형성**:
- (111) 측벽 노출
- 54.74° 각도
- (111)은 etch stop

**Angle**:
```
tan(θ) = √2
θ = 54.74°
```

**개구부**:
```
Width = 2×Depth×√2 + W_mask
```

#### (110) Wafer 구조

- (111)이 표면에 수직
- ~90° 측벽 가능

#### 응용

1. **Membrane**: 압력 센서
2. **Cantilever**: AFM
3. **V-Groove**: 광섬유 정렬
4. **Through-hole**: Inkjet nozzle

### Etch Stop 기술

1. **Boron Etch Stop**
   - >10¹⁹ cm⁻³ doping
   - KOH 속도 급감

2. **재료 선택**
   - (111) plane
   - Buried oxide (SOI)
   - Si₃N₄

### 비정질 이방성

**Glass**:
- Multistream laminar flow
- Etching/non-etching solution
- Aspect ratio = 1 달성

---

## 에칭 속도 데이터 활용

### 측정 조건

**웨이퍼**: 4-inch (100mm)
- Transparent: 전체
- Opaque: 절반

**방법**:
- 두께 측정 (전후)
- 시간 기록
- Rate = Δthickness/time

**표기법**:
- `-`: 미수행
- `W`: Work (>100 Å/min)
- `F`: Fast (>10 kÅ/min)
- `P`: Peeled (벗겨짐)
- `A`: Attached & roughened

### 사용 시 주의

⚠️ **변동성**:
- 온도, 농도, 사용 이력
- 노출 면적
- 재료 존재

⚠️ **검증 필요**:
- 자체 공정으로 검증
- 예비 실험 권장
- Critical 공정은 더욱 주의

### 데이터 해석

**참고 데이터로 활용**:
- 초기 공정 개발
- 에천트 선택
- 대략적 시간 예측

**정밀 제어**:
- 실험적 최적화
- In-situ monitoring
- Endpoint detection

---

## 실전 적용 가이드

### 에천트 선택 절차

1. **목표 정의**
   - 제거할 재료
   - 보존할 재료
   - 필요 선택비

2. **후보 에천트**
   - 문헌 조사
   - Williams 데이터 참조

3. **예비 실험**
   - Dummy wafer
   - 에칭 속도 측정
   - 선택비 확인

4. **최적화**
   - 온도 조절
   - 농도 조정
   - 시간 최적화

### 공정 개발 팁

#### Oxide 에칭
- Thermal: BHF (안정적)
- CVD: 더 빠름, 농도↓
- Critical: BOE 사용

#### Nitride 에칭
- Hot H₃PO₄ 표준
- 온도 제어 중요
- SiO₂ 손실 고려

#### Metal 에칭
- 온도 ±2°C
- Undercut 계산
- 건식 선호 추세

#### Silicon 에칭
- 이방성: KOH/TMAH
- 등방성: HNA
- MEMS: 결정 방향 활용

### 안전 수칙

⚠️ **일반**:
- 흄후드 필수
- 적절한 PPE
- MSDS 숙지

⚠️ **특별 주의**:
- **HF**: 극독성, Ca gluconate 비치
- **Hot H₃PO₄**: 화상 위험
- **Aqua regia**: NO₂ 발생
- **EDP**: 발암물질

### 문제 해결

**에칭 속도 느림**:
- 온도↑
- 농도 확인
- 교반 강화
- 신선한 용액

**불균일**:
- 온도 균일성
- 교반 개선
- Pattern density 고려

**선택비 낮음**:
- 에천트 변경
- 마스크 재료 변경
- 건식 에칭 고려

**Undercut 과다**:
- 시간 단축
- 온도↓
- 건식 에칭 고려

---

## 관련 문서

### Internal Links
- [[Semiconductor Wet Cleaning Solutions]]
- [[Dry Etch]]
- [[Dry Etch Mechanisms]]
- [[Tungsten Etching Comprehensive Guide]]
- [[Aluminum-Cl-Etch-ACT945-Cleaning]]

### 추가 참고

**원문**:
- Williams & Muller, Part I (1996)
- Williams et al., Part II (2003)

**관련 리뷰**:
- K.E. Petersen, "Silicon as Mechanical Material" (1982)
- A.R. Clawson, "III-V Chemical Etching Guide" (2001)

**웹 리소스**:
- Utah Nanofab: Williams tables
- LNF Wiki: Wet etching
- BYU Cleanroom: Etch recipes

---

## 핵심 요약

### 선택비 비교

| 에천트 | 대상 | 선택비 | 특징 |
|--------|------|-------|------|
| HF/BHF | SiO₂ | vs Si >1000:1 | 표준 |
| KOH | Si | <100>/<111> 400:1 | 이방성 |
| Hot H₃PO₄ | Si₃N₄ | vs SiO₂ 10:1 | 낮음 |
| Al etch | Al | vs Si 보통 | 온도 중요 |

### 에칭 속도 범위

- **HF on oxide**: 10-1000 nm/min
- **BHF on oxide**: 50-350 nm/min
- **KOH on Si<100>**: ~1 μm/min
- **H₃PO₄ on nitride**: 5-10 nm/min
- **Metal etchants**: 100-500 nm/min

### Best Practices

1. **항상 예비 실험**
2. **온도 정밀 제어**
3. **신선한 화학물질**
4. **안전 최우선**
5. **Endpoint 모니터링**

---

## Tags

#wet-etch #etch-rate #Williams #MEMS #semiconductor #microfabrication #KOH #HF #BHF #anisotropic-etch #silicon-etch #oxide-etch #nitride-etch #metal-etch #process-reference

---

**작성일**: 2025-10-06
**최종 수정**: 2025-10-06
**기반 자료**: Williams & Muller (1996, 2003)