# Python 데이터 분석 가이드 (Pandas & NumPy)

## 목차
- [[#NumPy 기초]]
- [[#Pandas 기초]]
- [[#데이터 로딩]]
- [[#데이터 탐색]]
- [[#데이터 정제]]
- [[#데이터 변환]]
- [[#데이터 집계]]
- [[#데이터 시각화]]
- [[#반도체 분석 예제]]

---

## NumPy 기초

### NumPy 배열 생성

```python
import numpy as np

# 리스트에서 생성
arr = np.array([1, 2, 3, 4, 5])
arr2d = np.array([[1, 2, 3], [4, 5, 6]])

# 특수 배열
zeros = np.zeros((3, 4))        # 0으로 채움
ones = np.ones((2, 3))          # 1로 채움
full = np.full((2, 3), 7)       # 특정 값으로 채움

# 범위 배열
range_arr = np.arange(0, 10, 2)     # [0, 2, 4, 6, 8]
linspace = np.linspace(0, 1, 5)     # [0, 0.25, 0.5, 0.75, 1]

# 난수
random_arr = np.random.random((3, 3))
random_int = np.random.randint(0, 10, size=(3, 3))
normal = np.random.normal(0, 1, 100)  # 평균 0, 표준편차 1
```

### 배열 연산

```python
# 기본 연산
arr = np.array([1, 2, 3, 4])

print(arr + 10)      # [11, 12, 13, 14]
print(arr * 2)       # [2, 4, 6, 8]
print(arr ** 2)      # [1, 4, 9, 16]

# 수학 함수
print(np.sqrt(arr))  # 제곱근
print(np.exp(arr))   # 지수
print(np.log(arr))   # 로그
```

### 통계 함수

```python
data = np.array([95.5, 96.2, 94.8, 97.1, 95.0])

# 기본 통계
mean = np.mean(data)        # 평균
median = np.median(data)    # 중앙값
std = np.std(data)          # 표준편차
var = np.var(data)          # 분산
min_val = np.min(data)      # 최소값
max_val = np.max(data)      # 최대값

# 백분위수
p25 = np.percentile(data, 25)
p75 = np.percentile(data, 75)
```

---

## Pandas 기초

### Series와 DataFrame

```python
import pandas as pd

# Series 생성
s = pd.Series([1, 2, 3, 4, 5])
s_named = pd.Series([95.5, 96.2, 94.8], 
                    index=["W001", "W002", "W003"])

# DataFrame 생성
df = pd.DataFrame({
    "wafer_id": ["W001", "W002", "W003"],
    "yield": [95.5, 96.2, 94.8],
    "lot_id": ["LOT001", "LOT001", "LOT002"],
    "defects": [5, 3, 8]
})

print(df)
```

### 데이터 로딩

```python
# CSV 읽기
df = pd.read_csv("wafer_data.csv")

# 특정 컬럼만 읽기
df = pd.read_csv("wafer_data.csv", 
                 usecols=["wafer_id", "yield"])

# 구분자 지정
df = pd.read_csv("data.txt", sep="\t")

# 헤더가 없는 경우
df = pd.read_csv("data.csv", header=None,
                 names=["col1", "col2", "col3"])

# Excel 읽기
df = pd.read_excel("wafer_data.xlsx", sheet_name="Sheet1")

# 여러 시트 읽기
dfs = pd.read_excel("data.xlsx", sheet_name=None)

# JSON 읽기
df = pd.read_json("data.json")

# SQL 읽기 (sqlite3 예제)
import sqlite3
conn = sqlite3.connect("database.db")
df = pd.read_sql("SELECT * FROM wafer_data", conn)
```

---

## 데이터 탐색

### 기본 정보

```python
# 처음 5행
df.head()

# 마지막 5행
df.tail()

# 데이터 형태
print(df.shape)  # (행, 열)

# 컬럼 정보
print(df.columns)
print(df.dtypes)

# 전체 정보
df.info()

# 통계 요약
df.describe()

# 특정 컬럼 통계
df["yield"].describe()
```

### 데이터 선택

```python
# 컬럼 선택
wafer_ids = df["wafer_id"]
subset = df[["wafer_id", "yield"]]

# 행 선택 (인덱스)
row = df.iloc[0]        # 첫 번째 행
rows = df.iloc[0:3]     # 처음 3행

# 행 선택 (라벨)
row = df.loc[0]
rows = df.loc[0:2]

# 조건 선택
high_yield = df[df["yield"] >= 95]
lot001 = df[df["lot_id"] == "LOT001"]

# 복합 조건
filtered = df[(df["yield"] >= 95) & (df["defects"] < 5)]

# isin
selected_lots = df[df["lot_id"].isin(["LOT001", "LOT002"])]
```

---

## 데이터 정제

### 결측치 처리

```python
# 결측치 확인
print(df.isnull().sum())
print(df.isna().any())

# 결측치 제거
df_clean = df.dropna()              # 결측치가 있는 행 제거
df_clean = df.dropna(axis=1)        # 결측치가 있는 열 제거
df_clean = df.dropna(subset=["yield"])  # 특정 컬럼 기준

# 결측치 채우기
df_filled = df.fillna(0)            # 0으로 채우기
df_filled = df.fillna(method="ffill")  # 앞 값으로 채우기
df_filled = df.fillna(method="bfill")  # 뒤 값으로 채우기
df_filled = df.fillna(df.mean())    # 평균으로 채우기
```

### 중복 데이터

```python
# 중복 확인
print(df.duplicated().sum())

# 중복 제거
df_unique = df.drop_duplicates()
df_unique = df.drop_duplicates(subset=["wafer_id"])
df_unique = df.drop_duplicates(keep="last")  # 마지막 것만 유지
```

### 데이터 타입 변환

```python
# 타입 변환
df["yield"] = df["yield"].astype(float)
df["wafer_id"] = df["wafer_id"].astype(str)

# 날짜 변환
df["date"] = pd.to_datetime(df["date"])
df["date"] = pd.to_datetime(df["date"], format="%Y-%m-%d")
```

---

## 데이터 변환

### 새 컬럼 생성

```python
# 계산으로 생성
df["yield_squared"] = df["yield"] ** 2
df["good_dies"] = df["total_dies"] * df["yield"] / 100

# 조건부 생성
df["grade"] = df["yield"].apply(
    lambda x: "A" if x >= 98 else "B" if x >= 95 else "C"
)

# np.where 사용
df["status"] = np.where(df["yield"] >= 95, "PASS", "FAIL")

# 여러 조건
conditions = [
    df["yield"] >= 98,
    df["yield"] >= 95,
    df["yield"] >= 90
]
choices = ["A", "B", "C"]
df["grade"] = np.select(conditions, choices, default="D")
```

### 문자열 처리

```python
# 문자열 메서드
df["wafer_id_upper"] = df["wafer_id"].str.upper()
df["wafer_id_lower"] = df["wafer_id"].str.lower()

# 분할
df[["prefix", "number"]] = df["wafer_id"].str.split("-", expand=True)

# 포함 여부
df["is_lot001"] = df["lot_id"].str.contains("LOT001")

# 치환
df["lot_id_clean"] = df["lot_id"].str.replace("LOT", "L")
```

### 날짜/시간 처리

```python
# 날짜 파싱
df["date"] = pd.to_datetime(df["date"])

# 날짜 요소 추출
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
df["dayofweek"] = df["date"].dt.dayofweek

# 날짜 연산
df["next_week"] = df["date"] + pd.Timedelta(days=7)
df["days_ago"] = (pd.Timestamp.now() - df["date"]).dt.days
```

---

## 데이터 집계

### groupby

```python
# 그룹별 평균
lot_avg = df.groupby("lot_id")["yield"].mean()

# 여러 통계
lot_stats = df.groupby("lot_id")["yield"].agg(["mean", "std", "min", "max"])

# 여러 컬럼 집계
summary = df.groupby("lot_id").agg({
    "yield": ["mean", "std"],
    "defects": ["sum", "mean"]
})

# 사용자 정의 함수
def range_func(x):
    return x.max() - x.min()

lot_range = df.groupby("lot_id")["yield"].agg(range_func)

# 그룹별 필터링
good_lots = df.groupby("lot_id").filter(lambda x: x["yield"].mean() >= 95)
```

### pivot_table

```python
# 피벗 테이블
pivot = df.pivot_table(
    values="yield",
    index="lot_id",
    columns="equipment_id",
    aggfunc="mean"
)

# 여러 집계 함수
pivot = df.pivot_table(
    values="yield",
    index="lot_id",
    aggfunc=["mean", "count"]
)
```

### 정렬

```python
# 단일 컬럼 정렬
df_sorted = df.sort_values("yield")
df_sorted = df.sort_values("yield", ascending=False)

# 여러 컬럼 정렬
df_sorted = df.sort_values(["lot_id", "yield"])

# 인덱스 정렬
df_sorted = df.sort_index()
```

---

## 데이터 결합

### merge (조인)

```python
# 두 DataFrame 준비
df1 = pd.DataFrame({
    "wafer_id": ["W001", "W002", "W003"],
    "yield": [95.5, 96.2, 94.8]
})

df2 = pd.DataFrame({
    "wafer_id": ["W001", "W002", "W004"],
    "lot_id": ["LOT001", "LOT001", "LOT002"]
})

# Inner join (기본)
merged = pd.merge(df1, df2, on="wafer_id")

# Left join
merged = pd.merge(df1, df2, on="wafer_id", how="left")

# Outer join
merged = pd.merge(df1, df2, on="wafer_id", how="outer")

# 다른 컬럼명으로 조인
merged = pd.merge(df1, df2, 
                  left_on="id", right_on="wafer_id")
```

### concat

```python
# 세로 방향 결합
df_combined = pd.concat([df1, df2])

# 가로 방향 결합
df_combined = pd.concat([df1, df2], axis=1)

# 인덱스 재설정
df_combined = pd.concat([df1, df2], ignore_index=True)
```

---

## 데이터 시각화

### Matplotlib 기초

```python
import matplotlib.pyplot as plt

# 선 그래프
plt.plot(df["date"], df["yield"])
plt.xlabel("Date")
plt.ylabel("Yield (%)")
plt.title("Yield Trend")
plt.show()

# 산점도
plt.scatter(df["defects"], df["yield"])
plt.xlabel("Defects")
plt.ylabel("Yield (%)")
plt.show()

# 히스토그램
plt.hist(df["yield"], bins=20)
plt.xlabel("Yield (%)")
plt.ylabel("Frequency")
plt.show()

# 막대 그래프
lot_avg = df.groupby("lot_id")["yield"].mean()
plt.bar(lot_avg.index, lot_avg.values)
plt.xlabel("Lot ID")
plt.ylabel("Average Yield (%)")
plt.show()
```

### Pandas 내장 시각화

```python
# 선 그래프
df.plot(x="date", y="yield")

# 여러 컬럼
df[["yield", "defects"]].plot()

# 막대 그래프
df.groupby("lot_id")["yield"].mean().plot(kind="bar")

# 상자 그림
df.boxplot(column="yield", by="lot_id")

# 히스토그램
df["yield"].plot(kind="hist", bins=20)

# 산점도
df.plot(kind="scatter", x="defects", y="yield")
```

---

## 반도체 분석 예제

### 예제 1: 웨이퍼 수율 분석

```python
import pandas as pd
import numpy as np

# 데이터 로드
df = pd.read_csv("wafer_data.csv")

# 기본 통계
print("=== 수율 통계 ===")
print(f"평균: {df['yield'].mean():.2f}%")
print(f"중앙값: {df['yield'].median():.2f}%")
print(f"표준편차: {df['yield'].std():.2f}%")
print(f"최소: {df['yield'].min():.2f}%")
print(f"최대: {df['yield'].max():.2f}%")

# Lot별 분석
lot_summary = df.groupby("lot_id").agg({
    "yield": ["mean", "std", "count"],
    "defects": "sum"
})
print("\n=== Lot별 요약 ===")
print(lot_summary)

# 등급 분류
df["grade"] = pd.cut(df["yield"], 
                     bins=[0, 90, 95, 98, 100],
                     labels=["C", "B", "A", "Premium"])

grade_counts = df["grade"].value_counts()
print("\n=== 등급 분포 ===")
print(grade_counts)
```

### 예제 2: 트렌드 분석

```python
# 날짜별 수율 트렌드
df["date"] = pd.to_datetime(df["date"])
daily_yield = df.groupby("date")["yield"].mean()

# 이동 평균
df_trend = daily_yield.rolling(window=7).mean()

# 시각화
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(daily_yield.index, daily_yield.values, 
         label="Daily Yield", alpha=0.5)
plt.plot(df_trend.index, df_trend.values, 
         label="7-day MA", linewidth=2)
plt.xlabel("Date")
plt.ylabel("Yield (%)")
plt.title("Yield Trend with Moving Average")
plt.legend()
plt.grid(True)
plt.show()
```

### 예제 3: 장비별 성능 비교

```python
# 장비별 통계
equipment_stats = df.groupby("equipment_id").agg({
    "wafer_id": "count",
    "yield": ["mean", "std"],
    "defects": "sum"
}).round(2)

equipment_stats.columns = ["Count", "Avg_Yield", "Std_Yield", "Total_Defects"]

# 성능 순위
equipment_stats["Rank"] = equipment_stats["Avg_Yield"].rank(ascending=False)

print(equipment_stats.sort_values("Rank"))

# 시각화
equipment_stats.plot(kind="bar", y="Avg_Yield", 
                     title="Equipment Performance")
plt.ylabel("Average Yield (%)")
plt.xticks(rotation=45)
plt.show()
```

### 예제 4: 불량 파레토 분석

```python
# 불량 유형별 집계
defect_summary = df.groupby("defect_type")["defect_count"].sum()
defect_summary = defect_summary.sort_values(ascending=False)

# 누적 비율 계산
cumsum = defect_summary.cumsum()
cum_percent = 100 * cumsum / defect_summary.sum()

# 파레토 차트
fig, ax1 = plt.subplots(figsize=(12, 6))

ax1.bar(range(len(defect_summary)), defect_summary.values)
ax1.set_xlabel("Defect Type")
ax1.set_ylabel("Count")
ax1.set_xticks(range(len(defect_summary)))
ax1.set_xticklabels(defect_summary.index, rotation=45)

ax2 = ax1.twinx()
ax2.plot(range(len(cum_percent)), cum_percent.values, 
         color="red", marker="o")
ax2.set_ylabel("Cumulative %")
ax2.axhline(y=80, color="green", linestyle="--")

plt.title("Defect Pareto Analysis")
plt.tight_layout()
plt.show()
```

### 예제 5: 상관관계 분석

```python
# 상관계수 계산
correlation = df[["yield", "defects", "temperature", "pressure"]].corr()

print("=== 상관계수 행렬 ===")
print(correlation)

# 히트맵
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 6))
plt.imshow(correlation, cmap="coolwarm", aspect="auto")
plt.colorbar()
plt.xticks(range(len(correlation.columns)), correlation.columns, rotation=45)
plt.yticks(range(len(correlation.columns)), correlation.columns)
plt.title("Correlation Heatmap")

# 값 표시
for i in range(len(correlation)):
    for j in range(len(correlation)):
        plt.text(j, i, f"{correlation.iloc[i, j]:.2f}",
                ha="center", va="center")

plt.tight_layout()
plt.show()
```

### 예제 6: 이상치 탐지

```python
# Z-score 방법
from scipy import stats

df["z_score"] = np.abs(stats.zscore(df["yield"]))
outliers = df[df["z_score"] > 3]

print(f"이상치 개수: {len(outliers)}")
print(outliers[["wafer_id", "yield", "z_score"]])

# IQR 방법
Q1 = df["yield"].quantile(0.25)
Q3 = df["yield"].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers_iqr = df[(df["yield"] < lower_bound) | 
                  (df["yield"] > upper_bound)]

print(f"\nIQR 방법 이상치: {len(outliers_iqr)}")
```

### 예제 7: 시계열 분석

```python
# 날짜 인덱스 설정
df_ts = df.set_index("date")
df_ts = df_ts.sort_index()

# 리샘플링 (주간 평균)
weekly = df_ts["yield"].resample("W").mean()

# 변화율
df_ts["yield_change"] = df_ts["yield"].pct_change()

# 추세 분석
from scipy import stats
x = np.arange(len(df_ts))
slope, intercept, r_value, p_value, std_err = stats.linregress(
    x, df_ts["yield"]
)

print(f"추세 기울기: {slope:.4f}")
print(f"R²: {r_value**2:.4f}")
```

---

## 유용한 팁

### 성능 최적화

```python
# 메모리 사용량 확인
print(df.memory_usage(deep=True))

# 데이터 타입 최적화
df["wafer_id"] = df["wafer_id"].astype("category")
df["yield"] = df["yield"].astype("float32")

# chunk로 큰 파일 읽기
chunks = pd.read_csv("large_file.csv", chunksize=10000)
for chunk in chunks:
    process(chunk)

# 특정 컬럼만 읽기
df = pd.read_csv("data.csv", usecols=["col1", "col2"])
```

### 데이터 저장

```python
# CSV로 저장
df.to_csv("output.csv", index=False)

# Excel로 저장
df.to_excel("output.xlsx", index=False)

# 여러 시트로 저장
with pd.ExcelWriter("output.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sheet1")
    df2.to_excel(writer, sheet_name="Sheet2")

# Pickle로 저장 (빠름)
df.to_pickle("data.pkl")
df_loaded = pd.read_pickle("data.pkl")
```

---

## 관련 문서

- [[Python 기초 가이드]] - Python 기본 문법
- [[SciPy 가이드]] - 과학 계산 및 통계
- [[Python + SQL 연동 가이드]] - 데이터베이스 연동

---

**Pandas와 NumPy로 데이터를 마스터하세요!** 📊
