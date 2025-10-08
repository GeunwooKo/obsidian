# Etch 메서드 - 식각 공정

## 기본 사용법

```ruby
l1 = layer("1/0")
substrate = bulk
mask(l1).etch(0.3, :into => substrate)
output("1/0", substrate)
```

마스크가 그려진 위치의 재료를 제거하여 홀을 생성합니다.

## 문법

```ruby
etch(height, lateral, options...)
```

### 필수 매개변수
- **height**: 식각 깊이 (µm 단위, 필수)

### 선택 매개변수
- **lateral**: 측면 확장(언더컷) (µm 단위, 기본값 0)

## 주요 옵션

| 옵션 | 값 | 설명 |
|------|-----|------|
| `:mode` | `:square`, `:round`, `:octagon` | 프로파일 모드 (기본값: `:square`) |
| `:taper` | 각도 (도) | 테이퍼 각도 지정 (`:mode`와 함께 사용 불가) |
| `:bias` | 값 (µm) | 프로파일을 도형 내부로 이동 (양수 = 축소) |
| `:into` | 재료 또는 배열 | 식각할 재료 지정 (필수!) |
| `:through` | 재료 또는 배열 | 선택적 식각 (특정 재료 통과) |
| `:buried` | 깊이 (µm) | 표면 아래 깊이에서 식각 (캐비티 생성) |

## ⚠️ 중요: :into 옵션은 필수!

```ruby
# 식각할 재료를 반드시 지정해야 함
mask(l1).etch(0.3, :into => substrate)

# 여러 재료를 배열로 지정
mask(l1).etch(0.3, :into => [metal, oxide, substrate])
```

- 지정된 재료만 식각됨
- 지정하지 않은 재료는 식각 정지층(etch stop) 역할

## 프로파일 모드

### Square (사각형)
```ruby
# 기본 식각: 깊이 0.3 µm
etch(0.3, :into => substrate)
```
- 사각형 트렌치 생성

### 측면 확장 (언더컷)
```ruby
# 깊이 0.3 µm, 언더컷 0.1 µm
etch(0.3, 0.1, :into => substrate)
```
- 측면으로 0.1 µm 추가 제거
- 하단에서 마스크보다 커짐

### Round (타원형)
```ruby
# 타원형 프로파일
etch(0.3, 0.1, :mode => :round, :into => substrate)
```
- 수직축: 0.3 µm (깊이)
- 수평축: 0.1 µm (언더컷)
- 하단에서 마스크보다 커짐

### 음수 측면 확장
```ruby
# 상단에서 마스크와 정렬
etch(0.3, -0.1, :mode => :round, :into => substrate)
```
- 오버에칭 방지
- 상단이 마스크와 정렬됨

### Octagon (팔각형)
```ruby
# 계산상 효율적인 타원 근사
etch(0.3, 0.1, :mode => :octagon, :into => substrate)
```

## Bias 조정

```ruby
# 프로파일을 0.05 µm 내부로 이동
etch(0.3, 0.1, :mode => :round, :bias => 0.05, :into => substrate)
```
- 양수 bias: 트렌치 축소
- 상단 가장자리 위치 미세 조정

## Taper 모드

```ruby
# 10도 테이퍼 각도
etch(0.3, :taper => 10, :into => substrate)

# bias와 함께 사용
etch(0.3, :taper => 10, :bias => -0.1, :into => substrate)
```
- 원뿔형 트렌치 생성
- 측벽 각도 = 테이퍼 각도
- `:mode`와 함께 사용 불가

## 단차 식각 프로파일

```ruby
etch(0.3, 0.1, :mode => :round, :into => substrate)
```

30° 경사면과 수직 단차에서:
- 측벽 제거 두께 = 0.1 µm (언더컷과 동일)
- 경사면과 수직면 모두 프로파일에 따라 식각

## 선택적 식각 (기본 동작)

일반적으로 식각은 **공기와 :into 재료의 경계**에서만 발생:

```ruby
# 입력 레이어
m1 = layer("1/0")
m2 = layer("2/0")
substrate = bulk

# 정지층 생성
stop = mask(m2).grow(0.05, :into => substrate)

# 식각 시도 - stop 재료가 차단
mask(m1).etch(0.3, 0.1, :mode => :round, :into => substrate)

# 출력
output("0/0", substrate)
output("2/0", stop)
```

**결과**: 청색 재료(stop)가 공기/기판 경계를 차단하여 식각 방지

## Through 옵션

특정 재료를 **통과**하여 식각:

```ruby
# 입력 레이어
m1 = layer("1/0")
m2 = layer("2/0")
substrate = bulk

# 정지층 생성
stop = mask(m2).grow(0.05, :into => substrate)

# through 재료를 통과하여 식각
mask(m1).etch(0.3, 0.1, :mode => :round, 
              :into => substrate, :through => stop)

# 출력
output("0/0", substrate)
output("2/0", stop)
```

### Through 옵션 동작 방식
- `:through` 재료와 공기의 경계에서 식각 시작
- `:through` 재료를 통과 (소비하지 않음)
- `:into` 재료를 식각
- 선택적 식각을 역전시킴

## Buried 옵션 (캐비티 생성)

표면 아래 깊이에서 식각:

```ruby
# 입력 레이어
m1 = layer("1/0")
substrate = bulk

# 표면 0.4 µm 아래에서 식각
mask(m1).etch(0.3, 0.1, :mode => :round, 
              :into => substrate, :buried => 0.4)

# 출력
output("0/0", substrate)
```

