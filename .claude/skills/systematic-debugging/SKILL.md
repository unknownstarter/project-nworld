---
name: systematic-debugging
description: 체계적 4단계 디버깅 프로세스로 버그의 근본 원인을 분석하고 수정한다. 버그 수정이 필요할 때 사용한다.
argument-hint: "[버그 설명 또는 에러 메시지]"
---

# Systematic Debugging

## 4-Phase Root Cause Analysis

### Phase 1: 재현 (Reproduce)
- 버그를 **100% 재현** 가능한 단계 정리
- 환경 정보 기록 (OS, 버전, 설정)
- 최소 재현 사례(MRE) 만들기
- 예상 동작 vs 실제 동작 명확히 정의

### Phase 2: 격리 (Isolate)
- 이진 탐색으로 범위 좁히기
- 최근 변경 사항 확인 (git log, git bisect)
- 관련 로그, 스택 트레이스 분석
- 가설 목록 작성 (가능성 높은 순서)

### Phase 3: 진단 (Diagnose)
- 가설을 하나씩 검증
- 디버거, 로그, 테스트로 확인
- 근본 원인(Root Cause) 확정
- "왜?"를 5번 묻기 (5 Whys)

### Phase 4: 수정 (Fix)
- 근본 원인을 해결하는 최소한의 수정
- 회귀 테스트 작성 (이 버그가 다시 발생하면 잡히도록)
- 부작용 확인
- 유사한 버그가 다른 곳에도 있는지 확인

## Rules
- IMPORTANT: 추측으로 코드를 수정하지 않는다. 반드시 원인을 확인한 후 수정한다.
- 한 번에 하나의 가설만 검증한다
- 수정 후 반드시 원래 재현 단계로 검증한다

## Task: $ARGUMENTS
