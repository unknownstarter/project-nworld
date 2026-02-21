# 14. OpenClaw & AI 에이전트 (음성통화/브라우저) 현황 리서치 (2026.02)

## 목차
1. [OpenClaw(오픈클로) 상세 분석](#1-openclaw오픈클로-상세-분석)
2. [AI 음성 통화/전화 에이전트 기술 현황](#2-ai-음성-통화전화-에이전트-기술-현황)
3. [AI 브라우저 자동화/컴퓨터 사용 에이전트 현황](#3-ai-브라우저-자동화컴퓨터-사용-에이전트-현황)
4. [한국 AI 전화/음성 서비스](#4-한국-ai-전화음성-서비스)
5. [핵심 인사이트](#5-핵심-인사이트)

---

## 1. OpenClaw(오픈클로) 상세 분석

### 1.1 개요
- **정식명**: OpenClaw (구 Clawdbot, Moltbot)
- **개발자**: Peter Steinberger (오스트리아 바이브 코더)
- **출시**: 2025년 11월 (Clawdbot으로), 2026년 1월 말 OpenClaw로 대중적 인기 폭발
- **GitHub Stars**: **196,000+** (34,000 forks)
- **라이선스**: MIT (완전 오픈소스, 무료)
- **한마디**: 내 컴퓨터에서 돌아가는 AI 비서로, 메신저로 명령을 내리면 실제 행동을 수행

### 1.2 실제로 할 수 있는 것들 (프로덕션에서 검증됨)

#### 전화 통화 (Voice Call)
- **레스토랑 예약**: OpenTable에서 예약이 안 되면, 자동으로 Google Maps에서 전화번호를 찾아 직접 전화해서 예약 완료
- **대량 전화**: Dave Lee 데모에서 5개 도시의 80개+ 한식/베트남 식당에 전화해서 MSG/진짜 사골 사용 여부 문의
- **매장 문의**: Dick's Sporting Goods, 오락실 등에 특정 제품 재고 문의 전화
- **음성사서함 감지**: 음성사서함이면 자동으로 끊음

#### 브라우저 자동화
- 웹 브라우징, 폼 작성, 데이터 추출
- Google Cloud Console에서 OAuth 설정 + 토큰 발급을 자율적으로 수행한 사례
- 항공편 체크인, 건강보험 환급 처리

#### 메시지/이메일 관리
- 847개 뉴스레터 중 203개 구독 해제를 자동으로 수행
- 이메일 관리, 캘린더 일정 조정

#### 파일/코드 작업
- 파일 시스템 읽기/쓰기
- 셸 명령 실행, 코드 작성/실행
- 100개+ 상세 PDF 가이드 자동 생성 (레스토랑, 활동, 트레이딩 전략 등)

#### 모니터링/알림
- 특정 트윗 모니터링 + 관련 정보 발견 시 문자 알림
- 테니스 일정 추적, 스케줄 변경 알림

### 1.3 기술 스택

```
┌─────────────────────────────────────────┐
│              OpenClaw 아키텍처            │
├─────────────────────────────────────────┤
│ 런타임: TypeScript/Node.js (Node ≥22)    │
│ 패키지: pnpm                             │
│ 제어: WebSocket Gateway (ws://127.0.0.1) │
├─────────────────────────────────────────┤
│ LLM 백엔드:                              │
│  - Anthropic Claude (Opus 4.6 권장)      │
│  - OpenAI GPT-4o                         │
│  - Ollama 로컬 모델 (무료)                │
├─────────────────────────────────────────┤
│ 음성 통화 (Voice Call Plugin):            │
│  - 전화 제공자: Twilio / Telnyx / Plivo  │
│  - STT: OpenAI Whisper                   │
│  - TTS: ElevenLabs / OpenAI TTS          │
│  - 음성 대화: Vapi / ElevenLabs Agents   │
│  - 실시간 음성: OpenAI Realtime API      │
├─────────────────────────────────────────┤
│ 채널 (13개+):                            │
│  WhatsApp, Telegram, Discord, Slack,     │
│  Signal, iMessage, Google Chat,          │
│  Microsoft Teams, Matrix, Zalo 등        │
├─────────────────────────────────────────┤
│ 브라우저: 전용 Chromium 인스턴스           │
│ 스킬: ClawHub (커뮤니티 스킬 레지스트리)   │
│ 앱: macOS (Swift), iOS, Android          │
└─────────────────────────────────────────┘
```

#### 음성 통화 구현 방식 (상세)
1. **STT + LLM + TTS 파이프라인**: 전화 수신/발신 → 음성을 텍스트로 변환(STT) → LLM이 응답 생성 → 텍스트를 음성으로 변환(TTS) → 전화로 출력
2. **전화 제공자**: Twilio Programmable Voice + Media Streams, Telnyx Call Control v2, Plivo Voice API
3. **음성 합성**: ElevenLabs (다국어 지원, eleven_multilingual_v2) 또는 OpenAI TTS
4. **대안 구성**: Vapi를 전화 제공자로, ElevenLabs를 TTS로 사용하는 통합 구성 가능
5. **ElevenLabs Agents 방식**: ElevenLabs 플랫폼이 음성/턴테이킹을 전담하고, OpenClaw Gateway를 커스텀 LLM으로 연결
6. **지연시간**: Deepgram Voice Agent API 사용 시 턴당 ~2.2-3.4초 (Haiku 4.5 기준)

### 1.4 비즈니스 모델 / 비용

| 구분 | 비용 |
|------|------|
| **소프트웨어** | 완전 무료 (MIT 라이선스) |
| **가벼운 사용** (10-50 메시지/일) | $5-10/월 (API 비용) |
| **일반 사용** (50-200 메시지/일) | $15-30/월 |
| **파워 사용** (200-500 메시지/일) | $40-100/월 |
| **호스팅** (자기 컴퓨터) | $0 |
| **호스팅** (VPS) | $4-6/월 |
| **클라우드 호스팅** (OpenClaw Cloud) | $39/월~ |
| **완전 무료 구성** | Ollama 로컬 모델 + Oracle Cloud Free Tier |

### 1.5 한계점 및 문제점

#### 보안 문제 (심각)
- **원클릭 RCE 취약점**: 악성 링크를 통한 원격 코드 실행 (토큰 탈취 + WebSocket 하이재킹)
- **135,000개+ 인터넷 노출 인스턴스**: 기본 설정이 `0.0.0.0:18789`로 공개 인터넷에 노출
- **40개+ 보안 취약점**: 2026.2.12 버전에서 대규모 보안 패치
- **스킬 스토어 악성 소프트웨어**: API 키, 신용카드, 개인정보 유출 위험
- **프롬프트 인젝션**: 자율 모드에서 공격 성공률 11.2%

#### 기능적 한계
- 설정 복잡성: 일반 사용자가 세팅하기 어려움 (Twilio 계정, API 키, ngrok 설정 등)
- 음성 통화 지연: 턴당 2-3초 지연 (자연스러운 대화에 부족)
- 안정성: WebSocket 연결 끊김, 레이트 리밋 이슈
- LLM 의존성: AI 모델의 환각/오류가 그대로 실세계 행동으로 이어짐

---

## 2. AI 음성 통화/전화 에이전트 기술 현황

### 2.1 주요 플랫폼 비교 (2026년 2월 기준)

| 플랫폼 | 핵심 특징 | 가격 | 지연시간 | 용도 |
|--------|----------|------|---------|------|
| **Vapi** | 개발자 중심, API-first, 고도 커스터마이징 | $0.05/분 (플랫폼) + 서드파티 비용 = ~$0.30-0.33/분 | 500-800ms | 인바운드/아웃바운드, 예약, 리드 |
| **Bland.ai** | 감성적 음성, 자체 인프라, 100만 동시통화 | $0.09/분 (연결), $0.015/10초 미만 | - | 대규모 아웃바운드, 영업, 알림 |
| **Retell AI** | 드래그앤드롭 에이전트 빌더, 99.99% 가동 | 사용량 기반, 플랫폼 수수료 없음 | ~600ms | 고객지원, 예약, IVR 네비게이션 |
| **ElevenLabs** | 최고 음질, 90개+ 언어, 감정표현 | - | - | 음성 합성, 컨버세이셔널 AI |
| **Synthflow** | 노코드, 산업별 템플릿 | - | - | 예약, 클레임, 소매 |
| **Lindy** | 올인원, 멀티 동시통화 | $18-375/월 | - | 리드, 지원, 다목적 |
| **OpenAI Realtime API** | SIP 전화 지원, 비언어적 단서 감지 | - | - | 고객지원, 개인비서, 교육 |

### 2.2 실제 프로덕션에서 작동하는 것들

#### 레스토랑 예약 (AI가 실제 전화 걸기)
- **OpenClaw + Twilio + OpenAI Realtime API**: 실제 레스토랑에 전화해서 예약 완료 (Dave Lee 데모, 80개+ 식당)
- **OpenTable Voice AI**: 레스토랑 입장에서 AI가 전화를 받아 예약 처리 → 월 $3,000-$18,000 추가 매출 (2026 Forbes)
- **Google Duplex** (현재 "Google Assistant 전화"): 49개 미국 주에서 레스토랑 예약, 이발 예약 등 작동 중

#### 구독 취소/요금 협상 (AI가 대신 전화)
- **Pine AI**: 93% 협상 성공률, 사용자당 연 평균 $300 절약, 건당 270분 절약
  - 요금 협상, 구독 취소, 환불 요청, 민원 접수
  - 전화 메뉴 네비게이션, 대기, AI 기반 협상 수행
  - 성과 기반 수수료 (절약한 금액의 일부) 또는 $2-10 정액
- **Unsub AI**: AI 컨시어지가 구독 서비스에 전화해서 요금 면제 협상 (연 $2K+ 절약 사례)

#### 고객 지원 자동화 (인바운드)
- **Retell AI**: 의료 환경에서 통화 처리 비용 80% 절감, 85% containment rate
- **호텔 AI 음성**: 놓친 전화 80% 감소, 부가 수익 25% 증가, 고객 만족도 27% 상승
- **GPT-4o 기반 에이전트**: 멀티턴 함수 호출에서 70%+ 성공률

### 2.3 핵심 기술 요소

```
전화 수신/발신 ──→ STT (Whisper/Deepgram) ──→ LLM (GPT-4o/Claude)
       ↑                                              │
       │                                              ↓
   Twilio/Telnyx          ←── TTS (ElevenLabs/OpenAI) ←── 응답 생성
   SIP 게이트웨이
```

- **지연시간**: 업계 최고 ~500-800ms (Vapi, Retell), 일반적으로 1-3초
- **자연스러움**: 끼어들기 처리, 비언어적 단서 감지, 언어 전환, 톤 적응
- **턴테이킹**: 독자적 턴테이킹 모델로 언제 말하고 들을지 판단

---

## 3. AI 브라우저 자동화/컴퓨터 사용 에이전트 현황

### 3.1 벤치마크 성적 요약

| 에이전트 | WebVoyager | WebArena | OSWorld | 비고 |
|---------|-----------|---------|---------|------|
| **Browser Use** | **89.1%** | - | - | 오픈소스 SOTA |
| **OpenAI CUA (Operator)** | 87% | 58.1% | 38.1% | ChatGPT 통합됨 |
| **Google Mariner** | 83.5% | - | - | Gemini 2.0 기반 |
| **Anthropic Computer Use** | - | - | 61% (하이브리드) | Claude, MCP 활용 |
| **Simular Agent S2** | - | - | 34.5% | - |
| **인간** | ~95% | 78.2% | 72.4% | 참고 기준 |

### 3.2 핵심 포인트: 범용 벤치마크 vs 특정 태스크 성공률

**노아님 말씀이 정확합니다.** CUB 10.4%는 "7개 산업에 걸친 106개 엔드투엔드 워크플로우"의 평균이며, 특정 잘 정의된 태스크의 성공률과는 크게 다릅니다.

#### 사이트별 성공률 차이 (Browser Use 기준)
| 사이트 유형 | 성공률 |
|-----------|--------|
| Wolfram Alpha | **95.7%** |
| Google Search | **90.7%** |
| Google Maps | **87.8%** |
| 정적/단순 사이트 | **85-95%** |
| Booking.com | **27.3%** |
| Google Flights | **35.7%** |
| 동적/복잡 사이트 | **25-40%** |

**결론**: "레스토랑 전화번호 찾기" 같은 특정 태스크는 90%+ 성공률이지만, "항공권 비교/예약" 같은 복잡한 태스크는 30% 수준. 태스크의 정의와 복잡도에 따라 성공률이 극적으로 달라짐.

### 3.3 실제 프로덕션 사용 사례

#### OpenAI Operator (CUA)
- **식료품 주문**: Instacart, DoorDash와 공식 통합 → ChatGPT 안에서 식료품 주문부터 배달까지
- **레스토랑 예약**: OpenTable에서 날짜/시간/요리 유형 필터 설정 → 예약 가능 옵션 찾기 → 확인 후 예약 완료
- **티켓 구매**: 농구 경기 티켓 등 예매
- **반복 태스크**: Instacart 장보기 등 저장해두고 반복 사용
- **한계**: 복잡한 인터페이스 (슬라이드쇼 생성, 캘린더 관리)에서 실패, 일부 사이트 AI 차단

#### Google Project Mariner
- **동시 10개 태스크** 처리 가능
- **Teach & Repeat**: 워크플로우 학습 후 반복 실행
- **가용성**: Google AI Ultra 구독자만 (미국 한정)
- **2026 로드맵**: Q2 Mariner Studio (비주얼 빌더), Q3 크로스 디바이스, Q4 에이전트 마켓플레이스

#### Anthropic Computer Use (Claude)
- **OSWorld 성공률**: 14.9% → 61%로 급상승 (MCP 프로토콜 덕분)
- **실제 파트너**: Asana, Canva, DoorDash, Replit, The Browser Company
- **Claude for Chrome**: 브라우저 직접 제어 (제한적 베타)
- **한계**: 프롬프트 인젝션 공격 성공률 11.2% (개선 중이나 아직 불충분)

### 3.4 실제 성공률 해석 가이드

```
태스크 복잡도별 예상 성공률 (2026.02 기준):

[단순/구조화]                              [복잡/비구조화]
  95%    90%    85%    70%    50%    30%    10%
   │      │      │      │      │      │      │
   ├──────┤      │      │      │      │      │
   │ 검색/정보 조회                          │
   │      ├──────┤      │      │      │      │
   │      │ 폼 작성 (단순)                   │
   │      │      ├──────┤      │      │      │
   │      │      │ 예약/주문 (파트너 사이트)  │
   │      │      │      ├──────┤      │      │
   │      │      │      │ 가격 비교/쇼핑      │
   │      │      │      │      ├──────┤      │
   │      │      │      │      │ 복잡 UI 조작  │
   │      │      │      │      │      ├──────┤
   │      │      │      │      │      │ 범용 CUB │
```

---

## 4. 한국 AI 전화/음성 서비스

### 4.1 SK텔레콤 에이닷 (A.)
- **가입자**: 1,000만+ (2025년 8월 기준)
- **에이닷 전화**: T전화의 AI 진화 버전
- **주요 기능**:
  - AI 통화 요약: 녹음 후 핵심 내용 자동 정리 (월 30건)
  - 보이스피싱 실시간 탐지: 통화 중 위험 알림
  - 통역 통화: 한국어-영어-일본어-중국어 실시간 동시통역
  - AI 브리핑: 일정/날씨/콘텐츠 종합 분석 (베타)
- **한계**: 대신 전화를 걸어주는 "전화 대행" 기능은 아직 없음. 통화 관리/보안 중심

### 4.2 LG U+ ixi-O
- LG AI Research의 Exaone + Google Gemini 기반
- 15만 가입자 (2025년 말)
- 주로 고객 서비스/상담 자동화 초점

### 4.3 한국 시장 현황
- **소비자 대상 AI 전화 대행 서비스**: 아직 없음
  - 에이닷/ixi-O는 통화 관리(녹음, 요약, 보안)에 집중
  - OpenClaw이나 Pine AI 같은 "AI가 대신 전화 걸어주는" 서비스는 한국에 없음
- **B2B AI 콜센터**: AICX (마이리얼트립 자회사), 채널코퍼레이션 (채널톡) 등
  - 주로 인바운드 고객 상담 자동화
- **기회**: 한국어 AI 전화 대행 서비스는 블루오션

---

## 5. 핵심 인사이트

### 5.1 이전 분석의 보수성에 대해

노아님의 지적이 맞습니다. CUB 10.4%를 AI 에이전트의 한계로 인용한 것은 과도하게 보수적이었습니다.

**실제 현황 정리:**

| 태스크 유형 | 실제 성공률 | 프로덕션 준비도 |
|-----------|-----------|--------------|
| AI가 전화로 레스토랑 예약 | **실제 작동** | 프로덕션 (OpenClaw, Google Duplex) |
| AI가 전화로 구독 취소/요금 협상 | **93% 성공률** | 프로덕션 (Pine AI) |
| AI가 식료품 주문 | **실제 작동** | 프로덕션 (Operator + Instacart) |
| AI가 브라우저로 예약 | **70-87%** | 프로덕션 (Operator, Mariner) |
| AI가 이메일/구독 관리 | **실제 작동** | 프로덕션 (OpenClaw) |
| AI가 복잡한 웹 워크플로우 | **30-60%** | 베타/제한적 |
| AI가 범용 컴퓨터 작업 | **10-38%** | 연구 단계 |

### 5.2 노아님 프로젝트에 대한 시사점

1. **AI 전화 대행은 이미 현실**: Pine AI(93% 성공률), OpenClaw, Google Duplex가 실제 프로덕션에서 작동. "기술적으로 가능한가?"가 아니라 "어떤 UX로 제공할 것인가?"의 문제

2. **한국 시장 공백**: 한국어로 "AI가 대신 전화 걸어주는" 소비자 서비스가 없음. 에이닷은 통화 관리 도구이지 전화 대행 서비스가 아님

3. **특정 태스크 > 범용 에이전트**: 범용 벤치마크(CUB 10.4%)보다 특정 태스크(레스토랑 예약 93%, 구독 취소 93%)의 성공률이 압도적으로 높음. 좁은 범위에서 확실하게 작동하는 제품이 가능

4. **기술 스택 성숙**: Twilio/Telnyx(전화) + ElevenLabs/OpenAI(음성) + GPT-4o/Claude(두뇌)의 조합으로 500ms 지연시간의 자연스러운 통화 가능

5. **OpenClaw의 교훈**: 196K 스타가 증명하듯 사람들은 "실제로 행동하는 AI"를 원함. 하지만 보안 이슈(135K 노출 인스턴스)와 설정 복잡성이 대중화의 최대 장벽

6. **비즈니스 모델 참고**: Pine AI의 성과 기반 수수료 모델 (절약한 금액의 일부) 또는 건당 $2-10 정액은 소비자 AI 에이전트의 유력한 수익 모델

---

## 출처

### OpenClaw
- [OpenClaw 공식 사이트](https://openclaw.ai/)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw Voice Call Plugin 문서](https://docs.openclaw.ai/plugins/voice-call)
- [Dave Lee의 OpenClaw 전화 데모 (X)](https://x.com/heydave7/status/2020279547745226803)
- [OpenClaw 보안 이슈 - The Register](https://www.theregister.com/2026/02/02/openclaw_security_issues/)
- [OpenClaw 가격 가이드](https://www.thecaio.ai/blog/openclaw-pricing-guide)
- [1Password - OpenClaw 분석](https://1password.com/blog/its-openclaw)

### AI 음성 통화
- [Vapi AI 공식](https://vapi.ai/)
- [Bland.ai 공식](https://www.bland.ai/)
- [Retell AI 공식](https://www.retellai.com/)
- [Pine AI 공식](https://www.19pine.ai/)
- [OpenAI Realtime API](https://openai.com/index/introducing-gpt-realtime/)
- [Lindy AI 음성 에이전트 리뷰](https://www.lindy.ai/blog/ai-voice-agents)
- [Voiceflow - AI Call Agent for Restaurants](https://www.voiceflow.com/blog/ai-call-agent-for-restaurants)
- [OpenTable Voice AI](https://www.opentable.com/restaurant-solutions/products/reservation-management/voice-ai/)

### AI 브라우저 에이전트
- [OpenAI CUA 소개](https://openai.com/index/computer-using-agent/)
- [OpenAI Operator 소개](https://openai.com/index/introducing-operator/)
- [Google Project Mariner](https://deepmind.google/models/project-mariner/)
- [Anthropic Computer Use](https://www.anthropic.com/news/3-5-models-and-computer-use)
- [Browser Use SOTA 리포트](https://browser-use.com/posts/sota-technical-report)
- [AI Computer-Use Benchmarks Guide](https://o-mega.ai/articles/the-2025-2026-guide-to-ai-computer-use-benchmarks-and-top-ai-agents)
- [Instacart + OpenAI 파트너십](https://openai.com/index/instacart-partnership/)

### 한국 서비스
- [SK텔레콤 에이닷 전화 2026](https://news.sktelecom.com/218051)
- [한국 통신사 AI 에이전트 경쟁 - Korea Herald](https://www.koreaherald.com/article/10390038)
