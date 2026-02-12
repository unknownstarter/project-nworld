---
name: rag-pipeline
description: RAG(Retrieval-Augmented Generation) 파이프라인 전체 설계 및 구현. 청킹, 임베딩, 벡터 검색, 리랭킹, 생성까지 다룬다.
argument-hint: "[RAG 과제 또는 질문]"
user-invocable: true
---

# RAG Pipeline Architecture

## End-to-End Pipeline

```
Documents → Chunking → Embedding → Vector Store → Retrieval → Reranking → Generation → Response
```

## 1. Document Processing

### Chunking Strategies
| 전략 | 설명 | 적합한 경우 |
|------|------|-------------|
| Fixed Size | 고정 토큰 수 (500-1000) | 빠른 시작, 균일한 문서 |
| Semantic | 의미 단위로 분할 | 구조화된 문서 |
| Recursive | 큰 → 작은 단위로 재귀 분할 | 일반적 텍스트 |
| Document-aware | 제목/섹션 기반 | Markdown, HTML |
| Sliding Window | 오버랩 포함 분할 | 맥락 유지 필요시 |

### Chunk Size 가이드
- 너무 작으면 (< 200 토큰): 맥락 부족
- 너무 크면 (> 2000 토큰): 노이즈 증가
- 최적: 500-1000 토큰, 10-20% 오버랩

## 2. Embedding

### 모델 선택 (2026)
| 모델 | 차원 | 특징 |
|------|------|------|
| text-embedding-3-large | 3072 | 가장 높은 정확도 |
| text-embedding-3-small | 1536 | 비용 효율적 |
| Cohere embed-v4 | 1024 | 다국어 강점 |
| BGE-M3 | 1024 | 오픈소스, 다국어 |

### 임베딩 최적화
- Query와 Document를 다른 프롬프트로 임베딩 (비대칭 검색)
- Matryoshka embedding: 차원 축소 가능 (3072 → 512)
- Late interaction: ColBERT 스타일 토큰 레벨 매칭

## 3. Vector Store

| Store | 특징 | 적합한 경우 |
|-------|------|-------------|
| pgvector | PostgreSQL 확장 | 이미 Postgres 사용 시 |
| Pinecone | 완전 관리형 | 빠른 시작, 스케일 |
| Qdrant | 고성능, 필터링 | 복잡한 메타데이터 필터 |
| Weaviate | 하이브리드 검색 내장 | 키워드+벡터 결합 |
| ChromaDB | 경량, 로컬 | 프로토타입, 개발 |

## 4. Retrieval Strategy

### Hybrid Search (권장)
```
Final Score = α × Vector Score + (1-α) × BM25 Score
```
- Vector: 의미적 유사성 (동의어, 패러프레이즈)
- BM25: 키워드 정확 매칭 (고유명사, 코드)
- α = 0.5~0.7 일반적

### 리랭킹 (Re-ranking)
- Cross-encoder (Cohere Rerank, BGE Reranker)
- 초기 검색 top-50 → 리랭킹 → top-5
- 정확도 10-30% 향상

## 5. Generation

### Context Window 최적화
```
System Prompt (지시)
+ Retrieved Context (검색 결과, 가장 관련 있는 것 먼저)
+ User Query (질문)
= LLM Response (답변 + 출처 인용)
```

### Citation 패턴
- 각 검색 결과에 [1], [2] 등 번호 부여
- LLM이 답변에 인용 번호 포함하도록 지시
- 사용자가 출처 확인 가능

## 6. Evaluation

| 지표 | 설명 | 도구 |
|------|------|------|
| Context Relevance | 검색된 문서의 관련성 | RAGAS |
| Faithfulness | 답변이 컨텍스트에 충실한지 | RAGAS |
| Answer Relevance | 답변이 질문에 적절한지 | RAGAS |
| Hallucination Rate | 컨텍스트에 없는 정보 비율 | Custom eval |

## Task: $ARGUMENTS
