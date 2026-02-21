# iOS Custom Keyboard Extension: 기술적 실현 가능성 분석

> 작성일: 2026-02-16
> 목적: AI 키보드 iOS 제품 실현 가능성 평가를 위한 기술 리서치

---

## 1. Apple 공식 제한사항 (iOS Keyboard Extension Sandbox)

### 1.1 기본 샌드박스 (Full Access 비활성화 상태)

커스텀 키보드는 기본적으로 **매우 제한적인 샌드박스**에서 실행된다. Apple의 공식 문서에 따르면:

| 항목 | 접근 가능 여부 | 비고 |
|------|-------------|------|
| **네트워크** | **차단** | 모든 네트워크 접근 불가 |
| **클립보드 (UIPasteboard)** | **차단** | General pasteboard 및 named pasteboard 모두 차단 |
| **연락처 (Address Book)** | **차단** | |
| **위치 서비스** | **차단** | |
| **카메라** | **차단** | iMessage Extension만 카메라 접근 가능 |
| **마이크** | **차단** | 받아쓰기(dictation) 불가 |
| **갤러리 (Camera Roll)** | **차단** | |
| **iCloud** | **차단** | |
| **컨테이닝 앱과 데이터 공유** | **차단** | |
| **UITextDocumentProxy** | **허용** | 커서 전후 텍스트 읽기, 텍스트 삽입/삭제 |
| **키보드 UI 렌더링** | **허용** | UIInputViewController의 primary view 내에서만 |
| **AudioToolbox (키 소리)** | **허용** | 시스템 사운드 재생 가능 |
| **Haptic Feedback** | **허용** | UIFeedbackGenerator 사용 가능 |

### 1.2 Full Access (Open Access) 활성화 상태

`Info.plist`에서 `RequestsOpenAccess = true` 설정 시, 사용자가 설정 > 키보드에서 "풀 액세스 허용"을 활성화하면:

| 항목 | 접근 가능 여부 | 비고 |
|------|-------------|------|
| **네트워크** | **허용** | 서버 측 처리 가능 (API 호출 등) |
| **클립보드 (UIPasteboard)** | **허용** | 읽기/쓰기 모두 가능 |
| **연락처 (Address Book)** | **허용** | 별도 사용자 권한 필요 (최초 접근 시) |
| **위치 서비스** | **허용** | 별도 사용자 권한 필요 (최초 접근 시) |
| **갤러리 (Camera Roll)** | **허용** | 별도 사용자 권한 필요 |
| **iCloud** | **허용** | 완전한 iCloud 기능 |
| **공유 컨테이너 (App Groups)** | **허용** | 컨테이닝 앱과 UserDefaults, 파일 공유 |
| **Game Center / IAP** | **허용** | 컨테이닝 앱 통해 |
| **카메라** | **여전히 차단** | 키보드 익스텐션에서는 불가 |
| **마이크** | **여전히 차단** | 키보드 익스텐션에서는 불가 |
| **캘린더** | **명시적 언급 없음** | EventKit 접근 불확실 |

### 1.3 절대 불가능한 것들 (Full Access와 무관)

Apple이 키보드 익스텐션에서 **명시적으로 금지**하는 것들:

- **`UIApplication` 접근 불가**: `openURL()` 호출 불가 → URL 열기, 다른 앱 실행 불가
  - 우회법: responder chain 탐색으로 UIApplication 참조 획득 가능하나, **App Store 리뷰에서 거절됨**
- **`UIAlertView` / `UIAlertController` 사용 불가**: 시스템 알림 표시 불가
- **텍스트 선택 불가**: 텍스트 선택은 호스트 앱이 제어. 키보드는 선택 영역에 접근 불가
- **편집 메뉴 접근 불가**: Cut/Copy/Paste 메뉴 접근 불가
- **Core Bluetooth 사용 불가**
- **HealthKit 접근 불가**
- **HomeKit 접근 불가**
- **Touch ID / Face ID 접근 불가**: 생체인증 API 사용 불가
- **새 윈도우/다이얼로그 표시 불가**: UIInputViewController의 primary view 내에서만 UI 가능
- **캘린더 이벤트 직접 생성 불가**: 키보드에서 직접적으로 외부 액션 트리거 불가

