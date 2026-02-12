---
name: trend-analysis
description: 산업, 기술, 사회 트렌드를 체계적으로 분석한다. 메가트렌드, 시그널, 와일드카드를 식별하고 미래 시나리오를 구성한다.
argument-hint: "[분석할 산업 또는 트렌드]"
user-invocable: true
---

# Trend Analysis

## Framework: STEEP + Signals + Scenarios

### 1. Signal Scanning
트렌드의 초기 신호(Signal)를 포착:
- **Strong Signals**: 주류 미디어에서 다루는 명확한 트렌드
- **Weak Signals**: 니치 커뮤니티, 학술 논문, 특허에서 포착되는 초기 신호
- **Wild Cards**: 발생 확률 낮지만 임팩트 큰 사건

### 2. STEEP 분석
| 차원 | 분석 질문 |
|------|-----------|
| **S**ocial | 인구, 라이프스타일, 가치관 변화 |
| **T**echnological | 신기술, 혁신 속도, 채택 곡선 |
| **E**conomic | 시장 규모, 투자 흐름, 비즈니스 모델 |
| **E**nvironmental | 지속가능성, 규제, 자원 |
| **P**olitical | 정책, 규제, 지정학 |

### 3. Trend Classification
```
Megatrend (10년+): 방향 확실, 역전 불가
  예: AI 일상화, 고령화, 기후변화

Macro Trend (3-5년): 뚜렷한 성장 궤적
  예: AI 에이전트, 바이브코딩, AI 커머스

Micro Trend (1-2년): 현재 뜨고 있는 것
  예: AI 브라우저, 음성 인터페이스, 웨어러블 AI

Fad (< 1년): 반짝 유행
  구분법: 근본적 니즈를 해결하는가?
```

### 4. Trend Impact Matrix
```
         High Impact
              |
    Monitor   |   Act Now
              |
Low Certainty -------- High Certainty
              |
    Ignore    |   Plan For
              |
         Low Impact
```

### 5. Future Scenarios
3가지 시나리오 구성:
- **Optimistic**: 트렌드가 가속되고 우리에게 유리한 경우
- **Base Case**: 현재 추세가 지속되는 경우
- **Pessimistic**: 역풍이 부는 경우 (규제, 경쟁, 기술 한계)

## Output Format
```
## 트렌드 분석: [주제]

### 핵심 트렌드 (Top 5)
1. [트렌드명] — [영향도] / [확실성]

### 시그널 맵
Strong: ...
Weak: ...
Wild Cards: ...

### 시나리오
1. Optimistic: ...
2. Base Case: ...
3. Pessimistic: ...

### 기회 영역
[우리가 활용할 수 있는 지점]

### Sources
[출처 목록]
```

## Task: $ARGUMENTS
