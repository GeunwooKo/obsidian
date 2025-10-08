# Power MOSFET 측정항목

Power MOSFET의 성능과 특성을 평가하기 위한 주요 측정항목들을 정리한 문서입니다.

## 1. IDSS (Drain-Source Leakage Current)

**드레인-소스 누설전류**

### 정의
- Gate가 OFF 상태(VGS = 0V)일 때, Drain과 Source 사이에 흐르는 누설전류
- MOSFET이 차단 상태에서의 전류 누설 정도를 나타냄

### 측정 조건
- VGS = 0V (Gate-Source 전압 0)
- VDS = 정격전압 (Drain-Source에 전압 인가)
- 일반적으로 상온(25°C) 및 고온(예: 125°C)에서 측정

### 중요성
- 낮을수록 좋음 (일반적으로 μA 수준)
- 대기전력 손실과 직접 관련
- 온도가 높아질수록 증가하는 경향

### 일반적인 규격
- 상온: 수십 μA 이하
- 고온: 수백 μA 이하

---

## 2. IGSS (Gate-Source Leakage Current)

**게이트-소스 누설전류**

### 정의
- Gate와 Source 사이에 전압을 인가했을 때 흐르는 누설전류
- Gate 절연막(oxide)의 품질을 나타내는 지표

### 측정 조건
- VGS = ±정격 게이트 전압 (예: ±20V)
- VDS = 0V
- 양/음 전압 모두에서 측정

### 중요성
- Gate oxide의 절연 특성 평가
- 극히 낮아야 함 (일반적으로 nA 수준)
- 높을 경우 Gate 절연막 손상 의심

### 일반적인 규격
- ±100 nA 이하 (대부분의 Power MOSFET)

---

## 3. BV 또는 BVDSS (Breakdown Voltage)

**드레인-소스 항복전압**

### 정의
- Gate가 OFF 상태에서 Drain-Source 간 절연이 파괴되는 전압
- MOSFET이 견딜 수 있는 최대 전압

### 측정 조건
- VGS = 0V
- ID = 일정한 작은 전류 (예: 250μA 또는 1mA)
- Drain 전압을 증가시키며 항복점 측정

### 중요성
- 정격 동작 전압의 기준
- 안전 마진을 고려하여 실제 사용 전압은 BV의 70-80% 이하 권장
- 시스템 설계 시 가장 중요한 파라미터 중 하나

### 일반적인 범위
- Low Voltage: 20-100V
- Medium Voltage: 100-600V
- High Voltage: 600V 이상

---

## 4. RDS(ON) (On-State Resistance)

**온-저항**

### 정의
- MOSFET이 완전히 ON 되었을 때 Drain-Source 간의 저항
- 전도 손실을 결정하는 가장 중요한 파라미터

### 측정 조건
- VGS = 정격 게이트 전압 (예: 10V, 4.5V 등)
- ID = 정격 전류
- 일반적으로 25°C 및 고온에서 측정

### 중요성
- 전도 손실(Conduction Loss) = I²D × RDS(ON)
- 낮을수록 효율이 높음
- 온도가 증가하면 RDS(ON)도 증가 (양의 온도계수)
- Die 크기 및 비용과 직접 관련

### 일반적인 범위
- Low Voltage (< 30V): 수 mΩ 이하
- Medium Voltage (100-200V): 수십 mΩ
- High Voltage (> 600V): 수백 mΩ ~ 수 Ω

### 온도 의존성
- RDS(ON)@Tj = RDS(ON)@25°C × (1 + α × ΔT)
- α: 온도계수 (일반적으로 0.003 ~ 0.006/°C)

---

## 측정항목 간 관계 및 트레이드오프

### BV vs RDS(ON)
- 고전압 MOSFET일수록 RDS(ON)이 증가
- BV ∝ RDS(ON) × Area (Chip 면적 일정 시)

### 온도 영향
- IDSS: 온도 증가 시 지수적으로 증가
- IGSS: 온도에 상대적으로 둔감
- RDS(ON): 온도 증가 시 선형적으로 증가

### 응용별 중점 항목
- **스위칭 전원**: RDS(ON), BVDSS
- **저전력 응용**: IDSS
- **고신뢰성 응용**: IGSS, BVDSS

---

## 측정 시 주의사항

1. **정전기 방전(ESD) 보호**
   - Gate는 정전기에 매우 민감
   - 측정 전 접지 필수

2. **온도 관리**
   - 파라미터는 온도에 따라 크게 변화
   - 측정 온도 명시 필수

3. **측정 장비**
   - IDSS, IGSS: 고감도 전류계 필요 (pA ~ μA)
   - BV: 고전압 소스 및 보호회로
   - RDS(ON): Kelvin 연결 방식 권장

---

## 관련 문서
- [[MOSFET 기본 동작원리]]
- [[전력 손실 계산]]
- [[반도체 측정 장비]]

## 참고자료
- JEDEC Standards (JESD24 시리즈)
- IEC 60747-8 (Discrete semiconductor devices - Part 8: Field-effect transistors)

---
*작성일: 2025-10-06*
*태그: #PowerMOSFET #반도체측정 #전력반도체*
