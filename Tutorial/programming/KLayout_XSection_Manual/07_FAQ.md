# FAQ - 자주 묻는 질문

## 설치 및 시작

### Q1. KLayout XSection을 어떻게 설치하나요?

**A:** KLayout 0.25 이상 버전에서:
1. Tools → Manage Packages 메뉴 실행
2. "xsection" 검색
3. 체크박스 선택 후 Apply 클릭

---

### Q2. XS 파일을 어떻게 실행하나요?

**A:** 
1. KLayout에서 레이아웃 파일 열기
2. Tools → XSection Scripts → [스크립트 선택]
3. 또는 직접 .xs 파일 실행

---

### Q3. 예제 파일은 어디에 있나요?

**A:** 설치 후 위치:
- Windows: `C:\Users\[사용자]\.klayout\salt\xsection\docs\`
- Linux/Mac: `~/.klayout/salt\xsection\docs\`

`cmos.xs` 파일이 좋은 시작점입니다.

---

## 기본 개념

### Q4. XSection은 정확한 공정 시뮬레이터인가요?

**A:** 아니오. XSection은:
- ✅ 문서화 및 시각화 도구
- ✅ 교육 및 개념 설명
- ❌ 정밀 공정 시뮬레이션
- ❌ 정량적 분석

실제 물리 현상을 완벽히 모델링하지 않으므로 참고용으로만 사용하세요.

---

### Q5. 마이크로미터와 나노미터 변환은?

**A:** XSection은 µm 단위 사용:
- 1 nm = 0.001 µm
- 10 nm = 0.01 µm
- 100 nm = 0.1 µm
- 1000 nm = 1 µm

```ruby
gate_ox = deposit(0.003)  # 3 nm
poly = deposit(0.2)       # 200 nm
```

---

### Q6. 레이어 번호는 어떻게 지정하나요?

**A:** 
```ruby
# 레이어/데이터타입 형식
layer("1/0")      # 레이어 1, 데이터타입 0
layer("METAL1")   # 이름으로
layer("file.gds", "1/0")  # 파일에서
```

---

## 공정 관련

### Q7. Grow와 Etch의 차이는?

**A:**

| | Grow | Etch |
|---|------|------|
| 기본 동작 | 재료 추가 | 재료 제거 |
| :into 없이 | 위로 증착 | ❌ 오류 (필수!) |
| :into 있으면 | 재료 변환 | 재료 제거 |
| 결과 | 새 재료 | 공기 |

---

### Q8. :into 옵션을 왜 항상 써야 하나요?

**A:** 
- **Grow**: 선택사항 (변환 시만 필요)
- **Etch**: 필수! (제거할 재료 지정)

```ruby
# Grow - :into 선택
metal = mask(m1).grow(0.3)  # ✅ OK
implant = mask(m1).grow(0.2, :into => substrate)  # ✅ OK

# Etch - :into 필수
mask(m1).etch(0.3)  # ❌ 오류!
mask(m1).etch(0.3, :into => substrate)  # ✅ OK
```

---

### Q9. 마스크 극성을 어떻게 반전하나요?

**A:**
```ruby
# 원본 마스크
mask_orig = layer("1/0")

# 반전
mask_inv = mask_orig.inverted

# 또는 in-place
mask_orig.invert
```

**사용 사례:**
- 그려진 부분 제거: 마스크 그대로
- 그려진 부분 보존: 마스크 반전

---

### Q10. lateral 값이 양수/음수일 때 차이는?

**A:**

**Grow:**
- 양수: 하단에서 마스크보다 큼 (overgrow)
- 음수: 하단에서 마스크와 정렬

**Etch:**
- 양수: 하단에서 마스크보다 큼 (undercut)
- 음수: 상단에서 마스크와 정렬

```ruby
# Overgrow
grow(0.3, 0.1, :mode => :round)  # 하단 확장

