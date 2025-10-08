# Ti/TiN/W 다층막 면저항 분석 및 두께 추정

## 1. 박막 구조 개요

### 구조
- **기판**: SiO2 (300nm)
- **Layer 1**: Ti (고정 두께)
- **Layer 2**: TiN (고정 두께)
- **Layer 3**: W (가변 두께)

### 목적
Ti/TiN 두께가 일정할 때, W 두께 변화에 따른 면저항(Rs) 측정 데이터를 fitting하여, 추후 Rs 측정값으로부터 W 두께를 역산하는 모델 구축

---

## 2. 다층막 면저항 이론

### 2.1 병렬 저항 모델 (Parallel Resistor Model)

다층막 구조에서 각 층이 전기적으로 병렬로 연결되어 있다고 가정할 때:

$$\frac{1}{R_s} = \frac{1}{R_{s,Ti}} + \frac{1}{R_{s,TiN}} + \frac{1}{R_{s,W}}$$

각 층의 면저항은:

$$R_{s,i} = \frac{\rho_i}{t_i}$$

여기서:
- $R_{s,i}$ = 각 층의 면저항 (Ω/sq)
- $\rho_i$ = 각 층의 비저항 (Ω·cm)
- $t_i$ = 각 층의 두께 (cm)

### 2.2 Ti/TiN이 고정된 경우

Ti와 TiN의 두께가 고정되어 있으므로, $R_{s,Ti}$와 $R_{s,TiN}$은 상수입니다.

$$\frac{1}{R_{s,Ti}} + \frac{1}{R_{s,TiN}} = C_{base}$$

따라서:

$$\frac{1}{R_s} = C_{base} + \frac{t_W}{\rho_W}$$

정리하면:

$$R_s = \frac{1}{C_{base} + \frac{t_W}{\rho_W}}$$

---

## 3. Fitting 모델

### 3.1 추천 Fitting 식

실제 데이터 fitting을 위해 다음 형태를 사용:

$$R_s = \frac{A}{B + t_W}$$

여기서:
- $A$ = fitting 상수 (Ω·nm/sq)
- $B$ = offset 상수 (nm)
- $t_W$ = W 두께 (nm)

이 식은 물리적 의미:
- $A = \rho_W$ (W의 비저항, Ω·nm 단위)
- $B = \rho_W \cdot C_{base}$ (Ti/TiN의 기여도를 반영)

### 3.2 대체 모델 (선형 영역)

W 두께가 충분히 두꺼워서 W가 지배적인 경우:

$$R_s \approx \frac{\rho_W}{t_W}$$

또는 역수 관계:

$$\frac{1}{R_s} = a \cdot t_W + b$$

여기서:
- $a = 1/\rho_W$
- $b = C_{base}$ (Ti/TiN 기여도)

---

## 4. Python Fitting 코드

### 4.1 비선형 Fitting (추천)

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit

# 데이터 입력 (예시)
# t_W: W 두께 (nm), Rs: 면저항 (Ω/sq)
data = {
    't_W': np.array([50, 100, 150, 200, 250, 300]),  # W 두께 (nm)
    'Rs': np.array([0.50, 0.35, 0.28, 0.24, 0.21, 0.19])  # 면저항 (Ω/sq)
}

# Fitting 함수 정의
def sheet_resistance_model(t_W, A, B):
    """
    Rs = A / (B + t_W)
    
    Parameters:
    - t_W: W 두께 (nm)
    - A: fitting 상수 (Ω·nm/sq)
    - B: offset 상수 (nm)
    """
    return A / (B + t_W)

# Curve fitting 수행
popt, pcov = curve_fit(sheet_resistance_model, data['t_W'], data['Rs'], 
                       p0=[50, 100])  # 초기 추정값

A_fit, B_fit = popt
print(f"Fitting 결과:")
print(f"A = {A_fit:.2f} Ω·nm/sq")
print(f"B = {B_fit:.2f} nm")

# W 비저항 추정
rho_W = A_fit  # Ω·nm
print(f"W 비저항 추정값: {rho_W:.2f} Ω·nm = {rho_W*1e-9:.2e} Ω·m")

# 시각화
t_W_fit = np.linspace(data['t_W'].min(), data['t_W'].max(), 100)
Rs_fit = sheet_resistance_model(t_W_fit, A_fit, B_fit)

plt.figure(figsize=(10, 6))
plt.scatter(data['t_W'], data['Rs'], label='Measured Data', s=100)
plt.plot(t_W_fit, Rs_fit, 'r-', label=f'Fit: Rs = {A_fit:.1f}/(t_W + {B_fit:.1f})')
plt.xlabel('W Thickness (nm)', fontsize=12)
plt.ylabel('Sheet Resistance (Ω/sq)', fontsize=12)
plt.title('Sheet Resistance vs W Thickness', fontsize=14)
plt.legend(fontsize=11)
plt.grid(True, alpha=0.3)
plt.show()

# R² 계산
Rs_predicted = sheet_resistance_model(data['t_W'], A_fit, B_fit)
ss_res = np.sum((data['Rs'] - Rs_predicted)**2)
ss_tot = np.sum((data['Rs'] - np.mean(data['Rs']))**2)
r_squared = 1 - (ss_res / ss_tot)
print(f"R² = {r_squared:.4f}")
```

### 4.2 선형 Fitting (역수 관계)

```python
# 1/Rs vs t_W 선형 fitting
def linear_conductance_model(t_W, a, b):
    """
    1/Rs = a*t_W + b
    """
    return a * t_W + b

