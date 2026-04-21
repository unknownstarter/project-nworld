# AI 숏폼 드라마 제작 가능성 종합 리서치

> 작성일: 2026-02-20
> 작성자: 아리 (Ari)
> 목적: AI API를 활용한 숏폼 드라마 자동화 제작의 기술적 가능성, 비용, 실제 사례, 전략적 방향 종합 분석
> 관련 문서: [14-opportunity-landscape-2026.md](14-opportunity-landscape-2026.md)

---

## Executive Summary

**결론: AI 숏폼 드라마 제작은 지금 당장 가능하며, 이미 상업적 성공 사례가 존재한다.**

- 80에피소드 시리즈 제작비: **$600~$2,500** (풀 AI) vs $150,000~$300,000 (전통) → **99% 절감**
- 검증된 사례: 구미호 AI 드라마 **1.7억 뷰**, Vigloo **4인 팀**으로 풀시리즈 제작, Topco Media **5배 ROI**
- 추천 접근: **웹툰/만화 스타일** → 캐릭터 일관성 문제 회피, 한국 시장 친화적
- 핵심 파이프라인: LLM(각본) → Midjourney/Flux(스틸) → Kling/Veo 3/Seedance(영상) → ElevenLabs(음성) → Suno(음악) → CapCut(편집)

---

## 1. AI 비디오 생성 도구 현황 (2026년 2월)

### 1-1. 도구별 상세 분석

#### Sora 2 (OpenAI)
- **품질**: 가장 뛰어난 스토리텔링, 감정 표현, 대화 장면. 시네마틱 퀄리티
- **해상도**: 480p ~ 1080p (플랜별)
- **영상 길이**: 최대 20-35초/클립 (Pro 기준)
- **가격**: API $0.10~$0.50/초 → **1분 영상: $6~$30**
- **강점**: 내러티브 코히어런스, 감정 깊이, 대화 장면
- **약점**: 짧은 클립 길이, 높은 가격

#### Veo 3 / 3.1 (Google DeepMind)
- **품질**: 물리적 리얼리즘 최상위. **네이티브 오디오 생성** (대화+효과음+환경음 동시 생성)
- **해상도**: 720p ~ 2K
- **영상 길이**: 최대 60초/클립
- **가격**: API $0.15~$0.75/초 → **1분 영상: $9~$45 (오디오 포함)**
- **강점**: **업계 유일 네이티브 립싱크 오디오 생성**, 자연스러운 바디랭귀지
- **약점**: 가장 비쌈, Ultra 플랜 필요

#### Kling 2.6 (Kuaishou / 쾌수)
- **품질**: 시네마틱 수준의 비주얼, 가격 대비 최고 품질
- **해상도**: 720p ~ 1080p
- **영상 길이**: 최대 3분/클립 (경쟁사 중 최장)
- **가격**: API $0.07~$0.14/초 → **1분 영상: ~$1.04**
- **강점**: **가성비 최강**, 긴 클립 길이 (3분), 안정적 품질
- **약점**: 다중 캐릭터 장면에서 일관성 약함

#### Runway Gen-4 / Gen-4.5
- **품질**: 스타일라이즈드 창작에 최강. **캐릭터 일관성 기능 내장**
- **해상도**: 720p ~ 4K (Pro 이상)
- **영상 길이**: 최대 40초/클립
- **가격**: 구독 $12~$76/월 (크레딧 기반)
- **강점**: **Reference 기능으로 캐릭터 고정 가능**, Act-Two 모캡
- **약점**: 짧은 클립 길이 (40초)

#### Seedance 2.0 (ByteDance)
- **품질**: **2026년 2월 기준 가장 화제**. 할리우드급 리얼리즘으로 논란 중
- **해상도**: 720p ~ 2K
- **영상 길이**: 최대 15초/클립 (스티칭으로 연장 가능)
- **가격**: API $0.10~$0.80/분, 구독 ~$9.60/월
- **강점**: **90%+ 사용 가능 결과물 비율**, 멀티모달 입력, 매우 저렴
- **약점**: 저작권 논란 중, 15초 제한, 중국 플랫폼 종속