# No overgrow
grow(0.3, -0.1, :mode => :round)  # 하단 정렬
```

---

### Q11. Mode 옵션 중 어떤 것을 선택해야 하나요?

**A:**

| Mode | 속도 | 현실성 | 용도 |
|------|------|--------|------|
| :square | 빠름 | 낮음 | 빠른 확인 |
| :octagon | 중간 | 중간 | 절충안 |
| :round | 느림 | 높음 | 최종 문서 |

일반적으로 `:round` 추천 (현실적인 프로파일)

---

### Q12. 평탄화(CMP)의 정지 조건은?

**A:** 세 가지 중 하나만 사용:

```ruby
# 1. 재료까지 (가장 일반적)
planarize(:downto => substrate, :into => metal)

# 2. 상대 깊이
planarize(:less => 0.5, :into => oxide)

# 3. 절대 높이
planarize(:to => 1.0, :into => metal)
```

---

## 고급 기능

### Q13. 뒷면 공정은 어떻게 하나요?

**A:**
```ruby
# 1. 웨이퍼 두께 설정
depth(100)

# 2. 앞면 공정
# ...

# 3. 뒷면으로 전환
flip

# 4. 뒷면 공정
# ...

# 5. 다시 앞면으로 (필요시)
flip
```

⚠️ `depth`를 먼저 설정해야 함!

---

### Q14. Through 옵션은 언제 사용하나요?

**A:** 특정 재료를 **통과**하여 공정할 때:

```ruby
# 일반: 공기/재료 경계에서만 식각
mask(m1).etch(0.3, :into => substrate)
# → stop 재료가 있으면 식각 안 됨

# Through: stop 재료를 통과하여 식각
mask(m1).etch(0.3, :into => substrate, :through => stop)
# → stop 재료 통과해서 substrate 식각
```

---

### Q15. Buried 옵션은 무엇인가요?

**A:** 표면 아래 깊이에서 공정:

```ruby
# 표면 0.5 µm 아래에서 이온주입
implant = mask(m1).grow(0.3, 0.1, 
                        :mode => :round,
                        :into => substrate,
                        :buried => 0.5)
```

**용도:**
- 깊은 이온주입
- 내부 캐비티 (MEMS)

---

### Q16. 여러 재료를 동시에 처리하려면?

**A:** 배열 사용:

```ruby
# 여러 재료 식각
mask(via).etch(0.5, :into => [oxide1, oxide2, metal])

# 여러 재료 평탄화
planarize(:less => 0.3, :into => [metal, oxide])

# 선택적 증착
metal = mask(m1).grow(0.3, :on => [substrate, poly])
```

---

## 디버깅

### Q17. 중간 공정을 확인하려면?

**A:** 세 가지 방법:

```ruby
# 1. Snapshot (자동 계속)
metal = deposit(0.5)
snapshot("Metal deposition")

# 2. Pause (수동 계속)
metal = deposit(0.5)
pause("Check metal thickness")

# 3. 중간 Output
metal = deposit(0.5)
output("100/0", metal)  # 디버그 레이어
```

---

### Q18. 식각이 작동하지 않아요!

**A:** 체크리스트:

```ruby
# ❌ :into 빠짐
mask(m1).etch(0.3)

# ✅ :into 추가
mask(m1).etch(0.3, :into => substrate)

# ❌ 재료가 변수에 없음
mask(m1).etch(0.3, :into => bulk)  # 오류!

# ✅ 변수에 저장
substrate = bulk
mask(m1).etch(0.3, :into => substrate)
```

---

### Q19. 경계에 이상한 무늬가 생겨요

**A:** `extend` 값 증가:

```ruby
# 문제: lateral이 extend보다 큼
grow(0.5, 0.3, ...)  # lateral=0.3, extend=2 (기본값)

# 해결: extend 증가
extend(1.0)
grow(0.5, 0.3, ...)
```

---

### Q20. 정밀도가 부족해요

**A:** `dbu` 또는 `delta` 조정:

```ruby
# 해상도 향상
dbu(0.0001)  # 0.1 nm

