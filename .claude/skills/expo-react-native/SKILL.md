---
name: expo-react-native
description: Expo + React Native 앱 개발 베스트 프랙티스. 설계, 빌드, 배포, 성능 최적화를 다룬다.
argument-hint: "[모바일 앱 과제]"
user-invocable: true
---

# Expo + React Native Best Practices

출처: Expo 공식 skills + callstackincubator/react-native-best-practices

## Project Setup (Expo SDK 52+)
```bash
npx create-expo-app@latest my-app --template tabs
```

### 프로젝트 구조
```
src/
  app/              # Expo Router (파일 기반 라우팅)
    (tabs)/          # 탭 네비게이션 그룹
    _layout.tsx      # Root layout
  components/        # 공유 컴포넌트
  features/          # 기능별 모듈
  hooks/             # Custom hooks
  lib/               # 유틸리티
  constants/         # 상수, 테마
  assets/            # 이미지, 폰트
```

## Expo Router (파일 기반 라우팅)
```
app/
  _layout.tsx          # Root layout (Stack)
  index.tsx            # / 홈
  (tabs)/
    _layout.tsx        # Tab layout
    home.tsx           # /home
    profile.tsx        # /profile
  [id].tsx             # 동적 라우트
  modal.tsx            # 모달
```

## Performance Optimization

### New Architecture (권장)
- Fabric Renderer: 동기 렌더링
- TurboModules: 네이티브 모듈 지연 로딩
- JSI: JavaScript ↔ Native 직접 통신

### 리스트 성능
```tsx
// FlashList (Shopify) > FlatList
import { FlashList } from "@shopify/flash-list";

<FlashList
  data={items}
  renderItem={({ item }) => <Item data={item} />}
  estimatedItemSize={80}  // 필수!
/>
```

### 이미지 최적화
```tsx
// expo-image (Expo 공식, 캐싱 내장)
import { Image } from 'expo-image';

<Image
  source={{ uri: url }}
  placeholder={blurhash}
  contentFit="cover"
  transition={200}
/>
```

### 애니메이션
```tsx
// React Native Reanimated (UI Thread)
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from 'react-native-reanimated';
```

## 네이티브 기능
- **카메라**: `expo-camera`
- **위치**: `expo-location`
- **알림**: `expo-notifications` + EAS Push
- **생체인증**: `expo-local-authentication`
- **저장소**: `expo-secure-store` (민감), `@react-native-async-storage` (일반)
- **인앱결제**: `expo-in-app-purchases` / RevenueCat

## 배포 (EAS Build + Submit)
```bash
# 빌드
eas build --platform all

# 제출 (App Store + Play Store)
eas submit --platform all

# OTA 업데이트 (코드 변경만, 네이티브 변경 제외)
eas update --branch production
```

## AI Integration in Mobile
- `expo-speech`: TTS (텍스트 → 음성)
- `@react-native-voice/voice`: STT (음성 → 텍스트)
- Streaming: Server-Sent Events로 AI 응답 실시간 표시
- Offline: ONNX Runtime 또는 TFLite로 온디바이스 추론

## Task: $ARGUMENTS
