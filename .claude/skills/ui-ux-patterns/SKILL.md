---
name: ui-ux-patterns
description: 67가지 UI 스타일, 96가지 컬러 팔레트, 57가지 폰트 페어링, 25가지 랜딩페이지 패턴 등 종합 UI/UX 패턴 라이브러리. 디자인 영감이나 구체적 UI 패턴이 필요할 때 사용한다.
argument-hint: "[UI/UX 디자인 과제]"
user-invocable: true
---

# UI/UX Pattern Library

출처: nextlevelbuilder/ui-ux-pro-max-skill, ibelick/ui-skills

## UI Styles (주요 20가지)

### 모던 & 미니멀
1. **Glassmorphism**: 반투명 유리 효과, backdrop-filter blur
2. **Neumorphism**: 소프트 그림자로 돌출/함몰 효과
3. **Claymorphism**: 3D 클레이 오브젝트 느낌
4. **Minimalism**: 최소한의 요소, 여백 극대화
5. **Brutalism**: 의도적으로 거칠고 날것의 디자인

### 구조 & 레이아웃
6. **Bento Grid**: 격자 기반 카드 레이아웃 (Apple 스타일)
7. **Asymmetric Layout**: 비대칭 그리드
8. **Split Screen**: 좌우 분할 레이아웃
9. **Card-Based**: 카드 중심 인터페이스
10. **Dashboard**: 데이터 대시보드 레이아웃

### 인터랙티브 & 이머시브
11. **Parallax Scrolling**: 깊이감 있는 스크롤
12. **Micro-interactions**: 섬세한 피드백 애니메이션
13. **Dark Mode First**: 다크 모드 우선 디자인
14. **Gradient Mesh**: 복잡한 그라디언트 배경
15. **3D Elements**: CSS/WebGL 3D 요소

### AI-Native
16. **Conversational UI**: 채팅 인터페이스 + GUI 하이브리드
17. **Command Palette**: Cmd+K 스타일 검색/명령 인터페이스
18. **Streaming Content**: AI 실시간 생성 콘텐츠 UI
19. **Adaptive Layout**: 컨텍스트에 따라 변하는 UI
20. **Voice-First**: 음성 우선 인터페이스

## Color Palette Guide (산업별)

### SaaS/Tech
- Primary: Blue (#2563EB) 또는 Purple (#7C3AED)
- 신뢰와 혁신의 균형
- 밝은 배경 + 강한 CTA 색상

### E-commerce
- Primary: Orange (#EA580C) 또는 Green (#16A34A)
- 구매 유도 심리: 긴급성(빨강), 신뢰(초록)
- 고대비 CTA 버튼

### Healthcare/Fintech
- Primary: Teal (#0D9488) 또는 Navy (#1E40AF)
- 안정감, 전문성, 신뢰
- 부드러운 그라디언트, 차분한 톤

## Font Pairing Rules
1. **Heading + Body**: 대비가 있되 조화로운 조합
   - Inter + Inter (안전한 선택)
   - Cal Sans + Inter (모던 SaaS)
   - Playfair Display + Source Sans Pro (클래식 + 모던)
2. 서체 2개 이하 사용 (3개 이상은 지저분해 보임)
3. Weight 대비로 계층 만들기 (Bold title + Regular body)

## Landing Page Patterns
1. **Hero-Centric**: 큰 헤드라인 + CTA + 시각적 요소
2. **Feature Grid**: 기능 카드 그리드
3. **Social Proof**: 고객 로고 + 추천사 중심
4. **Interactive Demo**: 제품 데모가 메인 히어로
5. **Storytelling**: 스크롤 기반 스토리텔링

## Design Quality Checklist
- [ ] 시각적 계층 구조가 명확한가? (F-pattern / Z-pattern)
- [ ] CTA가 3초 안에 눈에 띄는가?
- [ ] 여백(whitespace)이 충분한가?
- [ ] 색상 대비가 4.5:1 이상인가?
- [ ] 로딩/에러/빈 상태가 디자인되었는가?
- [ ] 반응형이 자연스러운가?

## Task: $ARGUMENTS
