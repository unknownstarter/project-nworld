---
name: architecture-review
description: 시스템 아키텍처를 리뷰하고 개선점을 제안한다. 확장성, 안정성, 유지보수성 관점에서 평가한다.
argument-hint: "[리뷰할 시스템 또는 컴포넌트]"
---

# Architecture Review

## Review Checklist

### Scalability (확장성)
- 수평 확장(scale-out)이 가능한가?
- 상태(state)는 어디에 저장되는가?
- 병목 지점은 어디인가?
- 10배 트래픽에 대응 가능한가?

### Reliability (안정성)
- 단일 장애점(SPOF)이 있는가?
- 장애 복구 전략은?
- 데이터 백업/복구 계획은?
- 서킷 브레이커 패턴 적용?

### Maintainability (유지보수성)
- 모듈 간 결합도는 낮은가?
- 응집도는 높은가?
- 변경 영향 범위가 예측 가능한가?
- 모니터링/로깅이 충분한가?

### Security (보안)
- 인증/인가 구조는?
- 데이터 암호화 (전송 중, 저장 시)?
- 비밀 관리 방식은?
- 공격 표면은 최소화되었는가?

### Cost (비용)
- 리소스 사용이 효율적인가?
- 오토스케일링은 적절한가?
- 비용 모니터링이 있는가?

## Output: 아키텍처 결정 기록 (ADR)
```
## ADR-[번호]: [제목]
- Status: [Proposed/Accepted/Deprecated]
- Context: [배경과 문제]
- Decision: [결정 사항]
- Consequences: [결과와 트레이드오프]
```

## Task: $ARGUMENTS
