---
name: security-audit
description: OWASP Top 10 기반 보안 감사를 수행한다. 취약점을 식별하고 수정 방안을 제시한다. 보안 점검이 필요할 때 사용한다.
argument-hint: "[감사할 코드 또는 시스템]"
---

# Security Audit

## OWASP Top 10 (2021) Checklist

### A01: Broken Access Control
- [ ] 수평적 권한 상승 가능한가?
- [ ] 수직적 권한 상승 가능한가?
- [ ] CORS 정책이 적절한가?
- [ ] 직접 객체 참조(IDOR) 취약점?

### A02: Cryptographic Failures
- [ ] 민감 데이터가 평문 저장/전송되는가?
- [ ] 약한 암호 알고리즘 사용?
- [ ] 키 관리가 안전한가?

### A03: Injection
- [ ] SQL injection
- [ ] NoSQL injection
- [ ] Command injection
- [ ] LDAP injection
- [ ] XSS (Reflected, Stored, DOM)

### A04: Insecure Design
- [ ] Threat modeling 수행했는가?
- [ ] 비즈니스 로직 취약점?
- [ ] Rate limiting 적용?

### A05: Security Misconfiguration
- [ ] 기본 자격증명 변경?
- [ ] 불필요한 기능 비활성화?
- [ ] 에러 메시지에 내부 정보 노출?
- [ ] 보안 헤더 설정 (CSP, HSTS)?

### A06-A10
- Server-Side Request Forgery (SSRF)
- Software and Data Integrity
- Security Logging and Monitoring
- Vulnerable and Outdated Components
- Identification and Authentication Failures

## Severity Scale
- **Critical**: 즉시 수정 (데이터 유출, RCE)
- **High**: 24시간 내 수정
- **Medium**: 다음 스프린트에 수정
- **Low**: 백로그에 추가

## Task: $ARGUMENTS