#### 기타 도구
- **Pika 2.5**: 빠른 렌더링, 74% 사용 가능 결과물 (1~10초, 드라마 제작용으론 부족)
- **Hailuo / MiniMax 2.3**: 캐릭터 미세 표정 우수, 아트스타일 다양 (수묵화, 게임CG)
- **Luma Dream Machine (Ray3)**: 시네마틱 모션, 네이티브 1080p

### 1-2. 도구별 1분 영상 비용 비교표

| 도구 | 1분 비용 (API) | 최대 클립 길이 | 캐릭터 일관성 | 드라마 적합도 |
|------|--------------|-------------|------------|-----------|
| **Kling 2.6** | **~$1.04** | 3분 | 중 | ★★★★ |
| **Seedance 2.0** | **~$0.10~$0.80** | 15초 | 중 | ★★★☆ |
| **Hailuo 2.3** | ~$1~2 | 불명 | 중 | ★★★☆ |
| **Runway Gen-4** | ~$3~5 (크레딧) | 40초 | **상 (Reference)** | ★★★★☆ |
| **Sora 2** | ~$6~30 | 35초 | 중상 | ★★★★ |
| **Veo 3.1** | ~$9~45 | 60초 | 중상 | ★★★★★ |
| **Luma Ray3** | ~$3~5 | 불명 | 중 | ★★★ |
| **Pika 2.5** | ~$2~4 | 10초 | 중하 | ★★ |

---

## 2. 캐릭터 일관성 문제 — 가장 큰 기술적 장벽

### 2-1. 현재 상태: "해결 중이지만 완벽하지 않다"

캐릭터 일관성은 AI 드라마 제작의 **가장 큰 기술적 장벽**이다.

### 2-2. 도구별 솔루션

| 도구 | 접근법 | 수준 |
|------|--------|------|
| **Runway Gen-4 Reference** | 레퍼런스 이미지로 동일 캐릭터 다른 장면 배치, entity-level encoding | 가장 진보됨 |
| **SenseTime Seko 2.0** | 캐릭터 디지털 ID 바인딩 + 다각도 프리셋 + 스타일 락킹 | **100에피소드 일관성 유지** (모션코믹) |
| **LTX Studio** | Persistent Character Profiles (나이, 인종, 헤어, 의상 한번 정의) | 워크플로우 레벨 |
| **ByteDance StoryMem** | 키프레임 메모리 뱅크, 새 장면 생성 시 참조 | 연구 단계 |

### 2-3. 현실적 평가

- **단일 장면 내**: 거의 완벽하게 유지 가능
- **같은 에피소드 내 여러 장면**: Runway Reference나 LTX Studio로 80~90% 일관성 가능
- **수십 에피소드에 걸쳐**: Seko 2.0 (모션 코믹 스타일) 외에는 아직 어려움
- **다중 캐릭터 상호작용**: 현재 기술의 가장 큰 약점. 2인 이상 장면은 품질 급격히 저하

### 2-4. 실전 Best Practice

1. **캐릭터 디자인 먼저** — 비디오 모델에 캐릭터 발명과 애니메이션을 동시에 시키지 말 것
2. **레퍼런스 이미지 갤러리** — 캐릭터별 "정규" 프레임 세트 유지
3. **프레임 체이닝** — 이전 클립의 마지막 프레임을 다음 클립의 레퍼런스로 사용
4. **상세 프롬프트** — "캐주얼 복장" 대신 "파란 데님 재킷, 롤업 소매, 흰 티셔츠, 검정 진"
5. **Reference Strength 조절** — 첫 패스에서 높게(ID 고정), 이후 약간 낮춰(모션 자연스럽게)

---

## 3. AI 음성/더빙/음악

### 3-1. AI 음성 — ElevenLabs (업계 리더)

