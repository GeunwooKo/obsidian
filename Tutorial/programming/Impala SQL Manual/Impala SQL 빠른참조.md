# Impala SQL 빠른 참조 (Cheat Sheet)

## 기본 쿼리

```sql
-- 조회
SELECT column1, column2 FROM table;
SELECT * FROM table LIMIT 10;
SELECT DISTINCT column FROM table;

-- 정렬
ORDER BY column ASC/DESC;
ORDER BY column DESC NULLS LAST;

-- 필터
WHERE column = value;
WHERE column IN (v1, v2, v3);
WHERE column BETWEEN min AND max;
WHERE column IS NULL;
WHERE column LIKE 'pattern%';
```

---

## 집계 함수

```sql
COUNT(*)                 -- 전체 행 수
COUNT(DISTINCT column)   -- 고유값 개수
SUM(column)             -- 합계
AVG(column)             -- 평균
MIN(column)             -- 최소값
MAX(column)             -- 최대값
STDDEV(column)          -- 표준편차
VARIANCE(column)        -- 분산
```

---

## 그룹화

```sql
-- 기본
SELECT column, COUNT(*)
FROM table
GROUP BY column;

-- HAVING
SELECT column, AVG(value)
FROM table
GROUP BY column
HAVING AVG(value) > 100;

-- 다중 그룹
SELECT col1, col2, COUNT(*)
FROM table
GROUP BY col1, col2;
```

---

## 조인

```sql
-- INNER JOIN
SELECT *
FROM t1
INNER JOIN t2 ON t1.id = t2.id;

-- LEFT JOIN
SELECT *
FROM t1
LEFT JOIN t2 ON t1.id = t2.id;

-- RIGHT JOIN
SELECT *
FROM t1
RIGHT JOIN t2 ON t1.id = t2.id;

-- FULL OUTER JOIN
SELECT *
FROM t1
FULL OUTER JOIN t2 ON t1.id = t2.id;
```

---

## 서브쿼리

```sql
-- WHERE 절
SELECT *
FROM table
WHERE column > (SELECT AVG(column) FROM table);

-- FROM 절
SELECT *
FROM (SELECT column FROM table) AS subquery;

-- IN
SELECT *
FROM table
WHERE id IN (SELECT id FROM other_table);
```

---

## CASE문

```sql
SELECT 
    column,
    CASE 
        WHEN value >= 98 THEN 'A'
        WHEN value >= 95 THEN 'B'
        ELSE 'C'
    END AS grade
FROM table;
```

---

## 윈도우 함수

```sql
-- 순위
ROW_NUMBER() OVER (ORDER BY col)
RANK() OVER (ORDER BY col)
DENSE_RANK() OVER (ORDER BY col)

-- 파티션별 순위
ROW_NUMBER() OVER (PARTITION BY col1 ORDER BY col2)

-- 이전/다음 값
LAG(column, 1) OVER (ORDER BY date)
LEAD(column, 1) OVER (ORDER BY date)

-- 이동 평균
AVG(column) OVER (
    ORDER BY date
    ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)

-- 누적 합계
SUM(column) OVER (ORDER BY date)
```

---

## CTE (WITH)

```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM table
    WHERE condition
)
SELECT *
FROM cte_name;

-- 다중 CTE
WITH 
cte1 AS (SELECT ...),
cte2 AS (SELECT ...)
SELECT * FROM cte1 JOIN cte2;
```

---

## 문자열 함수

```sql
CONCAT(str1, str2)           -- 연결
SUBSTRING(str, pos, len)     -- 부분 문자열
UPPER(str)                   -- 대문자
LOWER(str)                   -- 소문자
LENGTH(str)                  -- 길이
TRIM(str)                    -- 공백 제거
REPLACE(str, old, new)       -- 치환
```

---

## 날짜 함수

```sql
-- 현재
NOW()
CURRENT_TIMESTAMP()

-- 추출
YEAR(date)
MONTH(date)
DAY(date)
DAYOFWEEK(date)             -- 1(일) ~ 7(토)
HOUR(timestamp)
MINUTE(timestamp)

-- 연산
DATE_ADD(date, INTERVAL n DAY)
DATE_SUB(date, INTERVAL n DAY)
DATEDIFF(date1, date2)      -- 일 차이

-- 변환
CAST(string AS TIMESTAMP)
TO_DATE(timestamp)
```