# 정밀도 향상
delta(0.0005)  # 0.5 nm
```

⚠️ 너무 작은 값은 좌표 오버플로우 발생 가능

---

## 고급 Ruby

### Q21. 변수 복사 시 주의사항은?

**A:**
```ruby
# ❌ 잘못된 복사 (참조)
a = layer("1/0")
b = a
a.invert  # b도 반전됨!

# ✅ 올바른 복사
a = layer("1/0")
b = a.dup  # 또는 a.inverted
a.invert  # b는 영향 없음
```

---

### Q22. 반복 공정을 간단히 하려면?

**A:**
```ruby
# 다층 금속
(1..5).each do |i|
  metal_mask = layer("#{i*10}/0")
  
  ild = deposit(0.5)
  planarize(:less => 0.3, :into => ild)
  
  mask(metal_mask.inverted).etch(0.4, :into => ild)
  metal = deposit(0.3)
  planarize(:downto => ild, :into => metal)
  
  output("#{i*10}/0", metal)
end
```

---

### Q23. 조건부 공정은?

**A:**
```ruby
# 변수로 제어
USE_CMP = true
METAL_THICKNESS = 0.5

metal = deposit(METAL_THICKNESS)

if USE_CMP
  planarize(:less => 0.1, :into => metal)
end
```

---

## 실전 팁

### Q24. 서로 다른 깊이의 Via 식각은?

**A:** 마스크를 분리:

```ruby
via_mask = layer("VIA")
metal1 = layer("M1")
metal2 = layer("M2")

# Via를 위치별로 분리
via_to_m1 = via_mask.not(metal2)
via_to_m2 = via_mask.and(metal2)

# 각각 다른 깊이로 식각
mask(via_to_m1).etch(0.8, :into => [ild1, ild2])
mask(via_to_m2).etch(0.4, :into => ild2)
```

---

### Q25. 여러 재료를 하나로 출력하려면?

**A:** Boolean OR 사용:

```ruby
metal1 = mask(m1).grow(0.3)
metal2 = mask(m2).grow(0.3)
via = deposit(0.4)

# 모든 금속 병합
all_metal = metal1.or(metal2).or(via)
output("1/0", all_metal)
```

---

### Q26. 레이어 속성 파일(.lyp)은?

**A:** 색상 및 표시 설정:

```ruby
# xs 파일에서 지정
layers_file("./process_colors.lyp")
```

.lyp 파일은 KLayout에서 생성:
1. Layer 팔레트에서 색상 설정
2. File → Save Layer Properties

---

### Q27. 공정 단계마다 이미지 저장하려면?

**A:** Snapshot과 스크린샷 조합:

```ruby
# 각 단계
poly = deposit(0.2)
snapshot("Poly deposition")

mask(poly_mask.inverted).etch(0.25, :into => poly)
snapshot("Poly etch")

