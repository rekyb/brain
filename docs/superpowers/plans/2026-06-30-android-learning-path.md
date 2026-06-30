# Android Learning Path Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Generate a complete vault-resident Android self-study curriculum — an MOC index, a capstone spec, and 30 fully-written Module 1 chapter notes — in `brain/20-projects/learn-android/`.

**Architecture:** Pure Markdown content generation in an Obsidian vault (no code, no app, no tests-as-software). Each chapter is one focused note. "Verification" means checking each note against a fixed quality checklist. Work is batched by phase; each task ends with a git commit in the `brain` repo.

**Tech Stack:** Markdown + YAML frontmatter, Obsidian `[[wikilinks]]`. Subject matter taught in the notes: Kotlin, Jetpack Compose, coroutines/Flow, Hilt, Room, DataStore, WorkManager, Material 3.

## Global Constraints

- **Location:** all files under `brain/20-projects/learn-android/`. Commit to the `brain` repo only.
- **One chapter = one note (one `.md` file).** 30 chapter notes + `learn-android.md` (MOC) + `capstone.md` = 32 files.
- **Module 1 only** is written in full. **Module 2** is a titled stub outline inside the MOC — no chapter files, no digests.
- **Modern-only:** never teach legacy (XML layouts/Views, Fragments, RecyclerView adapters, data binding, nav XML graph, RxJava, Java) as the build path. Contrasting "the 2021 way vs now" inside a digest is encouraged; building new with legacy is not.
- **No AI-written solution code for drills.** Drills describe the task + success criteria; they must NOT include a worked solution. Short illustrative snippets in the *digest* (to explain a concept) are fine; the *drill* is the learner's to solve.
- **Learner profile (write to this level):** experienced dev, Kotlin dormant ~5 years, knew Kotlin+XML/ViewModel/LiveData/Retrofit/RecyclerView in 2021. Don't explain "what is a variable"; do explain what changed and what's new.
- **No task/progress tracking inside notes** (vault rule — that lives in ClickUp).
- **Filename convention:** `<phase>.<chapter> <Title>.md`, e.g. `2.4 Lazy lists.md`. Match titles in the CHAPTER BRIEFS section exactly.
- **Vault conventions:** follow `brain/CLAUDE.md` frontmatter schema.

## Chapter Note Skeleton (every Module 1 chapter note uses this)

```markdown
---
title: <Phase.Chapter Title>
aliases: []
type: knowledge
project: learn-android
status: active
created: 2026-06-30
updated: 2026-06-30
tags: [android, <topic tags>]
source:
---

# <Phase.Chapter Title>

> [!abstract] TL;DR
> 1–2 sentences: what this chapter gives you.

## Digest
<Short explanation — a few paragraphs. For refresh topics, explicitly contrast
"the 2021 way" with "now." Illustrative snippets allowed here.>

### What changed since 2021
<Bullets — only on chapters where there's a meaningful delta. Omit otherwise.>

## Drill
<Manual, hand-coded exercise. State the task and concrete success criteria.
NO solution code. Use a checklist of what "done" looks like.>

> [!example] Success criteria
> - [ ] ...
> - [ ] ...

## Capstone step
<Only on chapters with a 🏗️ milestone. What to add to the habit tracker now.
Describe the feature + acceptance, not the solution code.>

## Related
- Prev: [[<prev chapter>]]
- Next: [[<next chapter>]]
- See also: [[<dependency>]]
```

---

### Task 1: Scaffold — folder, MOC, capstone

**Files:**
- Create: `brain/20-projects/learn-android/learn-android.md` (MOC)
- Create: `brain/20-projects/learn-android/capstone.md`

**Interfaces:**
- Produces: the canonical chapter filenames/titles (from CHAPTER BRIEFS) that every later task links to; the capstone milestone→chapter mapping that Tasks 4–8 reference.