---

## 수학 함수

```sql
ROUND(number, decimals)      -- 반올림
CEIL(number)                 -- 올림
FLOOR(number)                -- 내림
ABS(number)                  -- 절대값
POWER(base, exp)             -- 거듭제곱
SQRT(number)                 -- 제곱근
MOD(n, m)                    -- 나머지
```

---

## NULL 처리

```sql
IS NULL
IS NOT NULL
COALESCE(col1, col2, default)  -- NULL 대체
IFNULL(column, default)         -- NULL 대체
NULLIF(col1, col2)              -- 같으면 NULL
```

---

## 조건 연산자

```sql
-- 비교
=, !=, <>, <, >, <=, >=

-- 논리
AND, OR, NOT

-- 범위
BETWEEN min AND max
IN (value1, value2, ...)
NOT IN (...)

-- 패턴
LIKE 'pattern%'
REGEXP 'regex_pattern'
```

---

## UNION

```sql
-- UNION (중복 제거)
SELECT col FROM table1
UNION
SELECT col FROM table2;

-- UNION ALL (중복 포함)
SELECT col FROM table1
UNION ALL
SELECT col FROM table2;
```

---

## 자주 쓰는 패턴

### 상위 N개

```sql
SELECT *
FROM table
ORDER BY column DESC
LIMIT 10;
```

### 중복 제거

```sql
SELECT DISTINCT column
FROM table;
```

### 카운트 및 비율

```sql
SELECT 
    category,
    COUNT(*) AS count,
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS percentage
FROM table
GROUP BY category;
```

### 이동 평균

```sql
SELECT 
    date,
    value,
    AVG(value) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7
FROM table;
```

### 누적 합계

```sql
SELECT 
    date,
    amount,
    SUM(amount) OVER (ORDER BY date) AS cumulative
FROM table;
```

### 전년 동기 대비

```sql
SELECT 
    date,
    value,
    LAG(value, 12) OVER (ORDER BY date) AS prev_year_value,
    value - LAG(value, 12) OVER (ORDER BY date) AS yoy_change
FROM monthly_data;
```

### 파레토 분석

```sql
WITH summary AS (
    SELECT 
        category,
        SUM(amount) AS total
    FROM table
    GROUP BY category
)
SELECT 
    category,
    total,
    SUM(total) OVER (ORDER BY total DESC) AS cumulative,
    100.0 * SUM(total) OVER (ORDER BY total DESC) / 
        SUM(total) OVER () AS cumulative_pct
FROM summary
ORDER BY total DESC;
```

### 피벗 (수동)

```sql
SELECT 
    date,
    SUM(CASE WHEN category = 'A' THEN value END) AS cat_a,
    SUM(CASE WHEN category = 'B' THEN value END) AS cat_b,
    SUM(CASE WHEN category = 'C' THEN value END) AS cat_c
FROM table
GROUP BY date;
```

---

## 성능 최적화 팁

### 파티션 활용

```sql
-- 좋음: 파티션 컬럼 먼저
WHERE year = 2025 
  AND month = 10
  AND other_condition

-- 나쁨: 함수 사용
WHERE YEAR(date) = 2025
```

### SELECT 최소화

```sql
-- 좋음
SELECT col1, col2 FROM table;

-- 나쁨
SELECT * FROM table;
```

### JOIN 순서

```sql
-- 작은 테이블을 먼저
SELECT *
FROM small_table s
JOIN large_table l ON s.id = l.id;
```

### 서브쿼리 vs JOIN

```sql
-- 나쁨: 서브쿼리
WHERE id IN (SELECT id FROM table2)

-- 좋음: JOIN
JOIN table2 ON table1.id = table2.id
```

---

## 반도체 업무 특화 쿼리

### 수율 분석

```sql
-- 일별 평균 수율
SELECT 
    process_date,
    AVG(yield) AS avg_yield,
    COUNT(*) AS wafer_count
FROM wafer_data
WHERE process_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
GROUP BY process_date
ORDER BY process_date;
```

### Lot 통계

```sql
-- Lot별 요약
SELECT 
    lot_id,
    COUNT(*) AS wafer_count,
    AVG(yield) AS avg_yield,
    MIN(yield) AS min_yield,
    MAX(yield) AS max_yield,
    STDDEV(yield) AS std_yield
FROM wafer_data
GROUP BY lot_id;
```