### 1.4 메모리 제한

| 기기 | 메모리 제한 | 출처 |
|-----|----------|------|
| 일반적인 범위 | **30~66MB** | 기기별 상이 |
| iPhone XS Max | ~66MB | Apple Developer Forums |
| 구형 기기 | ~30MB | React Native issue 보고 |
| 실무적 안전 범위 | **~40-50MB** | 개발자 경험 기반 권장 |

- 메모리 초과 시 Jetsam 메커니즘에 의해 **즉시 프로세스 종료** (crash)
- OOM crash는 일반적인 crash 모니터링으로 잡히지 않음
- **AI 모델 로딩에 매우 제한적**: Core ML 모델도 이 제한 내에서 동작해야 함

### 1.5 백그라운드 프로세스

- 키보드 익스텐션은 **호스트 앱 요청 완료 후 곧 종료**됨
- 백그라운드 태스크 요청은 가능하나, **실제 실행은 컨테이닝 앱에서** 이루어짐
- `NSURLSession` 백그라운드 전송은 별도 프로세스에서 실행되므로 익스텐션 종료 후에도 지속 가능
- **결론: 키보드 익스텐션 자체에서 백그라운드 처리 불가. 컨테이닝 앱 + App Groups로 우회 필요**

### 1.6 UITextDocumentProxy (텍스트 상호작용 API)

키보드가 텍스트 필드와 상호작용하는 유일한 공식 채널:

```swift
// 텍스트 삽입
textDocumentProxy.insertText("hello")

// 백스페이스
textDocumentProxy.deleteBackward()

// 개행
textDocumentProxy.insertText("\n")

// 커서 앞 텍스트 읽기
let before = textDocumentProxy.documentContextBeforeInput

// 커서 뒤 텍스트 읽기
let after = textDocumentProxy.documentContextAfterInput

// 현재 입력 모드 확인
let returnKeyType = textDocumentProxy.returnKeyType
```

**제한점:**
- `documentContextBeforeInput`은 커서 앞 전체 텍스트를 반환하나, 일부 앱에서는 잘릴 수 있음
- 텍스트 선택/하이라이트 불가
- 텍스트 필드의 전체 내용을 보장하지 않음
- 앱마다 반환하는 컨텍스트 양이 다름

### 1.7 키보드가 사용되지 않는 곳

- **패스코드 입력 화면**: 시스템 키보드만 사용
- **Secure Text Fields** (`isSecureTextEntry = true`): 비밀번호 필드 등
- **앱 개발자가 명시적으로 차단한 경우**: 앱에서 서드파티 키보드를 비활성화 가능

---

## 2. 최근 변화 (2024-2026)

### 2.1 iOS 26 (WWDC 2025, 2025년 9월 출시)

**주요 변화:**
- Apple이 OS 버전 번호를 통합 → iOS 18 다음이 iOS 26
- **Liquid Glass** 디자인 언어 도입 (iOS 7 이후 최대 디자인 변경)
- 아랍어 양방향 키보드 등 지역 키보드 개선

**Foundation Models 프레임워크 (가장 중요):**
- Apple이 **온디바이스 3B 파라미터 LLM**을 서드파티 앱에 공개
- `FoundationModels` 프레임워크를 통해 Swift에서 3줄의 코드로 통합 가능
- 기능: 자연어 이해/생성, 추론, 앱 커스텀 도구 호출
- **프라이버시**: 모든 데이터가 디바이스에 머물고, 오프라인 동작, 무료
- **키보드 익스텐션에서의 호환성**: 명시적으로 문서화되지 않음
  - 앱 익스텐션에서의 지원 여부가 불명확
  - 메모리 제한(30-66MB)으로 인해 3B 모델을 키보드 익스텐션에서 직접 실행하기는 어려울 가능성 높음
  - **컨테이닝 앱에서 모델 실행 → App Groups로 결과 전달이 현실적 방안**

