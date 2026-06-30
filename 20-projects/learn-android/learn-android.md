---
title: learn-android
aliases: []
type: moc
project: learn-android
status: active
created: 2026-06-30
updated: 2026-06-30
tags: [android, kotlin, compose, learning, moc]
---

# learn-android

> [!abstract]
> A structured self-study path to modern Android development (Compose, coroutines/Flow, Hilt, Room, WorkManager, Material 3).
> Written for an experienced dev whose Kotlin+Android skills are ~5 years dormant (last active 2021 — knew Kotlin, XML/ViewModel/LiveData/Retrofit/RecyclerView); goal is to build and ship personal apps with no legacy baggage.

## How to use this path

1. Open the chapter note and read the **Digest** — it gives you the concept and, where relevant, contrasts the 2021 way with what you do now.
2. Close your laptop, open a blank file or Kotlin Playground, and do the **Drill by hand** — no AI assistance, no vibe-coding. Struggle is the curriculum.
3. When you reach a 🏗️ milestone chapter, complete the **Capstone step** before moving on. See [[capstone]] for the full milestone map.

---

## Module 1 — Foundations & Shipping

### Phase 0 — Reorientation

1. [[0.1 Modern Android landscape]]

### Phase 1 — Kotlin reactivation

1. [[1.1 Kotlin syntax refresher]]
2. [[1.2 Collections & functional style]]
3. [[1.3 Classes the Kotlin way]]
4. [[1.4 Coroutines fundamentals]]
5. [[1.5 Flow and StateFlow]]

### Phase 2 — Jetpack Compose

1. [[2.1 The Compose mental model]]
2. [[2.2 State and state hoisting]]
3. [[2.3 Layouts and modifiers]]
4. [[2.4 Lazy lists]]
5. [[2.5 Material 3 and theming]]
6. [[2.6 Navigation in Compose]]
7. [[2.7 Side effects and lifecycle]] 🏗️ — Capstone milestone: UI shell

### Phase 3 — Modern architecture

1. [[3.1 Architecture overview]]
2. [[3.2 ViewModel and UI state]]
3. [[3.3 Dependency injection with Hilt]]
4. [[3.4 Repository pattern and data layer]] 🏗️ — Capstone milestone: reactive ViewModel + Hilt

### Phase 4 — Data & persistence

1. [[4.1 Room]]
2. [[4.2 DataStore]]
3. [[4.3 Networking]]
4. [[4.4 Offline-first and caching]] 🏗️ — Capstone milestone: Room + DataStore + optional networking

### Phase 5 — Platform capabilities

1. [[5.1 Runtime permissions]]
2. [[5.2 WorkManager]]
3. [[5.3 Notifications]] 🏗️ — Capstone milestone: reminders + notifications
4. [[5.4 Lifecycle, process death and edge-to-edge]]

### Phase 6 — Quality

1. [[6.1 Testing]] 🏗️ — Capstone milestone: test suite
2. [[6.2 Performance]]
3. [[6.3 Accessibility and adaptive UI]]

### Phase 7 — Ship it

1. [[7.1 Build and release]]
2. [[7.2 Play Store launch]] 🏗️ — Capstone milestone: signed release build, upload-ready

---

## Module 2 — Advanced (stub — later session)

No chapter files. Topics to cover in a future session:

- Clean Architecture & MVVM/MVI patterns
- Advanced DI — Hilt scopes/components, multi-bindings, assisted injection, DI across modules
- Multi-module architecture
- Compose internals (slot table, recomposition mechanics)
- Custom layouts + animation/motion
- Build-system internals (convention plugins, composite builds)
- Firebase / backend integration
- Deep performance & profiling (Perfetto, system traces, ANR analysis)
- CI/CD for Android

**No KMP (Kotlin Multiplatform).** This path is Android-only.

---

> [!note]
> Progress and task status are tracked in ClickUp, not in this note.
