# Semiconductor Wet Cleaning Solutions

## 목차
- [[#SPM (Sulfuric Acid-Hydrogen Peroxide Mixture)]]
- [[#DSP (Diluted Sulfuric Peroxide)]]
- [[#SC1 (Standard Clean 1)]]
- [[#SC2 (Standard Clean 2)]]
- [[#DHF (Diluted Hydrofluoric Acid)]]
- [[#ACT 945]]
- [[#세정 순서 예시]]
- [[#참고자료]]

---

## SPM (Sulfuric Acid-Hydrogen Peroxide Mixture)

### 개요
**SPM**은 황산(H₂SO₄)과 과산화수소(H₂O₂)의 혼합 용액으로, 반도체 공정에서 가장 강력한 유기물 제거 세정제입니다. **Piranha 용액**이라고도 불립니다.

### 조성 및 공정 조건
- **조성**: H₂SO₄ : H₂O₂ = 4:1 ~ 3:1 (부피비)
- **온도**: 120~150°C
- **시간**: 5~10분
- **특징**: 발열 반응으로 자체 가열됨

### 용도
1. **유기물(Photoresist) 제거**
   - PR strip 후 잔류 유기물 완전 제거
   - 공정 중 생성된 카본 제거
   
2. **표면 청정화**
   - Metal contamination 제거 (일부)
   - Particle 제거
   
3. **표면 친수화**
   - 후속 세정 효과 향상

### 반응 메커니즘

#### 1. Caro's Acid 생성
```
H₂SO₄ + H₂O₂ → H₂SO₅ (Caro's acid) + H₂O
```
- Caro's acid는 강력한 산화제
- Peroxymonosulfuric acid라고도 불림

#### 2. 유기물 산화 반응
```
C_nH_m + H₂SO₅ → CO₂ + H₂O + H₂SO₄
```
- 유기물을 CO₂와 H₂O로 완전 산화
- 황산은 촉매 역할을 하며 재생됨

#### 3. 표면 친수화
```
Si (hydrophobic) + H₂SO₅ → Si-OH (hydrophilic) + oxidation products
```

### 장점 및 단점

**장점:**
- 유기물 제거 효과가 매우 우수
- Metal contamination도 일부 제거 가능
- Particle 제거 효과

**단점:**
- 매우 고온 (120-150°C)으로 열적 손상 가능
- Al, Cu 등 일부 금속 부식 가능
- 폐액 처리 비용이 높음
- 안전 관리 필요 (강산, 발열 반응)

### 주의사항
⚠️ **매우 발열성 반응**: H₂O₂를 H₂SO₄에 천천히 첨가해야 함 (역순 금지)
⚠️ **Al, Cu 공정 후 사용 금지**: 금속 부식 발생

---

## DSP (Diluted Sulfuric Peroxide)

### 개요
**DSP**는 SPM의 희석 버전으로, 더 낮은 온도에서 사용하여 열적 손상을 줄이면서 유기물을 제거합니다.

### 조성 및 공정 조건
- **조성**: H₂SO₄ : H₂O₂ : H₂O = 1:1:5 (부피비, 예시)
- **온도**: 60~80°C
- **시간**: 10~20분
- **특징**: SPM보다 온화한 조건

### 용도
1. **PR 잔류물 제거** (Post-etch 등)
2. **후속 공정 전 표면 청정화**
3. **열에 민감한 구조물이 있을 때**

### 반응 메커니즘
SPM과 동일하지만 반응 속도가 느림:
```
H₂SO₄ + H₂O₂ → H₂SO₅ + H₂O
C_nH_m + H₂SO₅ → CO₂ + H₂O + H₂SO₄
```

### SPM vs DSP 비교

| 항목 | SPM | DSP |
|------|-----|-----|
| 온도 | 120-150°C | 60-80°C |
| 세정력 | 매우 강함 | 중간 |
| 열적 손상 | 높음 | 낮음 |
| 적용 | 강력한 유기물 제거 | 온화한 세정 |

---

## SC1 (Standard Clean 1)

### 개요
**SC1**은 RCA Clean의 첫 번째 단계로, **APM (Ammonia-Peroxide Mixture)**이라고도 불립니다. 입자(particle)와 유기물 제거에 사용됩니다.

### 조성 및 공정 조건
- **조성**: NH₄OH : H₂O₂ : H₂O = 1:1:5 ~ 1:2:7 (부피비)
- **온도**: 70~80°C
- **시간**: 10~15분
- **pH**: 약 10-11 (염기성)

### 용도
1. **Particle 제거**
   - 표면에 흡착된 입자 제거
   - 정전기적 반발력 이용
   
2. **유기물 제거**
   - 가벼운 유기 오염물 제거
   
3. **금속 오염물 제거**
   - Group I, II 금속 이온 제거
   - Cu, Zn 등 일부 중금속 제거

### 반응 메커니즘

#### 1. 표면 산화 및 에칭
```
Si + 2H₂O₂ → SiO₂ + 2H₂O
SiO₂ + 2NH₄OH → (NH₄)₂SiO₃ + H₂O
```
- 얇은 산화막 형성 후 에칭
- 약 1-2Å의 Si 소모

#### 2. 표면 하전
```
Si-OH + NH₄OH → Si-O⁻ + NH₄⁺ + H₂O
```
- 표면이 음전하를 띠게 됨
- Particle도 음전하를 띠어 정전기적 반발로 제거

#### 3. Metal Complex 형성
```
Cu²⁺ + 4NH₃ → [Cu(NH₃)₄]²⁺ (soluble)
```
- 금속 이온이 암모니아와 착물 형성
- 용액으로 이동하여 제거

### 장점 및 단점

**장점:**
- Particle 제거 효과가 우수
- 유기물 및 금속 오염 동시 제거
- 비교적 안전한 공정 조건

**단점:**
- Si 표면을 약간 에칭함 (roughness 증가 가능)
- Al, Cu gate가 있는 경우 부식 위험
- 후속 세정(SC2) 필요

### 주의사항
⚠️ **Al, Cu 구조물 부식 가능**: 염기성 용액이므로 주의
⚠️ **금속 오염 가능성**: 용액에 녹아있던 금속이 재부착될 수 있음

---

## SC2 (Standard Clean 2)

### 개요
**SC2**는 RCA Clean의 두 번째 단계로, **HPM (Hydrochloric-Peroxide Mixture)**이라고도 불립니다. 금속 오염물 제거에 특화되어 있습니다.

### 조성 및 공정 조건
- **조성**: HCl : H₂O₂ : H₂O = 1:1:6 ~ 1:2:8 (부피비)
- **온도**: 70~80°C
- **시간**: 10~15분
- **pH**: 약 1-2 (산성)

### 용도
1. **금속 오염물 제거**
   - SC1에서 제거되지 않은 금속 제거
   - Fe, Cu, Zn, Ni 등 transition metal 제거
   
2. **Native oxide 형성**
   - 얇은 화학 산화막 형성
   - 표면 보호

### 반응 메커니즘

#### 1. 금속 산화 및 용해
```
2Fe + 3H₂O₂ → Fe₂O₃ + 3H₂O
Fe₂O₃ + 6HCl → 2FeCl₃ + 3H₂O
```
- H₂O₂가 금속을 산화
- HCl이 산화된 금속을 용해

#### 2. Metal Chloride Complex 형성
```
Cu + H₂O₂ + 2HCl → CuCl₂ + 2H₂O
Ni + H₂O₂ + 2HCl → NiCl₂ + 2H₂O
```
- 가용성 염화물 형성
- 용액으로 제거

#### 3. 표면 산화
```
Si + H₂O₂ → SiO₂ + H₂O
```
- 얇은 화학 산화막 형성 (5-10Å)
- 표면 passivation

### SC1 vs SC2 비교

| 항목 | SC1 (APM) | SC2 (HPM) |
|------|-----------|-----------|
| pH | 염기성 (~10-11) | 산성 (~1-2) |
| 주요 목적 | Particle + 유기물 | 금속 오염 |
| 표면 에칭 | 약간 에칭 | 거의 없음 |
| 순서 | 1단계 | 2단계 |

### 장점 및 단점

**장점:**
- 금속 오염 제거 효과 우수
- Si 에칭이 거의 없음
- 표면에 보호 산화막 형성

**단점:**
- Particle 제거 효과는 SC1보다 낮음
- HCl 가스 발생 주의 필요

### RCA Clean 전체 Flow
```
SPM (유기물 제거)
    ↓
DI Water Rinse
    ↓
SC1 (Particle + 유기물 + 일부 금속)
    ↓
DI Water Rinse
    ↓
DHF (산화막 제거)
    ↓
DI Water Rinse
    ↓
SC2 (금속 오염 제거)
    ↓
DI Water Rinse
    ↓
Dry
```

---

## DHF (Diluted Hydrofluoric Acid)

### 개요
**DHF**는 희석 불산으로, SiO₂(산화막) 제거에 사용됩니다. Native oxide 및 화학 산화막 제거의 표준 공정입니다.

### 조성 및 공정 조건
- **조성**: HF : H₂O = 1:10 ~ 1:100 (부피비)
  - 통상 0.5~5% HF 농도
- **온도**: 상온 (20~25°C)
- **시간**: 30초 ~ 5분 (농도와 산화막 두께에 따라)

### 용도
1. **Native oxide 제거**
   - 자연 산화막 제거 (15-20Å)
   
2. **Chemical oxide 제거**
   - SC1, SC2에서 형성된 산화막 제거
   
3. **Oxide 식각**
   - Field oxide 식각
   - Contact opening
   
4. **표면 활성화**
   - 후속 공정을 위한 Si 표면 노출

### 반응 메커니즘

#### 1. SiO₂ 에칭
```
SiO₂ + 6HF → H₂SiF₆ + 2H₂O
```
또는
```
SiO₂ + 4HF → SiF₄ + 2H₂O
SiF₄ + 2HF → H₂SiF₆
```
- H₂SiF₆ (hexafluorosilicic acid)는 수용성
- SiO₂ 선택적 제거 (Si는 거의 에칭 안됨)

#### 2. Surface Termination
```
Si-O-Si + HF → Si-F + Si-OH
```
- 표면이 Si-F 및 Si-H로 종결됨
- 소수성(hydrophobic) 표면 형성

### 에칭 속도
- **SiO₂**: 100~1000 Å/min (농도 및 온도 의존)
- **Si₃N₄**: 매우 느림 (~1 Å/min)
- **Si**: 매우 느림
- **Al**: 에칭됨 (주의 필요)

### 장점 및 단점

**장점:**
- SiO₂에 대한 높은 선택비
- 상온 공정 가능
- 빠른 에칭 속도

**단점:**
- 매우 위험한 화학약품 (안전 관리 필수)
- Al, Cu 등 금속 부식
- 표면이 소수성이 되어 후속 세정 어려움

### 주의사항
⚠️ **극독성**: 피부 접촉 시 심각한 화상, 특별한 안전 장비 필수
⚠️ **Metal 부식**: Al, Cu 구조물이 있을 때 사용 금지 또는 짧은 시간
⚠️ **표면 거칠기**: 장시간 노출 시 Si 표면 roughness 증가 가능

---

## ACT 945

### 개요
**ACT 945**는 반도체 공정에서 **알루미늄 에칭 후 세정**에 사용되는 상용 세정 용액입니다. 주로 염소계 드라이 에칭 후 잔류하는 부식성 잔류물을 제거합니다.

### 조성 (추정)
상용 제품으로 정확한 조성은 비공개이나, 일반적으로 다음 성분 포함:
- **Organic solvent** (극성 용매)
- **Corrosion inhibitor** (부식 방지제)
- **Surfactant** (계면활성제)
- **pH adjuster** (pH 조절제)

### 용도
1. **Al 에칭 후 세정**
   - Cl 기반 드라이 에칭 후 잔류물 제거
   - AlClₓ, BCl₃, SiClₓ residue 제거
   
2. **부식 방지**
   - Cl 잔류물에 의한 Al 부식 방지
   - Al 표면 보호
   
3. **Particle 제거**
   - 에칭 중 생성된 입자 제거

### 반응 메커니즘

#### 1. Chloride Residue 제거
```
AlClₓ (residue) + H₂O → Al(OH)₃ + HCl
```
또는
```
AlClₓ (residue) + Organic complexing agent → Soluble complex
```
- 염화물 잔류물을 가용화
- 유기 용매에 용해

#### 2. 부식 방지
```
Al + Corrosion inhibitor → Protected Al surface
```
- 표면에 보호막 형성
- Cl에 의한 추가 부식 방지

#### 3. 입자 분산
```
Particle + Surfactant → Dispersed in solution
```
- 계면활성제가 입자를 분산
- 재부착 방지

### 공정 조건
- **온도**: 30~60°C (제품 사양에 따라)
- **시간**: 5~15분
- **방법**: Immersion 또는 spray
- **후처리**: DI water rinse

### Al Etch 후 세정 Flow 예시
```
Al Dry Etch (BCl₃/Cl₂)
    ↓
ACT 945 Clean
    ↓
DI Water Rinse
    ↓
(Optional) Solvent rinse (IPA 등)
    ↓
Dry
```

### 장점 및 단점

**장점:**
- Al에 대해 비부식성
- Cl 잔류물 효과적 제거
- Al 부식 방지 효과
- 상업적으로 검증된 제품

**단점:**
- 비용이 높음
- 조성이 비공개라 문제 발생 시 troubleshooting 어려움
- 폐액 처리 필요

### 주의사항
⚠️ **제품별 조건 확인**: 제조사 권장 조건 준수
⚠️ **배합 금지**: 다른 화학약품과 혼합 금지
⚠️ **유효기간 관리**: 개봉 후 유효기간 확인

---

## 세정 순서 예시

### 1. 일반적인 Pre-Gate Clean
```
SPM (또는 DSP)
    ↓
DI Water Rinse
    ↓
SC1
    ↓
DI Water Rinse
    ↓
DHF (quick dip)
    ↓
DI Water Rinse
    ↓
SC2
    ↓
DI Water Rinse
    ↓
DHF (final)
    ↓
DI Water Rinse
    ↓
Dry
```

### 2. Al Etch 후 세정
```
Al Dry Etch
    ↓
ACT 945
    ↓
DI Water Rinse
    ↓
IPA Rinse (optional)
    ↓
Dry
```

### 3. Post-Litho PR Strip
```
Plasma Ash (O₂ plasma)
    ↓
SPM (또는 DSP)
    ↓
DI Water Rinse
    ↓
(Optional) SC1 for particle
    ↓
DI Water Rinse
    ↓
Dry
```

---

## 세정 효과 비교

| 용액 | 유기물 | Particle | 금속 오염 | Oxide 제거 | 온도 | 비고 |
|------|--------|----------|-----------|------------|------|------|
| **SPM** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ❌ | 120-150°C | 가장 강력 |
| **DSP** | ⭐⭐⭐⭐ | ⭐⭐ | ⭐ | ❌ | 60-80°C | SPM 온화 버전 |
| **SC1** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ❌ | 70-80°C | Particle 최고 |
| **SC2** | ❌ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ❌ | 70-80°C | Metal 최고 |
| **DHF** | ❌ | ⭐ | ❌ | ⭐⭐⭐⭐⭐ | 상온 | Oxide only |
| **ACT 945** | ⭐⭐ | ⭐⭐⭐ | N/A | ❌ | 30-60°C | Al 전용 |

---

## 안전 주의사항

### 공통
- 모든 화학약품은 흄후드 내에서 취급
- 적절한 PPE 착용 (화학복, 고글, 장갑)
- MSDS 숙지

### 특별 주의
- **SPM**: 발열 반응 주의, H₂O₂를 H₂SO₄에 천천히 첨가
- **DHF**: 극독성, 피부 접촉 절대 금지, Ca gluconate gel 비치
- **SC1/SC2**: NH₃ 및 HCl 가스 발생 주의

---

## 참고자료

### 관련 문서
- [[Dry Etch]]
- [[Aluminum-Cl-Etch-ACT945-Cleaning]]
- [[Photolithography Process Guide]]

### 추가 정보
- RCA Clean: Radio Corporation of America에서 개발한 표준 세정 방법
- Modern RCA Clean: 최신 공정에서는 SC1/SC2 조건이 변형되어 사용됨
- Megasonic cleaning: 물리적 세정과 화학적 세정을 결합한 기술

---

## Tags
#wet-clean #semiconductor-cleaning #SPM #DSP #SC1 #SC2 #DHF #ACT945 #RCA-clean #chemical-process

---

**작성일**: 2025-10-06
**최종 수정**: 2025-10-06
