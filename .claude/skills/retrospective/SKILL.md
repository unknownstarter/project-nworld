---
name: retrospective
description: 회고를 진행한다. KPT, 5 Whys, Start/Stop/Continue 등의 프레임워크로 학습과 개선점을 도출한다.
argument-hint: "[회고 대상 (스프린트, 프로젝트, 실험)]"
---

# Retrospective

## Frameworks

### KPT (Keep / Problem / Try)
```
## Keep (계속할 것)
- 잘 되고 있어서 유지할 것들

## Problem (문제)
- 불편하거나 비효율적인 것들

## Try (시도할 것)
- 다음에 새로 시도할 것들
```

### 5 Whys
문제가 있을 때 "왜?"를 5번 연속 물어 근본 원인 도출:
```
Problem: [문제]
Why 1: [첫 번째 원인]
Why 2: [더 깊은 원인]
Why 3: [더 깊은 원인]
Why 4: [더 깊은 원인]
Why 5: [근본 원인] → Action Item
```

### Start / Stop / Continue
```
## Start (시작할 것)
- 아직 안 하고 있지만 해야 할 것

## Stop (그만할 것)
- 하고 있지만 효과 없는 것

## Continue (계속할 것)
- 효과 있어서 계속할 것
```

## Retrospective Process
1. **데이터 수집** (5분): 팩트 기반 타임라인
2. **인사이트 도출** (15분): 패턴, 놀라운 점
3. **액션 아이템** (10분): 구체적, 담당자 지정
4. **기록** (5분): `docs/retrospectives/`에 저장

## Rules
- 사람이 아닌 시스템/프로세스에 집중
- 비난하지 않기 (blame-free)
- 액션 아이템은 SMART (구체적, 측정 가능, 달성 가능, 관련성, 기한)
- 이전 회고 액션 아이템 점검부터 시작

## Output
```
## Retrospective: [제목]
Date: [날짜]
Scope: [대상 기간/프로젝트]

### Data
[팩트 기반 타임라인]

### Insights
[발견한 패턴과 교훈]

### Action Items
1. [ ] [액션] - 담당: [누구] - 기한: [언제]
```

저장 위치: `docs/retrospectives/YYYY-MM-DD-[제목].md`

## Task: $ARGUMENTS