# 수동으로 각 탭 스크린샷
```

또는 외부 스크립트로 자동화 가능

---

### Q28. 좌표계는 어떻게 되나요?

**A:**
- X축: 레이아웃 좌표
- Y축: 룰러(ruler) 방향
- Z축: 웨이퍼 두께 방향 (위가 양수)

원점(0,0,0)은 웨이퍼 표면

---

### Q29. Taper 각도는 어떻게 정의되나요?

**A:** 측벽 각도 (수직 기준):

```ruby
# 10도 테이퍼
etch(0.5, :taper => 10, :into => substrate)
```

- 0도 = 수직
- 양수 = 상단이 좁음
- 음수 = 상단이 넓음 (일부 경우)

---

### Q30. 식각 선택비는 지원하나요?

**A:** 아니오. 모든 `:into` 재료는 동일한 속도로 식각:

```ruby
# oxide와 substrate 동일 속도로 식각
mask(m1).etch(0.5, :into => [oxide, substrate])
```

선택적 식각은 `:into`에서 재료 제외로 구현:
```ruby
# substrate만 식각 (oxide는 정지층)
mask(m1).etch(0.5, :into => substrate)
```

---

## 문제 해결

### Q31. "Result too long, truncated" 오류는?

**A:** 이건 XSection 오류가 아님. 다른 도구(Gmail 등)의 메시지. XSection에서는 발생하지 않음.

---

### Q32. 스크립트가 느려요

**A:** 최적화 방법:

1. **Mode 변경**: `:round` → `:square`
2. **DBU 증가**: 더 큰 격자
3. **Delta 증가**: 낮은 정밀도
4. **복잡도 감소**: 불필요한 단계 제거

```ruby
# 빠른 버전
dbu(0.001)  # 1 nm (기본보다 큼)
delta(0.005)  # 5 nm
mask(m1).grow(0.3, 0.1, :mode => :square)
```

---

### Q33. 재료가 사라졌어요!

**A:** 체크사항:

1. **Output 확인**: 해당 재료 출력했는지?
2. **변수 추적**: 재료가 변수에 저장되었는지?
3. **덮어쓰기**: 같은 레이어에 여러 번 output?

```ruby
# ❌ 문제
output("1/0", metal1)
output("1/0", metal2)  # metal1 덮어씀!

# ✅ 해결
output("1/0", metal1.or(metal2))
```

---

### Q34. XSection과 Python은?

**A:** XSection은 **Ruby 전용**:
- Python 버전 없음
- Python에서 KLayout API로 비슷한 기능 구현 가능
- 하지만 XSection의 단순함 상실

---

### Q35. 실제 공정과 차이가 많이 나요

**A:** 정상입니다. XSection의 한계:

**지원 안 함:**
- 3D 효과 (룰러 밖 구조)
- 복잡한 물리 (확산, 응력 등)
- 선택비, 로딩 효과
- Line-of-sight 증착

**용도:**
- 개념적 이해
- 문서화
- 교육

정밀 시뮬레이션은 Sentaurus, Comsol 등 사용

---

## 추가 학습

### Q36. Ruby를 더 배우고 싶어요

**A:** 리소스:
- https://www.ruby-lang.org
- https://try.ruby-lang.org (인터랙티브)
- KLayout Ruby 문서

XSection에 필요한 Ruby:
- 변수, 함수 호출
- 배열 `[a, b, c]`
- 해시 옵션 `:key => value`
- 기본 제어 구조 (if, each)

---

### Q37. 더 많은 예제는?

**A:**
- GitHub: https://github.com/klayoutmatthias/xsection
- KLayout 포럼: https://www.klayout.de/forum
- 이 매뉴얼: [[05_실전_예제]]

---

### Q38. 버그를 발견했어요

**A:** 리포트:
- GitHub Issues: https://github.com/klayoutmatthias/xsection/issues
- KLayout 포럼: https://www.klayout.de/forum

포함할 정보:
- XSection 버전
- KLayout 버전
- 최소 재현 스크립트
- 예상/실제 결과

---

### Q39. 기여하고 싶어요

**A:** 환영합니다!
- Fork: https://github.com/klayoutmatthias/xsection
- 수정 후 Pull Request
- 커뮤니티 프로젝트입니다

---

### Q40. 상업적으로 사용 가능한가요?

**A:** 
- KLayout: GPL 라이선스
- XSection: GPL 라이선스
- 상업적 사용 가능
- 라이선스 조건 준수 필요

자세한 내용은 LICENSE 파일 참조

---

## 다음 단계

- [[00_시작하기]] - 처음부터 다시
- [[06_참고자료]] - API 전체 레퍼런스
- [[05_실전_예제]] - 더 많은 예제

## 도움이 필요하신가요?

- 🌐 공식 문서: https://klayoutmatthias.github.io/xsection/
- 💬 포럼: https://www.klayout.de/forum
- 🐛 이슈: https://github.com/klayoutmatthias/xsection/issues

## 태그
#faq #troubleshooting #tips #howto
