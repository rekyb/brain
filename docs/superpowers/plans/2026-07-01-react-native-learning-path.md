# React Native + Expo Learning Path Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Generate a complete vault-resident React Native + Expo self-study curriculum — an MOC index, a capstone spec, and 18 fully-written chapter notes — in `brain/20-projects/learn-react-native/`.

**Architecture:** Pure Markdown content generation in an Obsidian vault (no code, no app, no tests-as-software). Each chapter is one focused note. "Verification" means checking each note against a fixed quality checklist. Work is batched by phase; each task ends with a git commit in the `brain` repo.

**Tech Stack:** Markdown + YAML frontmatter, Obsidian `[[wikilinks]]`. Subject matter taught in the notes: React Native Core Components, Expo Router, Custom Hooks, TanStack Query, MMKV, Expo SQLite + Drizzle ORM, Expo TaskManager + BackgroundFetch, Jest + React Native Testing Library (RNTL), EAS Build & Submit, Expo Updates.

## Global Constraints

- **Location:** All files under `brain/20-projects/learn-react-native/`. Commit to the `brain` repo only.
- **One chapter = one note (one `.md` file).** 18 chapter notes + `learn-react-native.md` (MOC) + `capstone.md` = 20 files.
- **Modern-only:** Teach modern Expo (Expo Router v3+, Expo SDK 51+, EAS, custom dev clients). Avoid legacy (bare React Native CLI, React Navigation traditional setup without file-based, class components, AsyncStorage, SQLite without ORM).
- **No AI-written solution code for drills:** Drills describe the task + success criteria; they must NOT include a worked solution. Short illustrative snippets in the *digest* (to explain a concept/pattern) are fine; the *drill* is the learner's to solve.
- **Learner profile (write to this level):** Experienced web React developer (or Kotlin developer transitioning to mobile web-like environment). Understard hooks, virtual DOM, JSX, components. Focus on mobile differences: declarative native views vs web divs, touch responders, mobile navigation paradigms, local filesystem databases, device-level APIs, building/submitting to app stores.
- **No task/progress tracking inside notes** (vault rule — that lives in ClickUp).
- **Filename convention:** `<phase>.<chapter> <Title>.md`, e.g., `2.3 Dynamic Routing and Params.md`.
- **Vault conventions:** Follow `brain/CLAUDE.md` frontmatter schema.

## Chapter Note Skeleton (every chapter note uses this)

```markdown
---
title: <Phase.Chapter Title>
aliases: []
type: knowledge
project: learn-react-native
status: active
created: 2026-07-01
updated: 2026-07-01
tags: [react-native, expo, <topic tags>]
source:
---

# <Phase.Chapter Title>

> [!abstract] TL;DR
> 1–2 sentences: what this chapter gives you.

## Digest
<Short explanation — a few paragraphs. Explicitly contrast web/Android concepts with React Native + Expo equivalents. Illustrative code snippets allowed here.>

### What changed (Web -> Mobile / Classic RN -> Modern Expo)
<Bullets — key deltas. Omit if not applicable.>

## Drill
<Manual, hand-coded exercise. State the task and concrete success criteria. NO solution code. Use a checklist of what "done" looks like.>

> [!example] Success criteria
> - [ ] ...
> - [ ] ...

## Capstone step
<Only on chapters with a 🏗️ milestone. What to add to the habit tracker now. Describe the feature + acceptance, not the solution code.>

## Related
- Prev: [[<prev chapter>]]
- Next: [[<next chapter>]]
- See also: [[<dependency>]]
```

---

### Task 1: Scaffold — folder, MOC, capstone

**Files:**
- Create: `brain/20-projects/learn-react-native/learn-react-native.md` (MOC)
- Create: `brain/20-projects/learn-react-native/capstone.md`

**Interfaces:**
- Produces: the canonical chapter filenames/titles that every later task links to; the capstone milestone→chapter mapping that Tasks 3–7 reference.

- [ ] **Step 1: Create the MOC** `learn-react-native.md`
  - Frontmatter: `type: moc`, `project: learn-react-native`, `status: active`, `created: 2026-07-01`, `tags: [react-native, expo, learning, moc]`.
  - Body sections:
    - Abstract: overview of React Native + Expo learning path.
    - How to use this path.
    - Curriculum map: Modules and phases with links to chapter notes.
