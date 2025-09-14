# Spotfire Information Link+ Impala SQL 문법 가이드

> 📝 **참고**: Spotfire Information Link+에서 Impala SQL을 사용하여 동적 데이터 연결을 구성하는 방법을 설명합니다.

> 🔗 **연관 매뉴얼**: [[Spotfire Custom Expression & Document Property Manual]] | [[README]]

## 개요
Information Link+는 Spotfire에서 외부 데이터베이스와 동적으로 연결하여 실시간 데이터를 가져오는 기능입니다. Impala SQL을 사용하여 복잡한 쿼리와 파라미터화된 데이터 소스를 구성할 수 있습니다.

## Impala SQL 기본 문법

### 1. 기본 SELECT 문
```sql
-- 기본 구조
SELECT column1, column2, column3
FROM table_name
WHERE condition
ORDER BY column1 DESC
LIMIT 1000;

-- 예시: 웨이퍼 데이터 조회
SELECT wafer_id, lot_id, process_step, yield_rate, test_date
FROM wafer_test_results
WHERE test_date >= '2024-01-01'
ORDER BY test_date DESC
LIMIT 500;
```

### 2. 데이터 타입 및 변환
```sql
-- 문자열 함수
SELECT 
    UPPER(product_name) as product_upper,
    LOWER(equipment_id) as equipment_lower,
    SUBSTRING(lot_id, 1, 8) as short_lot_id,
    CONCAT(line_name, '_', equipment_id) as line_equipment
FROM production_data;

-- 날짜/시간 함수
SELECT 
    NOW() as current_time,
    UNIX_TIMESTAMP() as current_timestamp,
    FROM_UNIXTIME(timestamp_col) as formatted_date,
    DATE_FORMAT(test_date, 'yyyy-MM-dd') as date_only,
    YEAR(test_date) as test_year,
    MONTH(test_date) as test_month
FROM test_results;

-- 수치 함수
SELECT 
    ROUND(yield_rate, 2) as yield_rounded,
    CEIL(processing_time) as time_ceiling,
    FLOOR(temperature) as temp_floor,
    ABS(deviation) as abs_deviation
FROM process_data;
```

### 3. 집계 함수
```sql
-- 기본 집계
SELECT 
    equipment_id,
    COUNT(*) as total_runs,
    AVG(yield_rate) as avg_yield,
    MIN(yield_rate) as min_yield,
    MAX(yield_rate) as max_yield,
    STDDEV(yield_rate) as yield_stddev,
    SUM(wafer_count) as total_wafers
FROM production_summary
GROUP BY equipment_id
HAVING AVG(yield_rate) > 90.0;

-- 윈도우 함수
SELECT 
    wafer_id,
    test_date,
    yield_rate,
    ROW_NUMBER() OVER (PARTITION BY lot_id ORDER BY test_date) as seq_num,
    RANK() OVER (ORDER BY yield_rate DESC) as yield_rank,
    LAG(yield_rate, 1) OVER (ORDER BY test_date) as prev_yield,
    LEAD(yield_rate, 1) OVER (ORDER BY test_date) as next_yield
FROM wafer_results
ORDER BY test_date;
```

## Spotfire 파라미터 사용

### 1. 기본 파라미터 문법
```sql
-- ${parameter_name} 형식으로 파라미터 참조
SELECT *
FROM production_data
WHERE product_type = '${ProductType}'
  AND test_date >= '${StartDate}'
  AND test_date <= '${EndDate}'
  AND equipment_id IN (${EquipmentList});
```

### 2. 파라미터 타입별 예시
```sql
-- 문자열 파라미터
SELECT *
FROM process_data
WHERE line_name = '${LineName}'  -- 단일 문자열
  AND product_family = '${ProductFamily}';

-- 숫자 파라미터  
SELECT *
FROM quality_metrics
WHERE yield_rate >= ${MinYield}  -- 숫자 (따옴표 없음)
  AND defect_count <= ${MaxDefects};

-- 날짜 파라미터
SELECT *
FROM test_results
WHERE test_datetime >= TIMESTAMP '${StartDateTime}'
  AND test_datetime <= TIMESTAMP '${EndDateTime}';

-- 리스트 파라미터 (IN 절)
SELECT *
FROM equipment_status
WHERE equipment_id IN (${EquipmentIdList})  -- 'EQP001','EQP002','EQP003'
  AND status_type IN (${StatusTypeList});
```

