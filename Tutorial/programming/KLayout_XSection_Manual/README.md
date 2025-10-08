# KLayout XSection 매뉴얼 인덱스

> KLayout XSection은 평면 레이아웃을 수직 단면도로 변환하는 스크립트 도구입니다.  
> 반도체 공정을 시각화하고 문서화하는데 사용됩니다.

## 📚 목차

### 기초
1. **[[00_시작하기]]** - XSection 소개 및 첫 번째 스크립트
2. **[[01_레이어_연산]]** - Boolean 연산, 크기 조정, 마스크 생성
3. **[[02_Grow_메서드]]** - 증착 및 재료 변환
4. **[[03_Etch_메서드]]** - 식각 공정

### 고급
5. **[[04_고급_기능]]** - 평탄화, 뒷면 공정, 디버깅
6. **[[05_실전_예제]]** - CMOS, TSV, MEMS, FinFET 등
7. **[[06_참고자료]]** - API 레퍼런스 및 빠른 참조
8. **[[07_FAQ]]** - 자주 묻는 질문

---

## 🚀 빠른 시작

### 설치
```
Tools → Manage Packages → "xsection" 검색 → 설치
```

### 첫 번째 스크립트
```ruby
# 입력 레이어
m1 = layer("1/0")

# 금속 증착
metal = mask(m1).grow(0.3, 0.1, :mode => :round)

# 출력
output("0/0", bulk)
output("1/0", metal)
```

---

## 🔑 핵심 개념

### 스크립트 구조
1. **레이어 준비**: `layer()`, Boolean 연산
2. **공정 설명**: `grow()`, `etch()`, `planarize()`
3. **출력**: `output()`

### 주요 함수

| 함수 | 용도 | 예제 |
|------|------|------|
| `layer()` | 레이어 가져오기 | `layer("1/0")` |
| `mask()` | 마스크 생성 | `mask(l1)` |
| `grow()` | 증착/변환 | `grow(0.3, 0.1)` |
| `etch()` | 식각 | `etch(0.3, :into => sub)` |
| `deposit()` | 균일 증착 | `deposit(0.5)` |
| `planarize()` | 평탄화 | `planarize(:less => 0.3)` |
| `output()` | 출력 | `output("1/0", metal)` |

---

## 💡 자주 사용하는 패턴

### 금속 패터닝
```ruby
metal_mask = layer("METAL")

oxide = deposit(0.5)
mask(metal_mask.inverted).etch(0.6, :into => oxide)
metal = deposit(0.5)
planarize(:downto => oxide, :into => metal)
```

### 이온주입
```ruby
implant_mask = layer("IMPLANT")
substrate = bulk

implant = mask(implant_mask).grow(0.2, 0.1, 
                                   :mode => :round,
                                   :into => substrate)
```

### Via 형성
```ruby
via_mask = layer("VIA")

ild = deposit(0.6)
mask(via_mask).etch(0.6, :into => ild)
via = deposit(0.6)
planarize(:downto => ild, :into => via)
```

---

## ⚠️ 주의사항

### 필수 사항
- ✅ **Etch에는 :into 필수**: `etch(0.3, :into => substrate)`
- ✅ **bulk은 변수에 저장**: `substrate = bulk`
- ✅ **단위는 마이크로미터**: `0.003` = 3 nm

### 일반적인 실수
- ❌ `mask(m1).etch(0.3)` → `:into` 없음
- ❌ `a = layer("1/0"); b = a` → 참조, 복사 아님
- ❌ `output("1/0", bulk)` → bulk 직접 사용

---

## 📊 옵션 빠른 참조

### Grow/Etch 공통 옵션

| 옵션 | 값 | 설명 |
|------|-----|------|
| `:mode` | `:square`, `:round`, `:octagon` | 프로파일 |
| `:taper` | 각도(도) | 테이퍼 |
| `:bias` | 거리(µm) | 이동 |

### Grow 전용
- `:on` - 선택적 증착
- `:into` - 재료 변환
- `:through` - 통과 재료
- `:buried` - 깊이

### Etch 전용
- `:into` - **(필수)** 식각 재료
- `:through` - 통과 재료
- `:buried` - 깊이

---

## 🛠️ 문제 해결

### 식각 안 됨?
```ruby
# ❌ 오류
mask(m1).etch(0.3)

# ✅ 수정
mask(m1).etch(0.3, :into => substrate)
```

### 경계 아티팩트?
```ruby
extend(5)  # 기본 2보다 크게
```

### 마스크 극성?
```ruby
# 그려진 부분 보존 → 반전
mask(pattern.inverted).etch(...)
```

---

## 🎯 학습 경로

### 초보자
1. [[00_시작하기]] - 기본 개념
2. [[01_레이어_연산]] - 레이어 다루기
3. [[02_Grow_메서드]] - 증착 배우기
4. [[07_FAQ]] - Q&A

### 중급자
1. [[03_Etch_메서드]] - 식각 마스터
2. [[04_고급_기능]] - 평탄화, 뒷면 공정
3. [[05_실전_예제]] - 실전 프로젝트

### 고급자
1. [[06_참고자료]] - 전체 API
2. Ruby 활용 (조건문, 반복문)
3. 복잡한 공정 플로우

---

## 🔗 외부 리소스