- [ ] **Step 2: Create `capstone.md`**
  - Frontmatter: `type: spec`, `project: learn-react-native`, `status: active`, `tags: [react-native, capstone, habit-tracker]`.
  - Body: App concept (Offline-First Syncable Habit Tracker). Feature list. Milestone map table matching the design spec.
- [ ] **Step 3: Verify**
  - Run: `ls brain/20-projects/learn-react-native/`
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/learn-react-native.md brain/20-projects/learn-react-native/capstone.md
  git commit -m "learn-react-native: scaffold MOC and capstone tracker"
  ```

---

### Task 2: Phase 0 + Phase 1 notes (Landscape, Components, Layouts, Gestures)

**Files:** Create 3 notes —
`0.1 Modern React Native and Expo landscape.md`, `1.1 Core Components and Layouts.md`, `1.2 Interaction and Gestures.md`

- [ ] **Step 1: Write all 3 notes** using the skeleton. Focus on web vs. native mapping (e.g., `<View>` vs `div`, `<Text>` vs `span`/`p`, `<Image>` vs `img`). Detail styling (Flexbox defaults in RN, StyleSheet). Explain Gestures (Pressable, Gesture Handler).
- [ ] **Step 2: Verify** each note matches skeleton and has no solution code.
- [ ] **Step 3: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 0-1 notes (landscape, layouts, gestures)"
  ```

---

### Task 3: Phase 2 notes (Expo Router Navigation)

**Files:** Create 3 notes —
`2.1 App Structure and Stacks.md`, `2.2 Tabs and Nested Navigators.md`, `2.3 Dynamic Routing and Params.md`

- [ ] **Step 1: Write all 3 notes**. Focus on File-Based Navigation (Expo Router v3). Compare to Next.js or React Router. Highlight stack navigation, tab layouts, and passing route parameters safely.
- [ ] **Step 2: Add Capstone step to `2.3`**: Build the UI shell with Bottom Tabs (Home, Habits, Settings) and Stacks (Habit Detail), fully typed with Expo Router.
- [ ] **Step 3: Verify** all notes.
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 2 notes (Expo Router navigation)"
  ```

---

### Task 4: Phase 3 notes (Architecture, Hooks, TanStack Query)

**Files:** Create 3 notes —
`3.1 Architecture and Hooks Pattern.md`, `3.2 Server State with TanStack Query.md`, `3.3 Dependency Injection via React Context.md`

- [ ] **Step 1: Write all 3 notes**. Contrast custom React hooks as ViewModels. Explain server-state caching via TanStack Query. Discuss clean dependency injection via React Context.
- [ ] **Step 2: Add Capstone step to `3.3`**: Wire up fake networking with TanStack Query, separating UI layers from custom hooks (acting as ViewModels), injected via React Context.
- [ ] **Step 3: Verify** notes.
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 3 notes (architecture, hooks, state)"
  ```

---

### Task 5: Phase 4 notes (Data & Persistence)

**Files:** Create 3 notes —
`4.1 High-Performance Key-Value Storage.md`, `4.2 Relational Storage with SQLite and Drizzle.md`, `4.3 Offline-First Architecture.md`

- [ ] **Step 1: Write all 3 notes**. Teach high-performance synchronous storage using `react-native-mmkv` (replacing slow AsyncStorage). Teach relational storage using Expo SQLite + Drizzle ORM. Design an offline-first architecture with mutation queues.
- [ ] **Step 2: Add Capstone step to `4.3`**: Replace fake store with MMKV for settings/auth and SQLite + Drizzle for habits storage. Support offline updates (queueing mutations when offline and syncing back when online).
- [ ] **Step 3: Verify** notes.
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 4 notes (MMKV, SQLite, Drizzle, offline-first)"
  ```

---

### Task 6: Phase 5 notes (Native Capabilities)

**Files:** Create 2 notes —
`5.1 System Permissions and App Lifecycle.md`, `5.2 Notifications and Background Tasks.md`

- [ ] **Step 1: Write both notes**. Teach system permissions flow, AppState hook, and background capabilities via Expo TaskManager + BackgroundFetch. Detail local and push notifications.
- [ ] **Step 2: Add Capstone step to `5.2`**: Request notification permissions and register a background fetch task that triggers a local notification reminder if the habits for the day haven't been completed.
- [ ] **Step 3: Verify** notes.
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 5 notes (permissions, background tasks, notifications)"
  ```