- [ ] **Step 1: Create the MOC** `learn-android.md`
  - Frontmatter: `type: moc`, `project: learn-android`, `status: active`, `created: 2026-06-30`, `tags: [android, kotlin, compose, learning, moc]`.
  - Body sections:
    - `> [!abstract]` one-line purpose + learner profile one-liner.
    - **How to use this path** — read digest, do the drill by hand (no AI), advance capstone at 🏗️ milestones.
    - **Module 1 — Foundations & Shipping** — the 8 phases as `##` headings, each listing its chapters as `[[wikilinks]]` in order, with 🏗️ capstone milestones noted.
    - **Module 2 — Advanced (stub, later session)** — bullet list of the Module 2 outline (see spec): Clean Architecture & MVVM/MVI; Advanced DI (Hilt scopes/components, multi-bindings, assisted injection, DI across modules); multi-module architecture; Compose internals; custom layouts + animation/motion; build-system internals (convention plugins); Firebase/backend; deep performance & profiling; CI/CD. Note "No KMP."
    - `> [!note]` tracking lives in ClickUp.
- [ ] **Step 2: Create `capstone.md`**
  - Frontmatter: `type: spec`, `project: learn-android`, `status: active`, `tags: [android, capstone, habit-tracker]`.
  - Body: app concept (offline-first habit/task tracker — habits, daily check-off, streaks, reminders). Feature list. **Milestone map** table: each 🏗️ chapter → what the capstone gains (P2 UI shell → P3 ViewModel+Hilt → P4 Room+DataStore+optional quote API → P5 reminders/notifications → P6 tests → P7 signed release). Link `[[learn-android]]`.
- [ ] **Step 3: Verify** against checklist
  - Run: `ls brain/20-projects/learn-android/` → shows both files.
  - Confirm MOC lists all 30 chapter titles exactly as in CHAPTER BRIEFS, Module 2 stub present, no Module 2 chapter files implied.
- [ ] **Step 4: Commit**

```bash
git add brain/20-projects/learn-android/learn-android.md brain/20-projects/learn-android/capstone.md
git commit -m "learn-android: scaffold MOC + capstone spec"
```

---

### Task 2: Phase 0 + Phase 1 notes (Reorientation + Kotlin reactivation)

**Files:** Create 6 notes —
`0.1 Modern Android landscape.md`, `1.1 Kotlin syntax refresher.md`, `1.2 Collections & functional style.md`, `1.3 Classes the Kotlin way.md`, `1.4 Coroutines fundamentals.md`, `1.5 Flow and StateFlow.md`

**Interfaces:**
- Consumes: chapter titles + skeleton from Task 1 / Global Constraints.
- Produces: notes `1.4`/`1.5` are linked as dependencies by Phase 2/3 chapters.

- [ ] **Step 1: Write all 6 notes** using the skeleton, content from CHAPTER BRIEFS P0–P1. Phase 1 drills run in Kotlin Playground / a scratch `.kt` file (no Android yet). Set prev/next links across the chain (0.1↔1.1↔…↔1.5↔2.1).
- [ ] **Step 2: Verify** each note: valid frontmatter, TL;DR, digest, ≥1 drill with success-criteria checklist, no solution code, prev/next links resolve. `0.1` has a "What changed since 2021" section.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 0-1 notes (reorientation + Kotlin reactivation)"
```

---

### Task 3: Phase 2 notes (Jetpack Compose)

**Files:** Create 7 notes —
`2.1 The Compose mental model.md`, `2.2 State and state hoisting.md`, `2.3 Layouts and modifiers.md`, `2.4 Lazy lists.md`, `2.5 Material 3 and theming.md`, `2.6 Navigation in Compose.md`, `2.7 Side effects and lifecycle.md`

**Interfaces:**
- Consumes: `1.4`/`1.5` (coroutines/Flow) for side-effects chapter; titles from Task 1.
- Produces: the capstone UI-shell milestone (lands on `2.7`), consumed by Task 4.

- [ ] **Step 1: Write all 7 notes** from CHAPTER BRIEFS P2. Each contrasts the Compose way vs the learner's XML past where relevant (esp. `2.3` vs ConstraintLayout, `2.4` vs RecyclerView, `2.6` vs Fragments/nav graph). Add the 🏗️ **Capstone step** to `2.7`: build the tracker UI shell (habit-list screen + add-habit screen + navigation, static data). Wire prev/next links (1.5↔2.1↔…↔2.7↔3.1).
- [ ] **Step 2: Verify** checklist (as Task 2) + capstone step present on `2.7` and matches `capstone.md` milestone.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 2 notes (Jetpack Compose)"
```

