# Dry Etch Mechanisms (드라이 에칭 메커니즘)

## 개요

드라이 에칭은 물리적 스퍼터링과 화학적 반응이 결합된 복합 메커니즘으로 진행됩니다. 이 문서는 에칭 메커니즘의 기본 원리와 공정 최적화 전략을 다룹니다.

---

## 기본 에칭 메커니즘

### 1. 물리적 에칭 (Physical Sputtering)

#### 원리
- 플라즈마에서 가속된 양이온이 웨이퍼 표면을 bombardment
- 운동 에너지로 원자를 물리적으로 제거
- 이온 에너지: 수십 ~ 수백 eV

#### 특징
```
✅ 장점:
- 높은 이방성 (수직 프로파일)
- Field 제거 효과적
- 치수 제어 정밀

⚠️ 단점:
- 낮은 선택비
- 하부층 손상 가능
- 표면 거칠기 증가
- Footing/Notching 발생
```

#### 메커니즘
```
1. 이온 가속: 플라즈마 sheath에서 에너지 획득
2. 표면 충돌: 운동량 전달
3. 원자 방출: 결합 에너지 극복
4. 배출: 진공 펌프로 제거
```

### 2. 화학적 에칭 (Chemical Etching)

#### 원리
- 플라즈마 생성 중성 라디칼이 표면과 화학 반응
- 휘발성 부산물 형성
- 등방성 특성

#### 특징
```
✅ 장점:
- 높은 에칭률
- 높은 선택비
- 하부층 손상 최소
- 균일한 에칭

⚠️ 단점:
- 등방성 (측면 에칭)
- 수직 측벽 어려움
- 언더컷 발생
```

#### 주요 반응 예시
```
SiO2 + 4F → SiF4 + O2
Si3N4 + 12F → 3SiF4 + 2N2
W + 6F → WF6
Al + 3Cl → AlCl3
```

---

## RIE (Reactive Ion Etching): 복합 메커니즘

### 개념

물리적 + 화학적 메커니즘의 최적 조합:

```
RIE = Physical Sputtering + Chemical Reaction

목표:
- 화학적 → 효율적 제거, 높은 선택비
- 물리적 → 방향성 제어, 프로파일 제어
```

### 균형 제어

#### 화학 성분이 우세할 때
```
- 더 등방성
- 높은 선택비
- 측면 에칭 증가
- 언더컷 위험
```

#### 물리 성분이 우세할 때
```
- 더 이방성
- 수직 프로파일
- 선택비 감소
- 손상 증가
```

### 파라미터 제어

#### 이온 에너지 증가 (↑ Bias Power)
```
→ 더 많은 물리적 스퍼터링
→ 더 나은 수직성
→ 손상 위험 증가
```

#### 반응성 가스 증가 (↑ SF6, Cl2 등)
```
→ 더 많은 화학적 에칭
→ 더 높은 선택비
→ 등방성 증가
```

#### Polymer 형성 가스 추가 (CHF3, C4F8)
```
→ 측벽 보호층 형성
→ 수직성 향상
→ 측면 에칭 방지
```

---

## Etchback 메커니즘

Etchback은 via/contact 충진 후 과잉 물질을 제거하는 공정으로, 특별한 메커니즘 제어가 필요합니다.

### 주요 메커니즘

#### 1. 물리적 Sputtering (이온)

**공정**
- 플라즈마 sheath에서 양이온 가속
- 표면에 방향성 bombardment
- 이방성 제거

**역할**
- 깔끔한 field W 제거
- via fill 보호
- 날카로운 프로파일 형성

#### 2. 화학적 반응 (라디칼)

**공정**
- 중성 라디칼 (F, Cl 등)이 표면 반응
- 휘발성 부산물 (WF6 등) 형성
- 진공 펌프로 배출

**제어**
- 가스 종류/압력/파워로 조절
- 화학 vs 물리 비율 최적화

**역할**
- 효율적이고 선택적인 W 제거
- Barrier/via 내부 보호

#### 3. 복합 메커니즘 (RIE)

**메커니즘**
- 물리+화학의 시너지 효과
- 이방성(수직성)과 선택비 동시 달달

**결과**
- 정밀한 프로파일 제어
- via fill 유지
- 측벽 수직성
- Residue 최소화

### 추가 고려사항

#### 에칭 프로파일 제어

**저압 + 고바이어스**
```
→ 방향성 이온 bombardment
→ 수직 측벽
→ 효과적 etchback
```