- **한국어 지원**: 32개 언어 중 하나로 포함 (eleven_v3 모델)
- **품질**: 감정 표현과 리얼리즘에서 업계 최고. Audio Tags로 감정/톤 정밀 제어 가능
- **가격**: $0.12~$0.30/분 (플랜별)
- **1시간 분량 비용**: $7.20~$18.00
- **Voice Cloning**: 특정 성우 목소리 복제 가능

| 서비스 | 1분 비용 | 한국어 품질 | 감정 표현 |
|--------|---------|-----------|---------|
| ElevenLabs (Pro) | $0.24 | 상 | ★★★★★ |
| ElevenLabs (Flash) | $0.12 | 중상 | ★★★★ |
| PlayHT | ~$0.20 | 중상 | ★★★★ |
| LOVO AI | ~$0.15 | 중 | ★★★ |
| Veo 3 네이티브 | 영상 비용에 포함 | 불명 | ★★★★ |

### 3-2. AI 배경음악 — Suno / Udio

- **Suno Pro**: $10/월 (2,500크레딧 ~500곡), 상업적 사용 가능
- **Udio Pro**: $30/월 (4,800곡), 오디오 충실도 우위
- **곡당 비용**: ~$0.008~0.01 (기존 커스텀 $500~2,000 대비 **99.9%+ 절감**)
- 80에피소드 시리즈 전체: $30/월 하나면 충분 → **사실상 무시할 수 있는 비용**

### 3-3. AI 효과음 — ElevenLabs SFX V2

- 최대 30초 클립, 48kHz 프로페셔널 오디오
- 텍스트로 설명하면 어떤 효과음이든 생성
- 모든 생성 효과음 로열티 프리

---

## 4. 풀 제작 파이프라인: Script → Video

### 4-1. 표준 파이프라인

```
[1단계: 각본/스토리보드] — 5분
  LLM (Claude/GPT) → 각본 생성
  LTX Studio / AI 이미지 생성 → 비주얼 스토리보드
  ↓

[2단계: 캐릭터 디자인 & 레퍼런스] — 설정 1회
  Midjourney / Flux / Runway Gen-4 → 캐릭터 스틸 이미지
  Genra Reference Seeds / LTX 캐릭터 프로필 → 전 에피소드 유지
  ↓

[3단계: 영상 생성] — 15-30분
  이미지→영상: Kling / Runway Gen-4 / Veo 3 / Seedance 2.0
  텍스트→영상: Sora 2 / Veo 3 / Seedance 2.0
  ↓

[4단계: 음성 생성 & 립싱크] — 10분
  대사: ElevenLabs (29개 언어, 립싱크 포함)
  립싱크: Seedance 2.0 네이티브 / 별도 도구
  ↓

[5단계: 배경음악 & 효과음 생성] — 5분
  배경음악: Suno / Udio
  효과음: ElevenLabs SFX V2
  ↓

[6단계: 편집 & 합성] — 15-30분
  CapCut / Adobe Premiere / DaVinci
  스마트 비디오 스티칭, 리듬 컨트롤
  ↓

[7단계: 검수 & 내보내기]
```

**1에피소드 (1-2분) 제작 시간: 약 1~2시간** (올인원 도구 사용 시 30분)

### 4-2. 올인원 플랫폼

| 플랫폼 | 특징 | 핵심 장점 |
|--------|------|-----------|
| **SenseTime Seko 2.0** | 100에피소드 시리즈 한 문장으로 생성 시작 | 캐릭터/장면/소품 일관성 100에피소드 유지 |
| **MicroDrama AI** | 아이디어 입력 → 30분 만에 완성 | 90% 비용 절감, 20배 출력량 증가 |
| **LTX Studio** | 스크립트→스토리보드→캐릭터→영상 엔드투엔드 | Persistent Character Profiles, 4K 렌더링 |
| **Genra AI** | 쓰기, 연출, 보이스, 편집 올인원 | Character Reference로 수백 개 샷 얼굴 ID 유지 |

---

## 5. 실제 성공 사례

### 5-1. 중국 — 대규모 AI 단편 드라마 생산

