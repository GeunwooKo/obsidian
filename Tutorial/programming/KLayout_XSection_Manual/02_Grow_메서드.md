# Grow 메서드 - 증착 및 재료 변환

## 기본 사용법

```ruby
l1 = layer("1/0")
metal = mask(l1).grow(0.3)
output("1/0", metal)
```

마스크가 그려진 위치에 사각형 프로파일의 재료를 증착합니다.

## 문법

```ruby
grow(height, lateral, options...)
```

### 필수 매개변수
- **height**: 증착 두께 (µm 단위, 필수)

### 선택 매개변수
- **lateral**: 측면 확장(확산) (µm 단위, 기본값 0)

## 주요 옵션

| 옵션 | 값 | 설명 |
|------|-----|------|
| `:mode` | `:square`, `:round`, `:octagon` | 프로파일 모드 (기본값: `:square`) |
| `:taper` | 각도 (도) | 테이퍼 각도 지정 (`:mode`와 함께 사용 불가) |
| `:bias` | 값 (µm) | 프로파일을 도형 내부로 이동 (양수 = 축소) |
| `:on` | 재료 또는 배열 | 선택적 증착 (특정 재료 위에만) |
| `:into` | 재료 또는 배열 | 위로 자라지 않고 재료 변환 |
| `:through` | 재료 또는 배열 | `:into`와 함께, 선택적 변환 |
| `:buried` | 깊이 (µm) | 표면 아래 깊이에서 변환 (깊은 이온주입) |

## 프로파일 모드

### Square (사각형)
```ruby
# 기본 증착: 두께 0.3 µm
grow(0.3)
```
- 사각형 프로파일로 증착

### 측면 확장 (Square)
```ruby
# 두께 0.3 µm, 측면 확장 0.1 µm
grow(0.3, 0.1)
```
- 측면으로 0.1 µm 추가 확장
- 선폭이 양쪽으로 0.1 µm씩 증가

### Round (타원형)
```ruby
# 타원형 프로파일
grow(0.3, 0.1, :mode => :round)
```
- 수직축: 0.3 µm (두께)
- 수평축: 0.1 µm (측면 확장)
- 하단에서 마스크보다 커짐 (overgrow)

### 음수 측면 확장
```ruby
# 하단에서 마스크와 정렬
grow(0.3, -0.1, :mode => :round)
```
- Overgrow 방지
- 하단이 마스크와 정렬됨

### Octagon (팔각형)
```ruby
# 계산상 효율적인 타원 근사
grow(0.3, 0.1, :mode => :octagon)
```

## Bias 조정

```ruby
# 프로파일을 0.05 µm 내부로 이동
grow(0.3, 0.1, :mode => :round, :bias => 0.05)
```
- 양수 bias: 도형 축소 (선폭 2×bias만큼 감소)
- 하단 가장자리 위치 미세 조정

## Taper 모드

```ruby
# 10도 테이퍼 각도
grow(0.3, :taper => 10)

# bias와 함께 사용
grow(0.3, :taper => 10, :bias => -0.1)
```
- 원뿔형 패치 생성
- 측벽 각도 = 테이퍼 각도
- `:mode`와 함께 사용 불가

## 단차 피복 (Step Coverage)

```ruby
grow(0.3, 0.1, :mode => :round)
```

30° 경사면과 수직 단차에서:
- 측벽 두께 = 0.1 µm (측면 확장과 동일)
- 트렌치 하단 충진
- 작은 트렌치는 완전 충진

## 선택적 증착 (:on 옵션)

특정 재료 표면에만 증착:

```ruby
# 입력 레이어
m1 = layer("1/0")
m2 = layer("2/0")

# 정지층 증착
stop = mask(m2).grow(0.05)

# 기판 표면에만 금속 증착
metal = mask(m1).grow(0.3, 0.1, :mode => :round, :on => bulk)

# 출력
output("0/0", bulk)
output("1/0", metal)
output("2/0", stop)
```

### 여러 재료 지정
```ruby
# 배열로 여러 재료 지정
metal = mask(m1).grow(0.3, :on => [substrate, oxide])
```

## 재료 변환 (:into 옵션)

위로 자라는 대신 아래 재료를 변환 (이온주입 등):

```ruby
# 입력 레이어
m1 = layer("1/0")
substrate = bulk

# 기판 내부로 재료 변환
metal = mask(m1).grow(0.3, 0.1, :mode => :round, :into => substrate)

# 출력
output("0/0", substrate)
output("1/0", metal)
```

