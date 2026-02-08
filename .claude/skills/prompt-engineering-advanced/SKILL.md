---
name: prompt-engineering-advanced
description: 고급 프롬프트 엔지니어링 기법. DSPy, Instructor, 구조화된 출력, 체계적 프롬프트 최적화를 다룬다. AI 기능의 프롬프트 품질을 높일 때 사용한다.
argument-hint: "[프롬프트 최적화 대상]"
user-invocable: true
---

# Advanced Prompt Engineering

출처: DSPy, Instructor, Guidance, Outlines + 업계 베스트 프랙티스

## Structured Output Patterns

### JSON Mode
```
System: 항상 다음 JSON 스키마에 맞는 응답을 생성하세요.
{ "answer": string, "confidence": number, "sources": string[] }
```

### Instructor (Pydantic 기반)
```python
import instructor
from pydantic import BaseModel

class Analysis(BaseModel):
    summary: str
    key_findings: list[str]
    confidence: float  # 0.0 ~ 1.0

client = instructor.from_anthropic(Anthropic())
result = client.messages.create(
    model="claude-sonnet-4-5-20250929",
    response_model=Analysis,
    messages=[{"role": "user", "content": "분석해줘..."}]
)
```

### DSPy (프로그래밍적 프롬프트)
```python
import dspy

class ResearchModule(dspy.Module):
    def __init__(self):
        self.analyze = dspy.ChainOfThought("question -> analysis, sources")

    def forward(self, question):
        return self.analyze(question=question)

# 자동 프롬프트 최적화
optimizer = dspy.MIPROv2(metric=quality_metric)
optimized = optimizer.compile(module, trainset=examples)
```

## Prompt Optimization Techniques

### 1. Chain-of-Thought (CoT)
```
[질문] → "Let's think step by step" → [추론 과정] → [답변]
```
- 복잡한 추론에 효과적
- 수학, 논리, 다단계 분석

### 2. Few-Shot with Diverse Examples
```
좋은 예: 다양한 케이스를 커버하는 3-5개 예제
나쁜 예: 비슷한 케이스만 반복하는 예제
```

### 3. Self-Consistency
- 같은 질문에 여러 번 답변 생성 (temperature > 0)
- 가장 빈번한 답변 선택 (majority voting)
- 정확도 향상에 효과적

### 4. Constitutional AI 패턴
```
1단계: 초기 응답 생성
2단계: 원칙에 따라 자체 평가
3단계: 원칙에 맞게 수정
```

### 5. Meta-Prompting
```
"이 작업에 가장 적합한 접근 방식을 먼저 결정한 후,
그 방식에 따라 답변하세요."
```

## Evaluation Framework (promptfoo)
```yaml
prompts:
  - "Analyze: {{input}}"
  - "You are an expert analyst. Analyze: {{input}}"
providers:
  - anthropic:messages:claude-sonnet-4-5-20250929
tests:
  - vars: { input: "test case 1" }
    assert:
      - type: contains
        value: "expected keyword"
      - type: llm-rubric
        value: "Response should be factual and cite sources"
```

## Anti-Patterns
- 모호한 지시 ("잘 분석해줘" → "다음 3가지 관점에서 분석해줘: ...")
- 과도한 지시 (토큰 낭비) → 핵심만 간결하게
- 예제 없는 복잡한 포맷 요구 → Few-shot 추가
- Temperature 고정 → 용도에 맞게 조절 (0: 일관성, 1: 창의성)

## Task: $ARGUMENTS
