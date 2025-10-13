# Obsidian Tasks 쿼리 가이드

## 목차
- [[#기본 사용법]]
- [[#필터링 옵션]]
- [[#정렬 옵션]]
- [[#그룹화 옵션]]
- [[#표시 옵션]]
- [[#실용 예제]]

---

## 기본 사용법

Tasks 쿼리는 코드 블록으로 작성합니다:

`````markdown
````tasks
# 여기에 쿼리 작성
````
`````

### 기본 작업 형식

```markdown
- [ ] 이것은 일반 작업입니다
- [ ] 마감일이 있는 작업 📅 2025-10-20
- [ ] 반복 작업 🔁 every week
- [ ] 시작일이 있는 작업 🛫 2025-10-15
- [ ] 예정일이 있는 작업 ⏳ 2025-10-18
- [x] 완료된 작업 ✅ 2025-10-14
```

**날짜 이모지**:
- 📅 Due date (마감일)
- 🛫 Start date (시작일)  
- ⏳ Scheduled date (예정일)
- ✅ Done date (완료일)
- 🔁 Recurrence (반복)

---

## 필터링 옵션

### 1. 상태별 필터

`````markdown
````tasks
# 미완료 작업만
not done

# 완료된 작업만
done
````
`````

### 2. 날짜 필터

`````markdown
````tasks
# 오늘 마감인 작업
due today

# 내일 마감인 작업
due tomorrow

# 이번 주 마감인 작업
due this week

# 다음 주 마감인 작업
due next week

# 특정 날짜 이전 마감
due before 2025-10-20

# 특정 날짜 이후 마감
due after 2025-10-15

# 마감일이 없는 작업
no due date
````
`````

**날짜 키워드**:
- `due` - 마감일
- `scheduled` - 예정일
- `start` - 시작일
- `done` - 완료일

### 3. 텍스트 검색

`````markdown
````tasks
# 설명에 특정 단어 포함
description includes 반도체

# 설명에 특정 단어 제외
description does not include 회의

# 정규식 사용
description regex matches /[Pp]ython/
````
`````

### 4. 경로 필터

`````markdown
````tasks
# 특정 폴더의 작업만
path includes Process

# 특정 파일의 작업만
path includes daily notes/2025-10-14.md

# 특정 경로 제외
path does not include archive
````
`````

### 5. 태그 필터

`````markdown
````tasks
# 특정 태그가 있는 작업
tags include #중요

# 특정 태그가 없는 작업
tags do not include #보류
````
`````

### 6. 제목 필터

`````markdown
````tasks
# 특정 제목 아래의 작업
heading includes 프로젝트 A
````
`````

---

## 정렬 옵션

`````markdown
````tasks
# 마감일 기준 정렬 (오름차순)
sort by due

# 마감일 기준 역순 정렬
sort by due reverse

# 여러 조건으로 정렬
sort by status
sort by due
sort by priority
````
`````

**정렬 가능한 필드**:
- `status` - 상태
- `due` - 마감일
- `scheduled` - 예정일
- `start` - 시작일
- `done` - 완료일
- `priority` - 우선순위
- `description` - 설명
- `path` - 파일 경로

---

## 그룹화 옵션

`````markdown
````tasks
# 폴더별 그룹화
group by folder

# 마감일별 그룹화
group by due

# 상태별 그룹화
group by status

# 태그별 그룹화
group by tags

# 여러 조건으로 그룹화
group by folder
group by status
````
`````

---

## 표시 옵션

`````markdown
````tasks
# 특정 요소 숨기기
hide edit button
hide backlink
hide task count
hide done date

# 작업 개수 제한
limit 10

# 짧은 모드 (경로 숨김)
short mode
````
`````

---

## 실용 예제

### 예제 1: 오늘의 작업 목록

`````markdown
````tasks
not done
(due today) OR (scheduled today) OR (start before tomorrow)
sort by priority
sort by due
````
`````

### 예제 2: 주간 계획

`````markdown
````tasks
not done
due this week
sort by due
group by folder
````
`````

### 예제 3: 긴급 작업

`````markdown
````tasks
not done
due before in 3 days
sort by due
limit 5
````
`````

### 예제 4: 프로젝트별 진행 상황

`````markdown
````tasks
path includes Process
group by status
sort by due
````
`````

### 예제 5: 마감일 없는 작업 정리

`````markdown
````tasks
not done
no due date
sort by path
````
`````

### 예제 6: 완료된 작업 (최근 7일)

`````markdown
````tasks
done after 7 days ago
sort by done reverse
limit 20
````
`````

### 예제 7: 반복 작업 관리

`````markdown
````tasks
not done
has recurrence rule
sort by due
````
`````

### 예제 8: 태그로 카테고리 관리

`````markdown
````tasks
not done
tags include #업무
tags do not include #대기
sort by due
group by folder
````
`````

---

## 복잡한 쿼리 예제

### 예제 9: 조건 조합 (AND/OR)

`````markdown
````tasks
not done
(due today) OR (due tomorrow)
tags include #중요
sort by due
````
`````

### 예제 10: 폴더별 우선순위 작업

`````markdown
````tasks
not done
path includes programming
description includes python OR description includes KLayout
sort by priority
sort by due
limit 10
````
`````

### 예제 11: 대시보드 뷰

``````markdown
## 🔥 긴급
````tasks
not done
due before in 3 days
sort by due
limit 5
````

## 📅 이번 주
````tasks
not done
due this week
group by folder
````

## 📋 마감일 없음
````tasks
not done
no due date
path includes Process
limit 10
````
``````

---

## 날짜 표현 참고

- `today` - 오늘
- `tomorrow` - 내일
- `yesterday` - 어제
- `this week` - 이번 주
- `next week` - 다음 주
- `last week` - 지난 주
- `in X days` - X일 후
- `X days ago` - X일 전
- `YYYY-MM-DD` - 특정 날짜

---

## 우선순위

작업에 우선순위를 표시하려면:

```markdown
- [ ] 🔺 높은 우선순위
- [ ] 🔼 중간 우선순위  
- [ ] 🔽 낮은 우선순위
```

쿼리에서 사용:

`````markdown
````tasks
sort by priority
````
`````

---

## 팁과 요령

1. **여러 조건 조합**: AND 조건은 줄바꿈으로, OR 조건은 `OR` 키워드로 연결
2. **성능 최적화**: 너무 넓은 범위보다는 `path includes`로 범위 제한
3. **그룹화 활용**: 큰 목록은 `group by`로 구조화
4. **정기적 검토**: 완료된 작업과 마감일 없는 작업을 주기적으로 정리
5. **대시보드 구성**: 여러 쿼리를 조합하여 한 페이지에 전체 상황 파악

---

## 추가 리소스

- [공식 문서](https://publish.obsidian.md/tasks/)
- [쿼리 참조](https://publish.obsidian.md/tasks/Queries/Queries)
- [필터 참조](https://publish.obsidian.md/tasks/Queries/Filters)

---

## 실전 적용: 반도체 업무용 템플릿

``````markdown
## 📊 일일 작업

### 긴급 처리
````tasks
not done
(due today) OR (due before today)
path includes Process
sort by priority
````

### 오늘 시작
````tasks
not done
start today
sort by due
````

### 진행 중
````tasks
not done
start before today
no due date
path includes Process OR path includes Unit Process
limit 15
````

## 🔬 프로젝트별

### KLayout 개발
````tasks
not done
description includes KLayout OR description includes python
sort by due
````

### 공정 문서화
````tasks
not done
path includes Process
sort by due
group by heading
````

## 📈 주간 리뷰
````tasks
done after 7 days ago
sort by done reverse
limit 30
````
``````