---

### Task 4: Phase 3 notes (Modern architecture)

**Files:** Create 4 notes —
`3.1 Architecture overview.md`, `3.2 ViewModel and UI state.md`, `3.3 Dependency injection with Hilt.md`, `3.4 Repository pattern and data layer.md`

**Interfaces:**
- Consumes: `1.5` (StateFlow) for `3.2`; capstone UI shell from `2.7`.
- Produces: capstone "reactive ViewModel + Hilt" milestone (on `3.4`).

- [ ] **Step 1: Write all 4 notes** from CHAPTER BRIEFS P3. `3.2` explicitly contrasts modern StateFlow UI-state vs the learner's LiveData past. `3.3` frames Hilt as what replaced manual DI/raw Dagger (keep to Hilt basics; advanced DI is Module 2). 🏗️ Capstone step on `3.4`: wire tracker to a ViewModel + Hilt, UI now reactive. Links (2.7↔3.1↔…↔3.4↔4.1).
- [ ] **Step 2: Verify** checklist + capstone step on `3.4`.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 3 notes (modern architecture)"
```

---

### Task 5: Phase 4 notes (Data & persistence)

**Files:** Create 4 notes —
`4.1 Room.md`, `4.2 DataStore.md`, `4.3 Networking.md`, `4.4 Offline-first and caching.md`

**Interfaces:**
- Consumes: ViewModel/repository from Phase 3.
- Produces: capstone "persistence" milestone (on `4.4`).

- [ ] **Step 1: Write all 4 notes** from CHAPTER BRIEFS P4. `4.1` Room with Flow+coroutines; `4.2` DataStore as the SharedPreferences replacement; `4.3` Retrofit+coroutines+kotlinx.serialization (refresh+modernize); `4.4` single-source-of-truth, mention Paging 3 as "reach for when". 🏗️ Capstone step on `4.4`: habits persist in Room, settings in DataStore, optional daily-quote networking feature. Links (3.4↔4.1↔…↔4.4↔5.1).
- [ ] **Step 2: Verify** checklist + capstone step on `4.4`.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 4 notes (data & persistence)"
```

---

### Task 6: Phase 5 notes (Platform capabilities)

**Files:** Create 4 notes —
`5.1 Runtime permissions.md`, `5.2 WorkManager.md`, `5.3 Notifications.md`, `5.4 Lifecycle, process death and edge-to-edge.md`

**Interfaces:**
- Consumes: persistence + ViewModel from Phases 3–4.
- Produces: capstone "reminders" milestone (on `5.3`).

- [ ] **Step 1: Write all 4 notes** from CHAPTER BRIEFS P5. Emphasize Android 13+ notification permission across `5.1`/`5.3`; `5.4` covers `rememberSaveable`/`SavedStateHandle`, process death, edge-to-edge, predictive back. 🏗️ Capstone step on `5.3`: schedule habit reminders via WorkManager + post notifications (with the 13+ permission flow). Links (4.4↔5.1↔…↔5.4↔6.1).
- [ ] **Step 2: Verify** checklist + capstone step on `5.3`.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 5 notes (platform capabilities)"
```

---

### Task 7: Phase 6 notes (Quality)

**Files:** Create 3 notes —
`6.1 Testing.md`, `6.2 Performance.md`, `6.3 Accessibility and adaptive UI.md`

**Interfaces:**
- Consumes: full capstone app from Phases 2–5.
- Produces: capstone "test suite" milestone (on `6.1`).

- [ ] **Step 1: Write all 3 notes** from CHAPTER BRIEFS P6. `6.1` unit tests + coroutine/Flow testing (Turbine) + Compose UI tests + fakes; `6.2` recomposition debugging, stability, Baseline Profiles; `6.3` content descriptions, dynamic type, large-screen/adaptive. 🏗️ Capstone step on `6.1`: tests for the tracker ViewModel + one screen. Links (5.4↔6.1↔…↔6.3↔7.1).
- [ ] **Step 2: Verify** checklist + capstone step on `6.1`.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 6 notes (quality)"
```

