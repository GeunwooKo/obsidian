# Programming 폴더

프로그래밍 언어 및 도구 사용 가이드 모음입니다.

## 📂 문서 목록

### Virtuoso SKILL
반도체 레이아웃 설계 자동화를 위한 SKILL 언어 가이드

| 문서 | 난이도 | 설명 |
|------|--------|------|
| [[Virtuoso Skill 가이드]] | ⭐ 초급 | 기본 문법, 데이터 타입, 레이아웃 함수 |
| [[Virtuoso Skill 고급 기법]] | ⭐⭐ 중급 | 파일 I/O, GUI, 성능 최적화, 실전 프로젝트 |
| [[Virtuoso Skill 실전 레시피]] | ⭐⭐⭐ 고급 | 바로 사용 가능한 14가지 레시피 |
| [[Virtuoso Skill 예제 모음]] | 📚 참고 | 100+ 코드 스니펫 모음 |
| [[Virtuoso Skill 디버깅 가이드]] | 🔧 도구 | 체계적인 디버깅 방법 |
| [[Virtuoso Skill 성능 최적화 가이드]] | ⚡ 최적화 | 성능 향상 기법 |
| [[Virtuoso Skill GUI 가이드]] | 🖥️ GUI | GUI 프로그래밍 |
| [[Virtuoso Skill 빠른참조]] | 📋 참고 | Cheat Sheet, 빠른 검색용 |

### Python
반도체 데이터 분석 및 자동화를 위한 Python 가이드

| 문서 | 난이도 | 설명 |
|------|--------|------|
| [[Python 기초 가이드]] | ⭐ 초급 | 기본 문법, 반도체 업무 예제 |
| [[Python 데이터 분석 가이드]] | ⭐⭐ 중급 | Pandas, NumPy 활용 |
| [[Python SQL 연동 가이드]] | ⭐⭐ 중급 | DB 연동, ETL 파이프라인 |
| [[SciPy 가이드]] | ⭐⭐⭐ 고급 | 통계 분석, 최적화, 신호 처리 |

### Impala SQL
반도체 데이터 분석을 위한 SQL 쿼리 가이드

| 문서 | 난이도 | 설명 |
|------|--------|------|
| [[Impala SQL 가이드]] | ⭐ 초급 | 기본 문법, 조인, 집계, 고급 쿼리 |
| [[Impala SQL 실전 예제 모음]] | 📚 참고 | 25개 실전 쿼리 모음 |
| [[Impala SQL 빠른참조]] | 📋 참고 | Cheat Sheet, 빠른 검색용 |

### KLayout
반도체 레이아웃 뷰어 및 XSection 도구

| 문서 | 설명 |
|------|------|
| [[KLayout_XSection_Manual/README]] | XSection 사용 전체 가이드 |

### 기타 언어 가이드

| 문서 | 설명 |
|------|------|
| [[LaTeX 가이드]] | 수식 및 문서 작성 |
| [[Markdown 가이드]] | 마크다운 문법 |
| [[Regex 가이드]] | 정규표현식 |

---

## 🚀 빠른 시작

### Virtuoso SKILL을 처음 시작한다면
1. [[Virtuoso Skill 가이드]] - 기본 문법 (2-3시간)
2. [[Virtuoso Skill 빠른참조]] - 옆에 두고 참고
3. [[Virtuoso Skill 실전 레시피]] - 바로 사용

### Python을 처음 시작한다면
1. [[Python 기초 가이드]] - 기본 문법 (2-3시간)
2. [[Python 데이터 분석 가이드]] - Pandas, NumPy
3. [[SciPy 가이드]] - 통계 및 과학 계산

### Impala SQL을 처음 시작한다면
1. [[Impala SQL 가이드]] - 기본 문법 (1-2시간)
2. [[Impala SQL 빠른참조]] - 옆에 두고 참고
3. [[Impala SQL 실전 예제 모음]] - 쿼리 복사

---

## 💡 사용 시나리오

### 시나리오 1: 레이아웃 자동화
```
도구: Virtuoso SKILL
문서: Virtuoso Skill 가이드 → 실전 레시피
예제: 파라메트릭 셀 생성, DRC 체크
```

### 시나리오 2: 수율 데이터 분석
```
도구: Python + Impala SQL
문서: Impala SQL 가이드 → Python 데이터 분석 가이드
예제: SQL로 추출 → Pandas로 분석 → SciPy로 통계
```

### 시나리오 3: 통합 자동화
```
1. Impala SQL: 대용량 데이터 추출
2. Python Pandas: 데이터 정제 및 변환
3. SciPy: 통계 분석 및 최적화
4. Python: 보고서 자동 생성
```

### 시나리오 4: 실시간 모니터링
```
1. Python: DB 연결 및 주기적 쿼리
2. SciPy: 이상치 탐지
3. Python: 알림 전송
```

---

## 📚 학습 경로

### 🌱 1주차: 기초
**Python 기초**
- [ ] [[Python 기초 가이드]] 완독
- [ ] 기본 문법 실습
- [ ] 반도체 예제 따라하기

**SQL 기초**
- [ ] [[Impala SQL 가이드]] 완독
- [ ] 기본 쿼리 실습
- [ ] 웨이퍼 데이터 조회

### 🌿 2-4주차: 응용
**데이터 분석**
- [ ] [[Python 데이터 분석 가이드]] 학습
- [ ] Pandas 실습
- [ ] NumPy 배열 연산

**SQL 심화**
- [ ] [[Impala SQL 실전 예제 모음]] 실습
- [ ] 복잡한 조인 연습
- [ ] 집계 및 윈도우 함수

