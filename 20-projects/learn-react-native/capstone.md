---
title: capstone
aliases: []
type: spec
project: learn-react-native
status: active
created: 2026-07-01
updated: 2026-07-02
tags: [react-native, capstone, habit-tracker]
---

# Capstone — Habit Tracker

A single app built incrementally across Module 1. Each 🏗️ milestone chapter adds a vertical slice; by the end, the app is fully functional and ready for release.

## App concept

An **offline-first, syncable habit and task tracker**:

- Users define habits (name, category/tags, frequency, reminder time).
- Daily check-off grid — users track completed habits for today.
- Streak tracking — displays consecutive days completed.
- Offline persistence — stores all configuration and logs locally.
- Sync mechanism — queues changes when offline and syncs back to a mock remote server when online.
- Local reminders — schedules notifications based on unfinished daily habits.

The app is small enough to build independently but rich enough to serve as a complete vehicle for practicing modern mobile architectures, native components, and background tasks.

## Feature list

- **Habit CRUD**: Add, edit, list, and delete habits with options for frequency and reminder times.
- **Daily Check-Off**: Grid layout of today's habits with tap-to-complete toggles.
- **Streak & Analytics Engine**: Calculates current and historical streaks per habit.
- **Offline Persistence**: Fast-loading user preferences via MMKV, structured relational data via Expo SQLite and Drizzle ORM.
- **Reliable Sync**: A persistent mutation queue that tracks offline edits and replays them when network connectivity is restored.
- **Background Reminders**: Registers a native background task that checks status and fires local reminders when habits remain uncompleted.
- **OTA Updates & App Store Prep**: EAS configurations for production builds, custom development clients, and live bundle updates.

## Milestone map

Each milestone lands at the end of the named chapter. Complete the capstone step for that chapter before advancing.

| Chapter | What the capstone gains |
| ------- | ----------------------- |
| [[2.3 Dynamic Routing and Params]] | **UI shell** — Home screen, Habits dashboard, Settings view using Bottom Tabs, and Stack transition to Habit Detail (typed route parameters, no architecture/persistence layer). |
| [[3.3 Dependency Injection via React Context]] | **MVVM with custom hooks ViewModels + DI** — All UI views driven by custom ViewModel hooks. Mock backend service injected via React Context. TanStack Query managed query-cache states. |
| [[4.3 Offline-First Architecture]] | **Persistence & Offline Sync** — SQLite + Drizzle relational database for habits data. MMKV synchronous storage for preferences. Offline mutation queue tracking offline modifications and syncing back automatically on network restore. |
| [[5.2 Notifications and Background Tasks]] | **Background Reminders** — AppState lifecycle monitoring. BackgroundFetch task scheduled to run periodically in the background, firing local notifications to remind users to complete outstanding habits. |
| [[7.2 EAS Submit and Expo Updates]] | **Cloud Builds & Live Updates** — Configured `eas.json` profiles, signed production Android/iOS packages, and setup Expo Updates for OTA JS bundle publishing. |

## Related

- [[learn-react-native]] — the full learning path MOC

> [!note] Tracking
> Status/progress is tracked in ClickUp, NOT in this note.
