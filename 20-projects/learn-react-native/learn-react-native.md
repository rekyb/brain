---
title: learn-react-native
aliases: []
type: moc
project: learn-react-native
status: active
created: 2026-07-01
updated: 2026-07-02
tags: [react-native, expo, learning, moc]
---

# learn-react-native

> [!abstract]
> A structured self-study path to modern React Native and Expo development (Expo Router, Custom Hooks ViewModels, TanStack Query, MMKV, SQLite + Drizzle, TaskManager, EAS).
> Written for an experienced web React developer (or Kotlin developer transitioning to a mobile web-like environment). Focuses on mobile differences: declarative native views vs. web divs, touch gesture handling, mobile navigation paradigms, local filesystem databases, native device APIs, and building/submitting to the app stores.

## How to use this path

1. Open the chapter note and read the **Digest** — it gives you the concept and, where relevant, contrasts the Web/Android way with what you do now in modern Expo.
2. Close your laptop, open a blank file or sandbox, and do the **Drill by hand** — no AI assistance, no copy-pasting. Active struggle is the curriculum.
3. When you reach a 🏗️ milestone chapter, complete the **capstone step** before moving on. See [capstone](capstone.md) for the full milestone map.

---

## Module 1 — Foundations & Shipping

### Phase 0 — Reorientation

1. [0.1 Modern React Native and Expo landscape](0.1%20Modern%20React%20Native%20and%20Expo%20landscape.md)

### Phase 1 — Core & Gestures

1. [1.1 Core Components and Layouts](1.1%20Core%20Components%20and%20Layouts.md)
2. [1.2 Interaction and Gestures](1.2%20Interaction%20and%20Gestures.md)

### Phase 2 — Expo Router Navigation

1. [2.1 App Structure and Stacks](2.1%20App%20Structure%20and%20Stacks.md)
2. [2.2 Tabs and Nested Navigators](2.2%20Tabs%20and%20Nested%20Navigators.md)
3. [2.3 Dynamic Routing and Params](2.3%20Dynamic%20Routing%20and%20Params.md) 🏗️ — Capstone milestone: UI shell

### Phase 3 — Architecture, Hooks, State

1. [3.1 Architecture and Hooks Pattern](3.1%20Architecture%20and%20Hooks%20Pattern.md)
2. [3.2 Server State with TanStack Query](3.2%20Server%20State%20with%20TanStack%20Query.md)
3. [3.3 Dependency Injection via React Context](3.3%20Dependency%20Injection%20via%20React%20Context.md) 🏗️ — Capstone milestone: reactive ViewModels + fake networking + DI

### Phase 4 — Data & Persistence

1. [4.1 High-Performance Key-Value Storage](4.1%20High-Performance%20Key-Value%20Storage.md)
2. [4.2 Relational Storage with SQLite and Drizzle](4.2%20Relational%20Storage%20with%20SQLite%20and%20Drizzle.md)
3. [4.3 Offline-First Architecture](4.3%20Offline-First%20Architecture.md) 🏗️ — Capstone milestone: SQLite, Drizzle, MMKV, and offline mutation queue

### Phase 5 — Native Capabilities

1. [5.1 System Permissions and App Lifecycle](5.1%20System%20Permissions%20and%20App%20Lifecycle.md)
2. [5.2 Notifications and Background Tasks](5.2%20Notifications%20and%20Background%20Tasks.md) 🏗️ — Capstone milestone: background reminders + local notifications

### Phase 6 — Quality

1. [6.1 Testing React Native Components](6.1%20Testing%20React%20Native%20Components.md)
2. [6.2 Performance Tuning](6.2%20Performance%20Tuning.md)

### Phase 7 — Ship it

1. [7.1 EAS Build and Dev Clients](7.1%20EAS%20Build%20and%20Dev%20Clients.md)
2. [7.2 EAS Submit and Expo Updates](7.2%20EAS%20Submit%20and%20Expo%20Updates.md) 🏗️ — Capstone milestone: EAS builds, dev client, OTA update

---

## Module 2 — Advanced (stub — later session)

No chapter files. Topics to cover in a future session:

- React Native Architecture Internals (Hermes, JSI, Fabric renderer, TurboModules)
- Monorepos & Workspace management (Expo Router monorepos, Nx/Turborepo)
- Advanced Animations (Complex Reanimated gesture interactions, Shared Element transitions)
- Custom Native Modules & Swift/Kotlin integration (Expo Native Modules API)
- WebGL & High-Performance Rendering (`react-native-skia`)
- CI/CD automation for App Stores (GitHub Actions, EAS Credentials, Fastlane integration)
- Memory Leak detection & Advanced Performance Diagnostics (Hermes memory profiler, custom tracing)

---

> [!note]
> Progress and task status are tracked in ClickUp, not in this note.