### 3. 조건부 파라미터 처리
```sql
-- CASE문을 사용한 조건부 필터링
SELECT *
FROM production_data
WHERE 1=1
  AND (CASE 
       WHEN '${ProductFilter}' = 'ALL' THEN 1=1
       ELSE product_type = '${ProductFilter}'
       END)
  AND (CASE 
       WHEN '${LineFilter}' = 'ALL' THEN 1=1
       ELSE line_name = '${LineFilter}'
       END);

-- NULL 처리
SELECT *
FROM test_data
WHERE (CASE 
       WHEN '${OptionalParameter}' = '' OR '${OptionalParameter}' IS NULL 
       THEN 1=1
       ELSE column_name = '${OptionalParameter}'
       END);
```

## 반도체 업무 특화 쿼리 패턴

### 1. 웨이퍼 맵 데이터 쿼리
```sql
-- 웨이퍼 맵용 데이터 구조
SELECT 
    wafer_id,
    die_x,
    die_y,
    bin_number,
    test_result,
    parametric_value,
    CASE 
        WHEN bin_number = 1 THEN 'PASS'
        ELSE 'FAIL'
    END as pass_fail_status
FROM wafer_test_map
WHERE wafer_id = '${WaferId}'
  AND test_program = '${TestProgram}'
ORDER BY die_y, die_x;
```

### 2. 시계열 수율 데이터
```sql
-- 시간별 수율 트렌드
SELECT 
    DATE_FORMAT(test_datetime, 'yyyy-MM-dd HH:00:00') as hour_bucket,
    equipment_id,
    product_type,
    COUNT(*) as total_units,
    SUM(CASE WHEN final_bin = 1 THEN 1 ELSE 0 END) as pass_count,
    ROUND(
        100.0 * SUM(CASE WHEN final_bin = 1 THEN 1 ELSE 0 END) / COUNT(*), 
        2
    ) as yield_rate
FROM test_results
WHERE test_datetime >= '${StartDateTime}'
  AND test_datetime <= '${EndDateTime}'
  AND equipment_id IN (${EquipmentList})
GROUP BY 
    DATE_FORMAT(test_datetime, 'yyyy-MM-dd HH:00:00'),
    equipment_id,
    product_type
ORDER BY hour_bucket, equipment_id;
```

### 3. 공정 파라미터 분석
```sql
-- 공정 파라미터와 수율 상관관계
SELECT 
    lot_id,
    recipe_name,
    AVG(temperature) as avg_temp,
    AVG(pressure) as avg_pressure,
    AVG(time_duration) as avg_time,
    STDDEV(temperature) as temp_variation,
    final_yield_rate,
    CASE 
        WHEN final_yield_rate >= ${TargetYield} THEN 'GOOD'
        WHEN final_yield_rate >= ${MinAcceptableYield} THEN 'ACCEPTABLE'
        ELSE 'POOR'
    END as yield_category
FROM process_summary
WHERE process_step = '${ProcessStep}'
  AND start_date >= '${AnalysisStartDate}'
GROUP BY lot_id, recipe_name, final_yield_rate
HAVING COUNT(*) >= ${MinSampleSize}
ORDER BY final_yield_rate DESC;
```

### 4. 품질 관리 데이터
```sql
-- SPC 차트용 데이터
SELECT 
    measurement_datetime,
    equipment_id,
    parameter_name,
    measured_value,
    specification_lower,
    specification_upper,
    control_lower,
    control_upper,
    AVG(measured_value) OVER (
        PARTITION BY equipment_id, parameter_name 
        ORDER BY measurement_datetime 
        ROWS BETWEEN 19 PRECEDING AND CURRENT ROW
    ) as moving_avg_20,
    CASE 
        WHEN measured_value < specification_lower OR measured_value > specification_upper 
        THEN 'OUT_OF_SPEC'
        WHEN measured_value < control_lower OR measured_value > control_upper 
        THEN 'OUT_OF_CONTROL'
        ELSE 'IN_CONTROL'
    END as control_status
FROM quality_measurements
WHERE equipment_id = '${EquipmentId}'
  AND parameter_name = '${ParameterName}'
  AND measurement_datetime >= '${StartTime}'
ORDER BY measurement_datetime;
```

