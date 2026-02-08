---
name: data-visualization
description: D3.js, Plotly, 차트 패턴, 대시보드 설계 등 데이터 시각화 전문 스킬. 데이터를 시각적으로 전달할 때 사용한다.
argument-hint: "[시각화할 데이터 또는 대시보드 과제]"
user-invocable: true
---

# Data Visualization

출처: chrisvoncsefalvay/claude-d3js-skill + 시각화 베스트 프랙티스

## Chart Selection Guide

### 비교
| 차트 | 적합한 경우 |
|------|-------------|
| Bar Chart | 카테고리 간 비교 |
| Grouped Bar | 다중 카테고리 비교 |
| Radar/Spider | 다차원 비교 (5-8개 축) |

### 추세/시계열
| 차트 | 적합한 경우 |
|------|-------------|
| Line Chart | 시간에 따른 변화 |
| Area Chart | 누적 추세 |
| Sparkline | 인라인 미니 차트 |

### 구성/비율
| 차트 | 적합한 경우 |
|------|-------------|
| Pie/Donut | 전체 중 비율 (3-5개 이하) |
| Stacked Bar | 카테고리별 구성 비교 |
| Treemap | 계층적 비율 |

### 분포/관계
| 차트 | 적합한 경우 |
|------|-------------|
| Scatter Plot | 두 변수 관계 |
| Histogram | 값의 분포 |
| Heatmap | 2차원 밀도/상관관계 |
| Bubble Chart | 3변수 관계 (x, y, size) |

### 퍼널/플로우
| 차트 | 적합한 경우 |
|------|-------------|
| Funnel | 전환 퍼널 |
| Sankey | 플로우/이동 |
| Gantt | 타임라인/프로젝트 |

## Tech Stack

### 웹 기반
- **D3.js**: 완전한 커스텀 (학습 곡선 높음)
- **Recharts**: React + D3 래퍼 (빠른 개발)
- **Nivo**: React 기반, 선언적 API
- **Plotly.js**: 인터랙티브, 과학 데이터
- **Observable Plot**: D3의 고수준 API

### Python 기반
- **Matplotlib**: 기본, 정적
- **Seaborn**: 통계 시각화
- **Plotly**: 인터랙티브
- **Altair**: 선언적, Vega-Lite 기반

## Dashboard Design Principles

### Layout
- F-패턴: 가장 중요한 KPI를 좌상단
- 계층: 요약(상단) → 상세(하단)
- 여백: 차트 간 충분한 간격
- 반응형: 모바일에서도 핵심 지표 확인 가능

### Color
- 의미 기반 색상: 빨강=위험, 초록=좋음, 노랑=주의
- 색맹 고려: 빨강-초록 조합 피하기 (Blue-Orange 대안)
- 최대 7색 (인간이 구분 가능한 한계)
- 데이터 잉크 비율 최대화 (Tufte)

### Interactivity
- Tooltip: hover 시 상세 정보
- Filter: 날짜 범위, 세그먼트 선택
- Drill-down: 요약 → 상세로 탐색
- Cross-filtering: 하나의 차트 클릭 → 다른 차트 연동

## Anti-Patterns
- 3D 차트 (시각적 왜곡)
- 이중 Y축 (혼란)
- 파이 차트 5개 이상 조각
- 장식적 요소 (chartjunk)
- 축 원점이 0이 아닌 바 차트

## Task: $ARGUMENTS
