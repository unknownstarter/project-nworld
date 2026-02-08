---
name: seo-growth
description: SEO 감사, 프로그래매틱 SEO, 스키마 마크업, 컨텐츠 SEO 전략을 수립한다. 검색 유입 성장이 필요할 때 사용한다.
argument-hint: "[SEO 과제 또는 사이트 URL]"
user-invocable: true
---

# SEO & Search Growth

출처: coreyhaines31/marketingskills (seo-audit, programmatic-seo, schema-markup, competitor-alternatives)

## SEO Audit Checklist

### Technical SEO
- [ ] 사이트맵 (sitemap.xml) 존재 & 최신
- [ ] robots.txt 적절한 설정
- [ ] 페이지 로딩 속도 (Core Web Vitals)
- [ ] 모바일 친화성 (Mobile-first indexing)
- [ ] HTTPS 적용
- [ ] 구조화된 데이터 (Schema.org)
- [ ] canonical URL 설정
- [ ] 404/5xx 에러 없음
- [ ] 내부 링크 구조 최적화

### On-Page SEO
- [ ] Title 태그 (60자 이내, 키워드 포함)
- [ ] Meta Description (155자 이내, CTA 포함)
- [ ] H1 태그 (페이지당 1개)
- [ ] 헤딩 계층 구조 (H1→H2→H3)
- [ ] 이미지 alt 텍스트
- [ ] 내부 링크 (관련 페이지 연결)
- [ ] URL 구조 (간결하고 의미있게)

### Content SEO
- [ ] 검색 의도 매칭 (Informational/Navigational/Transactional)
- [ ] 키워드 밀도 (자연스럽게, 과도하지 않게)
- [ ] 컨텐츠 길이 (경쟁 페이지 대비 충분한 깊이)
- [ ] 업데이트 주기 (Freshness)

## Programmatic SEO
대규모 SEO 페이지를 템플릿 기반으로 생성:
```
/{city}-restaurants → 서울-맛집, 부산-맛집, ...
/{product}-alternatives → Notion-대안, Slack-대안, ...
/{tool}-vs-{tool} → ChatGPT-vs-Perplexity, ...
```

원칙:
1. 각 페이지가 고유한 가치를 제공해야 함 (얇은 콘텐츠 금지)
2. 데이터 소스가 풍부해야 함 (API, DB, 크롤링)
3. 내부 링크로 상호 연결
4. 인덱싱 상태 모니터링

## AI 시대 SEO (2026)
- **AEO (Answer Engine Optimization)**: AI가 답변 소스로 선택하도록 최적화
- 구조화된 콘텐츠 (FAQ, How-to, 비교표)
- 출처로 인용되기 쉬운 명확한 팩트 & 통계
- E-E-A-T: 경험, 전문성, 권위, 신뢰성 강화
- AI 크롤러 접근 허용 (robots.txt 설정)

## Schema Markup (구조화된 데이터)
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Product Name",
  "description": "...",
  "offers": { "@type": "Offer", "price": "0", "priceCurrency": "USD" },
  "aggregateRating": { "@type": "AggregateRating", "ratingValue": "4.5" }
}
```
주요 타입: Article, Product, FAQ, HowTo, Organization, BreadcrumbList

## Task: $ARGUMENTS