**고압**
```
→ 등방성 제거
→ 측면 에칭 증가
→ 언더컷 위험
```

#### Microloading & Footing 효과

**Microloading**
- Dense 영역이 open 영역보다 느리게 에칭
- 라디칼 고갈 때문
- 레시피 최적화로 완화

**Footing/Notching**
- W와 하부층 계면에서 발생
- 정밀한 레시피로 최소화

#### Residue 관리

**오버에치 단계**
- Cl2/O2 화학 사용
- Residue 제거 최적화
- 프로파일 개선

---

## Neutral-to-Ion Ratio (중성/이온 비율)

### 공정 윈도우

Neutral-to-ion 비율은 프로파일, 선택비, ARDE에 핵심적입니다.

#### 플라즈마 펄스 변조
```
Pulse off-time 증가:
→ Neutral/Ion 비율 증가
→ 에칭 깊이 향상 (중간 영역)
→ 과도 시 오히려 감소 (over-passivation)
```

#### Aspect Ratio 영향
```
AR 증가:
→ Bottom으로 neutral flux 급격히 감소
→ ARDE (Aspect Ratio Dependent Etching) 발생
→ Neutral/Ion 비율 최적화 필요
```

### 공정 윈도우 조정

#### 파라미터
- 플라즈마 펄스 타이밍
- 챔버 압력
- 가스 조성 (Fluorocarbon %)
- 이온 에너지

#### 최적 범위
```
Γ_n/Γ_i (Neutral/Ion flux ratio):
- 일반적: 5~20
- 공정/패턴별 최적화 필요
```

### 제한 및 리스크

#### 비율이 너무 높을 때
```
⚠️ Etch stop
⚠️ 과도한 측면 에칭
⚠️ Over-passivation
```

#### 비율이 너무 낮을 때
```
⚠️ 낮은 에칭률
⚠️ 하부층 손상
⚠️ Microloading 증가
```

### 실용 레시피

균형 잡힌 접근:
```
Neutral flux: 화학 에칭 + 부산물 제거
Ion flux: 이방성 유지 + 표면 활성화
```

---

## 공정별 메커니즘 비교

### Contact/Via Etch

| 단계 | 주 메커니즘 | 목적 |
|------|------------|------|
| Breakthrough | 화학적 (높은 F/C) | 산화막 제거 |
| Main Etch | 균형 RIE | Bulk 제거 |
| Overetch | 화학적 (낮은 F/C) | Residue 제거 |

### Metal Line Etch

| 단계 | 주 메커니즘 | 목적 |
|------|------------|------|
| Main Etch | 물리+화학 | 수직 프로파일 |
| Residue Clean | 화학적 | Redeposition 제거 |

### HAR (High Aspect Ratio) Etch

| 이슈 | 메커니즘 | 해결책 |
|------|---------|-------|
| RIE lag | Neutral flux 부족 | 압력/화학 최적화 |
| ARDE | AR에 따른 속도 감소 | Pulsed plasma |
| Bowing | 측벽 과도 에칭 | 온도/passivation 제어 |
| Bottom residue | 부산물 축적 | Overetch 최적화 |

---

## 실무 최적화 전략

### 1. Dense vs Isolated 패턴

#### Dense 패턴
```
문제: Microloading, 느린 에칭
해결:
- 물리적 성분 증가
- 가스 유량 증가
- 압력 최적화
```

#### Isolated 패턴
```
문제: 과도 에칭, 프로파일 손상
해결:
- 화학적 선택비 강화
- 낮은 바이어스
- Endpoint 정밀 제어
```

### 2. 선택비 vs 프로파일 Trade-off

#### 높은 선택비 필요 시
```
접근:
- 화학적 에칭 강화
- 낮은 이온 에너지
- 반응성 가스 증가

결과:
✅ 하부층 보호
⚠️ 이방성 감소
```

#### 높은 이방성 필요 시
```
접근:
- 물리적 에칭 강화
- 높은 이온 에너지
- Passivation gas 추가

결과:
✅ 수직 프로파일
⚠️ 선택비 저하
```

### 3. 온도 제어 전략

```
저온 (-20~-50°C):
→ Passivation 강화
→ 이방성 향상
→ 고해상도 패턴

중온 (0~40°C):
→ 균형잡힌 성능
→ 대부분의 공정

고온 (40~80°C):
→ 부산물 탈착 촉진
→ 에칭률 증가
→ 금속 에칭
```

---

## Python 시뮬레이션 예제

### Etch Rate vs Ion Energy

