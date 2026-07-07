---
title: React Native Architecture Open Questions
aliases: [RN architecture questions, RN brainstorm questions]
type: knowledge
project: global
status: draft
created: 2026-07-07
tags: [react-native, architecture, brainstorm, questions]
source: discussion
---

# React Native Architecture Open Questions

> [!abstract] TL;DR
> Brainstormed questions to consider when designing a scalable React Native app architecture. Organized by concern area. These emerged from analyzing production repos (Obytes, Ignite, TheCodingMachine) and their trade-offs.

## Architecture Decisions

1. **Feature-based vs. Feature-Sliced Design (FSD)?** FSD adds more layers (entities, widgets, processes). Is the simpler 4-zone model enough, or does it break down at 20+ features?
2. **Cross-feature shared state?** When two features need shared data (e.g., shopping cart needs product data from catalog), do you duplicate the API call, lift state to `lib/`, or create `features/shared/`?
3. **Barrel exports per feature?** Should `features/auth/` expose a public API through `index.ts`, or is direct deep-import acceptable?

## State Management

4. **When does Zustand become insufficient?** At what complexity should you introduce XState for flows like MFA, OAuth, biometric auth?
5. **React Query mutations → Zustand updates?** After a successful `createPost`, should a Zustand flag update, or is React Query cache invalidation enough?
6. **`createSelectors` at scale?** What's the performance profile with 50+ store fields? Is auto-generating selectors always better than manual?

## Navigation & Routing

7. **Deep linking + auth states?** User taps deep link to `/feed/123` but isn't logged in — how does the redirect chain work with Expo Router?
8. **React Navigation stacks inside Expo Router?** Multi-step flows (checkout, onboarding wizard) where "back" should go to the previous step, not the previous tab.
9. **Navigation analytics?** Where does screen tracking (`usePathname`, `useSegments`) live — layout, provider, or `lib/analytics/`?

## UI / Design System

10. **`tailwind-variants` vs. `cva`?** Compound variant handling, slot-based styling — which composes better at scale?
11. **Platform-specific styling?** NativeWind handles most cases, but components needing fundamentally different iOS/Android layouts?

## Testing & Quality

12. **React Query hook testing?** Mock axios, use MSW, or React Query's `renderHook` with custom `QueryClient`?
13. **Zustand store testing?** Direct (call actions, assert state) or indirect (render components, assert behavior)?
14. **Maestro vs. Detox?** Can Maestro handle gestures, animations, native dialogs at scale?

## Team & Scale

15. **Developer onboarding?** What's the first thing a new dev needs to understand — the 4-zone model, feature structure, or routing layer?

## Related

- [[rn-obytes-architecture-analysis]]
- [[rn-scalable-paradigm]]
- [[rn-best-practices]]

## Sources

- Discussion during Obytes architecture analysis (2026-07-07)