---

### Task 8: Phase 7 notes (Ship it)

**Files:** Create 2 notes —
`7.1 Build and release.md`, `7.2 Play Store launch.md`

**Interfaces:**
- Consumes: complete tested capstone.
- Produces: capstone "signed release" milestone (on `7.2`); `7.2` is the final chapter (next → `[[learn-android]]`).

- [ ] **Step 1: Write both notes** from CHAPTER BRIEFS P7. `7.1` version catalogs, build variants, signing, R8/minification, app bundles; `7.2` store listing, data-safety form, release tracks, Play vitals. 🏗️ Capstone step on `7.2`: produce a signed release build, upload-ready. Links (6.3↔7.1↔7.2↔back to `[[learn-android]]`).
- [ ] **Step 2: Verify** checklist + capstone step on `7.2`.
- [ ] **Step 3: Commit**

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: Phase 7 notes (ship it)"
```

---

### Task 9: Cross-link audit + MOC reconciliation

**Files:** Modify any note with a broken/missing link; `learn-android.md` if any title drifted.

- [ ] **Step 1: Audit links.** Confirm every chapter has resolving prev/next; the chain is unbroken 0.1→7.2; MOC links every one of the 30 chapters by exact filename; `capstone.md` milestone chapters match the notes that actually carry a 🏗️ step (2.7, 3.4, 4.4, 5.3, 6.1, 7.2).
  - Run: `ls brain/20-projects/learn-android/*.md | wc -l` → expect 32.
  - Run: `grep -rl "Capstone step" brain/20-projects/learn-android/ | wc -l` → expect 6.
- [ ] **Step 2: Fix** any drift inline.
- [ ] **Step 3: Commit** (skip if nothing changed)

```bash
git add brain/20-projects/learn-android/
git commit -m "learn-android: cross-link audit + MOC reconciliation"
```

---

## CHAPTER BRIEFS

Each brief = the digest's key points + the drill spec + (if any) capstone step. The executor turns these into prose using the skeleton. Keep digests short; drills concrete and hand-coded.

### Phase 0 — Reorientation

**0.1 Modern Android landscape** — *Digest:* the 2021→2026 map. Compose is now the default UI toolkit (XML/Views are legacy); Kotlin-first; coroutines+Flow replaced AsyncTask/RxJava and largely LiveData; Material 3 / Material You; Gradle **version catalogs** + Kotlin DSL; Android 13–15 shifts (runtime notification permission, edge-to-edge default, predictive back, stricter background/foreground-service rules). *What changed since 2021:* bullet the deltas. *Drill:* install latest Android Studio; create a new **Empty Compose Activity** project; open and annotate every generated file (`MainActivity`, `build.gradle.kts`, `libs.versions.toml`, theme files) noting what's new vs 2021 memory. *Success:* app runs on emulator; learner has written notes mapping each file's purpose.

### Phase 1 — Kotlin reactivation (drills in Kotlin Playground / scratch `.kt`)

**1.1 Kotlin syntax refresher** — *Digest:* vals/vars, function syntax, expression bodies, null safety (`?`, `?:`, `!!`, safe calls, `let`), string templates, `if`/`when` as expressions, smart casts. *Drill:* re-implement 6–8 tiny functions (e.g., safe parse-int-or-default, FizzBuzz with `when`, null-safe string join) from a provided signature list. *Success:* all compile and produce stated outputs; no `!!` used.

**1.2 Collections & functional style** — *Digest:* `List`/`Set`/`Map`, `map`/`filter`/`reduce`/`fold`/`groupBy`/`associate`, `sequence` for laziness, lambdas, higher-order functions, trailing-lambda syntax. *Drill:* given a list of sample "transactions" (data in the prompt), compute aggregates (total by category, top N, running balance) using only functional operators — no manual loops. *Success:* outputs match; zero `for` loops.

**1.3 Classes the Kotlin way** — *Digest:* `data class` (+`copy`/destructuring), `sealed class`/`sealed interface` + exhaustive `when`, `enum`, `object`/`companion object`, extension functions, interface delegation (`by`), scope functions (`apply`/`with`/`run`/`also`/`let`) and when to use each. *Drill:* model a small domain (e.g., a `Result`-like `sealed interface` with Success/Error/Loading + a `data class` entity), write an exhaustive `when`, add one extension function. *Success:* `when` compiles without `else`; uses at least one scope function appropriately.

**1.4 Coroutines fundamentals** — *Digest:* `suspend`, structured concurrency, `CoroutineScope`, `launch` vs `async`/`await`, `Dispatchers` (Main/IO/Default), `withContext`, cancellation/`Job`. Contrast with old `AsyncTask`/threads. *Drill:* in a `main` with `runBlocking`, run two suspend functions concurrently with `async`, combine results; then convert a fake callback-style API to `suspend` via `suspendCoroutine`. *Success:* concurrent path is measurably faster than sequential; cancellation prints expected output.

**1.5 Flow and StateFlow** — *Digest:* cold `Flow` vs hot `StateFlow`/`SharedFlow`, builders (`flow{}`, `flowOf`, `asFlow`), operators (`map`/`filter`/`debounce`/`combine`), `collect`, `MutableStateFlow` as observable state. This is what replaced LiveData. *Drill:* build a `flow` emitting a sequence with a delay, transform it with 2 operators and collect; then model a tiny counter with `MutableStateFlow` + a function that updates `.value`. *Success:* transformed values correct; StateFlow always exposes current value to a late collector. *Link:* note this replaces the learner's LiveData usage.

### Phase 2 — Jetpack Compose

**2.1 The Compose mental model** — *Digest:* declarative vs imperative XML; `@Composable` functions; recomposition (UI = f(state)); previews. *Drill:* recreate a simple screen the learner remembers from XML (e.g., a profile card: image + name + button) as composables; add `@Preview`. *Success:* preview renders; no XML.

**2.2 State and state hoisting** — *Digest:* `remember`, `mutableStateOf`, `rememberSaveable`, unidirectional data flow, **state hoisting** (stateless composable + state owner). *Drill:* build a stateful counter, then refactor to a **stateless** `Counter(count, onIncrement)` with state hoisted to the caller. *Success:* stateless composable has no internal state; caller owns it.

**2.3 Layouts and modifiers** — *Digest:* `Column`/`Row`/`Box`, arrangement/alignment, the `Modifier` chain (padding/size/weight/clickable/background) and order-sensitivity; contrast with ConstraintLayout/XML. *Drill:* reproduce a given mockup (a settings row: leading icon, title+subtitle column, trailing switch) using only layouts + modifiers; do a second version where modifier order visibly changes the result. *Success:* matches mockup; learner can explain the order effect.

**2.4 Lazy lists** — *Digest:* `LazyColumn`/`LazyRow`/`LazyVerticalGrid`, `items`, `key`, content padding; this fully replaces RecyclerView+Adapter+ViewHolder. *Drill:* render a scrollable list of 50 generated items with a custom item composable and stable `key`s. *Success:* smooth scroll; no adapter class anywhere; keys set.

**2.5 Material 3 and theming** — *Digest:* Material 3 components, `MaterialTheme` (color scheme/typography/shape), dynamic color (Material You), dark theme. *Drill:* define a custom color scheme + one typography style; toggle light/dark; apply dynamic color on Android 12+. *Success:* app restyles in dark mode; dynamic color visibly active on a 12+ emulator.

**2.6 Navigation in Compose** — *Digest:* `NavController`, `NavHost`, routes, passing args (incl. type-safe navigation), back stack; contrast with Fragments + XML nav graph. *Drill:* two screens (list → detail) with an argument passed to detail and back navigation. *Success:* arg arrives correctly; back stack behaves.

**2.7 Side effects and lifecycle** — *Digest:* `LaunchedEffect`, `rememberCoroutineScope`, `DisposableEffect`, `collectAsStateWithLifecycle`, lifecycle-aware collection. *Drill:* collect a `StateFlow` with `collectAsStateWithLifecycle`; use `LaunchedEffect(key)` to run a one-shot on first composition / key change. *Success:* effect runs exactly when expected; UI updates from flow. **🏗️ Capstone step:** build the habit-tracker **UI shell** — habit-list screen + add-habit screen + navigation between them, backed by static in-memory data (no persistence yet).

### Phase 3 — Modern architecture

**3.1 Architecture overview** — *Digest:* Google's recommended layering — UI layer (state holders), domain (optional), data layer (repositories); unidirectional data flow; separation of concerns. *Drill:* on paper/Markdown, diagram how the capstone's "add habit" flows through the layers; list which class owns what. *Success:* diagram names UI→ViewModel→Repository→data source with arrows and responsibilities.

**3.2 ViewModel and UI state** — *Digest:* `ViewModel`, exposing a single immutable **UI state** via `StateFlow`, handling events/actions, `viewModelScope`; contrast LiveData past. *Drill:* write a `ViewModel` exposing `StateFlow<UiState>` and an `onEvent()`; update state immutably with `.update{ copy(...) }`. *Success:* state is a single immutable data class; UI would render purely from it.

**3.3 Dependency injection with Hilt** — *Digest:* why DI; Hilt basics — `@HiltAndroidApp`, `@AndroidEntryPoint`, `@Inject`, `@Module`/`@Provides`, `@HiltViewModel`; what it replaced (manual DI / raw Dagger). (Advanced DI = Module 2.) *Drill:* set up Hilt in the project, provide a repository interface→impl binding, inject it into the ViewModel. *Success:* app builds with Hilt; ViewModel receives the repo without manual construction.

**3.4 Repository pattern and data layer** — *Digest:* repository as single source of truth, interface + implementation, exposing `Flow`, mapping data→domain models. *Drill:* define `HabitRepository` interface + an in-memory impl returning `Flow<List<Habit>>`; back the ViewModel with it. *Success:* ViewModel depends only on the interface; list updates reactively. **🏗️ Capstone step:** wire the tracker to a real **ViewModel + Hilt + repository**; the UI is now reactive (still in-memory data).

### Phase 4 — Data & persistence

**4.1 Room** — *Digest:* `@Entity`/`@Dao`/`@Database`, queries returning `Flow`, suspend inserts/updates, migrations basics, type converters; what's improved since 2021. *Drill:* add a `HabitEntity` + DAO (`getAll(): Flow`, `insert(suspend)`), build the database, swap the repository's in-memory source for Room. *Success:* habits survive app restart; DAO reads return Flow.

**4.2 DataStore** — *Digest:* Preferences DataStore (and Proto briefly) as the typed, coroutine/Flow replacement for SharedPreferences; reading/writing keys as Flow. *Drill:* store a user setting (e.g., "week starts on Monday" or theme choice) in Preferences DataStore; read it as Flow into state. *Success:* setting persists; no SharedPreferences used.

**4.3 Networking** — *Digest:* Retrofit + coroutine `suspend` endpoints + `kotlinx.serialization` converter + OkHttp logging; modeling responses; error handling with `try/catch`/`Result`. Refresh + modernize vs 2021 Gson/callbacks. *Drill:* call a free public JSON API (e.g., a quotes API) with a `suspend` Retrofit endpoint + `@Serializable` model; surface result/error in state. *Success:* real data renders; errors handled without crash.

**4.4 Offline-first and caching** — *Digest:* single-source-of-truth pattern (UI observes the DB; network writes to DB), cache invalidation basics, when to reach for Paging 3 (large remote lists — not needed here). *Drill:* design (in Markdown) the offline-first flow for the optional quote feature: network → Room → UI; list failure handling. *Success:* design shows DB as the single source the UI observes. **🏗️ Capstone step:** habits persist in **Room**; settings in **DataStore**; optional **daily-quote** feature exercises networking offline-first.

### Phase 5 — Platform capabilities

**5.1 Runtime permissions** — *Digest:* runtime permission model, requesting via Activity Result APIs / Accompanist-style Compose helpers, rationale UI, Android 13 `POST_NOTIFICATIONS`. *Drill:* implement a permission request flow for notifications with rationale handling. *Success:* grant/deny both handled; no crash on denial.

**5.2 WorkManager** — *Digest:* deferrable guaranteed background work, `Worker`/`CoroutineWorker`, constraints, one-time vs periodic, uniqueness. *Drill:* schedule a periodic `CoroutineWorker` that logs/produces output; verify it runs under constraints. *Success:* work executes on schedule; survives app kill.

**5.3 Notifications** — *Digest:* notification channels, building/posting notifications, scheduling (WorkManager/AlarmManager tradeoff), the 13+ permission gate, deep-linking into the app. *Drill:* post a notification on a channel that deep-links to a screen. *Success:* notification shows; tapping opens the right screen. **🏗️ Capstone step:** schedule **habit reminders** via WorkManager and post **notifications** (with the 13+ permission flow).

**5.4 Lifecycle, process death and edge-to-edge** — *Digest:* activity/composition lifecycle, **process death** & state restoration (`rememberSaveable`, `SavedStateHandle` in ViewModel), configuration changes, edge-to-edge (default in 15) + insets, predictive back. *Drill:* persist a transient UI value across process death using `SavedStateHandle`; make one screen draw edge-to-edge with proper insets. *Success:* value restored after simulated process death; content not hidden behind system bars.

### Phase 6 — Quality

**6.1 Testing** — *Digest:* unit tests (JUnit), testing coroutines (`runTest`, test dispatchers), testing Flow with **Turbine**, fakes over mocks, Compose UI tests (`createComposeRule`, finders/actions/assertions). *Drill:* write (a) a unit test for the ViewModel's state logic using a fake repository + `runTest`/Turbine, and (b) one Compose UI test asserting a screen renders a habit. *Success:* both tests pass; ViewModel test uses a fake, not the real DB. **🏗️ Capstone step:** add a **test suite** for the tracker's ViewModel + one screen.

**6.2 Performance** — *Digest:* recomposition causes & debugging (Layout Inspector recomposition counts), stability/`@Stable`/immutable types, `derivedStateOf`, lazy-list keys, **Baseline Profiles** for startup. *Drill:* find an over-recomposing composable (introduce one), fix it via stable params/keys, confirm fewer recompositions. *Success:* recomposition count drops after the fix; learner can explain why.

**6.3 Accessibility and adaptive UI** — *Digest:* content descriptions/semantics, touch target sizes, dynamic type/font scaling, large-screen & foldable adaptive layouts (window size classes). *Drill:* add semantics/content descriptions to the tracker, test with TalkBack + large font scale; make one screen adapt to a wide window size class. *Success:* TalkBack reads meaningful labels; layout adapts at the expanded width class.

### Phase 7 — Ship it

**7.1 Build and release** — *Digest:* Gradle **version catalogs** (`libs.versions.toml`), build types/flavors, signing config (keystore), `minifyEnabled`/R8 + keep rules, **app bundle (.aab)** vs APK. *Drill:* create a signed release build config + generate a signed `.aab`; enable R8 and confirm the release build runs. *Success:* signed `.aab` produced; minified release app runs without missing-class crashes.

**7.2 Play Store launch** — *Digest:* Play Console basics, store listing assets, **Data safety** form, content rating, release tracks (internal/closed/open/production), staged rollout, **Play vitals** post-launch. *Drill:* draft the tracker's store listing + complete a mock Data safety questionnaire + outline a rollout plan. *Success:* listing draft + data-safety answers + rollout steps written. **🏗️ Capstone step:** produce the final **signed release build, upload-ready**, with listing prepared. *(Final chapter — Next links back to `[[learn-android]]`.)*

---

## Self-Review

**Spec coverage:** all 30 chapters across 8 phases mapped to Tasks 2–8; MOC + capstone in Task 1; Module 2 stub in Task 1 (incl. advanced DI addition); cross-link/MOC integrity in Task 9. Non-goals enforced via Global Constraints (modern-only, no solution code, no KMP, no in-note tracking). ✓

**Placeholder scan:** no TBD/TODO; each chapter brief carries concrete digest points + a concrete drill + explicit success criteria; capstone steps named on exactly 6 chapters (2.7, 3.4, 4.4, 5.3, 6.1, 7.2). ✓

**Type/title consistency:** chapter filenames in task Files lists match the CHAPTER BRIEFS titles and the MOC link targets; milestone chapters consistent between `capstone.md` (Task 1), the per-phase capstone steps, and the Task 9 audit (`grep "Capstone step"` expects 6). ✓
