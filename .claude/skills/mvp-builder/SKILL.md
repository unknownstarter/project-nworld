---
name: mvp-builder
description: MVP(Minimum Viable Product)를 빠르게 설계하고 구축한다. 핵심 가설 추출, 최소 기능 정의, 빠른 구현 계획을 수립한다.
argument-hint: "[MVP 아이디어 또는 검증할 가설]"
---

# MVP Builder

## MVP Philosophy
> "MVP는 가장 적은 노력으로 가장 많은 학습을 얻는 것" - Eric Ries

## Process

### 1. 핵심 가설 추출
- 이 제품이 성공하려면 무엇이 참이어야 하는가?
- 가장 위험한(불확실한) 가설부터 검증
- 가설을 측정 가능한 형태로 구조화

### 2. MVP 유형 선택
| 유형 | 시간 | 적합한 경우 |
|------|------|-------------|
| Landing Page | 1일 | 수요 검증 |
| Wizard of Oz | 2-3일 | 가치 검증 (뒤에서 수동) |
| Concierge | 1주 | 프로세스 검증 (직접 서비스) |
| Single Feature | 1-2주 | 핵심 기능 검증 |
| Prototype | 2-4주 | 전체 경험 검증 |

### 3. Feature Scoping
```
Must Have (없으면 MVP가 아님):
- [핵심 기능 1]
- [핵심 기능 2]

Must NOT Have (MVP에서 제외):
- [나중에 할 것]
- [검증 후 추가할 것]
```

### 4. Quick Build Stack
- Frontend: Next.js + Tailwind + shadcn/ui
- Backend: Supabase (Auth + DB + Storage)
- AI: Vercel AI SDK
- Deploy: Vercel
- Analytics: PostHog

### 5. 성공 기준 (시작 전에 정의)
```
## Launch Criteria
- [X]명의 사용자가 가입
- [Y]%의 활성화율
- [Z]의 핵심 액션 완료율
```

## Anti-Patterns
- 출시 전 완벽 추구
- 핵심과 무관한 기능 추가
- 아무에게도 보여주지 않고 혼자 빌드
- 측정 없이 빌드
- 피드백 없이 반복

## Task: $ARGUMENTS
