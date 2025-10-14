# Python + SQL 연동 가이드 (bigdataquery)

## 목차
- [[#bigdataquery 소개]]
- [[#연결 및 기본 사용]]
- [[#쿼리 실행]]
- [[#Pandas 연동]]
- [[#데이터 처리 파이프라인]]
- [[#실전 예제]]

---

## bigdataquery 소개

### bigdataquery란?

bigdataquery는 Impala SQL을 Python에서 사용하기 위한 사내 모듈입니다.

**주요 기능:**
- Impala 데이터베이스 연결
- SQL 쿼리 실행
- Pandas DataFrame 변환
- 대용량 데이터 처리

### 설치 및 import

```python
# bigdataquery import
from bigdataquery import BigDataQuery

# 함께 사용하는 라이브러리
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
```

---

## 연결 및 기본 사용

### 기본 연결

```python
from bigdataquery import BigDataQuery

# BigDataQuery 객체 생성
bdq = BigDataQuery()

# 연결 확인
print("Impala 연결 성공!")
```

### 기본 쿼리 실행

```python
# 간단한 쿼리
query = "SELECT * FROM wafer_data LIMIT 10"
result = bdq.execute(query)

print(result)
```

---

## 쿼리 실행

### SELECT 쿼리

```python
from bigdataquery import BigDataQuery

bdq = BigDataQuery()

# 기본 SELECT
query = """
SELECT 
    wafer_id,
    lot_id,
    yield,
    process_date
FROM wafer_data
WHERE process_date >= '2025-10-01'
LIMIT 100
"""

result = bdq.execute(query)
print(result)
```

### 파라미터화된 쿼리

```python
# 날짜 파라미터
start_date = '2025-10-01'
end_date = '2025-10-15'

query = f"""
SELECT 
    COUNT(*) as total_wafers,
    AVG(yield) as avg_yield
FROM wafer_data
WHERE process_date BETWEEN '{start_date}' AND '{end_date}'
"""

result = bdq.execute(query)
print(f"전체 웨이퍼: {result}")
```

### 집계 쿼리

```python
# Lot별 통계
query = """
SELECT 
    lot_id,
    COUNT(*) as wafer_count,
    AVG(yield) as avg_yield,
    MIN(yield) as min_yield,
    MAX(yield) as max_yield
FROM wafer_data
WHERE process_date >= '2025-10-01'
GROUP BY lot_id
ORDER BY avg_yield DESC
"""

result = bdq.execute(query)
print(result)
```

---

## Pandas 연동

### DataFrame으로 변환

```python
from bigdataquery import BigDataQuery
import pandas as pd

bdq = BigDataQuery()

# 쿼리 실행 및 DataFrame 변환
query = """
SELECT 
    wafer_id,
    lot_id,
    yield,
    defect_count,
    process_date
FROM wafer_data
WHERE process_date >= '2025-10-01'
"""

# DataFrame으로 직접 받기
df = bdq.execute_to_df(query)

# 또는 execute 후 변환
result = bdq.execute(query)
df = pd.DataFrame(result)

print(df.head())
print(f"\n데이터 크기: {df.shape}")
```

### DataFrame 기본 분석

```python
# 기본 정보
print("=== 데이터 정보 ===")
print(df.info())

# 통계 요약
print("\n=== 통계 요약 ===")
print(df.describe())

# 결측치 확인
print("\n=== 결측치 ===")
print(df.isnull().sum())
```

---

## 데이터 처리 파이프라인

### ETL 파이프라인

```python
from bigdataquery import BigDataQuery
import pandas as pd

def extract_data(start_date, end_date):
    """Extract: Impala에서 데이터 추출"""
    bdq = BigDataQuery()
    
    query = f"""
    SELECT 
        wafer_id,
        lot_id,
        yield,
        defect_count,
        equipment_id,
        process_date
    FROM wafer_data
    WHERE process_date BETWEEN '{start_date}' AND '{end_date}'
    """
    
    df = bdq.execute_to_df(query)
    print(f"추출 완료: {len(df)}행")
    return df

def transform_data(df):
    """Transform: 데이터 변환"""
    # 결측치 처리
    df = df.fillna({'defect_count': 0})
    
    # 등급 계산
    df['grade'] = df['yield'].apply(
        lambda x: 'A' if x >= 98 else 'B' if x >= 95 else 'C'
    )
    
    # 날짜 타입 변환
    df['process_date'] = pd.to_datetime(df['process_date'])
    
    print(f"변환 완료: {len(df)}행")
    return df

def load_data(df, output_file):
    """Load: 결과 저장"""
    df.to_csv(output_file, index=False, encoding='utf-8-sig')
    print(f"저장 완료: {output_file}")

# ETL 실행
if __name__ == "__main__":
    # Extract
    df = extract_data('2025-10-01', '2025-10-15')
    
    # Transform
    df = transform_data(df)
    
    # Load
    load_data(df, 'processed_data.csv')
    
    print("\nETL 완료!")
```

---

## 실전 예제

### 예제 1: 일일 수율 보고서

```python
from bigdataquery import BigDataQuery
import pandas as pd
from datetime import datetime

def daily_yield_report(target_date):
    """일일 수율 보고서 생성"""
    
    bdq = BigDataQuery()
    
    # 쿼리
    query = f"""
    SELECT 
        lot_id,
        COUNT(*) as wafer_count,
        ROUND(AVG(yield), 2) as avg_yield,
        ROUND(STDDEV(yield), 2) as std_yield,
        ROUND(MIN(yield), 2) as min_yield,
        ROUND(MAX(yield), 2) as max_yield,
        SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) as pass_count
    FROM wafer_data
    WHERE process_date = '{target_date}'
    GROUP BY lot_id
    ORDER BY avg_yield DESC
    """
    
    df = bdq.execute_to_df(query)
    
    # Pass Rate 계산
    df['pass_rate'] = (df['pass_count'] / df['wafer_count'] * 100).round(2)
    
    # 결과 출력
    print(f"\n=== {target_date} 수율 보고서 ===\n")
    print(df.to_string(index=False))
    
    # CSV 저장
    output_file = f"yield_report_{target_date}.csv"
    df.to_csv(output_file, index=False, encoding='utf-8-sig')
    print(f"\n보고서 저장: {output_file}")
    
    return df

# 실행
today = datetime.now().strftime('%Y-%m-%d')
report = daily_yield_report(today)
```

### 예제 2: 장비별 성능 분석

```python
from bigdataquery import BigDataQuery
import pandas as pd

def equipment_performance(days=30):
    """장비별 성능 분석"""
    
    bdq = BigDataQuery()
    
    query = f"""
    SELECT 
        e.equipment_name,
        e.equipment_type,
        COUNT(*) as processed_wafers,
        ROUND(AVG(w.yield), 2) as avg_yield,
        ROUND(STDDEV(w.yield), 2) as std_yield,
        COUNT(DISTINCT w.lot_id) as lots_handled,
        COUNT(DISTINCT DATE(w.process_date)) as active_days
    FROM wafer_data w
    INNER JOIN equipment_master e ON w.equipment_id = e.equipment_id
    WHERE w.process_date >= DATE_SUB(NOW(), INTERVAL {days} DAY)
    GROUP BY e.equipment_name, e.equipment_type
    HAVING processed_wafers >= 100
    ORDER BY avg_yield DESC
    """
    
    df = bdq.execute_to_df(query)
    
    # 일일 평균 처리량
    df['daily_throughput'] = (df['processed_wafers'] / df['active_days']).round(2)
    
    # 성능 등급
    df['performance'] = df['avg_yield'].apply(
        lambda x: 'Excellent' if x >= 98 else 'Good' if x >= 95 else 'Fair'
    )
    
    print("\n=== 장비별 성능 분석 ===\n")
    print(df.to_string(index=False))
    
    return df

# 실행
equipment_stats = equipment_performance(days=30)
```

### 예제 3: 트렌드 분석

```python
from bigdataquery import BigDataQuery
import pandas as pd
import matplotlib.pyplot as plt

def yield_trend_analysis(days=90):
    """수율 트렌드 분석"""
    
    bdq = BigDataQuery()
    
    # 일별 수율 데이터
    query = f"""
    SELECT 
        process_date,
        COUNT(*) as wafer_count,
        AVG(yield) as avg_yield,
        STDDEV(yield) as std_yield
    FROM wafer_data
    WHERE process_date >= DATE_SUB(NOW(), INTERVAL {days} DAY)
    GROUP BY process_date
    ORDER BY process_date
    """
    
    df = bdq.execute_to_df(query)
    df['process_date'] = pd.to_datetime(df['process_date'])
    
    # 이동 평균 (7일)
    df['ma_7'] = df['avg_yield'].rolling(window=7).mean()
    
    # 시각화
    plt.figure(figsize=(14, 6))
    
    plt.subplot(1, 2, 1)
    plt.plot(df['process_date'], df['avg_yield'], 
             alpha=0.5, label='Daily Yield')
    plt.plot(df['process_date'], df['ma_7'], 
             linewidth=2, label='7-day MA')
    plt.xlabel('Date')
    plt.ylabel('Yield (%)')
    plt.title('Yield Trend')
    plt.legend()
    plt.grid(True)
    plt.xticks(rotation=45)
    
    plt.subplot(1, 2, 2)
    plt.bar(df['process_date'], df['wafer_count'])
    plt.xlabel('Date')
    plt.ylabel('Wafer Count')
    plt.title('Daily Production')
    plt.xticks(rotation=45)
    
    plt.tight_layout()
    plt.savefig('yield_trend.png', dpi=300)
    print("트렌드 차트 저장: yield_trend.png")
    plt.show()
    
    return df

# 실행
trend_data = yield_trend_analysis(days=90)
```

### 예제 4: 대량 데이터 처리

```python
from bigdataquery import BigDataQuery
import pandas as pd

def process_large_dataset(start_date, end_date, chunk_days=7):
    """대량 데이터를 기간별로 나눠서 처리"""
    
    bdq = BigDataQuery()
    
    # 날짜 범위 생성
    date_range = pd.date_range(start=start_date, end=end_date, freq=f'{chunk_days}D')
    
    all_results = []
    
    for i in range(len(date_range) - 1):
        chunk_start = date_range[i].strftime('%Y-%m-%d')
        chunk_end = date_range[i+1].strftime('%Y-%m-%d')
        
        print(f"처리 중: {chunk_start} ~ {chunk_end}")
        
        query = f"""
        SELECT 
            lot_id,
            AVG(yield) as avg_yield,
            COUNT(*) as wafer_count
        FROM wafer_data
        WHERE process_date >= '{chunk_start}'
          AND process_date < '{chunk_end}'
        GROUP BY lot_id
        """
        
        df = bdq.execute_to_df(query)
        all_results.append(df)
    
    # 결합
    final_df = pd.concat(all_results, ignore_index=True)
    
    # Lot별 재집계
    summary = final_df.groupby('lot_id').agg({
        'wafer_count': 'sum',
        'avg_yield': 'mean'
    }).reset_index()
    
    print(f"\n총 처리 완료: {len(summary)} Lot")
    return summary

# 실행
result = process_large_dataset('2025-01-01', '2025-10-15', chunk_days=7)
```

### 예제 5: 실시간 모니터링

```python
from bigdataquery import BigDataQuery
import pandas as pd
import time
from datetime import datetime

def monitor_yield(threshold=95.0, interval=300):
    """실시간 수율 모니터링 (5분 간격)"""
    
    bdq = BigDataQuery()
    
    print("수율 모니터링 시작...")
    print(f"경고 기준: {threshold}%")
    print(f"확인 간격: {interval}초\n")
    
    while True:
        try:
            # 최근 1시간 데이터
            query = """
            SELECT 
                COUNT(*) as count,
                AVG(yield) as avg_yield,
                MIN(yield) as min_yield
            FROM wafer_data
            WHERE process_time >= DATE_SUB(NOW(), INTERVAL 1 HOUR)
            """
            
            df = bdq.execute_to_df(query)
            
            if len(df) > 0 and df['count'][0] > 0:
                avg_yield = df['avg_yield'][0]
                min_yield = df['min_yield'][0]
                count = df['count'][0]
                
                timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
                print(f"[{timestamp}] 처리: {count}개, 평균: {avg_yield:.2f}%, 최소: {min_yield:.2f}%")
                
                # 경고 조건
                if avg_yield < threshold:
                    print(f"  ⚠️ 경고: 평균 수율이 {threshold}% 미만입니다!")
                    send_alert(f"수율 경고: {avg_yield:.2f}%")
            
            time.sleep(interval)
            
        except KeyboardInterrupt:
            print("\n모니터링 종료")
            break
        except Exception as e:
            print(f"에러 발생: {e}")
            time.sleep(interval)

def send_alert(message):
    """알림 전송 (이메일, Slack 등)"""
    print(f"  📧 알림: {message}")
    # 실제 알림 로직 구현

# 실행 (Ctrl+C로 종료)
# monitor_yield(threshold=95.0, interval=300)
```

### 예제 6: 배치 리포트 생성

```python
from bigdataquery import BigDataQuery
import pandas as pd
from datetime import datetime, timedelta

def generate_weekly_report():
    """주간 종합 보고서 생성"""
    
    bdq = BigDataQuery()
    
    # 지난 주 날짜
    today = datetime.now()
    last_week_end = today - timedelta(days=today.weekday() + 1)
    last_week_start = last_week_end - timedelta(days=6)
    
    start_date = last_week_start.strftime('%Y-%m-%d')
    end_date = last_week_end.strftime('%Y-%m-%d')
    
    print(f"=== 주간 보고서 ({start_date} ~ {end_date}) ===\n")
    
    # 1. 전체 요약
    query1 = f"""
    SELECT 
        COUNT(DISTINCT lot_id) as total_lots,
        COUNT(*) as total_wafers,
        ROUND(AVG(yield), 2) as avg_yield,
        ROUND(STDDEV(yield), 2) as std_yield,
        SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) as pass_count
    FROM wafer_data
    WHERE process_date BETWEEN '{start_date}' AND '{end_date}'
    """
    
    summary = bdq.execute_to_df(query1)
    summary['pass_rate'] = (summary['pass_count'] / summary['total_wafers'] * 100).round(2)
    
    print("1. 전체 요약")
    print(summary.to_string(index=False))
    
    # 2. 일별 트렌드
    query2 = f"""
    SELECT 
        process_date,
        COUNT(*) as wafer_count,
        ROUND(AVG(yield), 2) as avg_yield
    FROM wafer_data
    WHERE process_date BETWEEN '{start_date}' AND '{end_date}'
    GROUP BY process_date
    ORDER BY process_date
    """
    
    daily = bdq.execute_to_df(query2)
    
    print("\n2. 일별 트렌드")
    print(daily.to_string(index=False))
    
    # 3. TOP/BOTTOM Lot
    query3 = f"""
    WITH lot_stats AS (
        SELECT 
            lot_id,
            COUNT(*) as wafer_count,
            AVG(yield) as avg_yield
        FROM wafer_data
        WHERE process_date BETWEEN '{start_date}' AND '{end_date}'
        GROUP BY lot_id
        HAVING wafer_count >= 10
    )
    SELECT * FROM (
        SELECT 'TOP' as category, lot_id, wafer_count, ROUND(avg_yield, 2) as avg_yield
        FROM lot_stats
        ORDER BY avg_yield DESC
        LIMIT 5
    )
    UNION ALL
    SELECT * FROM (
        SELECT 'BOTTOM' as category, lot_id, wafer_count, ROUND(avg_yield, 2) as avg_yield
        FROM lot_stats
        ORDER BY avg_yield ASC
        LIMIT 5
    )
    """
    
    top_bottom = bdq.execute_to_df(query3)
    
    print("\n3. TOP/BOTTOM Lot")
    print(top_bottom.to_string(index=False))
    
    # Excel 저장
    output_file = f"weekly_report_{start_date}_{end_date}.xlsx"
    with pd.ExcelWriter(output_file, engine='openpyxl') as writer:
        summary.to_excel(writer, sheet_name='Summary', index=False)
        daily.to_excel(writer, sheet_name='Daily', index=False)
        top_bottom.to_excel(writer, sheet_name='TopBottom', index=False)
    
    print(f"\n보고서 저장: {output_file}")

# 실행
generate_weekly_report()
```

---

## 유용한 팁

### 쿼리 최적화

```python
from bigdataquery import BigDataQuery

bdq = BigDataQuery()

# 좋음: 필요한 컬럼만 선택
query_good = """
SELECT wafer_id, yield, lot_id
FROM wafer_data
WHERE process_date >= '2025-10-01'
"""

# 나쁨: SELECT *
query_bad = """
SELECT *
FROM wafer_data
WHERE process_date >= '2025-10-01'
"""

# 좋음: LIMIT 사용
query_test = """
SELECT * FROM wafer_data
LIMIT 100
"""
```

### 에러 처리

```python
from bigdataquery import BigDataQuery
import pandas as pd

def safe_query(query):
    """에러 처리가 포함된 쿼리 함수"""
    try:
        bdq = BigDataQuery()
        df = bdq.execute_to_df(query)
        return df
    
    except Exception as e:
        print(f"쿼리 실행 에러: {e}")
        print(f"쿼리: {query}")
        return None

# 사용
df = safe_query("SELECT * FROM wafer_data LIMIT 10")
if df is not None:
    print(df.head())
```

### 연결 재사용

```python
from bigdataquery import BigDataQuery

# BigDataQuery 객체를 한 번만 생성
bdq = BigDataQuery()

# 여러 쿼리 실행
df1 = bdq.execute_to_df("SELECT * FROM table1 LIMIT 10")
df2 = bdq.execute_to_df("SELECT * FROM table2 LIMIT 10")
df3 = bdq.execute_to_df("SELECT * FROM table3 LIMIT 10")
```

---

## 성능 최적화

### 파티션 활용

```python
# 좋음: 파티션 컬럼 필터
query = """
SELECT *
FROM wafer_data
WHERE year = 2025 AND month = 10
"""

# 나쁨: 함수 사용
query = """
SELECT *
FROM wafer_data
WHERE YEAR(process_date) = 2025
"""
```

### 대용량 데이터 처리

```python
from bigdataquery import BigDataQuery
import pandas as pd

def process_in_chunks(query, chunk_size=10000):
    """청크 단위로 데이터 처리"""
    bdq = BigDataQuery()
    
    # LIMIT과 OFFSET 사용
    offset = 0
    all_data = []
    
    while True:
        chunk_query = f"{query} LIMIT {chunk_size} OFFSET {offset}"
        df = bdq.execute_to_df(chunk_query)
        
        if len(df) == 0:
            break
        
        # 청크 처리
        processed = process_chunk(df)
        all_data.append(processed)
        
        offset += chunk_size
        print(f"처리 완료: {offset}행")
    
    return pd.concat(all_data, ignore_index=True)

def process_chunk(df):
    """청크 데이터 처리"""
    # 여기에 처리 로직 구현
    return df
```

---

## 관련 문서

- [[Python 기초 가이드]] - Python 기본 문법
- [[Python 데이터 분석 가이드]] - Pandas & NumPy
- [[Impala SQL 가이드]] - SQL 쿼리 작성
- [[Impala SQL 실전 예제 모음]] - SQL 예제

---

**bigdataquery로 Impala 데이터를 효율적으로 활용하세요!** 🔗