#### "구미호요정이 나에게 빠지다" — 1.7억 뷰
- **도구**: ChatGPT(각본) → Midjourney(스틸) → Kling(영상) → Suno(BGM) → 사람 편집/성우
- **품질**: 의도적 '병맛' 수준이지만 중독성 있는 서사 구조
- **시사점**: **품질이 낮아도 서사 중독성이면 1억 뷰 돌파 가능**

#### "삼성퇴: 미래의 계시" — 1.4억 뷰
- 13에피소드, 각 3분, Bona Film Group + Douyin 공동제작
- Douyin AIGC 플랫폼 'Jimeng AI' 활용

#### 중국 AI 드라마 산업 전체
- AI 팀들이 **월 20-30편** 생산 (전통 대비 10배)
- 1인 제작 가능
- 비용: 10만~15만 위안 ($14,000~$21,000) — 전통 대비 **75% 절감**
- 35만 개 회사가 경쟁 중

### 5-2. 한국 — Vigloo AI 드라마

#### "지옥에서 구원자를 만나다" (Met a Savior in Hell)
- **4인 팀**으로 풀시리즈 제작, 6주 파이프라인
- 비용 90% 절감, 제작 시간 50% 단축
- 도구: Google Imagen, ByteDance SeedDream
- 투자: **Krafton $8,600만 투자**

#### "서울: 2053"
- SF 드라마, 실사로는 거의 불가능한 장면을 AI로 구현
- 월 1편 AI 드라마 시리즈 출시 목표, 300+ 프리미엄 드라마 보유

### 5-3. 글로벌 주목 사례

| 작품 | 성과 | 비용/시간 |
|------|------|-----------|
| Dor Brothers "Apex" | 300+ AI 영상 프로젝트, 수억 뷰 | 24시간 만에 완성 |
| Kalshi NBA 광고 | 전국 방송 방영, 2,000만+ 노출 | **$2,000, 48시간** |
| Tribeca "Sora Shorts" | 메이저 영화제 공식 상영 최초 | OpenAI Sora 활용 |

### 5-4. 웹툰→AI 애니메이션 (한국 시장 핵심)

| 플랫폼 | 성과 | 핵심 데이터 |
|--------|------|------------|
| **Kakao Helix Shorts** | 웹툰 → 40초 숏폼 자동 변환 | 제작시간 3주→3시간, 비용 $1,800→**$55** |
| **Naver Cuts** | UGC 기반 2분 이하 숏폼 애니 | 월 $70,000 크리에이터 리워드 |
| **Topco Media** | "사내연애는 절대 금지" AI 애니메이션 | 원작 대비 **5배 수익**, 3~5배 빠른 제작 |

---

## 6. 비용 분석 — 80에피소드 시리즈 (에피소드당 1-2분)

### 전제: 총 120분 (80에피소드 x 1.5분), 대화/BGM/효과음 포함, 여러 캐릭터 등장

### A. 풀 AI 생성 ($600~$2,500)

| 항목 | 도구 | 비용 |
|------|------|------|
| 스크립트 | Claude/GPT API | ~$50 |
| 비디오 생성 (120분) | Kling Pro | ~$125 |
| 비디오 생성 대안 | Seedance API | ~$12~$96 |
| 비디오 생성 대안 | Seko 2.0 (만화스타일) | ~$50~$200 |
| 음성 (대화 60분) | ElevenLabs Scale | ~$10.80 |
| 배경음악 | Suno Pro | $20 (2개월) |
| 편집/후반 | CapCut Pro | $20 (2개월) |
| **낙관적 총 비용** | | **$200~$500** |
| **현실적 총 비용** (재생성/수정 x3~5) | | **$600~$2,500** |
| **에피소드당** | | **$7.50~$31.25** |

### B. 하이브리드 — AI 영상 + 인간 성우 ($6,000~$14,000)

