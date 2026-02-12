---
name: database-optimizer
description: 데이터베이스 쿼리 최적화, 인덱싱 전략, EXPLAIN 분석, 스키마 설계를 다룬다. DB 성능 이슈가 있을 때 사용한다.
argument-hint: "[쿼리 또는 DB 성능 이슈]"
user-invocable: true
---

# Database Optimizer

출처: SkillsMP database-optimizer + Supabase postgres-best-practices

## Query Optimization Process

### 1. EXPLAIN ANALYZE
```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT) SELECT ...
```
주요 지표:
- **Seq Scan vs Index Scan**: Seq Scan이 대용량 테이블에서 반복되면 인덱스 필요
- **Actual Time**: 실제 실행 시간
- **Rows Removed by Filter**: 필터링 비효율성 지표
- **Buffers**: I/O 패턴 분석

### 2. 인덱싱 전략
```
- B-tree: 기본 인덱스, 대부분의 경우 적합
- GIN: 배열, JSONB, 전문 검색
- GiST: 지리 데이터, 범위 쿼리
- BRIN: 시계열 데이터 (순서가 있는 큰 테이블)
- Partial Index: 조건부 인덱스 (WHERE 절 포함)
- Composite Index: 다중 컬럼 (순서 중요: 선택도 높은 것 먼저)
```

### 3. 쿼리 패턴별 최적화

**N+1 문제**
```sql
-- Bad: N+1 쿼리
SELECT * FROM orders WHERE user_id = 1;
-- 각 order에 대해 SELECT * FROM items WHERE order_id = ?

-- Good: JOIN 또는 서브쿼리
SELECT o.*, i.* FROM orders o
LEFT JOIN items i ON i.order_id = o.id
WHERE o.user_id = 1;
```

**Pagination**
```sql
-- Bad: OFFSET (느려짐)
SELECT * FROM posts ORDER BY created_at DESC LIMIT 20 OFFSET 1000;

-- Good: Cursor-based
SELECT * FROM posts
WHERE created_at < $cursor
ORDER BY created_at DESC LIMIT 20;
```

**Count 최적화**
```sql
-- Bad: 정확한 카운트 (느림)
SELECT COUNT(*) FROM large_table WHERE ...;

-- Good: 추정치 (빠름)
SELECT reltuples FROM pg_class WHERE relname = 'large_table';
```

## PostgreSQL 특화

### Connection Pooling
- PgBouncer 또는 Supabase Connection Pooler 사용
- Transaction mode 권장 (서버리스 환경)
- Max connections: CPU 코어 * 2 + effective_cache_size

### Vacuum & Bloat
- autovacuum 설정 확인
- Dead tuples 비율 모니터링
- `pg_stat_user_tables`로 bloat 확인

### RLS (Row-Level Security)
- Supabase 환경에서 필수
- RLS 정책이 쿼리 성능에 미치는 영향 주의
- 인덱스가 RLS 조건을 커버하는지 확인

## Schema Design Principles
1. 정규화 우선 (3NF), 성능 필요시 비정규화
2. UUID vs Serial: UUID는 분산에 유리, Serial은 인덱스 효율적
3. JSONB: 스키마리스 데이터에 적합하나 인덱싱 전략 필요
4. Soft delete: `deleted_at` 컬럼 + Partial index

## Task: $ARGUMENTS
