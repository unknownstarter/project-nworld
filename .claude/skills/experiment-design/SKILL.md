---
name: experiment-design
description: 통계적 실험 설계 전문 스킬. A/B 테스트, 샘플 사이즈 계산, 가설 검정, 인과 추론을 다룬다.
argument-hint: "[실험 과제 또는 질문]"
user-invocable: true
---

# Experiment Design

출처: coreyhaines31/marketingskills (ab-test-setup) + 통계학 베스트 프랙티스

## Experiment Lifecycle

### 1. 가설 수립
```
IF [변경사항]을 적용하면
THEN [측정 지표]가 [방향]으로 [수치]만큼 변할 것이다
BECAUSE [근거/이유]
```

### 2. 실험 설계

#### 샘플 사이즈 계산
필요 입력:
- 기본 전환율 (현재): e.g., 5%
- MDE (Minimum Detectable Effect): e.g., 10% 상대적 변화
- 통계적 유의수준 (α): 0.05 (일반적)
- 검정력 (1-β): 0.80 (일반적)

```python
# 예시 계산
from scipy.stats import norm
import math

def sample_size(p1, p2, alpha=0.05, power=0.80):
    z_alpha = norm.ppf(1 - alpha/2)
    z_beta = norm.ppf(power)
    n = ((z_alpha + z_beta)**2 * (p1*(1-p1) + p2*(1-p2))) / (p2 - p1)**2
    return math.ceil(n)

# 5% → 5.5% 변화 감지에 필요한 샘플
n = sample_size(0.05, 0.055)  # ~약 30,000명/그룹
```

#### 실험 기간
- 최소 1-2주 (요일 효과 포함)
- 충분한 샘플 도달까지 (peek 금지!)
- 시즌/이벤트 기간 피하기

### 3. 실행

#### Randomization
- 사용자 단위 (cookie/user ID 기반)
- 동일 사용자는 항상 같은 그룹
- 50/50 분할 (기본)

#### Guardrail Metrics
주요 지표 외에 반드시 모니터링:
- 이탈률: 급증하면 부정적 사용자 경험
- 페이지 로드 시간: 성능 영향
- 에러율: 기술적 문제

### 4. 분석

#### 통계적 유의성
```
p-value < 0.05: 통계적으로 유의
BUT 실질적 유의성도 확인 (효과 크기가 의미있는가?)
```

#### 주의사항
- IMPORTANT: p-hacking 금지 (매일 결과 확인하며 유의해질 때 멈추기)
- Multiple testing correction (여러 지표 동시 테스트 시)
- Simpson's Paradox (세그먼트별 다른 결과 가능)
- Novelty effect (새로운 것에 대한 일시적 관심)

### 5. 의사결정
```
Ship:    통계적 유의 + 실질적 효과 + Guardrail 안전
Iterate: 방향은 맞지만 효과 불충분 → 변형 테스트
Kill:    부정적 결과 또는 무의미 → 다음 가설로
```

## A/B Test 대안
| 방법 | 적합한 경우 |
|------|-------------|
| A/B Test | 1개 변수 변경, 충분한 트래픽 |
| Multivariate | 여러 변수 동시 (큰 트래픽 필요) |
| Bandit | 트래픽 적을 때, 빠른 최적화 |
| Pre/Post | 트래픽 분할 불가 시 (인과 약함) |
| Synthetic Control | 지역별 론칭 등 |

## Task: $ARGUMENTS
