---
name: web-performance
description: Core Web Vitals 최적화, 렌더링 성능, 번들 사이즈 최적화를 다룬다. 웹 성능 개선이 필요할 때 사용한다.
argument-hint: "[성능 이슈 또는 최적화 대상]"
user-invocable: true
---

# Web Performance Optimization

출처: Cloudflare web-perf skill + 업계 베스트 프랙티스

## Core Web Vitals (2026)

### LCP (Largest Contentful Paint) < 2.5s
- 원인 진단: 서버 응답 시간, 리소스 로드 지연, 렌더 블로킹
- 최적화:
  - 서버: CDN, Edge computing, 캐시 헤더
  - 이미지: WebP/AVIF, srcset, lazy loading (LCP 요소 제외)
  - 폰트: `font-display: swap`, preload critical fonts
  - CSS: Critical CSS 인라인, non-critical defer

### INP (Interaction to Next Paint) < 200ms
- 원인 진단: 긴 JavaScript 태스크, 무거운 이벤트 핸들러
- 최적화:
  - Long tasks 분할: `requestIdleCallback`, `scheduler.yield()`
  - Web Worker로 무거운 계산 이동
  - 이벤트 핸들러 경량화
  - `content-visibility: auto` (off-screen 렌더링 지연)

### CLS (Cumulative Layout Shift) < 0.1
- 원인 진단: 크기 미지정 이미지, 동적 콘텐츠 삽입
- 최적화:
  - 이미지/비디오에 width/height 명시
  - 폰트: `font-display: optional` 또는 `swap` + 크기 조정
  - 광고/동적 콘텐츠에 고정 크기 컨테이너

## Render-Blocking Audit
```
1. <head> 내 동기 스크립트 → async 또는 defer
2. 사용되지 않는 CSS → 제거 또는 지연 로드
3. Third-party 스크립트 → web worker 또는 lazy load
4. Google Fonts → self-hosting 또는 next/font
```

## Bundle Optimization
- Tree shaking: 사용하지 않는 코드 제거
- Code splitting: 라우트/컴포넌트별 분할
- Dynamic imports: 필요할 때 로드
- Bundle analyzer: `@next/bundle-analyzer`

## Caching Strategy
```
Static assets (JS/CSS/images): Cache-Control: public, max-age=31536000, immutable
HTML pages: Cache-Control: public, max-age=0, must-revalidate
API responses: Cache-Control: private, max-age=60
CDN: stale-while-revalidate pattern
```

## Measurement
- Lighthouse CI: 자동화된 성능 측정
- Web Vitals JS: 실제 사용자 측정 (RUM)
- Chrome DevTools Performance 탭

## Task: $ARGUMENTS