| 항목 | 비용 |
|------|------|
| 스크립트 (AI + 작가 감수) | ~$550 |
| 비디오 생성 (재생성 포함) | ~$500~$2,000 |
| 한국어 성우 (메인 3명) | ~$3,000~$6,000 |
| 보조 성우 | ~$1,000~$2,000 |
| 배경음악 | ~$50~$200 |
| 편집/후반 (전문 편집자) | ~$1,000~$3,000 |
| **총 비용** | **$6,000~$14,000** |
| **에피소드당** | **$75~$175** |

### C. 전통 제작 — ReelShort 스타일 ($150,000~$300,000)

| 항목 | 비용 |
|------|------|
| 각본 | $5,000~$15,000 |
| 배우 출연료 | $20,000~$50,000 |
| 촬영 (7-10일) | $30,000~$60,000 |
| 장소/스태프/후반/기타 | $70,000~$160,000 |
| **총 비용** | **$150,000~$300,000** |
| **에피소드당** | **$1,875~$3,750** |

### 비용 비교 요약

| 방식 | 총 비용 | 에피소드당 | 전통 대비 절감 | 품질 | 제작 기간 |
|------|---------|----------|-------------|------|---------|
| **풀 AI** | $600~$2,500 | $7~$31 | **99%** | 6/10 | 1-2개월 |
| **하이브리드** | $6,000~$14,000 | $75~$175 | **95%** | 7.5/10 | 2-3개월 |
| **전통** | $150,000~$300,000 | $1,875~$3,750 | 기준 | 9/10 | 3-6개월 |

---

## 7. 접근법별 비교 — 무엇을 선택할 것인가

### A. 풀 AI 실사 스타일
- **품질**: 6/10 (uncanny valley 존재, 다중 캐릭터 약함)
- **적합**: 초기 실험, '병맛/캠프' 장르

### B. AI 모션 코믹 / 만화 스타일 ★추천★
- **품질**: 8/10 (uncanny valley 회피, 스타일 일관성 높음)
- **적합**: 한국 시장, 웹툰 IP 활용, TikTok/Shorts
- **추천 이유**:
  1. **캐릭터 일관성 문제 회피** — 만화/웹툰 스타일은 일관성 유지가 훨씬 쉬움
  2. **Uncanny Valley 회피** — 실사 AI는 아직 "이상한 골짜기"를 완전히 넘지 못함
  3. **Seko 2.0 등 100에피소드 전용 도구 존재**
  4. **비용 압도적 절감** — 전통 대비 95%+ 절감
  5. **TikTok/Shorts에서 이미 트렌드** — 1.55억+ 뷰의 검증된 포맷
  6. **한국 웹툰 문화와 자연스러운 연결**

### C. AI 배경 + 실제 배우
- **품질**: 8.5/10
- **적합**: 프리미엄 콘텐츠, Virtual Production 스타일

### D. AI 영상 + 인간 성우
- **품질**: 7.5/10
- **적합**: 음성 품질이 중요한 장르 (로맨스, 감성)

---

## 8. 품질 현황 — 솔직한 평가

### 8-1. 현재 수준

- **최상의 경우**: Veo 3로 생성된 영상은 "공식적으로 언캐니 밸리를 벗어났다"는 평가
- **평균적 경우**: 아직 인간 눈으로 구분 가능한 아티팩트 존재
- **TikTok에서**: AI 영상이 2억+ 뷰 달성 — 많은 시청자가 실제로 속음

### 8-2. 주요 아티팩트/문제점

1. **손/손가락**: 여전히 가장 큰 약점 (손가락 개수 오류, 부자연스러운 움직임)
2. **입술 싱크**: 많이 개선됐지만 미세한 불일치 존재
3. **눈 움직임**: 부자연스러운 시선, '죽은 눈' 문제
4. **물리 법칙**: 복잡한 상호작용은 아직 부자연스러움
5. **캐릭터 일관성**: 장면마다 미묘하게 변함
6. **다중 캐릭터**: 2인 이상 장면에서 품질 급격히 저하

### 8-3. 시청자 수용도

