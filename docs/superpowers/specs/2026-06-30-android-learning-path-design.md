---
title: Android Learning Path — Design
aliases: [Android Curriculum Design, learn-android design]
type: spec
project: learn-android
status: draft
created: 2026-06-30
updated: 2026-06-30
tags: [android, kotlin, compose, learning, curriculum]
source:
---

# Android Learning Path — Design

> [!abstract] Goal
> A vault-resident, self-study Android curriculum that takes the learner from a refreshed baseline to shipping their own modern apps — each chapter a short digest plus a manual (hand-coded, no-AI) exercise, threaded through a single habit-tracker capstone.

## Background

Learner profile (gathered during brainstorming):

- **2021 baseline:** Kotlin + XML, intermediate. Shipped small apps. Knew `ViewModel`/`LiveData`, Retrofit, RecyclerView.
- **Gap:** ~4–5 years without writing Kotlin. Needs genuine Kotlin reactivation, not just a token refresher.
- **Goal:** Build & ship their own quality apps (solo dev, practical over exhaustive). Not currently job-hunting.
- **Learning style:** Short digest per chapter + **manual** exercises typed by hand. Explicitly **no vibe-coding / no AI-generated solutions** — the learner does the coding.
- **Exercise structure:** Hybrid — focused per-chapter drills **plus** one running capstone app.
- **Capstone:** Offline-first **habit / task tracker** (Room, WorkManager reminders, notifications, streaks).

This is a **brain vault** (Obsidian knowledge base), not a code repo. Deliverables are Markdown notes with YAML frontmatter, cross-linked with `[[wikilinks]]`. See vault conventions in `brain/CLAUDE.md`.

## Requirements

### Approach
- Approach **C — Condensed refresher + full modern coverage** (~30 chapters). Honors a "beginner→advanced" arc while respecting that the learner is an experienced dev with dormant skills, not a true beginner.
- **Modern-only.** Teach current best practice; do not teach legacy as the build path.

### Two modules
- **Module 1 (this session — written in full):** *Foundations & Shipping*. 30 chapters, fully fleshed (digest + drills + capstone steps).
- **Module 2 (stub outline only — built in a later session):** *Advanced Android*.
  - Centerpiece: **Clean Architecture + MVVM/MVI** (domain layer, use cases, mappers, MVI state machines).
  - Also: multi-module architecture, Compose internals, custom layouts + serious animation/motion, build-system internals (convention plugins), Firebase/backend, deep performance & profiling, CI/CD.
  - **No KMP** (learner does not need cross-platform).

### Per-chapter note format
Each Module 1 chapter is one note containing:
1. Frontmatter — `type: knowledge`, `project: learn-android`, `status: active`, `created`/`updated`, relevant `tags`.
2. **Digest** — short (a few paragraphs) "what changed / what to know," written for someone with 2021 context (explicitly contrasts with the old way where relevant).
3. **Drill(s)** — manual, hand-coded exercise(s) with clear success criteria. No solution code provided (learner does it).
4. **Capstone step** (where applicable) — what to add to the habit tracker after this chapter.
5. **Related** — `[[wikilinks]]` to prev/next chapter and dependencies.

### Vault structure
```
20-projects/learn-android/
├── learn-android.md          # MOC/index: roadmap, both modules, links every chapter
├── capstone.md               # habit-tracker spec + which milestone lands in which chapter
├── 0.1 Modern Android landscape.md
├── 1.1 Kotlin syntax refresher.md
├── 1.2 ...
└── ... (30 chapter notes total)
```

## Module 1 — Chapter list (30 chapters, 8 phases)

**Phase 0 — Reorientation (1)**
- 0.1 The modern Android landscape (2021→2026 map)

**Phase 1 — Kotlin reactivation (5)**
- 1.1 Syntax & fundamentals refresher
- 1.2 Collections & functional style
- 1.3 Classes the Kotlin way (data/sealed/object, extensions, delegation, scope functions)
- 1.4 Coroutines fundamentals
- 1.5 Flow & StateFlow

**Phase 2 — Jetpack Compose (7)**
- 2.1 The Compose mental model
- 2.2 State & state hoisting
- 2.3 Layouts & modifiers
- 2.4 Lazy lists (replaces RecyclerView)
- 2.5 Material 3 & theming
- 2.6 Navigation in Compose
- 2.7 Side effects & lifecycle
- 🏗️ Capstone: tracker UI shell (list + add screens, navigation, static data)