### 2.2 Apple Intelligence 통합

- iOS 18부터 Apple Intelligence 도입 (Writing Tools, 요약, 재작성 등)
- 이것은 **시스템 레벨** 기능으로, 서드파티 키보드와 독립적
- 사용자는 텍스트 선택 후 시스템의 Writing Tools를 사용 가능 → 서드파티 AI 키보드와 직접 경쟁

### 2.3 App Intents / Shortcuts 통합

- `App Intents` 프레임워크 (iOS 16+)로 Shortcuts 앱, Siri와 통합 가능
- **키보드 익스텐션이 직접 App Intent를 트리거하는 공식 API는 없음**
- 하지만 컨테이닝 앱에서 App Intents를 정의하면, 사용자가 Shortcuts에서 조합 가능
- 예: "클립보드 텍스트를 AI로 요약" 같은 Shortcut을 만들어 Siri나 위젯으로 트리거

### 2.4 제한 완화 여부

- **키보드 익스텐션 자체에 대한 주요 제한 완화는 없음** (2024-2026)
- Apple은 자체 Writing Tools로 AI 텍스트 기능을 제공하는 방향
- 서드파티 키보드 생태계에 대한 투자보다 시스템 레벨 AI에 집중하는 추세

---

## 3. 기존 iOS AI 키보드 분석

### 3.1 Microsoft SwiftKey

- **기능**: AI 기반 예측 타이핑, Microsoft Copilot 통합, 톤 조정, 스마트 리플라이, DALL-E 3 이미지 생성
- **언어**: 120+ 언어 지원, 신경 번역 기술
- **네트워크**: Full Access 필수 (Copilot, 번역 등)
- **가격**: 무료
- **접근법**: 클라우드 AI (Microsoft 서버) → Full Access 필요

### 3.2 Grammarly Keyboard

- **기능**: 실시간 문법/맞춤법 교정, 문장 구조 개선, "Context IQ" (이전 대화 분석)
- **프리미엄**: 톤 조정, 어휘 향상 ($12.99/월, 2026 기준)
- **네트워크**: Full Access 필수 (서버 측 분석)
- **접근법**: 클라우드 기반 NLP → Full Access 필요

### 3.3 Fleksy

- **기능**: 제스처 기반 빠른 타이핑, SDK 제공 (B2B)
- **차별점**: iPhone 최빠 타이핑 기네스 기록 보유
- **접근법**: 온디바이스 처리 중심, SDK로 다른 앱에 키보드 기능 제공
- **네트워크**: 기본 기능은 Full Access 없이 동작

### 3.4 Allora iOS (오픈소스)

- **기능**: LLM과 직접 상호작용하는 키보드 익스텐션
- **동작 방식**: REST API로 로컬 호스팅 LLM에 요청 → 응답을 텍스트 필드에 스트리밍 삽입
- **클립보드 활용**: 클립보드 내용을 LLM 프롬프트에 컨텍스트로 전달
- **특징**: 자체 서버 (oobabooga text-generation-webui) 사용 → 프라이버시 유지
- **한계**: Full Access 필수, 로컬 서버 설정 필요 (일반 사용자 접근성 낮음)

### 3.5 3Sparks Keys

- **기능**: AI 기반 텍스트 변환, 커스터마이징 가능한 프롬프트
- **동작 방식**: LLM 서버 (OpenAI, Mistral, OpenRouter, LM Studio, Ollama 등) 연결
- **특징**: 프롬프트 공유/발견/설치, iMessage/WhatsApp/이메일 등 모든 앱에서 사용
- **요구사항**: iOS 18.0+, Full Access 필요
- **App Store 등록 완료**: Apple 심사 통과

### 3.6 핵심 패턴 요약

| 키보드 | AI 방식 | 네트워크 필요 | 외부 서비스 통합 |
|--------|---------|-------------|---------------|
| SwiftKey | 클라우드 (Copilot) | Yes | 이미지 생성, 번역 |
| Grammarly | 클라우드 NLP | Yes | 문법 분석 |
| Fleksy | 온디바이스 | No (기본) | SDK B2B |
| Allora | 로컬/리모트 LLM | Yes | 자체 서버 |
| 3Sparks | 리모트 LLM API | Yes | 다중 LLM 제공자 |