## 고급 Impala SQL 기능

### 1. 복잡한 조인
```sql
-- 다중 테이블 조인
SELECT 
    wr.wafer_id,
    wr.lot_id,
    ls.recipe_name,
    ls.process_conditions,
    eq.equipment_type,
    eq.maintenance_status,
    wr.final_yield
FROM wafer_results wr
JOIN lot_summary ls ON wr.lot_id = ls.lot_id
JOIN equipment_master eq ON wr.equipment_id = eq.equipment_id
LEFT JOIN process_exceptions pe ON wr.wafer_id = pe.wafer_id
WHERE wr.test_date >= '${StartDate}'
  AND eq.equipment_type = '${EquipmentType}'
  AND pe.wafer_id IS NULL;  -- 예외 없는 웨이퍼만
```

### 2. 서브쿼리 활용
```sql
-- 평균 대비 성능 비교
SELECT 
    equipment_id,
    daily_yield,
    overall_avg_yield,
    ROUND(daily_yield - overall_avg_yield, 2) as yield_deviation,
    CASE 
        WHEN daily_yield > overall_avg_yield + 2 * overall_stddev THEN 'EXCELLENT'
        WHEN daily_yield > overall_avg_yield THEN 'ABOVE_AVERAGE'
        WHEN daily_yield > overall_avg_yield - 2 * overall_stddev THEN 'BELOW_AVERAGE'
        ELSE 'POOR'
    END as performance_category
FROM (
    SELECT 
        equipment_id,
        AVG(yield_rate) as daily_yield,
        (SELECT AVG(yield_rate) FROM daily_yields WHERE date_col >= '${ComparisonStartDate}') as overall_avg_yield,
        (SELECT STDDEV(yield_rate) FROM daily_yields WHERE date_col >= '${ComparisonStartDate}') as overall_stddev
    FROM daily_yields
    WHERE date_col = '${TargetDate}'
    GROUP BY equipment_id
) performance_data
ORDER BY yield_deviation DESC;
```

### 3. 피벗 테이블 구현
```sql
-- 수동 피벗 (제품별 월간 수율)
SELECT 
    equipment_id,
    SUM(CASE WHEN month_num = 1 THEN avg_yield ELSE 0 END) as jan_yield,
    SUM(CASE WHEN month_num = 2 THEN avg_yield ELSE 0 END) as feb_yield,
    SUM(CASE WHEN month_num = 3 THEN avg_yield ELSE 0 END) as mar_yield,
    SUM(CASE WHEN month_num = 4 THEN avg_yield ELSE 0 END) as apr_yield,
    SUM(CASE WHEN month_num = 5 THEN avg_yield ELSE 0 END) as may_yield,
    SUM(CASE WHEN month_num = 6 THEN avg_yield ELSE 0 END) as jun_yield
FROM (
    SELECT 
        equipment_id,
        MONTH(test_date) as month_num,
        AVG(yield_rate) as avg_yield
    FROM monthly_summary
    WHERE YEAR(test_date) = ${TargetYear}
    GROUP BY equipment_id, MONTH(test_date)
) monthly_data
GROUP BY equipment_id
ORDER BY equipment_id;
```

## 성능 최적화 팁

### 1. 효율적인 필터링
```sql
-- 파티션 컬럼을 먼저 필터링
SELECT *
FROM large_table
WHERE partition_date >= '${StartDate}'  -- 파티션 컬럼 먼저
  AND partition_date <= '${EndDate}'
  AND equipment_id = '${EquipmentId}'   -- 인덱스 컬럼 다음
  AND other_conditions = '${OtherFilter}';

-- 불필요한 컬럼 제외
SELECT equipment_id, test_date, yield_rate  -- 필요한 컬럼만
FROM test_results
WHERE test_date >= '${StartDate}'
LIMIT ${MaxRows};  -- 행 수 제한
```

### 2. 효율적인 조인
```sql
-- 작은 테이블을 먼저 조인
SELECT *
FROM small_lookup_table s
JOIN large_fact_table l ON s.id = l.lookup_id
WHERE s.category = '${Category}'
  AND l.date_col >= '${StartDate}';
```

### 3. 적절한 데이터 타입 사용
```sql
-- 적절한 타입 캐스팅
SELECT 
    CAST(numeric_string AS DECIMAL(10,2)) as numeric_value,
    CAST(date_string AS TIMESTAMP) as timestamp_value
FROM source_table;
```