### 공식 문서
- 📖 [XSection 홈](https://klayoutmatthias.github.io/xsection/)
- 📖 [소개](https://klayoutmatthias.github.io/xsection/DocIntro)
- 📖 [레퍼런스](https://klayoutmatthias.github.io/xsection/DocReference)

### 커뮤니티
- 💻 [GitHub](https://github.com/klayoutmatthias/xsection)
- 💬 [KLayout 포럼](https://www.klayout.de/forum)

### KLayout
- 🏠 [KLayout 홈](https://www.klayout.de)
- 📚 [KLayout 문서](https://www.klayout.de/doc.html)

---

## 📝 단위 변환표

| nm | µm | 용도 |
|----|----|------|
| 1 | 0.001 | 원자층 |
| 3 | 0.003 | 게이트 산화막 |
| 10 | 0.01 | 터널링 산화막 |
| 50 | 0.05 | 얇은 막 |
| 100 | 0.1 | 질화막 |
| 200 | 0.2 | 폴리실리콘 |
| 500 | 0.5 | ILD |
| 1000 | 1.0 | 두꺼운 금속 |

---

## 🏷️ 태그 인덱스

### 공정별
- #증착 #deposition - [[02_Grow_메서드]]
- #식각 #etch - [[03_Etch_메서드]]
- #이온주입 #implant - [[02_Grow_메서드]]
- #평탄화 #cmp - [[04_고급_기능]]

### 구조별
- #cmos - [[05_실전_예제]]
- #tsv - [[05_실전_예제]]
- #mems - [[05_실전_예제]]
- #finfet - [[05_실전_예제]]

### 기능별
- #레이어연산 #boolean - [[01_레이어_연산]]
- #디버깅 #debugging - [[04_고급_기능]]
- #api #reference - [[06_참고자료]]

---

## 📋 체크리스트

### 스크립트 작성 전
- [ ] KLayout XSection 설치 완료
- [ ] 입력 레이아웃 파일 준비
- [ ] 레이어 정의 확인
- [ ] 공정 플로우 이해

### 스크립트 작성 중
- [ ] 레이어 가져오기
- [ ] 마스크 생성
- [ ] 공정 단계 구현
- [ ] 중간 결과 확인 (snapshot/pause)
- [ ] 출력 레이어 지정

### 디버깅
- [ ] :into 옵션 확인
- [ ] 마스크 극성 확인
- [ ] 단위 변환 확인
- [ ] extend 값 확인
- [ ] 변수 저장 확인

---

## 🎓 추가 학습 자료

### Ruby 기초
- 변수 및 할당
- 배열 `[a, b, c]`
- 해시 옵션 `:key => value`
- 조건문 `if ... end`
- 반복문 `(1..5).each`

### XSection 고급
- 다층 구조 자동화
- 조건부 공정
- 외부 파라미터 활용
- 스크립트 모듈화

---

## 💼 실무 활용

### 문서화
```ruby
# 공정 단계별 주석
# Step 1: Well formation
nwell = ...
snapshot("Well formation")

# Step 2: Gate stack
gate_ox = ...
snapshot("Gate stack")
```

### 검증
```ruby
# 설계 룰 체크
if metal_thickness < 0.3
  puts "Warning: Metal too thin"
end
```

### 파라미터화
```ruby
# 공정 파라미터
GATE_OX = 0.003
POLY = 0.2
ILD = 0.5

gate_ox = deposit(GATE_OX)
poly = deposit(POLY)
ild = deposit(ILD)
```

---

## ⚡ 성능 팁

### 최적화
1. **Mode 선택**: `:square` > `:octagon` > `:round`
2. **DBU 조정**: 필요한 정밀도만
3. **Delta 조정**: 작은 형상 무시
4. **Extend 최소화**: 필요한 만큼만

### 예제
```ruby
# 빠른 프리뷰
dbu(0.001)  # 1 nm
mask(m1).grow(0.3, :mode => :square)

# 최종 문서
dbu(0.0001)  # 0.1 nm
mask(m1).grow(0.3, :mode => :round)
```

---

## 🔍 용어 정리

- **Bulk**: 웨이퍼 기판
- **Mask**: 리소그래피 마스크
- **Layer**: 레이아웃 레이어
- **Material**: 재료 데이터
- **Grow**: 증착 또는 재료 변환
- **Etch**: 식각
- **Planarize**: 평탄화 (CMP)
- **DBU**: 데이터베이스 단위 (해상도)
- **Delta**: 정밀도 파라미터
- **Extend**: 처리 마진

---

## 📞 도움말

### 문제가 있을 때
1. [[07_FAQ]] 확인
2. [[06_참고자료]] API 참조
3. [KLayout 포럼](https://www.klayout.de/forum) 검색
4. [GitHub Issues](https://github.com/klayoutmatthias/xsection/issues) 확인

### 기여하기
- GitHub에서 Fork
- 개선사항 구현
- Pull Request 제출

---

## 📄 라이선스

- KLayout: GPL
- XSection: GPL
- 상업적 사용 가능
- 라이선스 조건 준수 필요

---

## 🌟 마무리

이 매뉴얼은 KLayout XSection의 완전한 가이드입니다.

**시작하기:** [[00_시작하기]]로 이동하여 첫 스크립트를 작성해보세요!

---

**최종 업데이트:** 2025년 10월  
**버전:** 1.0  
**작성자:** KLayout XSection 커뮤니티

## 태그
#index #목차 #getting-started #klayout #xsection