```python
import numpy as np
import matplotlib.pyplot as plt

# 파라미터
ion_energy = np.linspace(0, 500, 100)  # eV
chemical_rate = 100  # Å/min (화학적 성분)
sputtering_yield = 0.5  # atoms/ion

# 에칭률 계산
physical_rate = sputtering_yield * ion_energy * 0.2
total_rate = chemical_rate + physical_rate

# 선택비 (역수)
selectivity = 1 / (1 + ion_energy / 200)

# 그래프
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 5))

# 에칭률
ax1.plot(ion_energy, chemical_rate * np.ones_like(ion_energy), 
         'b--', label='화학적')
ax1.plot(ion_energy, physical_rate, 'r--', label='물리적')
ax1.plot(ion_energy, total_rate, 'k-', linewidth=2, label='전체')
ax1.set_xlabel('Ion Energy (eV)')
ax1.set_ylabel('Etch Rate (Å/min)')
ax1.set_title('이온 에너지에 따른 에칭률')
ax1.legend()
ax1.grid(True, alpha=0.3)

# 선택비
ax2.plot(ion_energy, selectivity, 'g-', linewidth=2)
ax2.set_xlabel('Ion Energy (eV)')
ax2.set_ylabel('Selectivity (normalized)')
ax2.set_title('이온 에너지에 따른 선택비')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

print(f"최적 조건 (선택비 > 0.7):")
optimal_energy = ion_energy[selectivity > 0.7]
print(f"이온 에너지: {optimal_energy[0]:.1f} ~ {optimal_energy[-1]:.1f} eV")
```

### Neutral-to-Ion Ratio 영향

```python
import numpy as np
import matplotlib.pyplot as plt

# Neutral/Ion ratio
ratio = np.linspace(0, 30, 100)

# 에칭 깊이 (임의 모델)
def etch_depth(r):
    # Moderate neutral: 깊이 증가
    # High neutral: Over-passivation으로 감소
    return 100 * r * np.exp(-r/15)

depth = etch_depth(ratio)

# ARDE 영향 (aspect ratio별)
ar_5 = etch_depth(ratio * 0.9)  # AR=5
ar_10 = etch_depth(ratio * 0.7)  # AR=10
ar_15 = etch_depth(ratio * 0.5)  # AR=15

# 그래프
plt.figure(figsize=(10, 6))
plt.plot(ratio, depth, 'b-', linewidth=2, label='AR=1')
plt.plot(ratio, ar_5, 'g--', linewidth=2, label='AR=5')
plt.plot(ratio, ar_10, 'r--', linewidth=2, label='AR=10')
plt.plot(ratio, ar_15, 'm--', linewidth=2, label='AR=15')
plt.xlabel('Neutral-to-Ion Ratio')
plt.ylabel('Etch Depth (normalized)')
plt.title('Neutral/Ion 비율과 AR이 에칭 깊이에 미치는 영향')
plt.legend()
plt.grid(True, alpha=0.3)
plt.axvline(x=10, color='k', linestyle=':', alpha=0.5, 
            label='Typical optimal')
plt.show()

# 최적 범위 계산
optimal_idx = np.argmax(depth)
print(f"최적 Neutral/Ion ratio: {ratio[optimal_idx]:.1f}")
print(f"최대 에칭 깊이: {depth[optimal_idx]:.1f} (normalized)")
```

---

## 요약 및 Best Practice

### 메커니즘 선택 가이드

```
고선택비 필요:
→ 화학적 에칭 우세
→ SF6/O2, NF3 등
→ 낮은 바이어스

고이방성 필요:
→ 물리적 에칭 우세
→ Ar 추가, 높은 바이어스
→ Passivation gas

균형잡힌 공정:
→ RIE 최적화
→ 화학/물리 비율 조절
→ 온도/압력 미세 조정
```

### 트러블슈팅 체크리스트

- [ ] 프로파일 이상: 화학/물리 균형 재조정
- [ ] 선택비 문제: 이온 에너지 감소, 반응성 가스 증가
- [ ] Microloading: 가스 유량/압력 최적화
- [ ] ARDE: Neutral/Ion 비율 조정, Pulsed plasma
- [ ] Residue: Overetch 화학 최적화

---

## 관련 문서
- [[Tungsten Etching Comprehensive Guide]]
- [[Dry Etch]]
- [[Temperature Effect on Reaction Rate]]

---

**Tags**: #etch-mechanism #RIE #physical-etch #chemical-etch #neutral-ion-ratio #etchback

**작성일**: 2025-10-06
**최종 수정**: 2025-10-06