---

### Task 7: Phase 6 notes (Testing & Performance)

**Files:** Create 2 notes —
`6.1 Testing React Native Components.md`, `6.2 Performance Tuning.md`

- [ ] **Step 1: Write both notes**. Cover Jest, React Native Testing Library (RNTL), mocking native modules, and performance optimization techniques (FlatList optimization, avoiding excessive rerenders, memory leaks).
- [ ] **Step 2: Verify** notes.
- [ ] **Step 3: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 6 notes (testing and performance)"
  ```

---

### Task 8: Phase 7 notes (Shipping with EAS)

**Files:** Create 2 notes —
`7.1 EAS Build and Dev Clients.md`, `7.2 EAS Submit and Expo Updates.md`

- [ ] **Step 1: Write both notes**. Focus on EAS (Expo Application Services) Build, custom development clients, EAS Submit, and Over-The-Air (OTA) updates using Expo Updates.
- [ ] **Step 2: Add Capstone step to `7.2`**: Setup EAS configuration, build a dev client, install on emulator/device, publish a minor visual update via Expo OTA updates.
- [ ] **Step 3: Verify** notes.
- [ ] **Step 4: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: Phase 7 notes (EAS builds, dev clients, updates)"
  ```

---

### Task 9: Cross-link audit + MOC reconciliation

**Files:** Modify any note with a broken/missing link; `learn-react-native.md` if any title drifted.

- [ ] **Step 1: Audit links.** Confirm every chapter has resolving prev/next; the chain is unbroken 0.1→7.2; MOC links every one of the 18 chapters by exact filename; `capstone.md` milestone chapters match the notes that actually carry a 🏗️ step (2.3, 3.3, 4.3, 5.2, 7.2).
  - Run: `ls brain/20-projects/learn-react-native/*.md | wc -l` → expect 20.
  - Run: `grep -rl "Capstone step" brain/20-projects/learn-react-native/ | wc -l` → expect 5.
- [ ] **Step 2: Fix** any drift inline.
- [ ] **Step 3: Commit**
  ```bash
  git add brain/20-projects/learn-react-native/
  git commit -m "learn-react-native: cross-link audit + MOC reconciliation"
  ```

---

## CHAPTER BRIEFS

### Phase 0 — Reorientation

**0.1 Modern React Native and Expo landscape** — *Digest:* The shift from bare React Native CLI to Managed Expo. How Expo prebuilds native directories (`ios/`, `android/`) dynamically. The difference between Expo Go (rigid dependencies) vs. Development Clients (custom native modules). Introduction to EAS (Expo Application Services). *Drill:* Initialize a new Expo app using `npx create-expo-app@latest`, configure it to use Expo Router, run it on a simulator, and inspect the project structure. Explain `app.json`, `.expo`, and the entry point. *Success:* App running on simulator; learner has documented Expo prebuild mechanics.

### Phase 1 — Core & Gestures

**1.1 Core Components and Layouts** — *Digest:* Core components (`View`, `Text`, `Image`, `ScrollView`, `TextInput`, `FlatList`). Styling in React Native via `StyleSheet.create`. Flexbox default configurations in mobile (e.g., `flexDirection: 'column'`). Pixel density and scaling. *Drill:* Construct a profile card with an avatar, name, bio, and a vertical scrollable list of recent posts using `FlatList`. Stylize with borders, shadows, and flexbox alignment. *Success:* Card renders identically on iOS and Android simulators; FlatList is used and scrolling works.