**Phase 3 — Modern architecture (4)**
- 3.1 Architecture overview (UI/domain/data, UDF)
- 3.2 ViewModel + UI state (StateFlow)
- 3.3 Dependency injection with Hilt
- 3.4 Repository pattern & data layer
- 🏗️ Capstone: wire tracker to ViewModel + Hilt; reactive UI

**Phase 4 — Data & persistence (4)**
- 4.1 Room (modern, with Flow + coroutines)
- 4.2 DataStore (replaces SharedPreferences)
- 4.3 Networking (Retrofit + coroutines + kotlinx.serialization)
- 4.4 Offline-first & caching (single source of truth)
- 🏗️ Capstone: habits persist (Room); settings (DataStore); optional daily-quote feature for networking

**Phase 5 — Platform capabilities (4)**
- 5.1 Runtime permissions (modern + Android 13 notifications)
- 5.2 WorkManager
- 5.3 Notifications (channels, scheduling, 13+ permission)
- 5.4 Lifecycle, process death & edge-to-edge
- 🏗️ Capstone: habit reminders via WorkManager + notifications

**Phase 6 — Quality (3)**
- 6.1 Testing (unit, coroutine/Flow w/ Turbine, Compose UI tests, fakes)
- 6.2 Performance (recomposition, stability, Baseline Profiles)
- 6.3 Accessibility & adaptive UI
- 🏗️ Capstone: test suite for tracker ViewModel + a screen

**Phase 7 — Ship it (2)**
- 7.1 Build & release (version catalogs, variants, signing, R8, app bundles)
- 7.2 Play Store launch (listing, data safety, release tracks, Play vitals)
- 🏗️ Capstone: signed release build, upload-ready

## Module 2 — Stub outline (NOT written this session)

To be captured as a titled outline in `learn-android.md` only (no digests/drills yet):

- Clean Architecture & MVVM/MVI deep dive (domain entities, use cases, mappers, MVI state machines)
- Advanced dependency injection (Hilt scopes/components, multi-bindings, assisted injection, DI across modules)
- Multi-module architecture
- Compose internals (compiler, recomposition/snapshot system, stability)
- Custom Compose layouts + animation/motion
- Build-system internals (Gradle convention plugins, custom tasks)
- Firebase / backend integration (Auth, Firestore, FCM, cloud sync)
- Deep performance & profiling
- CI/CD (e.g., GitHub Actions)

## Non-goals

- **No legacy as the build path:** XML layouts/Views, Fragments, RecyclerView adapters, data binding, navigation XML graph, RxJava, Java. (Recognized in the wild, not built new.)
- **No KMP / Compose Multiplatform** (deferred out entirely, not even Module 2).
- **No AI-generated exercise solutions** — drills are hand-coded by the learner.
- **No task/progress tracking inside notes** — that lives in ClickUp per vault rules.
- **No app code in this repo** — this vault holds notes only; the capstone app is built by the learner in their own Android Studio project.

## Acceptance criteria

- [ ] `20-projects/learn-android/learn-android.md` MOC exists: roadmap, both modules, every Module 1 chapter linked in order, Module 2 stub outline present.
- [ ] `20-projects/learn-android/capstone.md` exists: habit-tracker spec + milestone-to-chapter mapping.
- [ ] All 30 Module 1 chapter notes exist, each with: correct frontmatter, a digest, at least one manual drill with success criteria, capstone step where applicable, and prev/next `[[wikilinks]]`.
- [ ] No chapter contains AI-written solution code for its drills.
- [ ] Notes follow vault frontmatter schema and conventions (`brain/CLAUDE.md`).
- [ ] Content is modern-only (no legacy taught as the build path).

## Affected repos / files

- `brain/` (this repo) — new folder `20-projects/learn-android/` with MOC, capstone, and 30 chapter notes.
- `brain/docs/superpowers/specs/2026-06-30-android-learning-path-design.md` — this design.

## Open questions

- None blocking. (Firebase / advanced clean-architecture deferred to Module 2 by decision.)

> [!note] Tracking
> Status/progress is tracked in ClickUp, NOT in this note.