### 🌳 1-2개월: 통합
**통계 분석**
- [ ] [[SciPy 가이드]] 학습
- [ ] 가설 검정 실습
- [ ] 최적화 문제 해결

**통합 프로젝트**
- [ ] SQL + Python 파이프라인
- [ ] 자동화 스크립트
- [ ] 분석 보고서 생성

---

## 📊 문서 통계

**총 문서 수**: 15개
- SKILL: 8개
- Python: 4개
- SQL: 3개

**총 코드 예제**: 250+개
**총 라인 수**: ~18,000줄

---

## 🔍 빠른 검색

### 언어별 검색
```
Python 기초 → Python 기초 가이드
데이터 분석 → Python 데이터 분석 가이드
통계 → SciPy 가이드
SQL 쿼리 → Impala SQL 가이드
레이아웃 → Virtuoso Skill 가이드
```

### 작업별 검색
```
수율 분석 → Impala SQL 실전 예제 + SciPy 가이드
장비 분석 → Impala SQL 실전 예제
공정 최적화 → SciPy 가이드 (최적화)
이상치 탐지 → SciPy 가이드 (통계)
데이터 정제 → Python 데이터 분석 가이드
자동화 → Python SQL 연동 가이드
```

---

## 💾 실전 활용

### Python 환경 구축

```bash
# Anaconda 설치 (권장)
# https://www.anaconda.com/download

# 가상환경 생성
conda create -n semiconductor python=3.9
conda activate semiconductor

# 필수 패키지 설치
pip install pandas numpy scipy matplotlib
pip install impyla  # Impala 연동
```

### 프로젝트 구조

```
project/
├── data/           # 데이터 파일
├── scripts/        # Python 스크립트
│   ├── extract.py  # SQL 추출
│   ├── analyze.py  # 데이터 분석
│   └── report.py   # 보고서 생성
├── sql/            # SQL 쿼리
├── notebooks/      # Jupyter 노트북
└── output/         # 결과 파일
```

---

## 🏆 학습 마일스톤

### ✅ 레벨 1: 입문 (1주)
- [ ] Python 첫 스크립트
- [ ] SQL 첫 쿼리
- [ ] 데이터 읽기/쓰기

### ✅ 레벨 2: 초급 (1개월)
- [ ] Pandas로 데이터 분석
- [ ] SQL 조인 및 집계
- [ ] 기본 통계 분석

### ✅ 레벨 3: 중급 (3개월)
- [ ] ETL 파이프라인 구축
- [ ] 복잡한 통계 분석
- [ ] 자동화 스크립트

### ✅ 레벨 4: 고급 (6개월+)
- [ ] 머신러닝 적용
- [ ] 실시간 모니터링
- [ ] 통합 분석 시스템

---

## 🎓 추가 학습 자료

### Python
- Python 공식 문서
- Pandas 문서
- SciPy 문서
- Real Python 튜토리얼

### SQL
- Cloudera Impala 문서
- SQL Tutorial (W3Schools)
- Mode Analytics SQL 튜토리얼

### 책 추천
- Python for Data Analysis (Wes McKinney)
- Think Stats (Allen Downey)
- Learning SQL (Alan Beaulieu)

---

## 🔗 관련 폴더

- [[Process/]] - 반도체 공정 문서
- [[Unit Process/]] - 단위 공정 가이드
- [[분석/]] - 측정 및 분석 방법
- [[spotfire/]] - Spotfire + SQL 연동
- [[templates/]] - 작업 템플릿

---

## 💡 성공 사례

### 자동화 효과
```
수작업 분석: 2시간
Python 자동화: 5분
→ 96% 시간 절약!
```

### 데이터 처리
```
Excel 한계: 100만 행
Python + SQL: 수억 행
→ 대용량 데이터 처리 가능!
```

### 분석 품질
```
수작업: 실수 가능성
자동화: 재현 가능한 분석
→ 신뢰성 향상!
```

---

## 📝 문서 업데이트 이력

**2025-10-15**: Python 문서 추가
- Python 기초 가이드
- Python 데이터 분석 가이드
- Python SQL 연동 가이드
- SciPy 가이드

**2025-10-15**: Impala SQL 문서 추가
- Impala SQL 가이드
- Impala SQL 실전 예제 모음
- Impala SQL 빠른참조

**2025-10-14**: Virtuoso SKILL 문서 생성
- 8개 SKILL 문서
- 150+ 예제 포함

---

## 🎯 다음 학습 단계

### 개인 학습
1. 기초부터 차근차근
2. 실습 위주 학습
3. 실제 업무 적용
4. 자동화 스크립트 작성

### 팀 활용
1. 코드 라이브러리 구축
2. 분석 템플릿 공유
3. 정기 스터디
4. 베스트 프랙티스 공유

---

## 🌟 학습 팁

### Python 학습
- 매일 조금씩 코딩
- 실제 데이터로 연습
- 에러를 두려워하지 말 것
- 코드를 읽고 따라하기

### SQL 학습
- 간단한 쿼리부터
- 결과를 항상 확인
- 성능을 고려
- 쿼리를 저장하고 재사용

### 통계 학습
- 개념 이해가 우선
- 실제 데이터로 검증
- 시각화 활용
- 결과 해석 연습

---

**문서 작성**: Claude Assistant  
**최종 업데이트**: 2025-10-15  
**버전**: 4.0

당신의 데이터 분석 여정을 응원합니다! 🚀📊🐍
