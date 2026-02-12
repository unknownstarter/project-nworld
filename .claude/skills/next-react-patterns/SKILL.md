---
name: next-react-patterns
description: Vercel 공식 팀의 Next.js + React 베스트 프랙티스. Server Components, 캐싱, 컴포지션 패턴, 성능 최적화를 다룬다.
argument-hint: "[Next.js/React 관련 과제]"
user-invocable: true
---

# Next.js + React Best Practices

출처: vercel-labs/agent-skills (Vercel 공식)

## Server Components (기본값)
```
- 서버 컴포넌트가 기본. 'use client'는 필요한 곳에서만
- 서버: 데이터 페칭, DB 접근, 무거운 의존성
- 클라이언트: 이벤트 핸들러, useState/useEffect, 브라우저 API
```

### Client/Server 경계 패턴
```tsx
// Good: 서버 컴포넌트가 데이터를 가져와서 클라이언트에 전달
async function ServerPage() {
  const data = await fetchData();
  return <ClientInteraction data={data} />;
}

// Bad: 클라이언트에서 불필요하게 fetch
'use client';
function ClientPage() {
  const [data, setData] = useState(null);
  useEffect(() => { fetch(...) }, []); // 이러지 마세요
}
```

## Composition Patterns
1. **Children Pattern**: 렌더링 위임
2. **Render Props**: 로직 공유, UI 분리
3. **Compound Components**: 관련 컴포넌트 묶음 (Tabs, Accordion)
4. **Slot Pattern**: 여러 삽입 지점 제공
5. **Provider Pattern**: Context로 상태 하위 전달

## Caching Strategy (Next.js 15+)
```
1. Request Memoization: 같은 요청 자동 중복 제거
2. Data Cache: fetch 결과 캐싱 (revalidate 옵션)
3. Full Route Cache: 정적 페이지 빌드 타임 캐싱
4. Router Cache: 클라이언트 사이드 페이지 캐시
```

### 캐시 무효화
- `revalidatePath('/path')` - 특정 경로
- `revalidateTag('tag')` - 태그 기반
- Time-based: `{ next: { revalidate: 3600 } }`

## App Router Patterns
- **Parallel Routes**: `@modal`, `@sidebar` 동시 렌더링
- **Intercepting Routes**: `(.)photo` 모달 라우팅
- **Route Groups**: `(marketing)`, `(app)` 레이아웃 분리
- **Loading UI**: `loading.tsx` 자동 Suspense
- **Error Handling**: `error.tsx` 에러 바운더리

## Performance Checklist
- [ ] Images: `next/image` with width/height, priority for LCP
- [ ] Fonts: `next/font` (자동 최적화)
- [ ] Dynamic imports: `next/dynamic` (코드 스플리팅)
- [ ] Metadata: `generateMetadata()` (SEO)
- [ ] Streaming: React Suspense 활용

## React 19+ Patterns
- **Server Actions**: `'use server'` 함수로 폼 처리
- **useActionState**: 액션 상태 관리
- **useOptimistic**: 낙관적 UI 업데이트
- **use()**: Promise/Context 직접 읽기

## Task: $ARGUMENTS
