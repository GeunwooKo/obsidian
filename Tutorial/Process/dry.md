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