**긍정적**: AI 드라마 1.7억 뷰 달성, "캠프한 혼돈"이 바이럴 요소로 작용

**부정적/리스크**:
- "AI Slop" 현상에 대한 피로감 증가
- YouTube 2025.07부터 AI 공개 의무화 (미공개 시 디모네타이즈)
- TikTok C2PA 기술로 자동 감지, AI 콘텐츠 라벨링 의무화
- Seedance 2.0 출시 직후 디즈니/파라마운트/SAG-AFTRA 조치
- 인간 크리에이터가 5.8배 더 나은 시청자 연결성

---

## 9. 전략적 시사점 & 결론

### 9-1. 지금 당장 가능한 것

1. **만화/모션코믹 스타일 AI 드라마**: Seko 2.0 + ElevenLabs로 100에피소드 시리즈 제작 가능
2. **5-15초 단위 AI 실사 클립**: 편집으로 연결하면 1-2분 에피소드 구성 가능
3. **AI 배경 + 실사 결합**: Virtual Production 스타일로 높은 품질 달성 가능
4. **AI 음성**: 한국어 포함, 감정 표현이 가능한 수준에 도달
5. **AI BGM**: 사실상 무료 수준으로 고품질 배경음악 생성 가능

### 9-2. 아직 어려운 것

1. **실사 퀄리티 AI 드라마**: 캐릭터 일관성, uncanny valley, 다중 캐릭터 상호작용 문제
2. **완벽한 립싱크 한국어 대화**: Veo 3가 가장 근접하나 정확한 대사 제어 어려움
3. **복잡한 액션/감정 장면**: 아직 인간 연기자 수준에 미달
4. **80에피소드에 걸친 실사 캐릭터 일관성**: 만화 스타일 외에는 매우 어려움

### 9-3. 추천 전략

1. **1단계 (즉시)**: 웹툰/만화 스타일로 시작 → 캐릭터 일관성 문제 회피, 비용 최소화
2. **2단계 (3-6개월 후)**: 실사 AI 기술 성숙도에 따라 하이브리드 또는 풀 실사로 전환
3. **핵심**: 중국이 이 시장을 리드 중 → 기술은 중국 도구, 콘텐츠는 한국 IP/스토리텔링

### 9-4. 올인원 파이프라인이 핵심 경쟁력

- 현재 가장 큰 비효율: 여러 AI 도구 사이를 왔다갔다하는 파편화된 워크플로우
- SenseTime Seko, Genra, Seedance 2.0이 올인원 솔루션으로 진화 중
- **기회**: 한국/글로벌 시장용 올인원 AI 드라마 파이프라인 구축

### 9-5. 수익화 모델 확인됨

- ReelShort/DramaBox: 에피소드 유료 해금 모델로 수억 달러 매출
- Topco Media: 웹툰 IP → AI 애니메이션으로 5배 ROI
- 중국 AI 단편 드라마: 비용 대비 수익성 입증

---

## Sources