**공통점**: 텍스트 예측/교정을 넘어 AI 기능을 제공하는 키보드는 **모두 Full Access + 네트워크 필수**

---

## 4. 알려진 우회 방법 (Workarounds)

### 4.1 컨테이닝 앱 + 키보드 익스텐션 패턴 (가장 중요)

```
[컨테이닝 앱]  ←→  [App Groups 공유 컨테이너]  ←→  [키보드 익스텐션]
     |                                                    |
     ├── AI 모델 실행                                      ├── 텍스트 입력/출력
     ├── 백그라운드 처리                                    ├── 컨텍스트 읽기
     ├── 캘린더/연락처 접근                                 ├── UI 표시
     ├── URL 열기                                          └── 클립보드 접근 (Full Access)
     └── Shortcuts/Siri 통합
```

**동작 흐름:**
1. 키보드에서 사용자 입력 캡처
2. App Groups `UserDefaults` 또는 공유 파일에 요청 기록
3. 네트워크 호출 (Full Access 필요)로 AI 서버에 요청 또는 컨테이닝 앱에 위임
4. 결과를 `textDocumentProxy.insertText()`로 삽입

### 4.2 Share Extension + Keyboard 조합

- Share Extension은 키보드보다 더 많은 API 접근 가능
- 사용자가 텍스트 선택 → 공유 → AI 처리 → 결과 반환
- **한계**: 사용자 동선이 복잡 (키보드처럼 즉각적이지 않음)

### 4.3 Shortcuts / Siri 통합

- 컨테이닝 앱에서 `App Intents` 정의
- 사용자가 Shortcuts에서 "선택 텍스트 AI 요약" 같은 자동화 생성
- Siri로 트리거 가능
- **한계**: 키보드에서 직접 Shortcut을 트리거하는 API 없음

### 4.4 Widget 통합

- 컨테이닝 앱의 Widget에서 빠른 AI 기능 접근
- 잠금 화면, 홈 화면에서 원탭으로 텍스트 처리
- **한계**: Widget에서 키보드 텍스트 필드에 직접 접근 불가

### 4.5 NSURLSession 백그라운드 전송

- 익스텐션에서 `NSURLSession` 백그라운드 전송 시작
- 별도 프로세스에서 실행되므로 익스텐션 종료 후에도 지속
- AI API 호출 시 유용 (응답이 오래 걸리는 경우)

### 4.6 Settings Bundle 우회

- Settings Bundle에 아무 콘텐츠나 등록하면 iOS 설정 앱에 표시됨
- Full Access 활성화/비활성화 안내에 활용

### 4.7 Full Access 감지 우회

- 공식 API로 Full Access 상태 확인 불가
- **우회법**: `UIPasteboard`에 쓰기 시도 → 성공하면 Full Access 활성화 상태

### 4.8 클립보드를 통한 데이터 전달

- Full Access 상태에서 `UIPasteboard`를 데이터 전달 채널로 활용
- 키보드 → 클립보드에 데이터 쓰기 → 컨테이닝 앱에서 읽기
- **주의**: 사용자 클립보드를 덮어쓰는 UX 문제

---

## 5. App Store 리뷰 가이드라인

### 5.1 키보드 익스텐션 관련 규칙 (Guideline 4.4.1)

Apple의 App Store Review Guidelines에서 키보드 익스텐션에 적용되는 규칙:

**필수 요구사항:**
1. **키보드 입력 기능 제공** 필수 (문자 입력)
2. **다음 키보드로 전환하는 방법** 제공 필수 (지구본 버튼)
3. **숫자 및 소수점 키보드 타입** 제공 필수 (App Extension Programming Guide 참조)
4. 키보드가 앱의 주요 기능일 경우 **Utilities 카테고리** 사용