### 장비별 성능

```sql
-- 장비별 처리량 및 수율
SELECT 
    equipment_id,
    COUNT(*) AS processed,
    AVG(yield) AS avg_yield
FROM wafer_data
WHERE process_date >= '2025-10-01'
GROUP BY equipment_id
ORDER BY avg_yield DESC;
```

### 불량 TOP 10

```sql
-- 주요 불량 유형
SELECT 
    defect_type,
    COUNT(*) AS occurrence,
    SUM(defect_count) AS total_defects
FROM defect_data
WHERE detection_date >= '2025-10-01'
GROUP BY defect_type
ORDER BY occurrence DESC
LIMIT 10;
```

### 트렌드 분석

```sql
-- 7일 이동 평균
SELECT 
    date,
    yield,
    AVG(yield) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS ma7
FROM daily_yield
ORDER BY date DESC;
```

### 이상치 탐지

```sql
-- 3-sigma 이상치
WITH stats AS (
    SELECT 
        AVG(yield) AS mean,
        STDDEV(yield) AS std
    FROM wafer_data
)
SELECT 
    wafer_id,
    yield,
    ABS(yield - stats.mean) / stats.std AS z_score
FROM wafer_data, stats
WHERE ABS(yield - stats.mean) > 3 * stats.std;
```

---

## 자주 발생하는 에러

### 1. Column not found

```sql
-- 에러
SELECT column_name FROM table;

-- 해결: 철자 확인, 대소문자 구분 확인
SELECT column_name FROM table;
```

### 2. Ambiguous column

```sql
-- 에러
SELECT id FROM table1 JOIN table2;

-- 해결: 테이블 별칭 사용
SELECT t1.id FROM table1 t1 JOIN table2 t2;
```

### 3. Group by 에러

```sql
-- 에러
SELECT col1, col2, COUNT(*)
FROM table
GROUP BY col1;

-- 해결: 모든 non-aggregated 컬럼 포함
SELECT col1, col2, COUNT(*)
FROM table
GROUP BY col1, col2;
```

### 4. NULL 비교

```sql
-- 에러
WHERE column = NULL

-- 해결
WHERE column IS NULL
```

---

## 키보드 단축키 (Hue/Impala Shell)

```
Ctrl + Enter    쿼리 실행
Ctrl + Space    자동완성
Ctrl + /        주석 토글
Ctrl + D        라인 삭제
Ctrl + F        찾기
```

---

## 유용한 시스템 쿼리

```sql
-- 테이블 목록
SHOW TABLES;

-- 테이블 구조
DESCRIBE table_name;

-- 파티션 정보
SHOW PARTITIONS table_name;

-- 테이블 통계
SHOW TABLE STATS table_name;

-- 컬럼 통계
SHOW COLUMN STATS table_name;

-- 통계 갱신
COMPUTE STATS table_name;
```

---

## 빠른 검색

| 찾고 싶은 것 | 키워드 |
|-------------|--------|
| 평균 | AVG() |
| 개수 | COUNT() |
| 합계 | SUM() |
| 최대/최소 | MAX(), MIN() |
| 그룹화 | GROUP BY |
| 정렬 | ORDER BY |
| 필터 | WHERE |
| 조인 | JOIN |
| 중복제거 | DISTINCT |
| 상위N | LIMIT |
| 순위 | RANK(), ROW_NUMBER() |
| 이동평균 | AVG() OVER() |
| 이전값 | LAG() |
| 다음값 | LEAD() |
| 조건분기 | CASE WHEN |
| NULL처리 | COALESCE(), IFNULL() |

---

## 치트시트 인쇄용

### 필수 5가지

```sql
-- 1. 조회
SELECT * FROM table LIMIT 10;

-- 2. 필터
WHERE column = value AND other = value2;

-- 3. 그룹
SELECT col, COUNT(*) FROM table GROUP BY col;

-- 4. 조인
SELECT * FROM t1 JOIN t2 ON t1.id = t2.id;

-- 5. 정렬
ORDER BY column DESC;
```

---

## 관련 문서

- [[Impala SQL 가이드]] - 상세 설명
- [[Spotfire Information Link+ Impala SQL Guide]] - Spotfire 연동

---

**이 문서를 출력하여 책상에 붙여두세요!** 📄
