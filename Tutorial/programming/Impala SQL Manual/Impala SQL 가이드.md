# Impala SQL 가이드

## 목차
- [[#Impala 소개]]
- [[#기본 문법]]
- [[#데이터 조회]]
- [[#데이터 필터링]]
- [[#집계 및 그룹화]]
- [[#조인]]
- [[#고급 쿼리]]
- [[#성능 최적화]]
- [[#실전 예제]]

---

## Impala 소개

### Impala란?

Impala는 Apache Hadoop용 오픈소스 대규모 병렬 처리(MPP) SQL 쿼리 엔진입니다.

**특징:**
- 빠른 쿼리 성능 (인메모리 처리)
- 표준 SQL 지원
- HDFS, HBase 데이터 직접 쿼리
- Hive 메타스토어 활용

**반도체 업무에서의 활용:**
- 웨이퍼 데이터 분석
- 수율 분석
- 장비 로그 분석
- 공정 통계

---

## 기본 문법

### SELECT 기본

```sql
-- 단순 조회
SELECT * 
FROM table_name;

-- 특정 컬럼 선택
SELECT column1, column2, column3
FROM table_name;

-- 컬럼 별칭
SELECT 
    wafer_id AS "Wafer ID",
    lot_id AS "Lot ID",
    yield AS "수율"
FROM wafer_data;

-- DISTINCT (중복 제거)
SELECT DISTINCT equipment_id
FROM process_log;
```

### LIMIT

```sql
-- 상위 N개 조회
SELECT *
FROM wafer_data
LIMIT 10;

-- 페이징 (OFFSET)
SELECT *
FROM wafer_data
LIMIT 100 OFFSET 1000;
```

### ORDER BY

```sql
-- 오름차순 정렬
SELECT *
FROM wafer_data
ORDER BY wafer_id ASC;

-- 내림차순 정렬
SELECT *
FROM wafer_data
ORDER BY yield DESC;

-- 다중 정렬
SELECT *
FROM wafer_data
ORDER BY lot_id ASC, wafer_id ASC;

-- NULL 처리
SELECT *
FROM wafer_data
ORDER BY yield DESC NULLS LAST;
```

---

## 데이터 조회

### WHERE 절

```sql
-- 단순 조건
SELECT *
FROM wafer_data
WHERE lot_id = 'LOT001';

-- 숫자 비교
SELECT *
FROM wafer_data
WHERE yield > 95.0;

-- 범위 조건
SELECT *
FROM wafer_data
WHERE yield BETWEEN 90 AND 100;

-- IN 조건
SELECT *
FROM wafer_data
WHERE equipment_id IN ('EQ001', 'EQ002', 'EQ003');

-- NOT IN
SELECT *
FROM wafer_data
WHERE status NOT IN ('SCRAP', 'HOLD');
```

### 문자열 검색

```sql
-- LIKE (패턴 매칭)
SELECT *
FROM product_data
WHERE product_name LIKE 'MOSFET%';

-- 와일드카드
-- % : 0개 이상의 문자
-- _ : 정확히 1개의 문자

-- 특정 문자로 시작
SELECT *
FROM lot_data
WHERE lot_id LIKE 'A%';

-- 특정 문자로 끝
SELECT *
FROM lot_data
WHERE lot_id LIKE '%001';

-- 포함
SELECT *
FROM equipment_log
WHERE message LIKE '%ERROR%';

-- REGEXP (정규표현식)
SELECT *
FROM wafer_data
WHERE wafer_id REGEXP '^W[0-9]{4}$';
```

### NULL 처리

```sql
-- NULL 체크
SELECT *
FROM wafer_data
WHERE defect_count IS NULL;

-- NULL이 아닌 데이터
SELECT *
FROM wafer_data
WHERE defect_count IS NOT NULL;

-- COALESCE (NULL 대체)
SELECT 
    wafer_id,
    COALESCE(defect_count, 0) AS defect_count
FROM wafer_data;

-- IFNULL
SELECT 
    wafer_id,
    IFNULL(defect_count, 0) AS defect_count
FROM wafer_data;
```

---

## 데이터 필터링

### 논리 연산자

```sql
-- AND
SELECT *
FROM wafer_data
WHERE yield > 95 
  AND defect_count < 10;

-- OR
SELECT *
FROM wafer_data
WHERE equipment_id = 'EQ001'
   OR equipment_id = 'EQ002';

-- NOT
SELECT *
FROM wafer_data
WHERE NOT (status = 'SCRAP');

-- 복합 조건
SELECT *
FROM wafer_data
WHERE (yield > 95 OR defect_count = 0)
  AND status = 'PASS';
```

### 날짜/시간 필터링

```sql
-- 특정 날짜
SELECT *
FROM process_log
WHERE process_date = '2025-10-14';

-- 날짜 범위
SELECT *
FROM process_log
WHERE process_date BETWEEN '2025-10-01' AND '2025-10-31';

-- 최근 7일
SELECT *
FROM process_log
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 7 DAY);

-- 특정 년월
SELECT *
FROM process_log
WHERE YEAR(process_date) = 2025
  AND MONTH(process_date) = 10;

-- 요일 필터
SELECT *
FROM process_log
WHERE DAYOFWEEK(process_date) IN (2, 3, 4, 5, 6); -- 월-금
```

---

## 집계 및 그룹화

### 집계 함수

```sql
-- COUNT (개수)
SELECT COUNT(*) AS total_wafers
FROM wafer_data;

-- COUNT DISTINCT (고유값 개수)
SELECT COUNT(DISTINCT lot_id) AS unique_lots
FROM wafer_data;

-- SUM (합계)
SELECT SUM(defect_count) AS total_defects
FROM wafer_data;

-- AVG (평균)
SELECT AVG(yield) AS average_yield
FROM wafer_data;

-- MIN, MAX (최소/최대)
SELECT 
    MIN(yield) AS min_yield,
    MAX(yield) AS max_yield
FROM wafer_data;

-- STDDEV (표준편차)
SELECT STDDEV(yield) AS yield_std
FROM wafer_data;
```

### GROUP BY

```sql
-- 기본 그룹화
SELECT 
    lot_id,
    COUNT(*) AS wafer_count,
    AVG(yield) AS avg_yield
FROM wafer_data
GROUP BY lot_id;

-- 다중 그룹화
SELECT 
    lot_id,
    equipment_id,
    COUNT(*) AS count,
    AVG(yield) AS avg_yield
FROM wafer_data
GROUP BY lot_id, equipment_id;

-- HAVING (그룹 필터)
SELECT 
    lot_id,
    AVG(yield) AS avg_yield
FROM wafer_data
GROUP BY lot_id
HAVING AVG(yield) > 95;

-- 복합 조건
SELECT 
    equipment_id,
    COUNT(*) AS wafer_count,
    AVG(yield) AS avg_yield
FROM wafer_data
WHERE process_date >= '2025-10-01'
GROUP BY equipment_id
HAVING COUNT(*) >= 100
ORDER BY avg_yield DESC;
```

---

## 조인

### INNER JOIN

```sql
-- 기본 INNER JOIN
SELECT 
    w.wafer_id,
    w.yield,
    l.lot_name,
    l.product_type
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id;

-- 다중 조인
SELECT 
    w.wafer_id,
    l.lot_name,
    e.equipment_name,
    p.process_step
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
INNER JOIN equipment_data e ON w.equipment_id = e.equipment_id
INNER JOIN process_data p ON w.process_id = p.process_id;
```

### LEFT JOIN

```sql
-- LEFT OUTER JOIN
SELECT 
    w.wafer_id,
    w.yield,
    d.defect_type,
    d.defect_count
FROM wafer_data w
LEFT JOIN defect_data d ON w.wafer_id = d.wafer_id;

-- NULL 포함 결과
SELECT 
    e.equipment_id,
    e.equipment_name,
    COUNT(w.wafer_id) AS wafer_count
FROM equipment_data e
LEFT JOIN wafer_data w ON e.equipment_id = w.equipment_id
GROUP BY e.equipment_id, e.equipment_name;
```

### RIGHT JOIN

```sql
-- RIGHT OUTER JOIN
SELECT 
    w.wafer_id,
    l.lot_id,
    l.lot_name
FROM wafer_data w
RIGHT JOIN lot_data l ON w.lot_id = l.lot_id;
```

### FULL OUTER JOIN

```sql
-- FULL OUTER JOIN
SELECT 
    w.wafer_id,
    l.lot_id
FROM wafer_data w
FULL OUTER JOIN lot_data l ON w.lot_id = l.lot_id;
```

### CROSS JOIN

```sql
-- 카테시안 곱
SELECT 
    e.equipment_name,
    s.step_name
FROM equipment_data e
CROSS JOIN process_step s;
```

---

## 고급 쿼리

### 서브쿼리

```sql
-- WHERE 절 서브쿼리
SELECT *
FROM wafer_data
WHERE yield > (
    SELECT AVG(yield)
    FROM wafer_data
);

-- IN 서브쿼리
SELECT *
FROM wafer_data
WHERE lot_id IN (
    SELECT lot_id
    FROM lot_data
    WHERE product_type = 'MOSFET'
);

-- FROM 절 서브쿼리
SELECT 
    lot_id,
    avg_yield,
    CASE 
        WHEN avg_yield >= 98 THEN 'Excellent'
        WHEN avg_yield >= 95 THEN 'Good'
        ELSE 'Normal'
    END AS grade
FROM (
    SELECT 
        lot_id,
        AVG(yield) AS avg_yield
    FROM wafer_data
    GROUP BY lot_id
) subquery;
```

### CASE 문

```sql
-- 단순 CASE
SELECT 
    wafer_id,
    yield,
    CASE 
        WHEN yield >= 98 THEN 'A'
        WHEN yield >= 95 THEN 'B'
        WHEN yield >= 90 THEN 'C'
        ELSE 'D'
    END AS grade
FROM wafer_data;

-- 검색 CASE
SELECT 
    wafer_id,
    CASE 
        WHEN defect_count = 0 THEN 'Perfect'
        WHEN defect_count <= 5 THEN 'Good'
        WHEN defect_count <= 10 THEN 'Acceptable'
        ELSE 'Poor'
    END AS quality
FROM wafer_data;

-- 집계와 함께
SELECT 
    lot_id,
    SUM(CASE WHEN yield >= 95 THEN 1 ELSE 0 END) AS good_wafers,
    SUM(CASE WHEN yield < 95 THEN 1 ELSE 0 END) AS bad_wafers
FROM wafer_data
GROUP BY lot_id;
```

### UNION

```sql
-- UNION (중복 제거)
SELECT wafer_id, 'Process A' AS source
FROM process_a_data
UNION
SELECT wafer_id, 'Process B' AS source
FROM process_b_data;

-- UNION ALL (중복 포함)
SELECT wafer_id, lot_id
FROM wafer_data_2024
UNION ALL
SELECT wafer_id, lot_id
FROM wafer_data_2025;
```

### WITH (CTE - Common Table Expression)

```sql
-- CTE 사용
WITH high_yield_lots AS (
    SELECT 
        lot_id,
        AVG(yield) AS avg_yield
    FROM wafer_data
    GROUP BY lot_id
    HAVING AVG(yield) > 95
)
SELECT 
    w.wafer_id,
    w.yield,
    h.avg_yield
FROM wafer_data w
INNER JOIN high_yield_lots h ON w.lot_id = h.lot_id;

-- 다중 CTE
WITH 
lot_summary AS (
    SELECT 
        lot_id,
        COUNT(*) AS wafer_count,
        AVG(yield) AS avg_yield
    FROM wafer_data
    GROUP BY lot_id
),
equipment_summary AS (
    SELECT 
        equipment_id,
        COUNT(*) AS wafer_count
    FROM wafer_data
    GROUP BY equipment_id
)
SELECT 
    l.lot_id,
    l.wafer_count,
    l.avg_yield,
    e.equipment_id
FROM lot_summary l
LEFT JOIN equipment_summary e ON l.wafer_count = e.wafer_count;
```

### 윈도우 함수

```sql
-- ROW_NUMBER (행 번호)
SELECT 
    wafer_id,
    lot_id,
    yield,
    ROW_NUMBER() OVER (PARTITION BY lot_id ORDER BY yield DESC) AS rank_in_lot
FROM wafer_data;

-- RANK (순위)
SELECT 
    wafer_id,
    yield,
    RANK() OVER (ORDER BY yield DESC) AS yield_rank
FROM wafer_data;

-- DENSE_RANK
SELECT 
    wafer_id,
    yield,
    DENSE_RANK() OVER (ORDER BY yield DESC) AS yield_rank
FROM wafer_data;

-- LAG (이전 행)
SELECT 
    process_date,
    yield,
    LAG(yield, 1) OVER (ORDER BY process_date) AS prev_yield,
    yield - LAG(yield, 1) OVER (ORDER BY process_date) AS yield_change
FROM daily_yield;

-- LEAD (다음 행)
SELECT 
    process_date,
    yield,
    LEAD(yield, 1) OVER (ORDER BY process_date) AS next_yield
FROM daily_yield;

-- 이동 평균
SELECT 
    process_date,
    yield,
    AVG(yield) OVER (
        ORDER BY process_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7day
FROM daily_yield;

-- 누적 합계
SELECT 
    process_date,
    wafer_count,
    SUM(wafer_count) OVER (ORDER BY process_date) AS cumulative_count
FROM daily_production;
```

---

## 성능 최적화

### 파티션 활용

```sql
-- 파티션 필터링 (빠름)
SELECT *
FROM wafer_data
WHERE year = 2025 
  AND month = 10;

-- 파티션 없이 (느림)
SELECT *
FROM wafer_data
WHERE process_date BETWEEN '2025-10-01' AND '2025-10-31';
```

### 통계 및 힌트

```sql
-- COMPUTE STATS (통계 갱신)
COMPUTE STATS wafer_data;

-- STRAIGHT_JOIN (조인 순서 고정)
SELECT STRAIGHT_JOIN
    w.wafer_id,
    l.lot_name
FROM large_table w
JOIN small_table l ON w.lot_id = l.lot_id;

-- SHUFFLE/BROADCAST 힌트
SELECT /* +BROADCAST(small_table) */
    w.wafer_id,
    s.parameter_value
FROM wafer_data w
JOIN small_table s ON w.param_id = s.param_id;
```

### 인덱싱 및 파티셔닝

```sql
-- 파티션 프루닝 활용
SELECT *
FROM wafer_data
WHERE year = 2025  -- 파티션 컬럼
  AND month = 10   -- 파티션 컬럼
  AND yield > 95;  -- 일반 필터

-- WHERE 절 최적화
-- 나쁜 예
SELECT *
FROM wafer_data
WHERE YEAR(process_date) = 2025;

-- 좋은 예
SELECT *
FROM wafer_data
WHERE process_date >= '2025-01-01'
  AND process_date < '2026-01-01';
```

---

## 실전 예제

### 예제 1: 일일 수율 분석

```sql
-- 일별 평균 수율 및 가공 웨이퍼 수
SELECT 
    process_date,
    COUNT(*) AS total_wafers,
    AVG(yield) AS avg_yield,
    MIN(yield) AS min_yield,
    MAX(yield) AS max_yield,
    STDDEV(yield) AS std_yield
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY process_date
ORDER BY process_date DESC;
```

### 예제 2: 장비별 성능 비교

```sql
-- 장비별 수율 및 처리량
WITH equipment_stats AS (
    SELECT 
        equipment_id,
        COUNT(*) AS wafer_count,
        AVG(yield) AS avg_yield,
        STDDEV(yield) AS std_yield
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
    GROUP BY equipment_id
)
SELECT 
    e.equipment_name,
    s.wafer_count,
    ROUND(s.avg_yield, 2) AS avg_yield,
    ROUND(s.std_yield, 2) AS std_yield,
    CASE 
        WHEN s.avg_yield >= 98 THEN 'Excellent'
        WHEN s.avg_yield >= 95 THEN 'Good'
        ELSE 'Needs Improvement'
    END AS performance
FROM equipment_stats s
INNER JOIN equipment_master e ON s.equipment_id = e.equipment_id
ORDER BY s.avg_yield DESC;
```

### 예제 3: Lot 추적

```sql
-- Lot 전체 이력 조회
SELECT 
    w.wafer_id,
    w.yield,
    p.process_step,
    p.process_time,
    e.equipment_name,
    d.defect_type,
    d.defect_count
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
LEFT JOIN process_history p ON w.wafer_id = p.wafer_id
LEFT JOIN equipment_master e ON p.equipment_id = e.equipment_id
LEFT JOIN defect_data d ON w.wafer_id = d.wafer_id
WHERE l.lot_id = 'LOT001'
ORDER BY w.wafer_id, p.process_step;
```

### 예제 4: 불량 분석

```sql
-- 주요 불량 유형 및 발생 빈도
SELECT 
    d.defect_type,
    COUNT(*) AS occurrence,
    AVG(d.defect_count) AS avg_count,
    COUNT(DISTINCT w.lot_id) AS affected_lots
FROM defect_data d
INNER JOIN wafer_data w ON d.wafer_id = w.wafer_id
WHERE w.process_date >= '2025-10-01'
GROUP BY d.defect_type
HAVING COUNT(*) >= 10
ORDER BY occurrence DESC
LIMIT 10;
```

### 예제 5: 트렌드 분석

```sql
-- 주간 수율 트렌드 (7일 이동 평균)
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
    avg_yield,
    AVG(avg_yield) OVER (
        ORDER BY process_date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7day,
    avg_yield - LAG(avg_yield, 1) OVER (ORDER BY process_date) AS daily_change
FROM daily_yield
ORDER BY process_date DESC;
```

### 예제 6: 파레토 분석

```sql
-- 불량 파레토 분석
WITH defect_summary AS (
    SELECT 
        defect_type,
        SUM(defect_count) AS total_defects
    FROM defect_data
    WHERE detection_date >= '2025-10-01'
    GROUP BY defect_type
),
defect_with_cumsum AS (
    SELECT 
        defect_type,
        total_defects,
        SUM(total_defects) OVER (ORDER BY total_defects DESC) AS cumulative_defects,
        SUM(total_defects) OVER () AS grand_total
    FROM defect_summary
)
SELECT 
    defect_type,
    total_defects,
    ROUND(100.0 * total_defects / grand_total, 2) AS percentage,
    ROUND(100.0 * cumulative_defects / grand_total, 2) AS cumulative_percentage
FROM defect_with_cumsum
ORDER BY total_defects DESC;
```

### 예제 7: 공정 능력 지수 (Cp/Cpk)

```sql
-- 수율의 공정 능력 계산
WITH yield_stats AS (
    SELECT 
        AVG(yield) AS mean_yield,
        STDDEV(yield) AS std_yield
    FROM wafer_data
    WHERE process_date >= '2025-10-01'
)
SELECT 
    mean_yield,
    std_yield,
    -- USL = 100, LSL = 90 가정
    (100 - 90) / (6 * std_yield) AS cp,
    LEAST(
        (100 - mean_yield) / (3 * std_yield),
        (mean_yield - 90) / (3 * std_yield)
    ) AS cpk
FROM yield_stats;
```

### 예제 8: 상관관계 분석

```sql
-- 공정 파라미터와 수율 상관관계
SELECT 
    ROUND(AVG(CASE WHEN p.temperature BETWEEN 800 AND 850 THEN w.yield END), 2) AS yield_800_850,
    ROUND(AVG(CASE WHEN p.temperature BETWEEN 850 AND 900 THEN w.yield END), 2) AS yield_850_900,
    ROUND(AVG(CASE WHEN p.temperature BETWEEN 900 AND 950 THEN w.yield END), 2) AS yield_900_950,
    COUNT(*) AS total_samples
FROM wafer_data w
INNER JOIN process_parameters p ON w.wafer_id = p.wafer_id
WHERE w.process_date >= '2025-10-01';
```

---

## 유용한 함수

### 문자열 함수

```sql
-- CONCAT (연결)
SELECT CONCAT(lot_id, '-', wafer_id) AS full_id
FROM wafer_data;

-- SUBSTRING (부분 문자열)
SELECT SUBSTRING(lot_id, 1, 3) AS lot_prefix
FROM wafer_data;

-- UPPER/LOWER (대소문자 변환)
SELECT UPPER(equipment_name) AS equipment_name_upper
FROM equipment_master;

-- LENGTH (길이)
SELECT wafer_id, LENGTH(wafer_id) AS id_length
FROM wafer_data;

-- TRIM (공백 제거)
SELECT TRIM(equipment_name) AS equipment_name
FROM equipment_master;
```

### 날짜 함수

```sql
-- 현재 시간
SELECT NOW();
SELECT CURRENT_TIMESTAMP();

-- 날짜 추출
SELECT 
    YEAR(process_date) AS year,
    MONTH(process_date) AS month,
    DAY(process_date) AS day,
    DAYOFWEEK(process_date) AS day_of_week
FROM wafer_data;

-- 날짜 연산
SELECT 
    DATE_ADD(process_date, INTERVAL 7 DAY) AS week_later,
    DATE_SUB(process_date, INTERVAL 1 MONTH) AS month_ago
FROM wafer_data;

-- 날짜 차이
SELECT 
    DATEDIFF(NOW(), process_date) AS days_ago
FROM wafer_data;

-- 날짜 포맷
SELECT 
    DATE_FORMAT(process_date, '%Y-%m-%d') AS formatted_date
FROM wafer_data;
```

### 수학 함수

```sql
-- ROUND (반올림)
SELECT ROUND(yield, 2) AS yield_rounded
FROM wafer_data;

-- CEIL/FLOOR (올림/내림)
SELECT 
    CEIL(yield) AS yield_ceil,
    FLOOR(yield) AS yield_floor
FROM wafer_data;

-- ABS (절대값)
SELECT ABS(defect_count - target_count) AS deviation
FROM wafer_data;

-- POWER (거듭제곱)
SELECT POWER(2, 10) AS result; -- 1024

-- SQRT (제곱근)
SELECT SQRT(variance) AS std_dev
FROM statistics_table;
```

---

## 팁과 요령

### 쿼리 작성 팁

```sql
-- 1. 항상 LIMIT 사용 (테스트 시)
SELECT *
FROM large_table
LIMIT 100;

-- 2. 파티션 컬럼 먼저 필터링
SELECT *
FROM wafer_data
WHERE year = 2025  -- 파티션 컬럼
  AND month = 10   -- 파티션 컬럼
  AND yield > 95;  -- 일반 필터

-- 3. SELECT * 피하기
-- 나쁜 예
SELECT * FROM wafer_data;

-- 좋은 예
SELECT wafer_id, lot_id, yield
FROM wafer_data;

-- 4. 서브쿼리보다 JOIN
-- 나쁜 예 (느림)
SELECT *
FROM wafer_data
WHERE lot_id IN (SELECT lot_id FROM lot_data WHERE product_type = 'MOSFET');

-- 좋은 예 (빠름)
SELECT w.*
FROM wafer_data w
INNER JOIN lot_data l ON w.lot_id = l.lot_id
WHERE l.product_type = 'MOSFET';
```

### 일반적인 실수

```sql
-- 1. COUNT(*) vs COUNT(column)
SELECT COUNT(*) FROM table;  -- 모든 행 (NULL 포함)
SELECT COUNT(column) FROM table;  -- NULL 제외

-- 2. NULL 비교
-- 잘못됨
WHERE column = NULL

-- 올바름
WHERE column IS NULL

-- 3. 문자열 비교
-- 대소문자 구분
WHERE name = 'WAFER'  -- 정확히 일치해야 함

-- 대소문자 무시
WHERE UPPER(name) = 'WAFER'
```

---

## 관련 문서

- [[Spotfire Information Link+ Impala SQL Guide]] - Spotfire 연동
- [[Spotfire Custom Expression & Document Property Manual]] - 계산식 작성

---

**Impala SQL로 데이터를 효율적으로 분석하세요!**
