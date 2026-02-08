---
name: sprint-planning
description: 스프린트 계획을 수립한다. 목표 설정, 작업 분해, 추정, 우선순위 결정을 수행한다.
argument-hint: "[스프린트 목표 또는 기능 목록]"
---

# Sprint Planning

## Process

### 1. Sprint Goal
- 이번 스프린트에서 달성할 하나의 명확한 목표
- "이번 스프린트가 끝나면 ___를 할 수 있다"

### 2. Task Breakdown
각 스토리를 실행 가능한 태스크로 분해:
- 각 태스크는 반나절~하루 안에 완료 가능해야 함
- 의존성 명시
- 완료 기준(DoD) 정의

### 3. Estimation
- 스토리 포인트: 1 (trivial), 2 (small), 3 (medium), 5 (large), 8 (epic → 분할)
- 8 이상이면 반드시 더 작은 스토리로 분할

### 4. Capacity Check
- 가용 시간 대비 계획량 확인
- 예비 시간 20% 확보 (예상치 못한 이슈 대비)
- 기술 부채 시간 포함

## Output Format
```
## Sprint [번호]: [목표]
기간: [시작일] - [종료일]

### Stories
1. [스토리 제목] (SP: X)
   - [ ] 태스크 1
   - [ ] 태스크 2
   Dependencies: [의존성]

### Risks
- [리스크] → [대응]

### Definition of Done
- 코드 리뷰 완료
- 테스트 통과
- 문서 업데이트
```

## Task: $ARGUMENTS