### 특징
- 표면이 아닌 지정된 깊이에서 식각 시작
- 식각이 위아래로 진행
- 극단적인 경우 (buried > etch depth): 내부 캐비티 생성
- MEMS, 센서 구조 모델링에 유용

## 실전 예제

### 예제 1: 금속 패터닝
```ruby
# 레이어 준비
m1 = layer("1/0")
m1i = m1.inverted  # 극성 반전

# 금속 증착
metal1 = deposit(0.25)
substrate = bulk

# 반전된 마스크로 금속 식각
mask(m1i).etch(0.3, 0.1, :mode => :round, :into => [metal1, substrate])

# 출력
output("0/0", substrate)
output("1/0", metal1)
```

**포인트**: 
- 그려진 부분을 남기려면 마스크 반전 필요
- 금속과 기판을 모두 식각 대상으로 지정

### 예제 2: TSV (Through Silicon Via)
```ruby
# 웨이퍼 두께 설정
depth(100)

# 뒷면 공정
flip

# 테이퍼 식각
via_mask = layer("VIA")
substrate = bulk

mask(via_mask).etch(100, :taper => 4, :into => substrate)

# 금속 충진
metal = deposit(50)
planarize(:downto => substrate, :into => metal)

# 출력
output("0/0", substrate)
output("1/0", metal)
```

### 예제 3: 다층 식각 (선택적 정지)

#### 문제: 서로 다른 깊이의 금속에서 정지

```ruby
# 마스크 분리를 통한 해결
mmetal1 = layer("1/0")
mmetal2 = layer("2/0")
mvia = layer("3/0")

# Via를 금속 위치에 따라 분리
mvia_on_metal2 = mvia.and(mmetal2)
mvia_on_metal1 = mvia.not(mmetal2)

# 공정
metal1 = mask(mmetal1).grow(1.0)
oxide1 = deposit(3.0)
planarize(:into => oxide1, :less => 1.0)

metal2 = mask(mmetal2).grow(1.0)
oxide2 = deposit(3.0)
planarize(:into => oxide2, :less => 1.0)

# 깊이별 식각
mask(mvia_on_metal1).etch(6.0, :into => [oxide1, oxide2])
mask(mvia_on_metal2).etch(3.0, :into => oxide2)

# 금속 충진
metal3 = mask(layer("4/0")).grow(6.0)
planarize(:into => metal3, :less => 5.0)
```

**핵심 아이디어**:
- Via 마스크를 위치별로 분리
- 각 위치에 맞는 깊이로 개별 식각

### 예제 4: MEMS 캐비티
```ruby
cavity_mask = layer("CAVITY")
substrate = bulk

# 표면 10 µm 아래에 캐비티 생성
mask(cavity_mask).etch(5.0, 2.0, :mode => :round,
                       :into => substrate, :buried => 10.0)

output("0/0", substrate)
```

## Uniform 식각

마스크 없이 전체 웨이퍼 식각:

```ruby
# 다음 두 방식은 동일
etch(0.5, :into => oxide)
all.etch(0.5, :into => oxide)

# lateral 및 옵션 사용 가능
etch(0.5, 0.1, :mode => :round, :into => [oxide, nitride])
```

## 식각 vs Grow :into

| 특징 | Etch | Grow :into |
|------|------|------------|
| 대체 재료 | 공기 (제거) | 새 재료 |
| 방향 | 아래로 | 아래로 |
| 용도 | 패턴 제거 | 이온주입, 확산 |

## 팁과 주의사항

1. **:into는 필수!**
   - 식각할 재료를 항상 명시
   - 여러 재료는 배열 `[mat1, mat2]`로 지정

2. **식각 정지층**
   - `:into`에 없는 재료는 자동 정지층
   - 의도하지 않은 정지 방지 주의

3. **마스크 극성**
   - 그려진 부분 제거: 마스크 그대로 사용
   - 그려진 부분 남김: `inverted` 사용

4. **언더컷 방향**
   - 양수: 하단에서 마스크보다 커짐
   - 음수: 상단에서 마스크와 정렬

5. **Through vs 기본**
   - 기본: 공기/재료 경계에서 식각
   - Through: 특정 재료/공기 경계에서 식각

6. **Buried 깊이**
   - 깊이 < 식각 깊이: 표면까지 도달
   - 깊이 > 식각 깊이: 내부 캐비티

## 일반적인 문제 해결

### 문제 1: 식각이 안 됨
```ruby
# 원인: :into에 재료 지정 안 함
mask(m1).etch(0.3)  # ❌ 오류!

# 해결: :into 추가
mask(m1).etch(0.3, :into => substrate)  # ✅
```

### 문제 2: 의도하지 않은 정지
```ruby
# 원인: 중간층이 :into에 없음
mask(via).etch(5.0, :into => substrate)  # oxide에서 정지

# 해결: 모든 재료 포함
mask(via).etch(5.0, :into => [oxide, substrate])  # ✅
```

### 문제 3: 잘못된 마스크 극성
```ruby
# 그려진 부분을 남기고 싶은데 제거됨
mask(pattern).etch(...)  # 그려진 부분 제거

# 해결: 마스크 반전
mask(pattern.inverted).etch(...)  # 그려진 부분 보존
```

## 다음 단계

- [[04_고급_기능]] - 평탄화, 뒷면 공정, 디버깅
- [[05_실전_예제]] - 완전한 공정 플로우

## 태그
#etch #식각 #트렌치 #via #캐비티
