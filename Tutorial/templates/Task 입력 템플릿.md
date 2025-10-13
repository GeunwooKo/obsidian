---
tags:
  - 업무
  - 진행중
created:
  "{ date:YYYY-MM-DD }":
---

# 작업 입력 템플릿

## ✅ 새 작업 추가

### 일반 작업
- [ ] 작업 설명을 입력하세요

### 마감일이 있는 작업
- [ ] 작업 설명 📅 {{date:YYYY-MM-DD}}

### 반복 작업
- [ ] 작업 설명 🔁 every week 📅 {{date:YYYY-MM-DD}}

### 프로젝트 작업 (시작일 + 마감일)
- [ ] 작업 설명 🛫 {{date:YYYY-MM-DD}} 📅 {{date:YYYY-MM-DD}}

### 예정된 작업
- [ ] 작업 설명 ⏳ {{date:YYYY-MM-DD}}

---

## 📋 작업 템플릿 모음

### 긴급 작업 (높은 우선순위)
- [ ] 🔺 긴급 작업 설명 📅 {{date:YYYY-MM-DD}}

### 일반 작업 (중간 우선순위)
- [ ] 🔼 일반 작업 설명 📅 {{date:YYYY-MM-DD}}

### 낮은 우선순위 작업
- [ ] 🔽 낮은 우선순위 작업 📅 {{date:YYYY-MM-DD}}

---

## 🔁 반복 작업 예시

### 매일
- [ ] 매일 하는 작업 🔁 every day 📅 {{date:YYYY-MM-DD}}

### 매주
- [ ] 주간 작업 🔁 every week 📅 {{date:YYYY-MM-DD}}
- [ ] 매주 월요일 🔁 every Monday 📅 {{date:YYYY-MM-DD}}

### 매월
- [ ] 월간 작업 🔁 every month 📅 {{date:YYYY-MM-DD}}
- [ ] 매월 1일 🔁 every month on the 1st 📅 {{date:YYYY-MM-DD}}

### 격주
- [ ] 격주 작업 🔁 every 2 weeks 📅 {{date:YYYY-MM-DD}}

---

## 🏷️ 태그 활용 예시

- [ ] #업무 관련 작업 📅 {{date:YYYY-MM-DD}}
- [ ] #개인 할 일 📅 {{date:YYYY-MM-DD}}
- [ ] #공정문서 작성 📅 {{date:YYYY-MM-DD}}
- [ ] #회의 준비 📅 {{date:YYYY-MM-DD}}

---

## 🔗 작업 체크리스트 (하위 작업)

- [ ] 큰 프로젝트 작업 📅 {{date:YYYY-MM-DD}}
	- [ ] 세부 작업 1
	- [ ] 세부 작업 2
	- [ ] 세부 작업 3

---

## 📌 빠른 참조

**날짜 이모지:**
- 📅 Due date (마감일)
- 🛫 Start date (시작일)
- ⏳ Scheduled date (예정일)
- ✅ Done date (완료일)
- 🔁 Recurrence (반복)

**우선순위:**
- 🔺 높음
- 🔼 중간
- 🔽 낮음

**반복 형식:**
- `every day` - 매일
- `every week` - 매주
- `every Monday` - 매주 월요일
- `every month` - 매월
- `every month on the 1st` - 매월 1일
- `every 2 weeks` - 격주

**날짜 형식:** YYYY-MM-DD (예: 2025-10-20)