# 역수 변환
inv_Rs = 1 / data['Rs']

# Curve fitting
popt_linear, _ = curve_fit(linear_conductance_model, data['t_W'], inv_Rs)

a_fit, b_fit = popt_linear
print(f"\n선형 Fitting 결과:")
print(f"a = {a_fit:.6f} (Ω/sq)^-1/nm")
print(f"b = {b_fit:.4f} (Ω/sq)^-1")

rho_W_linear = 1 / a_fit
print(f"W 비저항 (선형): {rho_W_linear:.2f} Ω·nm")
```

---

## 5. W 두께 역산 방법

### 5.1 비선형 모델 사용 (기존 방법)

Fitting 결과 $A$, $B$ 값을 얻었다면:

$$t_W = \frac{A}{R_s} - B$$

Python 코드:

```python
def calculate_thickness_from_Rs(Rs_measured, A, B):
    """
    면저항으로부터 W 두께 계산
    
    Parameters:
    - Rs_measured: 측정된 면저항 (Ω/sq)
    - A, B: fitting 상수
    
    Returns:
    - t_W: W 두께 (nm)
    """
    t_W = (A / Rs_measured) - B
    return t_W

# 사용 예시
Rs_new = 0.30  # 새로 측정된 면저항
t_W_estimated = calculate_thickness_from_Rs(Rs_new, A_fit, B_fit)
print(f"Rs = {Rs_new} Ω/sq → W 두께 = {t_W_estimated:.1f} nm")
```

### 5.2 선형 모델 사용 

선형 fitting 결과 $a$, $b$를 얻었다면:

$$t_W = \frac{\frac{1}{R_s} - b}{a}$$

```python
def calculate_thickness_linear(Rs_measured, a, b):
    """
    선형 모델로 W 두께 계산
    """
    t_W = (1/Rs_measured - b) / a
    return t_W
```

---

## 6. 실용적 워크플로우

### 6.1 초기 Calibration

1. **샘플 준비**: Ti/TiN 두께 고정, W 두께 변경 (최소 5개 이상)
2. **면저항 측정**: 4-point probe 등으로 측정
3. **데이터 정리**: Excel/CSV 파일로 저장
4. **Fitting 수행** : Excel slope, intercept 함수 사용 또는 파이썬 사용
5. **모델 검증**: R² > 0.95 확인
6. **파라미터 저장**: A, B 값 기록

### 6.2 일상 측정

1. **면저항 측정**: 새 샘플의 Rs 측정
2. **두께 계산**: 저장된 A, B 값으로 계산
3. **결과 기록**: 추정 두께 기록

### 6.3 Excel 수식 활용

Excel에서 간단히 계산하려면:

셀 A1: 측정된 Rs  
셀 A2: =($A_fit/$A1) - $B_fit

여기서 A_fit, B_fit은 fitting 결과값을 절대참조로 설정

---

## 7. 주의사항

### 7.1 모델 적용 범위

- **유효 범위**: Fitting에 사용한 W 두께 범위 내에서만 정확
- **외삽 주의**: 범위 밖 데이터는 오차 증가 가능
- **Ti/TiN 변경 시**: 새로운 calibration 필요

### 7.2 측정 정확도

- **4-point probe 정확도**: 통상 ±2-5%
- **두께 균일도**: 샘플 내 균일도 확인 필요
- **온도 효과**: 측정 온도 일정하게 유지

### 7.3 물리적 제약

- **매우 얇은 W (<20nm)**: 불연속 박막, 모델 부정확
- **계면 효과**: Ti/TiN/W 계면 저항 영향 가능
- **결정성**: W의 결정 구조에 따라 비저항 변화

---

## 8. 고급 분석

### 8.1 다층막 저항 분리

Ti, TiN, W의 개별 비저항을 분리하려면:

1. Ti 단독 증착 샘플 측정 → $\rho_{Ti}$
2. Ti/TiN 증착 샘플 측정 → $\rho_{TiN}$ 계산
3. 전체 구조에서 W 기여도 분리

### 8.2 온도 의존성

온도별 측정 시:

$$\rho(T) = \rho_0[1 + \alpha(T - T_0)]$$

여기서 $\alpha$는 온도계수

---

## 9. 참고자료

- Schroder, D. K. (2006). *Semiconductor Material and Device Characterization*
- Ohring, M. (2001). *Materials Science of Thin Films*
- 4-point probe measurement technique
- Sheet resistance calculation methods

---

## 10. 실습 템플릿

```python
# ===============================================
# Ti/TiN/W Sheet Resistance Analysis Template
# ===============================================

import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

# 1. 데이터 입력
t_W = np.array([])  # W 두께 (nm) - 여기에 데이터 입력
Rs = np.array([])   # 면저항 (Ω/sq) - 여기에 데이터 입력

# 2. Fitting 함수
def Rs_model(t_W, A, B):
    return A / (B + t_W)

# 3. Fitting 실행
popt, pcov = curve_fit(Rs_model, t_W, Rs, p0=[50, 100])
A, B = popt

# 4. 결과 출력
print(f"A = {A:.2f} Ω·nm/sq")
print(f"B = {B:.2f} nm")

# 5. 새 측정값으로 두께 계산
def get_thickness(Rs_measured):
    return (A / Rs_measured) - B

# 사용 예시:
# t_W_new = get_thickness(0.30)
# print(f"Estimated W thickness: {t_W_new:.1f} nm")
```

---

**작성일**: 2025-10-06  
**작성자**: Claude  
**버전**: 1.0
