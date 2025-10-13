# Impala SQL 실전 예제 모음

## 목차
- [[#웨이퍼 데이터 분석]]
- [[#수율 분석]]
- [[#장비 성능 분석]]
- [[#불량 분석]]
- [[#공정 분석]]
- [[#트렌드 분석]]
- [[#통계 분석]]
- [[#보고서 쿼리]]

---

## 웨이퍼 데이터 분석

### 예제 1: 기본 웨이퍼 정보 조회

```sql
-- 특정 Lot의 모든 웨이퍼 조회
SELECT 
    wafer_id,
    lot_id,
    process_date,
    equipment_id,
    yield,
    defect_count,
    status
FROM wafer_data
WHERE lot_id = 'LOT20251014001'
ORDER BY wafer_id;
```

### 예제 2: 최근 처리된 웨이퍼

```sql
-- 최근 24시간 내 처리된 웨이퍼
SELECT 
    wafer_id,
    lot_id,
    equipment_id,
    yield,
    process_date
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 1 DAY)
ORDER BY process_date DESC
LIMIT 100;
```

### 예제 3: 웨이퍼 품질 등급

```sql
-- 수율에 따른 품질 등급 분류
SELECT 
    wafer_id,
    lot_id,
    yield,
    CASE 
        WHEN yield >= 99 THEN 'Premium'
        WHEN yield >= 98 THEN 'Grade A'
        WHEN yield >= 95 THEN 'Grade B'
        WHEN yield >= 90 THEN 'Grade C'
        ELSE 'Reject'
    END AS quality_grade
FROM wafer_data
WHERE process_date >= '2025-10-01'
ORDER BY yield DESC;
```

---

## 수율 분석

### 예제 4: 일별 평균 수율

```sql
-- 최근 30일 일별 수율 트렌드
SELECT 
    process_date,
    COUNT(*) AS total_wafers,
    ROUND(AVG(yield), 2) AS avg_yield,
    ROUND(STDDEV(yield), 2) AS std_yield
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY process_date
ORDER BY process_date DESC;
```

### 예제 5: Lot별 수율 비교

```sql
-- Lot별 수율 통계 및 순위
SELECT 
    lot_id,
    COUNT(*) AS wafer_count,
    ROUND(AVG(yield), 2) AS avg_yield,
    RANK() OVER (ORDER BY AVG(yield) DESC) AS yield_rank
FROM wafer_data
WHERE process_date >= '2025-10-01'
GROUP BY lot_id
HAVING COUNT(*) >= 20
ORDER BY avg_yield DESC;
```

### 예제 6: 제품별 수율 분석

```sql
-- 제품 타입별 수율 비교
SELECT 
    l.product_type,
    COUNT(*) AS total_wafers,
    ROUND(AVG(w.yield), 2) AS avg_yield,
    SUM(CASE WHEN w.yield >= 95 THEN 1 ELSE 0 END) AS high_yield_count,
    ROUND(100.0 * SUM(CASE WHEN w.yield >= 95 THEN 1 ELSE 0 END) / COUNT(*), 2) AS high_yield_rate
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
WHERE w.process_date >= '2025-10-01'
GROUP BY l.product_type
ORDER BY avg_yield DESC;
```

---

## 장비 성능 분석

### 예제 7: 장비별 처리량 및 수율

```sql
-- 장비별 성능 종합 분석
SELECT 
    e.equipment_id,
    e.equipment_name,
    COUNT(*) AS processed_wafers,
    ROUND(AVG(w.yield), 2) AS avg_yield,
    ROUND(STDDEV(w.yield), 2) AS std_yield
FROM wafer_data w
INNER JOIN equipment_master e ON w.equipment_id = e.equipment_id
WHERE w.process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY e.equipment_id, e.equipment_name
HAVING COUNT(*) >= 100
ORDER BY avg_yield DESC;
```

### 예제 8: 장비 가동률

```sql
-- 장비별 가동 시간 및 가동률
SELECT 
    equipment_id,
    COUNT(DISTINCT process_date) AS active_days,
    DATEDIFF(MAX(process_date), MIN(process_date)) + 1 AS total_days,
    ROUND(100.0 * COUNT(DISTINCT process_date) / 
        (DATEDIFF(MAX(process_date), MIN(process_date)) + 1), 2) AS utilization_rate
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY equipment_id
ORDER BY utilization_rate DESC;
```

---

## 불량 분석

### 예제 9: 불량 유형별 발생 빈도

```sql
-- TOP 10 불량 유형
SELECT 
    defect_type,
    COUNT(DISTINCT wafer_id) AS affected_wafers,
    SUM(defect_count) AS total_defects,
    ROUND(AVG(defect_count), 2) AS avg_defects
FROM defect_data
WHERE detection_date >= '2025-10-01'
GROUP BY defect_type
ORDER BY total_defects DESC
LIMIT 10;
```

### 예제 10: 불량 파레토 분석

```sql
-- 불량 파레토 차트용 데이터
WITH defect_summary AS (
    SELECT 
        defect_type,
        SUM(defect_count) AS total_defects
    FROM defect_data
    WHERE detection_date >= '2025-10-01'
    GROUP BY defect_type
)
SELECT 
    defect_type,
    total_defects,
    ROUND(100.0 * total_defects / SUM(total_defects) OVER (), 2) AS percentage,
    ROUND(100.0 * SUM(total_defects) OVER (ORDER BY total_defects DESC) / 
        SUM(total_defects) OVER (), 2) AS cumulative_percentage
FROM defect_summary
ORDER BY total_defects DESC;
```

---

## 트렌드 분석

### 예제 11: 이동 평균 (7일)

```sql
-- 7일 이동 평균 수율
WITH daily_yield AS (
    SELECT 
        process_date,
        AVG(yield) AS avg_yield
    FROM wafer_data
    WHERE process_date >= DATE_SUB(NOW(), INTERVAL 90 DAY)
    GROUP BY process_date
)
SELECT 
    process_date,
    ROUND(avg_yield, 2) AS daily_yield,
    ROUND(AVG(avg_yield) OVER (
        ORDER BY process_date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ), 2) AS moving_avg_7day
FROM daily_yield
ORDER BY process_date DESC;
```

### 예제 12: 전일 대비 변화

```sql
-- 일별 수율 변화
WITH daily_stats AS (
    SELECT 
        process_date,
        AVG(yield) AS avg_yield
    FROM wafer_data
    GROUP BY process_date
)
SELECT 
    process_date,
    ROUND(avg_yield, 2) AS today_yield,
    ROUND(LAG(avg_yield) OVER (ORDER BY process_date), 2) AS yesterday_yield,
    ROUND(avg_yield - LAG(avg_yield) OVER (ORDER BY process_date), 2) AS daily_change
FROM daily_stats
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
ORDER BY process_date DESC;
```

### 예제 13: 주간 비교

```sql
-- 주간 수율 비교
SELECT 
    YEAR(process_date) AS year,
    WEEKOFYEAR(process_date) AS week,
    COUNT(*) AS wafer_count,
    ROUND(AVG(yield), 2) AS avg_yield,
    ROUND(MIN(yield), 2) AS min_yield,
    ROUND(MAX(yield), 2) AS max_yield
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 90 DAY)
GROUP BY YEAR(process_date), WEEKOFYEAR(process_date)
ORDER BY year DESC, week DESC;
```

---

## 통계 분석

### 예제 14: 기술 통계량

```sql
-- 수율 기술 통계
SELECT 
    COUNT(*) AS sample_size,
    ROUND(AVG(yield), 2) AS mean,
    ROUND(STDDEV(yield), 2) AS std_dev,
    ROUND(MIN(yield), 2) AS min_value,
    ROUND(MAX(yield), 2) AS max_value,
    ROUND(MAX(yield) - MIN(yield), 2) AS range_value
FROM wafer_data
WHERE process_date >= '2025-10-01';
```

### 예제 15: 백분위수

```sql
-- 수율 사분위수
WITH ranked_data AS (
    SELECT 
        yield,
        ROW_NUMBER() OVER (ORDER BY yield) AS row_num,
        COUNT(*) OVER () AS total_count
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
)
SELECT 
    'Q1 (25%)' AS percentile,
    ROUND(AVG(yield), 2) AS value
FROM ranked_data
WHERE row_num = CAST(total_count * 0.25 AS INT)
UNION ALL
SELECT 
    'Q2 (50% - Median)' AS percentile,
    ROUND(AVG(yield), 2) AS value
FROM ranked_data
WHERE row_num = CAST(total_count * 0.50 AS INT)
UNION ALL
SELECT 
    'Q3 (75%)' AS percentile,
    ROUND(AVG(yield), 2) AS value
FROM ranked_data
WHERE row_num = CAST(total_count * 0.75 AS INT);
```

### 예제 16: 이상치 탐지 (3-sigma)

```sql
-- 3-sigma 이상치 탐지
WITH stats AS (
    SELECT 
        AVG(yield) AS mean,
        STDDEV(yield) AS std_dev
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
)
SELECT 
    w.wafer_id,
    w.lot_id,
    w.yield,
    ROUND((w.yield - s.mean) / s.std_dev, 2) AS z_score,
    CASE 
        WHEN ABS(w.yield - s.mean) > 3 * s.std_dev THEN 'Outlier'
        ELSE 'Normal'
    END AS status
FROM wafer_data w, stats s
WHERE w.process_date >= '2025-10-01'
  AND ABS(w.yield - s.mean) > 3 * s.std_dev
ORDER BY ABS(w.yield - s.mean) DESC;
```

### 예제 17: 공정 능력 지수 (Cp/Cpk)

```sql
-- 공정 능력 지수 계산
WITH stats AS (
    SELECT 
        AVG(yield) AS mean_yield,
        STDDEV(yield) AS std_yield,
        100.0 AS usl,  -- Upper Spec Limit
        90.0 AS lsl    -- Lower Spec Limit
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
)
SELECT 
    ROUND(mean_yield, 2) AS mean,
    ROUND(std_yield, 2) AS std_dev,
    ROUND((usl - lsl) / (6 * std_yield), 2) AS cp,
    ROUND(LEAST(
        (usl - mean_yield) / (3 * std_yield),
        (mean_yield - lsl) / (3 * std_yield)
    ), 2) AS cpk
FROM stats;
```

---

## 보고서 쿼리

### 예제 18: 일일 생산 보고서

```sql
-- 일일 생산 요약 보고서
SELECT 
    process_date,
    COUNT(DISTINCT lot_id) AS lots_processed,
    COUNT(*) AS wafers_processed,
    ROUND(AVG(yield), 2) AS avg_yield,
    SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) AS pass_count,
    SUM(CASE WHEN yield < 95 THEN 1 ELSE 0 END) AS fail_count,
    ROUND(100.0 * SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) / COUNT(*), 2) AS pass_rate,
    COUNT(DISTINCT equipment_id) AS equipments_used
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
GROUP BY process_date
ORDER BY process_date DESC;
```

### 예제 19: 주간 품질 보고서

```sql
-- 주간 품질 종합 보고서
WITH weekly_data AS (
    SELECT 
        YEAR(process_date) AS year,
        WEEKOFYEAR(process_date) AS week,
        wafer_id,
        lot_id,
        yield,
        defect_count
    FROM wafer_data
    WHERE process_date >= DATE_SUB(NOW(), INTERVAL 8 WEEK)
)
SELECT 
    year,
    week,
    COUNT(*) AS total_wafers,
    COUNT(DISTINCT lot_id) AS unique_lots,
    ROUND(AVG(yield), 2) AS avg_yield,
    ROUND(STDDEV(yield), 2) AS std_yield,
    SUM(defect_count) AS total_defects,
    ROUND(AVG(defect_count), 2) AS avg_defects_per_wafer,
    SUM(CASE WHEN yield >= 99 THEN 1 ELSE 0 END) AS premium_count,
    ROUND(100.0 * SUM(CASE WHEN yield >= 99 THEN 1 ELSE 0 END) / COUNT(*), 2) AS premium_rate
FROM weekly_data
GROUP BY year, week
ORDER BY year DESC, week DESC;
```

### 예제 20: 장비 효율 보고서

```sql
-- 장비별 효율 보고서
SELECT 
    e.equipment_name,
    e.equipment_type,
    COUNT(*) AS total_processed,
    ROUND(AVG(w.yield), 2) AS avg_yield,
    COUNT(DISTINCT w.lot_id) AS lots_handled,
    COUNT(DISTINCT DATE(w.process_date)) AS active_days,
    ROUND(COUNT(*) * 1.0 / COUNT(DISTINCT DATE(w.process_date)), 2) AS avg_daily_throughput,
    SUM(CASE WHEN w.yield >= 95 THEN 1 ELSE 0 END) AS good_wafers,
    ROUND(100.0 * SUM(CASE WHEN w.yield >= 95 THEN 1 ELSE 0 END) / COUNT(*), 2) AS quality_rate
FROM wafer_data w
INNER JOIN equipment_master e ON w.equipment_id = e.equipment_id
WHERE w.process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY e.equipment_name, e.equipment_type
ORDER BY avg_yield DESC;
```

### 예제 21: TOP/BOTTOM 성능 보고서

```sql
-- TOP 5 및 BOTTOM 5 Lot
WITH lot_performance AS (
    SELECT 
        lot_id,
        COUNT(*) AS wafer_count,
        AVG(yield) AS avg_yield,
        RANK() OVER (ORDER BY AVG(yield) DESC) AS top_rank,
        RANK() OVER (ORDER BY AVG(yield) ASC) AS bottom_rank
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
    GROUP BY lot_id
    HAVING COUNT(*) >= 10
)
SELECT 
    'TOP 5' AS category,
    lot_id,
    wafer_count,
    ROUND(avg_yield, 2) AS avg_yield,
    top_rank AS rank
FROM lot_performance
WHERE top_rank <= 5
UNION ALL
SELECT 
    'BOTTOM 5' AS category,
    lot_id,
    wafer_count,
    ROUND(avg_yield, 2) AS avg_yield,
    bottom_rank AS rank
FROM lot_performance
WHERE bottom_rank <= 5
ORDER BY category, rank;
```

### 예제 22: 월간 요약 보고서

```sql
-- 월간 생산 및 품질 요약
SELECT 
    YEAR(process_date) AS year,
    MONTH(process_date) AS month,
    COUNT(DISTINCT lot_id) AS total_lots,
    COUNT(*) AS total_wafers,
    ROUND(AVG(yield), 2) AS avg_yield,
    ROUND(STDDEV(yield), 2) AS std_yield,
    ROUND(MIN(yield), 2) AS min_yield,
    ROUND(MAX(yield), 2) AS max_yield,
    SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) AS pass_wafers,
    ROUND(100.0 * SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) / COUNT(*), 2) AS pass_rate,
    COUNT(DISTINCT equipment_id) AS equipments_used
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 12 MONTH)
GROUP BY YEAR(process_date), MONTH(process_date)
ORDER BY year DESC, month DESC;
```

### 예제 23: 불량 분석 보고서

```sql
-- 불량 종합 분석 보고서
SELECT 
    d.defect_type,
    COUNT(DISTINCT d.wafer_id) AS affected_wafers,
    SUM(d.defect_count) AS total_defects,
    ROUND(AVG(d.defect_count), 2) AS avg_per_wafer,
    ROUND(AVG(w.yield), 2) AS avg_yield_affected,
    COUNT(DISTINCT w.lot_id) AS affected_lots,
    COUNT(DISTINCT w.equipment_id) AS affected_equipments,
    ROUND(100.0 * COUNT(DISTINCT d.wafer_id) / 
        (SELECT COUNT(*) FROM wafer_data WHERE process_date >= '2025-10-01'), 2) AS incidence_rate
FROM defect_data d
INNER JOIN wafer_data w ON d.wafer_id = w.wafer_id
WHERE d.detection_date >= '2025-10-01'
GROUP BY d.defect_type
ORDER BY total_defects DESC;
```

### 예제 24: 공정별 성능 보고서

```sql
-- 공정 단계별 성능 보고서
SELECT 
    p.process_step,
    COUNT(DISTINCT p.wafer_id) AS wafers_processed,
    ROUND(AVG(w.yield), 2) AS avg_yield_after_step,
    COUNT(DISTINCT d.wafer_id) AS wafers_with_defects,
    ROUND(100.0 * COUNT(DISTINCT d.wafer_id) / COUNT(DISTINCT p.wafer_id), 2) AS defect_rate,
    ROUND(AVG(CAST(p.end_time AS BIGINT) - CAST(p.start_time AS BIGINT)) / 60, 2) AS avg_duration_min,
    COUNT(DISTINCT p.equipment_id) AS equipments_used
FROM process_history p
INNER JOIN wafer_data w ON p.wafer_id = w.wafer_id
LEFT JOIN defect_data d ON p.wafer_id = d.wafer_id 
    AND d.process_step = p.process_step
WHERE w.process_date >= '2025-10-01'
GROUP BY p.process_step
ORDER BY p.process_step;
```

### 예제 25: 비교 분석 보고서

```sql
-- 이번 달 vs 지난 달 비교
WITH this_month AS (
    SELECT 
        COUNT(*) AS wafer_count,
        AVG(yield) AS avg_yield,
        SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) AS pass_count
    FROM wafer_data
    WHERE process_date >= DATE_TRUNC('month', NOW())
),
last_month AS (
    SELECT 
        COUNT(*) AS wafer_count,
        AVG(yield) AS avg_yield,
        SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) AS pass_count
    FROM wafer_data
    WHERE process_date >= DATE_SUB(DATE_TRUNC('month', NOW()), INTERVAL 1 MONTH)
      AND process_date < DATE_TRUNC('month', NOW())
)
SELECT 
    'This Month' AS period,
    t.wafer_count,
    ROUND(t.avg_yield, 2) AS avg_yield,
    ROUND(100.0 * t.pass_count / t.wafer_count, 2) AS pass_rate
FROM this_month t
UNION ALL
SELECT 
    'Last Month' AS period,
    l.wafer_count,
    ROUND(l.avg_yield, 2) AS avg_yield,
    ROUND(100.0 * l.pass_count / l.wafer_count, 2) AS pass_rate
FROM last_month l;
```

---

## 실전 팁

### 복잡한 쿼리 작성 순서

```sql
-- 1단계: 간단하게 시작
SELECT *
FROM wafer_data
LIMIT 10;

-- 2단계: 필요한 컬럼만 선택
SELECT wafer_id, lot_id, yield
FROM wafer_data;

-- 3단계: 필터 추가
SELECT wafer_id, lot_id, yield
FROM wafer_data
WHERE process_date >= '2025-10-01';

-- 4단계: 조인 추가
SELECT w.wafer_id, l.lot_name, w.yield
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
WHERE w.process_date >= '2025-10-01';

-- 5단계: 집계 추가
SELECT 
    l.lot_name,
    COUNT(*) AS wafer_count,
    AVG(w.yield) AS avg_yield
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
WHERE w.process_date >= '2025-10-01'
GROUP BY l.lot_name;
```

### 성능 최적화 체크리스트

```
□ 파티션 컬럼 필터 사용
□ SELECT * 대신 필요한 컬럼만
□ 인덱스가 있는 컬럼으로 JOIN
□ 서브쿼리 대신 JOIN 사용
□ LIMIT 사용 (테스트 시)
□ COMPUTE STATS 실행
```

---

## 관련 문서

- [[Impala SQL 가이드]] - 기본 문법
- [[Impala SQL 빠른참조]] - Quick Reference
- [[Spotfire Information Link+ Impala SQL Guide]] - Spotfire 연동

---

**실전 예제를 복사해서 바로 사용하세요!** 🚀
