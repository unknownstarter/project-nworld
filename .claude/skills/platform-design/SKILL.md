---
name: platform-design
description: 플랫폼별 디자인 가이드라인(Apple HIG, Material Design 3, Web Standards)을 기반으로 UI/UX를 설계한다. 특정 플랫폼에 맞는 디자인이 필요할 때 사용한다.
argument-hint: "[플랫폼과 디자인 과제]"
user-invocable: true
---

# Platform Design Guidelines

Apple HIG, Google Material Design 3, Web Standards 기반으로 각 플랫폼의 네이티브 경험을 설계한다.
출처: ehmo/platform-design-skills (300+ 규칙)

## iOS (iPhone)
- **Navigation**: Tab bar(하단, 최대 5개), Navigation bar(상단), Sheet/Modal
- **Layout**: Safe Area 준수, Dynamic Type 지원, 44pt 최소 터치 타겟
- **Gestures**: Swipe back, Pull to refresh, Long press context menu
- **Accessibility**: VoiceOver 완전 지원, Dynamic Type, Reduce Motion
- **Design Tokens**: SF Pro (시스템 폰트), SF Symbols, Semantic colors
- **iOS 18+**: Liquid Glass 이펙트, 반투명 배경, 물리적 깊이감

## iPadOS
- **Multitasking**: Split View, Slide Over, Stage Manager 대응
- **Sidebar**: 좌측 사이드바 네비게이션 (3-column layout)
- **Pointer**: 커서 hover 상태 지원 (iPad + 키보드/트랙패드)
- **Apple Pencil**: 필기, 그리기 입력 고려

## Android (Material Design 3)
- **Material You**: Dynamic Color (사용자 월페이퍼 기반 컬러)
- **Navigation**: Bottom navigation bar, Navigation drawer, Top app bar
- **Components**: FAB, Chips, Cards, Bottom sheets
- **Layout**: Responsive layout grid (compact/medium/expanded)
- **Typography**: Roboto 기본, Material Type Scale
- **Motion**: Shared element transitions, Container transforms

## Web
- **Responsive**: Mobile-first, breakpoints (sm/md/lg/xl/2xl)
- **Accessibility**: WCAG 2.1 AA 준수, Semantic HTML, ARIA
- **Progressive Enhancement**: 기본 기능은 JS 없이도 동작
- **Performance**: Core Web Vitals (LCP/INP/CLS)
- **SEO**: Semantic markup, Open Graph, Structured data

## Cross-Platform Principles
1. 각 플랫폼의 네이티브 패턴을 존중 (iOS 스와이프 ≠ Android 백버튼)
2. 브랜드 일관성과 플랫폼 규범 사이 균형
3. 접근성은 모든 플랫폼에서 최우선
4. 플랫폼 전환 시 데이터와 컨텍스트 유지

## Task: $ARGUMENTS