## Information Link+ 설정 예시

### 1. 동적 장비 목록
```sql
-- 파라미터: LineType (문자열)
SELECT DISTINCT equipment_id, equipment_name
FROM equipment_master
WHERE line_type = '${LineType}'
  AND status = 'ACTIVE'
ORDER BY equipment_name;
```

### 2. 계층적 필터 (제품군 → 제품)
```sql
-- 1단계: 제품군 목록
SELECT DISTINCT product_family
FROM product_master
WHERE active_flag = 'Y'
ORDER BY product_family;

-- 2단계: 제품 목록 (제품군에 따라)
SELECT DISTINCT product_type, product_name
FROM product_master
WHERE product_family = '${ProductFamily}'
  AND active_flag = 'Y'
ORDER BY product_name;
```

### 3. 날짜 범위 검증
```sql
-- 유효한 데이터 날짜 범위 확인
SELECT 
    MIN(test_date) as min_date,
    MAX(test_date) as max_date,
    COUNT(DISTINCT test_date) as available_days
FROM test_results
WHERE equipment_id IN (${EquipmentList});
```

## 에러 처리 및 디버깅

### 1. 일반적인 에러 패턴
```sql
-- NULL 값 처리
SELECT 
    COALESCE(yield_rate, 0) as safe_yield,
    CASE 
        WHEN temperature IS NULL THEN 'NO_DATA'
        ELSE CAST(temperature AS STRING)
    END as temp_string
FROM process_data;

-- 0으로 나누기 방지
SELECT 
    CASE 
        WHEN total_units = 0 THEN 0
        ELSE ROUND(100.0 * pass_units / total_units, 2)
    END as yield_rate
FROM summary_table;
```

### 2. 데이터 검증 쿼리
```sql
-- 파라미터 값 확인
SELECT 
    '${ParameterName}' as param_value,
    COUNT(*) as matching_records
FROM target_table
WHERE column_name = '${ParameterName}';

-- 날짜 범위 검증
SELECT 
    CASE 
        WHEN '${StartDate}' <= '${EndDate}' THEN 'VALID'
        ELSE 'INVALID'
    END as date_range_check;
```

## 모범 사례

### 1. 쿼리 구조화
```sql
-- 명확한 구조와 주석
-- Purpose: 장비별 일간 수율 요약
-- Parameters: StartDate, EndDate, EquipmentList
-- Updated: 2024-01-15

SELECT 
    -- 기본 식별자
    equipment_id,
    DATE_FORMAT(test_datetime, 'yyyy-MM-dd') as test_date,
    
    -- 수율 계산
    COUNT(*) as total_tests,
    SUM(CASE WHEN result = 'PASS' THEN 1 ELSE 0 END) as pass_count,
    ROUND(
        100.0 * SUM(CASE WHEN result = 'PASS' THEN 1 ELSE 0 END) / COUNT(*), 
        2
    ) as daily_yield_rate,
    
    -- 품질 지표
    AVG(cycle_time) as avg_cycle_time,
    STDDEV(measurement_value) as measurement_variation
    
FROM test_results
WHERE test_datetime >= '${StartDate}'
  AND test_datetime < DATE_ADD('${EndDate}', 1)  -- 다음날 00:00 제외
  AND equipment_id IN (${EquipmentList})
  AND test_status = 'COMPLETED'  -- 완료된 테스트만
  
GROUP BY 
    equipment_id,
    DATE_FORMAT(test_datetime, 'yyyy-MM-dd')
    
HAVING COUNT(*) >= 10  -- 최소 샘플 수 보장

ORDER BY 
    equipment_id,
    test_date DESC;
```

### 2. 성능 고려사항
- 파티션 컬럼을 WHERE 절에 포함
- 필요한 컬럼만 SELECT
- 적절한 LIMIT 사용
- 인덱스가 있는 컬럼으로 조인
- 큰 테이블 조인 시 작은 테이블을 먼저

### 3. 유지보수성
- 명확한 주석 추가
- 파라미터 의미 문서화
- 쿼리 버전 관리
- 테스트 데이터로 검증

---

*이 가이드를 참고하여 Spotfire Information Link+에서 효율적이고 안정적인 Impala SQL 쿼리를 작성하시기 바랍니다.*