**데이터 수집 제한:**
- 사용자 활동 수집은 **키보드 익스텐션의 기능 향상 목적으로만** 허용
- 다른 목적으로의 데이터 수집/전송은 거절 사유

**금지사항:**
- **광고 금지**: 키보드 내 디스플레이 광고 불가 (메인 앱에서만)
- **키 버튼 재목적 금지**: "return" 키를 길게 눌러 카메라 실행 같은 행위 불가
- **마케팅/인앱 구매 금지**: 익스텐션 내에서 직접적인 마케팅이나 IAP 불가

**Full Access 관련:**
- **Full Access 없이도 기본 기능이 동작**해야 함 (네트워크 없이도 사용 가능해야)
- Full Access가 필요한 이유를 사용자에게 명확히 설명해야 함

### 5.2 AI 키보드의 App Store 심사 실태

**성공 사례:**
- SwiftKey (Microsoft Copilot 통합) → 승인
- Grammarly (클라우드 NLP) → 승인
- 3Sparks Keys (LLM API 연결) → 승인
- 다수의 AI 키보드가 App Store에 등록되어 있음

**거절 위험 요소:**
- 개인정보 보호 위반이 2025년 기준 거절 사유 1위
- 키보드 데이터를 광고 목적으로 사용
- Full Access 없이 동작하지 않는 경우
- 키스트로크 데이터를 부적절하게 수집/전송
- Privacy manifest 미비

**결론: AI 키보드 자체는 거절 사유가 아님. 프라이버시 정책과 기능 요구사항을 준수하면 심사 통과 가능**

---

## 6. Android (IME) 비교

### 6.1 아키텍처 차이

| 항목 | iOS Keyboard Extension | Android IME (InputMethodService) |
|------|----------------------|--------------------------------|
| 기본 철학 | 제한적 샌드박스 + 사용자 동의 | 개방적 시스템 통합 |
| 시스템 접근 | 매우 제한적 | 깊은 시스템 레벨 접근 가능 |
| 네트워크 | Full Access 필요 | 기본 허용 |
| 클립보드 | Full Access 필요 | 기본 허용 (API 11+) |
| 카메라/마이크 | 불가 | 가능 |
| 리치 콘텐츠 | 제한적 | Commit Content API (API 25+): GIF, PNG, WebP, 스티커 |
| 인라인 오토필 | 불가 | Android 11+: IME에서 인라인 자동 완성 제안 표시 |
| 플로팅 키보드 | 불가 | 가능 |
| URL 열기 | 불가 | 가능 |
| 다른 앱 실행 | 불가 | Intent 시스템으로 가능 |
| 알림 접근 | 불가 | Accessibility Service 조합 시 가능 |
| 화면 콘텐츠 읽기 | 불가 | Accessibility Service 조합 시 가능 |
| 텍스트 선택 | 불가 | InputConnection으로 가능 |
| 메모리 제한 | 30-66MB (기기별) | 앱 수준 메모리 (수백 MB) |
| 온디바이스 AI | 메모리 제한으로 어려움 | 대형 모델 실행 가능 |
| 배포/설치 | Settings > 키보드에서 수동 활성화 | 설치 후 한 번 활성화 |

### 6.2 Android에서 가능하고 iOS에서 불가능한 것들

1. **이미지/GIF/스티커 직접 삽입**: Android는 Commit Content API로 리치 콘텐츠를 텍스트 필드에 직접 삽입 가능
2. **인라인 오토필 제안**: Android 11+에서 IME가 비밀번호/주소 등의 자동 완성을 인라인으로 제안
3. **딕테이션 (음성 입력)**: Android IME는 마이크 접근 가능
4. **플로팅/리사이즈 키보드**: 자유로운 키보드 위치/크기 조정
5. **시스템 레벨 번역**: 키보드에서 직접 실시간 번역
6. **대규모 온디바이스 AI 모델**: 메모리 제한이 iOS보다 훨씬 여유로움
7. **다른 앱과의 직접 통합**: Intent 시스템으로 캘린더 추가, 지도 열기 등

### 6.3 역량 비교 점수 (AI 키보드 관점)