### 동작 방식
- 방향이 역전됨 (아래로)
- `:into`에 지정된 재료가 소비되고 새 재료로 대체
- 식각(etch)과 유사하지만 "공기" 대신 새 재료로 대체

### 여러 재료 변환
```ruby
# 여러 재료를 배열로 지정
implant = mask(m1).grow(0.2, 0.1, :into => [substrate, oxide])
```

## Through 옵션

`:into`와 함께 사용하여 선택적 변환:

```ruby
# 입력 레이어
m1 = layer("1/0")
m2 = layer("2/0")
substrate = bulk

# 정지층 생성
stop = mask(m2).grow(0.05, :into => substrate)

# through 재료를 통과하여 into 재료 변환
metal = mask(m1).grow(0.3, 0.1, :mode => :round, 
                      :through => stop, :into => substrate)

# 출력
output("0/0", substrate)
output("1/0", metal)
output("2/0", stop)
```

### 동작 방식
- `:through` 재료와 공기의 경계에서 시작
- `:through` 재료를 통과 (소비하지 않음)
- `:into` 재료를 소비하고 변환
- `:on` 옵션과 유사하지만 변환에 사용

## Buried 옵션 (깊은 이온주입)

표면 아래 깊이에서 재료 변환:

```ruby
# 입력 레이어
m1 = layer("1/0")
substrate = bulk

# 표면 0.4 µm 아래에서 변환
metal = mask(m1).grow(0.3, 0.1, :mode => :round, 
                      :into => substrate, :buried => 0.4)

# 출력
output("0/0", substrate)
output("1/0", metal)
```

### 특징
- 표면이 아닌 지정된 깊이에서 공정 적용
- 깊은 이온주입 모델링에 주로 사용
- 변환 영역이 깊이 위아래로 확장

## 실전 예제

### 예제 1: 기본 금속 증착
```ruby
metal_mask = layer("METAL1")
metal = mask(metal_mask).grow(0.5, 0.2, :mode => :round)
output("10/0", metal)
```

### 예제 2: 선택적 에피택시
```ruby
active = layer("ACTIVE")
substrate = bulk

# 액티브 영역에만 에피 성장
epi = mask(active).grow(0.3, :on => substrate, :mode => :round)

output("0/0", substrate)
output("1/0", epi)
```

### 예제 3: 웰 이온주입
```ruby
nwell_mask = layer("NWELL")
substrate = bulk

# 1 µm 깊이, 0.5 µm 확산
nwell = mask(nwell_mask).grow(1.0, 0.5, :mode => :round, :into => substrate)

output("0/0", substrate)
output("2/0", nwell)
```

### 예제 4: 얕은/깊은 이중 이온주입
```ruby
implant_mask = layer("IMPLANT")
substrate = bulk

# 얕은 이온주입
shallow = mask(implant_mask).grow(0.1, 0.05, 
                                  :mode => :round, :into => substrate)

# 깊은 이온주입
deep = mask(implant_mask).grow(0.3, 0.2, 
                               :mode => :round, :into => substrate, 
                               :buried => 0.5)

output("0/0", substrate)
output("3/0", shallow)
output("4/0", deep)
```

## Uniform 증착

마스크 없이 전체 웨이퍼에 균일 증착:

```ruby
# 다음 두 방식은 동일
oxide = deposit(0.5)
oxide = all.grow(0.5)

# lateral 및 옵션 사용 가능
oxide = deposit(0.5, 0.1, :mode => :round)
```

## 팁과 주의사항

1. **측면 확장 방향**
   - 양수: 하단에서 마스크보다 커짐
   - 음수: 하단에서 마스크와 정렬

2. **bias 효과**
   - 양수: 도형 축소 (보수적)
   - 음수: 도형 확대 (공격적)

3. **모드 선택**
   - `:square`: 빠르고 단순
   - `:round`: 현실적인 프로파일
   - `:octagon`: 절충안
   - `:taper`: 특수 각도 필요시

4. **재료 변환 vs 증착**
   - `:into` 없음: 위로 증착
   - `:into` 있음: 재료 변환 (이온주입, 확산 등)

## 다음 단계

- [[03_Etch_메서드]] - 식각 공정
- [[04_고급_기능]] - 평탄화 및 뒷면 공정

## 태그
#grow #증착 #deposition #이온주입 #implant