**1.2 Interaction and Gestures** — *Digest:* Touch components (`Pressable`, `TouchableOpacity`). Handling active states and custom feedback styling. Introduction to `react-native-gesture-handler` and `react-native-reanimated` for smooth interaction. *Drill:* Build a card component that can be swiped horizontally to dismiss (using `GestureDetector` and `Animated` or `Reanimated`). *Success:* Card tracks finger movement smoothly and snaps back or flies off-screen when swiped past threshold.

### Phase 2 — Expo Router Navigation

**2.1 App Structure and Stacks** — *Digest:* File-based routing with Expo Router. Root layout structure (`_layout.tsx`). The stack navigator (`Stack`). Navigating via `Link` component and `router` imperative hook. Type safety in routing. *Drill:* Set up a basic Stack layout where clicking a card in the list navigates to a detail page with header customization. *Success:* Transition is smooth; navigation bar title matches detail item name.

**2.2 Tabs and Nested Navigators** — *Digest:* Bottom tab layouts (`Tabs`). Custom tab icons and active states. Nesting stacks inside tabs. Shared headers vs individual screen headers. *Drill:* Create a primary Tab layout with three screens: Home, Habits, Settings. Nest a detail Stack navigator inside the Habits tab. *Success:* Navigation between tabs functions; detail screen transitions correctly within the tabs view.

**2.3 Dynamic Routing and Params** — *Digest:* Dynamic route names (e.g., `[id].tsx`). Reading query params and route params using `useLocalSearchParams`. Typesafe route parameters. *Drill:* Modify detail screen to read dynamic ID from url path and fetch data according to that ID. Pass query parameters from list screen. *Success:* Detail screen displays ID and parameters correctly. **🏗️ Capstone step:** Build the UI shell with Bottom Tabs (Home, Habits, Settings) and Stacks (Habit Detail), fully typed with Expo Router.

### Phase 3 — Architecture, Hooks, State

**3.1 Architecture and Hooks Pattern** — *Digest:* Architectural layering in React Native: Container-Presenter pattern vs. Custom Hooks as ViewModels. Maintaining stateless UI components. Hook lifetimes and decoupling business logic. *Drill:* Write a `useHabitViewModel` custom hook that manages habit list fetching, adding, and filtering state. Back a stateless `<HabitList>` UI with this hook. *Success:* UI component does not contain any state variables (`useState`/`useEffect`); all state and functions come from the hook.

**3.2 Server State with TanStack Query** — *Digest:* Managing asynchronous state. Why TanStack Query (React Query) is preferred over global Redux/Zustand for server data. Query keys, fetching, caching, stale time, and mutations. *Drill:* Refactor custom hook `useHabitViewModel` to fetch data from a mock REST endpoint (using mock API wrapper) via `useQuery` and add items via `useMutation`. *Success:* Query states (loading, error, success) are correctly handled in the UI; mutations trigger invalidation and refetching.

**3.3 Dependency Injection via React Context** — *Digest:* DI in React apps. Using React Context as a dependency provider. Injecting services/repositories (like API client or storage services) into custom hooks instead of importing concrete instances directly. *Drill:* Create a `ServiceContext` providing a mock backend service. Inject this service into `useHabitViewModel` via `useContext`. *Success:* The custom hook accesses the service through context, enabling easy swapping of service implementation for tests. **🏗️ Capstone step:** Wire up fake networking with TanStack Query, separating UI layers from custom hooks (acting as ViewModels), injected via React Context.

### Phase 4 — Data & Persistence

**4.1 High-Performance Key-Value Storage** — *Digest:* Synchronous key-value storage. Why classic `AsyncStorage` (async, over the bridge) is deprecated for performance-critical tasks. Modern `react-native-mmkv` (C++ JSI-backed, synchronous reads/writes). *Drill:* Implement a theme provider and user preference store using MMKV. Verify preferences load instantly on app boot without layout flashes. *Success:* Preferences persist across app closures; read operations are fully synchronous.

**4.2 Relational Storage with SQLite and Drizzle** — *Digest:* Local databases in React Native. Using `expo-sqlite/next`. Moving from raw SQL strings to type-safe relational queries using Drizzle ORM. Schemas, migrations, inserts, updates, and reactive queries. *Drill:* Create a habits table schema using Drizzle. Write queries to insert a habit and fetch habits list. Run migrations on app startup. *Success:* Database queries execute successfully; schema changes are handled via migrations.

