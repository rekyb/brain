---
title: React Native Best Practices
aliases: [RN best practices]
type: knowledge
project: global
status: evergreen
created: 2026-07-07
updated: 2026-07-07
tags: [react-native, mobile, best-practices]
source: community-consensus
---

# React Native Best Practices

> [!abstract] TL;DR
> A curated set of production-proven best practices for building scalable React Native apps, covering project structure, performance, state management, navigation, styling, security, and testing.

## Project Structure

- **Feature-based folder structure** — group by feature (`/auth`, `/profile`, `/settings`) rather than by type (`/components`, `/screens`). Scales better.
- **Path aliases** — use `tsconfig.json` paths (e.g., `@/components`) to avoid `../../../` imports.
- **Monorepo for shared code** — if you have web + mobile, consider Turborepo or Nx.

## Performance

| Area | Best Practice |
|---|---|
| **Lists** | Use `FlashList` (Shopify) over `FlatList` — significantly faster |
| **Re-renders** | Memoize with `React.memo`, `useMemo`, `useCallback` — but only where profiling shows a bottleneck |
| **Images** | Use `expo-image` or `react-native-fast-image` with caching |
| **Animations** | Use `react-native-reanimated` (worklet-based, UI thread) instead of built-in `Animated` |
| **Hermes** | Enable Hermes engine (default in RN 0.70+) — faster startup, lower memory |
| **Bundle size** | Use `react-native-bundle-visualizer`; lazy-load heavy screens |

## Navigation

- **React Navigation** is the de facto standard. Use native stack for native performance.
- **Type-safe navigation** — define a `RootStackParamList` type and use it everywhere.
- **Deep linking** — configure from day one; retrofitting is painful.
- **Expo Router** — file-based routing, the direction the ecosystem is heading.

## State Management — The Modern Split

| State Type | Tool | Example |
|---|---|---|
| **Server state** | React Query (TanStack Query) | User profile, feed posts, notifications |
| **Client global state** | Zustand | Auth token, theme preference, onboarding flag |
| **Local UI state** | `useState` / `useReducer` | Form inputs, modal visibility |

**Never duplicate server data into Zustand/Redux.** React Query's cache is your global server state.

## Styling

- `StyleSheet.create` remains performant — styles are sent to native once.
- **NativeWind** (Tailwind for RN) or **Tamagui** for design-system-level consistency.
- Avoid inline styles in hot paths — they create new objects every render.
- **Dark mode** — use `useColorScheme()` and a theme provider from the start.

## Security

- Never store secrets in JS — use `react-native-keychain` or `expo-secure-store`.
- SSL pinning for sensitive API calls.
- Enable ProGuard (Android) and Bitcode (iOS).

## Testing

| Layer | Tool |
|---|---|
| Unit / Component | Jest + React Native Testing Library |
| Integration | MSW (Mock Service Worker) |
| E2E | Maestro (easiest) or Detox (most mature) |

## Build & CI/CD

- **EAS Build** (Expo) or **Fastlane** for automated builds.
- **OTA updates** — EAS Update or CodePush for instant JS-only patches.
- Environment configs via `react-native-config` or Expo's `.env` support.

## TypeScript — Always

TypeScript is the default for all new React Native projects. Both RN CLI and Expo scaffold TypeScript by default. ~95% of new RN projects use TypeScript.

## Related

- [[rn-feature-sliced-architecture]]
- [[rn-scalable-paradigm]]
- [[rn-obytes-architecture-analysis]]

## Sources

- React Native community consensus (2024-2025)
- [Expo documentation](https://docs.expo.dev)
- [React Navigation docs](https://reactnavigation.org)