### AI Video Generation Tools
- [Kling vs Sora vs Veo vs Runway Comparison](https://invideo.io/blog/kling-vs-sora-vs-veo-vs-runway/)
- [15 AI Video Models Tested (Feb 2026)](https://www.teamday.ai/blog/best-ai-video-models-2026)
- [Sora 2 Complete Guide 2026](https://wavespeed.ai/blog/posts/openai-sora-2-complete-guide-2026/)
- [Kling AI Pricing 2026](https://aitoolanalysis.com/kling-ai-pricing/)
- [Runway AI Pricing](https://runwayml.com/pricing)
- [Google Veo Pricing](https://costgoat.com/pricing/google-veo)
- [Seedance 2.0 Pricing](https://seedancevideo.com/pricing/)

### Character Consistency
- [Runway Gen-4 References](https://www.imagine.art/blogs/runway-gen-4-references-overview)
- [Runway Gen-4 Solves Character Consistency](https://venturebeat.com/ai/runways-gen-4-ai-solves-the-character-consistency-challenge-making-ai-filmmaking-actually-useful)
- [ByteDance StoryMem](https://the-decoder.com/bytedances-storymem-gives-ai-video-models-a-memory-so-characters-stop-shapeshifting-between-scenes/)
- [LTX Studio Consistent Character](https://ltx.studio/blog/how-to-create-a-consistent-character)

### AI Voice/Audio/Music
- [ElevenLabs Pricing](https://elevenlabs.io/pricing)
- [ElevenLabs v3 Model](http://unifiedtts.com/en/news/2026-02-12-elevenlabs-v3-model)
- [Suno Pricing](https://suno.com/pricing)
- [Suno vs Udio Comparison](https://aiflowreview.com/udio-vs-suno-2025/)
- [ElevenLabs Sound Effects](https://elevenlabs.io/sound-effects)

### Real Examples & Platforms
- [구미호 AI 드라마 1.7억 뷰 — RADII](https://radii.co/article/chinese-ai-tv-drama-nine-tailed-fox-demon-falls-for-me)
- [중국 AI SF 시리즈 — Sixth Tone](https://www.sixthtone.com/news/1015691)
- [Vigloo AI 드라마 — BusinessWire](https://www.businesswire.com/news/home/20250929893745/en/Vigloo-Unveils-the-First-Full-Length-Vertical-Dramas-Fully-Visualized-by-AI-for-Global-Audiences)
- [Dor Brothers Apex — Wilnick Magazine](https://www.wilnickmagazine.com/the-dor-brothers-apex-how-ai-created-a-200m-film-in-24-hours/)
- [Kalshi NBA AI 광고 — NPR](https://www.npr.org/2025/06/23/nx-s1-5432712/ai-video-ad-kalshi-advertising-nba-finals)
- [Tribeca Sora Shorts](https://tribecafilm.com/press-center/festival/press-releases/tribeca-festival-and-open-ai-announce-sora-shorts)
- [SenseTime Seko 2.0](https://www.prnewswire.com/apac/news-releases/sensetime-launches-seko-2-0-drama-series-generation-platform-302647940.html)
- [Seedance 2.0 — CNN](https://edition.cnn.com/2026/02/20/china/china-ai-seedance-intl-hnk-dst)

### 웹툰/만화 AI 애니메이션
- [Topco Media AI 웹툰 애니 — ANN](https://www.animenewsnetwork.com/news/2025-12-06/topco-media-plans-to-release-ai-animated-adaptations-of-its-adult-webtoons-globally-after-1st-work-/.231648)
- [Kakao Helix Shorts — ANN](https://www.animenewsnetwork.com/news/2025-10-04/kakao-entertainment-rolls-out-free-ai-short-form-video-tool-for-webtoon-creators/.229575)
- [Naver Cuts — Korea Herald](https://www.koreaherald.com/article/10548212)
- [한국 AI 웹툰 산업 — MIT Tech Review](https://www.technologyreview.com/2025/04/22/1114874/generative-ai-south-korea-webcomics/)

### Market & Cost Data
- [Microdrama Financing Guide 2026](https://vitrina.ai/blog/short-form-storytelling-financing)
- [ReelShort Production Costs](https://theredchains.com/the-rise-of-micro-drama-production-in-the-usa-a-look-at-reelshort-and-beyond/)
- [마이크로드라마 수십억달러 시장 — TechCrunch](https://techcrunch.com/2026/01/23/tiktok-like-microdramas-are-going-to-make-billions-this-year-even-though-they-kind-of-suck/)
- [AI Slop 현상 — NPR](https://www.npr.org/2025/12/24/nx-s1-5629169/2025-has-seen-an-explosion-of-ai-generated-slop)
- [AI 비디오 아티팩트 탐지법](https://focalml.com/blog/how-to-tell-if-a-video-is-ai-generated/)
- [AI 비디오 모델 전체 분석 2026.02 — Medium](https://medium.com/@cliprise/the-state-of-ai-video-generation-in-february-2026-every-major-model-analyzed-6dbfedbe3a5c)