**4.3 Offline-First Architecture** — *Digest:* Single-source-of-truth offline architecture. Sync state machine. Queueing mutations when offline. Optimistic UI updates. Reconciling network responses. *Drill:* Build an offline synchronization queue that saves pending mutations to MMKV or SQLite when offline, detects network connectivity changes, and syncs them to the backend when online. *Success:* Mutations execute locally immediately; queue syncs to network automatically when connection restores. **🏗️ Capstone step:** Replace fake store with MMKV for settings/auth and SQLite + Drizzle for habits storage. Support offline updates (queueing mutations when offline and syncing back when online).

### Phase 5 — Native Capabilities

**5.1 System Permissions and App Lifecycle** — *Digest:* Requesting system permissions (camera, location, notifications) using `expo-sensors`/`expo-permissions`. Handling app states (active, background, inactive) via `AppState`. *Drill:* Monitor AppState changes to refresh local database lock and log timestamp when app moves to background. Handle permission rejection with appropriate UI rationale. *Success:* App state logs print correctly; permission flow behaves correctly under grant/deny.

**5.2 Notifications and Background Tasks** — *Digest:* Local notifications vs. Push notifications (FCM/APNs) in Expo. Registering background tasks via `expo-task-manager` and executing periodic jobs with `expo-background-fetch`. *Drill:* Register a background fetch task that increments a count in MMKV and triggers a local notification if the increment matches a condition. *Success:* Notification shows when app is in background; fetch task triggers according to OS scheduling. **🏗️ Capstone step:** Request notification permissions and register a background fetch task that triggers a local notification reminder if the habits for the day haven't been completed.

### Phase 6 — Testing & Performance

**6.1 Testing React Native Components** — *Digest:* Setting up Jest for React Native. Mocking native modules (MMKV, SQLite, Router) in Jest. Component unit testing and integration testing using React Native Testing Library (RNTL). Asserting UI elements, firing events, and testing hooks with `@testing-library/react-hooks`. *Drill:* Write unit tests for a custom hook using `renderHook` and a screen component using RNTL, mock `expo-sqlite` and `expo-router`. *Success:* All tests pass; native modules are correctly mocked.

**6.2 Performance Tuning** — *Digest:* Finding bottlenecks. Using React DevTools Profiler. Optimizing `FlatList` (`getItemLayout`, `windowSize`, `initialNumToRender`, `removeClippedSubviews`). Avoiding inline functions in render. Using `useMemo` and `useCallback` effectively. Understanding the JS-Native bridge bottleneck. *Drill:* Profile a slow scrolling list, implement optimization properties on `FlatList`, and show reduction in rerenders. *Success:* List scrolling is measurably smoother; profile shows decreased render durations.

### Phase 7 — Shipping with EAS

**7.1 EAS Build and Dev Clients** — *Digest:* Understanding EAS (Expo Application Services). Configuration in `eas.json`. Building custom development clients (`npx expo run:android` / `run:ios` equivalents on EAS). Setting up credentials and provisioning profiles. *Drill:* Configure `eas.json` for development and production build profiles. Initiate a development client build via EAS CLI. *Success:* Build completes successfully on EAS; dev client installation package is produced.

**7.2 EAS Submit and Expo Updates** — *Digest:* Submitting builds to Apple App Store & Google Play Store via EAS Submit. Over-The-Air (OTA) updates architecture. How the runtime switches bundles dynamically. Setting up release channels and branches. *Drill:* Configure OTA updates in `app.json`. Trigger an OTA update publish via EAS. *Success:* Update is published; running app receives update and reloads. **🏗️ Capstone step:** Setup EAS configuration, build a dev client, install on emulator/device, publish a minor visual update via Expo OTA updates.

---

## Self-Review

**Spec coverage:** All 18 chapters and MOC + capstone spec mapped to Tasks 2–8. Milestone requirements aligned with design specification. ✓
**Placeholder scan:** No placeholders or TODOs. Detailed chapter digests, drills, and success criteria specified. ✓
**Consistency:** File paths, chapter numbers, and titles match the spec and MOC structure. ✓
