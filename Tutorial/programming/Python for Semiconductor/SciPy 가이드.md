# SciPy 가이드 (과학 계산 및 통계)

## 목차
- [[#SciPy 소개]]
- [[#통계 분석]]
- [[#최적화]]
- [[#신호 처리]]
- [[#선형대수]]
- [[#보간 및 근사]]
- [[#적분 및 미분]]
- [[#반도체 응용 예제]]

---

## SciPy 소개

### SciPy란?

SciPy는 과학 및 기술 계산을 위한 Python 라이브러리입니다.

**주요 모듈:**
- `scipy.stats` - 통계
- `scipy.optimize` - 최적화
- `scipy.signal` - 신호 처리
- `scipy.linalg` - 선형대수
- `scipy.interpolate` - 보간
- `scipy.integrate` - 적분

### 설치

```bash
pip install scipy
```

---

## 통계 분석

### 기술 통계

```python
from scipy import stats
import numpy as np

# 데이터
yield_data = np.array([95.5, 96.2, 94.8, 97.1, 95.0, 96.5, 94.3, 95.8])

# 기본 통계
desc_stats = stats.describe(yield_data)
print(f"개수: {desc_stats.nobs}")
print(f"평균: {desc_stats.mean:.2f}")
print(f"분산: {desc_stats.variance:.2f}")
print(f"왜도: {desc_stats.skewness:.2f}")
print(f"첨도: {desc_stats.kurtosis:.2f}")
```

### 확률 분포

```python
from scipy import stats

# 정규분포
mu, sigma = 95, 2
normal_dist = stats.norm(loc=mu, scale=sigma)

# 확률 밀도 함수
pdf_value = normal_dist.pdf(96.5)
print(f"PDF: {pdf_value:.4f}")

# 누적 분포 함수
cdf_value = normal_dist.cdf(96.5)
print(f"CDF: {cdf_value:.4f}")

# 백분위수
percentile_95 = normal_dist.ppf(0.95)
print(f"95th percentile: {percentile_95:.2f}")

# 난수 생성
samples = normal_dist.rvs(size=1000)
```

### 가설 검정

```python
from scipy import stats

# t-검정 (단일 표본)
data = np.array([95.5, 96.2, 94.8, 97.1, 95.0])
target_mean = 95.0

t_stat, p_value = stats.ttest_1samp(data, target_mean)
print(f"t-통계량: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")

# t-검정 (독립 표본)
group1 = np.array([95.5, 96.2, 94.8, 97.1])
group2 = np.array([94.2, 93.8, 95.1, 94.5])

t_stat, p_value = stats.ttest_ind(group1, group2)
print(f"독립 t-검정 p-value: {p_value:.4f}")

# 대응 표본
before = np.array([94.5, 95.2, 94.8, 95.5])
after = np.array([95.5, 96.2, 95.8, 96.1])

t_stat, p_value = stats.ttest_rel(before, after)
print(f"대응 t-검정 p-value: {p_value:.4f}")
```

### ANOVA (분산 분석)

```python
from scipy import stats

# 세 그룹 데이터
equipment1 = [95.5, 96.2, 94.8, 97.1, 95.0]
equipment2 = [94.2, 93.8, 95.1, 94.5, 93.9]
equipment3 = [96.1, 95.8, 96.5, 95.9, 96.3]

# 일원 분산분석
f_stat, p_value = stats.f_oneway(equipment1, equipment2, equipment3)

print(f"F-통계량: {f_stat:.4f}")
print(f"p-value: {p_value:.4f}")
```

### 상관관계

```python
from scipy import stats

temperature = [800, 825, 850, 875, 900]
yield_rate = [94.5, 95.2, 96.1, 95.8, 95.3]

# Pearson 상관계수
pearson_r, p_value = stats.pearsonr(temperature, yield_rate)
print(f"Pearson r: {pearson_r:.4f}")

# Spearman 상관계수
spearman_r, p_value = stats.spearmanr(temperature, yield_rate)
print(f"Spearman r: {spearman_r:.4f}")
```

### 정규성 검정

```python
from scipy import stats

data = np.random.normal(95, 2, 100)

# Shapiro-Wilk 검정
stat, p_value = stats.shapiro(data)
print(f"Shapiro-Wilk p-value: {p_value:.4f}")

if p_value > 0.05:
    print("정규분포를 따름")
```

---

## 최적화

### 최소값 찾기

```python
from scipy import optimize

# 함수 정의
def objective(x):
    return x**2 + 5*np.sin(x)

# 최소값 찾기
result = optimize.minimize(objective, x0=0)

print(f"최소값 위치: {result.x[0]:.4f}")
print(f"최소값: {result.fun:.4f}")

# 제약 조건
def constraint(x):
    return x - 1

cons = {'type': 'ineq', 'fun': constraint}
result = optimize.minimize(objective, x0=2, constraints=cons)
```

### 곡선 피팅

```python
from scipy import optimize
import matplotlib.pyplot as plt

# 데이터
temperature = np.array([800, 825, 850, 875, 900])
yield_rate = np.array([94.5, 95.2, 96.1, 95.8, 95.3])

# 2차 함수 피팅
def quadratic(x, a, b, c):
    return a*x**2 + b*x + c

params, cov = optimize.curve_fit(quadratic, temperature, yield_rate)
a, b, c = params

print(f"a = {a:.6f}, b = {b:.6f}, c = {c:.6f}")

# 예측
temp_pred = np.linspace(800, 900, 100)
yield_pred = quadratic(temp_pred, a, b, c)

# 시각화
plt.scatter(temperature, yield_rate, label="Data")
plt.plot(temp_pred, yield_pred, 'r-', label="Fit")
plt.xlabel("Temperature (°C)")
plt.ylabel("Yield (%)")
plt.legend()
plt.show()
```

### 선형 회귀

```python
from scipy import stats

x = np.array([1, 2, 3, 4, 5])
y = np.array([2.1, 3.9, 6.2, 7.8, 10.1])

# 선형 회귀
slope, intercept, r_value, p_value, std_err = stats.linregress(x, y)

print(f"기울기: {slope:.4f}")
print(f"절편: {intercept:.4f}")
print(f"R²: {r_value**2:.4f}")
print(f"p-value: {p_value:.4f}")

# 예측
y_pred = slope * x + intercept
```

---

## 신호 처리

### 필터링

```python
from scipy import signal

# 노이즈가 있는 신호
t = np.linspace(0, 1, 500)
clean_signal = np.sin(2*np.pi*5*t)
noise = 0.1 * np.random.randn(len(t))
noisy_signal = clean_signal + noise

# 저역 통과 필터 (Low-pass filter)
b, a = signal.butter(4, 0.1)
filtered = signal.filtfilt(b, a, noisy_signal)

# 시각화
plt.figure(figsize=(12, 4))
plt.plot(t, noisy_signal, alpha=0.5, label="Noisy")
plt.plot(t, filtered, label="Filtered")
plt.legend()
plt.show()
```

### 이동 평균

```python
from scipy import signal

data = np.random.randn(100) + 5

# 이동 평균 (윈도우 크기 10)
window = np.ones(10) / 10
smoothed = signal.convolve(data, window, mode='same')

plt.plot(data, alpha=0.5, label="Original")
plt.plot(smoothed, label="Smoothed")
plt.legend()
plt.show()
```

### FFT (고속 푸리에 변환)

```python
from scipy.fft import fft, fftfreq

# 신호 생성
sampling_rate = 1000
t = np.linspace(0, 1, sampling_rate)
signal_data = np.sin(2*np.pi*50*t) + 0.5*np.sin(2*np.pi*120*t)

# FFT
fft_values = fft(signal_data)
frequencies = fftfreq(len(signal_data), 1/sampling_rate)

# 진폭 스펙트럼
amplitude = np.abs(fft_values)

# 양의 주파수만
pos_freq = frequencies[:len(frequencies)//2]
pos_amp = amplitude[:len(amplitude)//2]

# 시각화
plt.plot(pos_freq, pos_amp)
plt.xlabel("Frequency (Hz)")
plt.ylabel("Amplitude")
plt.show()
```

---

## 선형대수

### 행렬 연산

```python
from scipy import linalg

# 행렬 정의
A = np.array([[1, 2], [3, 4]])
b = np.array([5, 6])

# 선형 방정식 풀기: Ax = b
x = linalg.solve(A, b)
print(f"해: {x}")

# 역행렬
A_inv = linalg.inv(A)
print(f"역행렬:\n{A_inv}")

# 행렬식
det = linalg.det(A)
print(f"행렬식: {det}")

# 고유값과 고유벡터
eigenvalues, eigenvectors = linalg.eig(A)
print(f"고유값: {eigenvalues}")
```

### SVD (특이값 분해)

```python
from scipy import linalg

A = np.array([[1, 2], [3, 4], [5, 6]])

# SVD
U, s, Vt = linalg.svd(A)

print(f"U shape: {U.shape}")
print(f"특이값: {s}")
print(f"Vt shape: {Vt.shape}")
```

---

## 보간 및 근사

### 1D 보간

```python
from scipy import interpolate

# 데이터 포인트
x = np.array([0, 1, 2, 3, 4])
y = np.array([0, 1, 4, 9, 16])

# 선형 보간
f_linear = interpolate.interp1d(x, y)

# 3차 스플라인 보간
f_cubic = interpolate.interp1d(x, y, kind='cubic')

# 새 포인트에서 평가
x_new = np.linspace(0, 4, 100)
y_linear = f_linear(x_new)
y_cubic = f_cubic(x_new)

# 시각화
plt.plot(x, y, 'o', label='Data')
plt.plot(x_new, y_linear, '--', label='Linear')
plt.plot(x_new, y_cubic, '-', label='Cubic')
plt.legend()
plt.show()
```

### 2D 보간

```python
from scipy import interpolate

# 2D 데이터
x = np.linspace(0, 4, 5)
y = np.linspace(0, 4, 5)
X, Y = np.meshgrid(x, y)
Z = np.sin(X) * np.cos(Y)

# 보간 함수 생성
f = interpolate.interp2d(x, y, Z, kind='cubic')

# 더 조밀한 그리드
x_new = np.linspace(0, 4, 50)
y_new = np.linspace(0, 4, 50)
Z_new = f(x_new, y_new)
```

---

## 적분 및 미분

### 수치 적분

```python
from scipy import integrate

# 함수 정의
def f(x):
    return x**2

# 정적분
result, error = integrate.quad(f, 0, 1)
print(f"적분 결과: {result:.6f}")
print(f"오차: {error:.2e}")

# 다중 적분
def f2d(y, x):
    return x * y

result, error = integrate.dblquad(f2d, 0, 1, 0, 1)
print(f"2D 적분: {result:.6f}")
```

### 수치 미분

```python
from scipy.misc import derivative

def f(x):
    return x**3

# x=2에서의 미분
x0 = 2
df = derivative(f, x0, dx=1e-6)
print(f"f'({x0}) = {df:.6f}")
```

---

## 반도체 응용 예제

### 예제 1: 공정 능력 분석

```python
from scipy import stats
import numpy as np

# 측정 데이터
measurements = np.array([
    850.5, 851.2, 849.8, 850.3, 851.0,
    850.7, 849.5, 850.9, 851.3, 850.2
])

# 규격 한계
USL = 852  # Upper Spec Limit
LSL = 848  # Lower Spec Limit

# 통계
mean = np.mean(measurements)
std = np.std(measurements, ddof=1)

# Cp (공정 능력)
Cp = (USL - LSL) / (6 * std)

# Cpk (공정 능력 지수)
Cpu = (USL - mean) / (3 * std)
Cpl = (mean - LSL) / (3 * std)
Cpk = min(Cpu, Cpl)

print(f"=== 공정 능력 분석 ===")
print(f"평균: {mean:.2f}")
print(f"표준편차: {std:.2f}")
print(f"Cp: {Cp:.2f}")
print(f"Cpk: {Cpk:.2f}")

if Cpk >= 1.33:
    print("공정 능력 우수")
elif Cpk >= 1.0:
    print("공정 능력 양호")
else:
    print("공정 능력 부족")
```

### 예제 2: 수율 예측 모델

```python
from scipy import optimize
import numpy as np

# 온도-수율 데이터
temperature = np.array([800, 825, 850, 875, 900, 925])
yield_rate = np.array([94.2, 95.1, 96.3, 96.5, 95.8, 94.5])

# 2차 모델 피팅
def yield_model(T, a, b, c):
    return a + b*T + c*T**2

params, cov = optimize.curve_fit(yield_model, temperature, yield_rate)

# 최적 온도 찾기
def neg_yield(T):
    return -yield_model(T, *params)

result = optimize.minimize_scalar(neg_yield, bounds=(800, 925), method='bounded')
optimal_temp = result.x
max_yield = -result.fun

print(f"최적 온도: {optimal_temp:.1f}°C")
print(f"예상 최대 수율: {max_yield:.2f}%")
```

### 예제 3: 이상치 탐지

```python
from scipy import stats
import numpy as np

# 데이터
wafer_yields = np.array([
    95.5, 96.2, 94.8, 97.1, 95.0, 96.5, 94.3, 95.8,
    92.1, 96.0, 95.7, 96.3, 95.2, 88.5, 96.1
])

# Z-score 계산
z_scores = np.abs(stats.zscore(wafer_yields))

# 이상치 (|Z| > 3)
threshold = 3
outliers = wafer_yields[z_scores > threshold]
outlier_indices = np.where(z_scores > threshold)[0]

print(f"=== 이상치 탐지 ===")
print(f"전체 데이터: {len(wafer_yields)}개")
print(f"이상치: {len(outliers)}개")
print(f"이상치 값: {outliers}")
print(f"이상치 인덱스: {outlier_indices}")

# IQR 방법
Q1 = np.percentile(wafer_yields, 25)
Q3 = np.percentile(wafer_yields, 75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers_iqr = wafer_yields[(wafer_yields < lower_bound) | 
                            (wafer_yields > upper_bound)]

print(f"\nIQR 방법 이상치: {len(outliers_iqr)}개")
print(f"범위: [{lower_bound:.2f}, {upper_bound:.2f}]")
```

### 예제 4: 실험 설계 분석

```python
from scipy import stats

# 실험 데이터 (3가지 온도 조건)
temp_800 = [94.5, 94.8, 94.2, 94.6]
temp_850 = [96.1, 96.3, 95.9, 96.2]
temp_900 = [95.2, 95.5, 95.3, 95.1]

# ANOVA
f_stat, p_value = stats.f_oneway(temp_800, temp_850, temp_900)

print(f"=== ANOVA 결과 ===")
print(f"F-통계량: {f_stat:.4f}")
print(f"p-value: {p_value:.4f}")

if p_value < 0.05:
    print("온도 조건 간 유의미한 차이 존재")
    
    # 사후 검정 (Tukey HSD)
    from scipy.stats import tukey_hsd
    
    all_data = np.concatenate([temp_800, temp_850, temp_900])
    groups = ['800']*4 + ['850']*4 + ['900']*4
    
    result = tukey_hsd(temp_800, temp_850, temp_900)
    print(f"\nTukey HSD 결과:")
    print(result)
```

### 예제 5: 신뢰 구간 계산

```python
from scipy import stats
import numpy as np

# 표본 데이터
sample = np.array([95.5, 96.2, 94.8, 97.1, 95.0, 96.5, 94.3, 95.8])

# 평균과 표준편차
mean = np.mean(sample)
std = np.std(sample, ddof=1)
n = len(sample)

# 95% 신뢰구간
confidence = 0.95
df = n - 1
t_critical = stats.t.ppf((1 + confidence) / 2, df)

margin_error = t_critical * std / np.sqrt(n)
ci_lower = mean - margin_error
ci_upper = mean + margin_error

print(f"=== 신뢰 구간 ===")
print(f"표본 크기: {n}")
print(f"평균: {mean:.2f}")
print(f"표준편차: {std:.2f}")
print(f"95% CI: [{ci_lower:.2f}, {ci_upper:.2f}]")
```

### 예제 6: 상관관계 매트릭스

```python
from scipy import stats
import numpy as np

# 여러 공정 변수
temperature = np.array([800, 825, 850, 875, 900])
pressure = np.array([100, 105, 110, 115, 120])
flow_rate = np.array([50, 52, 54, 56, 58])
yield_rate = np.array([94.5, 95.2, 96.1, 95.8, 95.3])

# 상관계수 매트릭스
data = np.array([temperature, pressure, flow_rate, yield_rate])

corr_matrix = np.corrcoef(data)

print("=== 상관계수 매트릭스 ===")
print("        Temp   Press  Flow   Yield")
labels = ["Temp", "Press", "Flow", "Yield"]
for i, label in enumerate(labels):
    print(f"{label:6s}", end="  ")
    for j in range(4):
        print(f"{corr_matrix[i,j]:6.3f}", end=" ")
    print()
```

---

## 유용한 팁

### 성능 최적화

```python
# NumPy 벡터화 사용
# 느림
result = []
for x in data:
    result.append(x**2)

# 빠름
result = data**2

# SciPy 함수는 이미 최적화됨
from scipy import stats
stats.norm.pdf(data)  # 벡터화됨
```

### 메모리 효율

```python
# 큰 배열 처리 시 dtype 지정
data = np.array(large_list, dtype=np.float32)  # float64 대신
```

---

## 관련 문서

- [[Python 기초 가이드]] - Python 기본 문법
- [[Python 데이터 분석 가이드]] - Pandas & NumPy
- [[Python SQL 연동 가이드]] - 데이터베이스 연동

---

**SciPy로 과학적 분석을 수행하세요!** 🔬