| 역량 | iOS | Android | 비고 |
|------|-----|---------|------|
| 텍스트 예측/교정 | 7/10 | 9/10 | Android가 더 많은 리소스 활용 가능 |
| 클라우드 AI 통합 | 7/10 | 9/10 | iOS는 Full Access 배리어 |
| 온디바이스 AI | 4/10 | 8/10 | 메모리 제한이 결정적 차이 |
| 리치 콘텐츠 | 3/10 | 9/10 | iOS는 텍스트만 |
| 외부 서비스 통합 | 2/10 | 8/10 | iOS는 URL 열기조차 불가 |
| 프라이버시/보안 | 9/10 | 6/10 | iOS의 샌드박스가 더 강력 |
| 사용자 신뢰 | 8/10 | 5/10 | Full Access 동의 과정이 신뢰 제공 |

---

## 7. 제품 실현 가능성 평가

### 7.1 iOS에서 가능한 AI 키보드 기능

**즉시 실현 가능 (키보드 익스텐션 내):**
- 클라우드 AI를 통한 텍스트 재작성/톤 조정 (Full Access 필요)
- 클라우드 AI 문법/맞춤법 교정 (Full Access 필요)
- LLM API 호출로 텍스트 생성/요약 (Full Access 필요)
- 커서 앞 텍스트 분석을 통한 컨텍스트 인식 제안
- 클립보드 내용을 AI 프롬프트 컨텍스트로 활용 (Full Access 필요)
- 다국어 번역 (Full Access 필요)
- 커스텀 프롬프트 기반 텍스트 변환

**컨테이닝 앱과 조합하여 가능:**
- 사용자 패턴 학습 (앱에서 학습, 공유 컨테이너로 전달)
- 캘린더/연락처 기반 스마트 제안 (앱에서 데이터 캐시 → 키보드에서 활용)
- 대화 기록/템플릿 관리 (앱에서 관리, 키보드에서 삽입)
- Shortcuts 자동화와 연계

**Foundation Models (iOS 26+) 활용 가능성:**
- 온디바이스 텍스트 처리 (프라이버시 보장, 오프라인 동작)
- 하지만 키보드 익스텐션의 메모리 제한으로 직접 실행은 어려울 수 있음
- 컨테이닝 앱에서 모델 실행 → 결과를 공유 컨테이너로 전달이 현실적

### 7.2 iOS에서 불가능한 AI 키보드 기능

- 키보드에서 직접 URL 열기/앱 실행
- 키보드에서 직접 캘린더 이벤트 생성
- 음성 입력/딕테이션
- 카메라로 이미지 캡처하여 분석
- 화면 콘텐츠 읽기/분석
- 이미지/GIF를 텍스트 필드에 직접 삽입
- 플로팅/오버레이 UI
- Full Access 없이 네트워크 AI 기능

### 7.3 핵심 리스크

1. **Full Access 배리어**: 많은 사용자가 Full Access 활성화를 꺼림 (프라이버시 우려)
2. **메모리 제한**: 온디바이스 AI 모델 실행이 극도로 제한적 (30-66MB)
3. **Apple Writing Tools 경쟁**: iOS 18+ 시스템 레벨 AI 기능이 직접 경쟁자
4. **사용자 설치 복잡**: Settings > 키보드에서 수동 활성화 필요
5. **앱별 차단**: 금융앱, 보안 앱 등에서 서드파티 키보드 차단
6. **크래시 리스크**: 메모리 초과 시 무경고 강제 종료 → UX 악화

### 7.4 전략적 권장사항

1. **클라우드 AI + 온디바이스 하이브리드**: 기본 기능은 온디바이스, 고급 기능은 클라우드
2. **컨테이닝 앱 필수**: 키보드 단독이 아닌 풍부한 컨테이닝 앱 + 키보드 익스텐션 조합
3. **Full Access 온보딩**: 왜 Full Access가 필요한지 투명하게 설명하는 온보딩 플로우
4. **프라이버시 차별화**: "키스트로크는 저장하지 않습니다" 같은 명확한 프라이버시 약속
5. **iOS 26 Foundation Models 적극 활용**: 메모리 제한 내에서 가능한 부분 탐색
6. **Android 우선 전략 고려**: AI 키보드의 기술적 자유도가 압도적으로 높음

