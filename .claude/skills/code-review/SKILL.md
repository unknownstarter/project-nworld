---
name: code-review
description: 다각도 코드 리뷰를 수행한다. 보안, 성능, 가독성, 유지보수성을 점검한다. 코드 리뷰가 필요할 때 사용한다.
argument-hint: "[리뷰할 파일 또는 변경사항]"
---

# Code Review

## Review Dimensions

### 1. Correctness (정확성)
- 비즈니스 로직이 정확한가?
- 엣지 케이스를 처리하는가?
- 에러 처리가 적절한가?
- 동시성/레이스 컨디션이 없는가?

### 2. Security (보안)
- 입력 검증이 충분한가?
- SQL injection, XSS 취약점이 없는가?
- 인증/인가가 올바른가?
- 민감 정보가 노출되지 않는가?

### 3. Performance (성능)
- N+1 쿼리가 없는가?
- 불필요한 메모리 할당이 없는가?
- 적절한 인덱싱이 되어 있는가?
- 캐싱이 필요한 곳에 적용되었는가?

### 4. Readability (가독성)
- 코드만 읽어도 의도가 파악되는가?
- 네이밍이 명확한가?
- 복잡한 로직에 주석이 있는가?
- 함수 크기가 적절한가?

### 5. Maintainability (유지보수성)
- 변경이 쉬운 구조인가?
- 적절한 추상화 수준인가?
- 테스트가 있는가?
- 문서화가 필요한 부분은?

## Output Format
```
## 코드 리뷰 결과

### Critical (즉시 수정)
- [file:line] 설명

### Suggestion (개선 제안)
- [file:line] 설명

### Praise (잘한 점)
- [file:line] 설명
```

## Task: $ARGUMENTS
