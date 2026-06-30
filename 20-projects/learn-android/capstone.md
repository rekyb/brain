---
title: capstone
aliases: []
type: spec
project: learn-android
status: active
created: 2026-06-30
updated: 2026-06-30
tags: [android, capstone, habit-tracker]
---

# Capstone — Habit Tracker

A single app built incrementally across Module 1. Each 🏗️ milestone chapter adds a vertical slice; by the end the app is releasable.

## App concept

An **offline-first habit and task tracker**:

- Users define habits (name, optional description, reminder time).
- Daily check-off — mark a habit as done for today.
- Streak tracking — consecutive days completed.
- Reminders — per-habit notifications at a scheduled time.

The app intentionally stays small. Its value is as a vehicle for practising every layer of the stack in a realistic context.

## Feature list

- Add, edit, and delete habits.
- Daily check-off grid (today's habits + done/not-done state).
- Streak counter per habit (consecutive completed days).
- Habit reminder: schedule a daily notification at a user-chosen time.
- Settings screen: at minimum, a "week starts on" preference.
- Optional: a daily motivational quote fetched from a public API and cached offline.

## Milestone map

Each milestone lands at the end of the named chapter. Complete the capstone step for that chapter before advancing.

| Chapter | What the capstone gains |
| ------- | ----------------------- |
| [[2.7 Side effects and lifecycle]] | **UI shell** — habit-list screen + add-habit screen, navigation wired between them, backed by static in-memory data. No persistence, no architecture layer yet. |
| [[3.4 Repository pattern and data layer]] | **Reactive ViewModel + Hilt + repository** — UI is now driven by a `StateFlow<UiState>`; Hilt provides the repository; data is still in-memory but the architecture is in place. |
| [[4.4 Offline-first and caching]] | **Persistence** — habits stored in Room (survive app restart); user settings in DataStore; optional daily-quote feature exercises the offline-first network → DB → UI pattern. |
| [[5.3 Notifications]] | **Reminders** — per-habit reminders scheduled via WorkManager; notifications posted on channel with Android 13+ permission flow handled. |
| [[6.1 Testing]] | **Test suite** — unit tests for ViewModel state logic (fake repository, `runTest`/Turbine) + at least one Compose UI test asserting a screen renders a habit. |
| [[7.2 Play Store launch]] | **Signed release build, upload-ready** — signed `.aab` produced, R8 minification confirmed working, store listing draft + Data safety form answers written. |

## Related

- [[learn-android]] — the full learning path MOC
