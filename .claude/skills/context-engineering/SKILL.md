---
name: context-engineering
description: AI 에이전트의 컨텍스트 관리, 메모리 시스템, 컨텍스트 압축, 멀티에이전트 패턴을 설계한다. 에이전트 시스템의 컨텍스트 효율성을 높일 때 사용한다.
argument-hint: "[컨텍스트 엔지니어링 과제]"
user-invocable: true
---

# Context Engineering

출처: muratcankoylan/Agent-Skills-for-Context-Engineering (8개 전문 스킬)

## Context가 중요한 이유
> "Prompt engineering은 한 턴의 입력 최적화. Context engineering은 에이전트의 전체 인지 환경 설계."

## 1. Context Anatomy
```
Context = System Instructions + Memory + Retrieved Knowledge + Conversation History + Tool Results
```

각 요소의 역할:
- **System**: 행동 지침, 제약조건, 페르소나
- **Memory**: 이전 세션의 학습 (persistent)
- **Knowledge**: RAG로 가져온 관련 정보
- **History**: 현재 대화 흐름
- **Tools**: 도구 호출 결과

## 2. Context Degradation Patterns
| 패턴 | 증상 | 해결 |
|------|------|------|
| Context Overflow | 지시 무시, 이전 내용 망각 | 압축, 요약, 선택적 포함 |
| Instruction Dilution | 초기 지시가 점점 약해짐 | 중요 지시 반복, 구조화 |
| Recency Bias | 최근 정보에 과도한 가중치 | 중요도 기반 정렬 |
| Context Poisoning | 잘못된 정보가 맥락에 포함 | 검증 레이어, 출처 추적 |

## 3. Context Compression Strategies
- **Summarization**: 이전 대화를 요약으로 대체
- **Selective Inclusion**: 관련 있는 것만 포함
- **Hierarchical**: 상세→요약→키워드 3단계
- **Sliding Window**: 최근 N턴만 유지 + 핵심 요약

## 4. Memory Systems
### Short-term (세션 내)
- 대화 히스토리 버퍼
- 압축/요약 전략

### Long-term (세션 간)
- Vector DB에 과거 상호작용 저장
- 사실 vs 에피소드 vs 절차적 메모리 분류
- 망각 곡선: 오래되고 사용되지 않는 기억 감쇠

### Graph-based
- Knowledge graph로 엔티티 관계 저장
- 구조화된 추론에 유리
- Neo4j, FalkorDB

## 5. Multi-Agent Context Patterns
### Orchestrator Pattern
```
Orchestrator (전체 컨텍스트)
  → Agent A (부분 컨텍스트 + 전문 지식)
  → Agent B (부분 컨텍스트 + 전문 지식)
  → Synthesizer (결과 통합)
```

### Blackboard Pattern
```
공유 메모리(Blackboard)에 에이전트들이 읽기/쓰기
각 에이전트는 자신의 전문 영역만 수정
```

### Context Handoff
```
Agent A가 작업 완료 → 요약 생성 → Agent B에 전달
전체 컨텍스트가 아닌 구조화된 핸드오프 문서
```

## 6. Tool Design for Context
- 도구 설명이 곧 컨텍스트 (명확하고 간결하게)
- 도구 결과 포맷: 구조화된 JSON (LLM이 파싱하기 쉽게)
- 도구 결과 크기 제한 (truncation 전략)
- 도구 실패 시 유용한 에러 메시지

## Task: $ARGUMENTS