---

## 8. 참고 프레임워크/도구

### 8.1 KeyboardKit

- SwiftUI 기반 iOS 키보드 개발 프레임워크
- 75개 로케일 지원
- Pro 버전: 오토컴플리트, 이모지 키보드, AI 지원, 테마, 딕테이션 트리거
- 현재 버전: 9.8 (2025)
- GitHub: [KeyboardKit/KeyboardKit](https://github.com/KeyboardKit/KeyboardKit)

### 8.2 Fleksy SDK

- B2B 키보드 SDK
- 제스처 기반 입력
- iOS/Android 모두 지원

### 8.3 Apple Foundation Models Framework

- iOS 26+
- 온디바이스 3B 파라미터 LLM
- Swift 네이티브 통합
- 무료, 오프라인, 프라이버시 보장

### 8.4 Core ML

- Apple의 ML 추론 프레임워크
- CPU, GPU, Neural Engine 최적화
- 키보드 익스텐션의 메모리 제한 내에서 소형 모델 실행 가능

---

## 출처

- [Apple: Creating a Custom Keyboard](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html)
- [Apple: Configuring Open Access for a Custom Keyboard](https://developer.apple.com/documentation/uikit/configuring-open-access-for-a-custom-keyboard)
- [Apple: UITextDocumentProxy](https://developer.apple.com/documentation/uikit/uitextdocumentproxy)
- [Apple: UIInputViewController](https://developer.apple.com/documentation/uikit/uiinputviewcontroller)
- [Apple: App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)
- [Apple: Foundation Models Framework](https://developer.apple.com/documentation/FoundationModels)
- [Apple: Supporting Extensions in iOS](https://support.apple.com/guide/security/supporting-extensions-secabd3504cd/web)
- [Fleksy: Limitations of Custom Keyboards on iOS](https://www.fleksy.com/blog/limitations-of-custom-keyboards-on-ios/)
- [Fleksy: iOS vs Android Keyboard SDK Comparison](https://www.fleksy.com/blog/ios-vs-android-keyboard-sdk-integration-comparison/)
- [inFullMobile: Limitations of Custom iOS Keyboards (Medium)](https://medium.com/@inFullMobile/limitations-of-custom-ios-keyboards-3be88dfb694)
- [Shyngys Kassymov: iOS Custom Keyboard Guide](https://shyngys.com/ios-custom-keyboard-guide)
- [KeyboardKit GitHub](https://github.com/KeyboardKit/KeyboardKit)
- [Allora iOS GitHub](https://github.com/nicksavarese/allora-ios)
- [3Sparks Keys](https://www.3sparks.net/keys)
- [Elephas: Best AI Keyboards for iPhone 2026](https://elephas.app/blog/best-ai-keyboard-iphone)
- [CleverType: AI Keyboards on Android vs iPhone](https://www.clevertype.co/post/ai-keyboards-on-android-vs-iphone-which-does-it-better)
- [Android: Image Keyboard Support](https://developer.android.com/develop/ui/views/touch-and-input/image-keyboard)
- [Android: InputMethodService](https://developer.android.com/reference/android/inputmethodservice/InputMethodService)
- [Android: Integrate Autofill with IMEs](https://developer.android.com/identity/autofill/ime-autofill)
- [Microsoft: SwiftKey Full Access](https://support.microsoft.com/en-us/topic/why-does-my-microsoft-swiftkey-keyboard-need-full-access-in-ios-72d7fb5e-76fd-4554-bc93-52c238958799)
- [Apple: Identifying High-Memory Use with Jetsam](https://developer.apple.com/documentation/xcode/identifying-high-memory-use-with-jetsam-event-reports)
- [Computerworld: Android's Underappreciated Keyboard Advantage](https://www.computerworld.com/article/1646838/android-keyboard-advantage.html)
