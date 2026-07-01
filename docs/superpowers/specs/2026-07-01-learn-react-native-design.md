# Spec: Learning Path Design - React Native + Expo

## Objective
Create a structured learning path for React Native + Expo inside the Obsidian vault (`/home/reky/workspace/brain/20-projects/learn-react-native/`), modeled after `learn-android`. 

This path is tailored for a developer with a strong web-React background who needs to learn modern mobile development best practices (declarative layouts, navigation, local persistence, background tasks, device APIs, and cloud building/OTA updates with EAS).

---

## Architectural Mapping (Web/Kotlin -> React Native + Expo)

We map familiar Web/Android concepts to React Native + Expo to accelerate understanding:

| Android Concept | Web/React Concept | React Native + Expo Equivalent |
| :--- | :--- | :--- |
| Jetpack Compose | React DOM | **React Native core views & components** |
| Kotlin Coroutines & Flow | Promises & RxJS | **`async/await` + TanStack Query** (React Query) |
| Jetpack Navigation | React Router | **Expo Router** (File-based navigation) |
| ViewModel | Custom React Hooks | **Custom Hooks pattern** (e.g., `useTodoViewModel`) |
| Room (SQLite DB) | Dexie / IndexedDB | **Expo SQLite + Drizzle ORM** |
| DataStore (Key-Value) | LocalStorage | **MMKV** (`react-native-mmkv`) |
| WorkManager | Service Worker | **Expo TaskManager + BackgroundFetch** |

---

## Target Structure inside the Vault

The notes will be written to `/home/reky/workspace/brain/20-projects/learn-react-native/`:

1. **`learn-react-native.md`** (MOC / Index): Maps all modules, phases, and links to each chapter note.
2. **`capstone.md`** (Milestone Tracker): Outlines the capstone project milestones associated with the phases.
3. **Chapter Notes**:
   - `0.1 Modern React Native and Expo landscape.md`
   - `1.1 Core Components and Layouts.md`
   - `1.2 Interaction and Gestures.md`
   - `2.1 App Structure and Stacks.md`
   - `2.2 Tabs and Nested Navigators.md`
   - `2.3 Dynamic Routing and Params.md`
   - `3.1 Architecture and Hooks Pattern.md`
   - `3.2 Server State with TanStack Query.md`
   - `3.3 Dependency Injection via React Context.md`
   - `4.1 High-Performance Key-Value Storage.md`
   - `4.2 Relational Storage with SQLite and Drizzle.md`
   - `4.3 Offline-First Architecture.md`
   - `5.1 System Permissions and App Lifecycle.md`
   - `5.2 Notifications and Background Tasks.md`
   - `6.1 Testing React Native Components.md`
   - `6.2 Performance Tuning.md`
   - `7.1 EAS Build and Dev Clients.md`
   - `7.2 EAS Submit and Expo Updates.md`

---

## Chapter Template

Each chapter note uses the following structured components:
- **YAML Frontmatter**: Specifies `title`, `aliases`, `type: knowledge`, `project: learn-react-native`, `status: active`, tags, and creation dates.
- **Abstract/TL;DR**: High-level summary of the chapter.
- **Digest**: Detailed conceptual explanation mapping web React concepts to native, explaining mobile-specific trade-offs and best practices.
- **Drill**: An offline, hands-on coding task (without AI help) that the user must perform to solidify the knowledge. Includes explicit **Success criteria** checklist.
- **Related**: Links to MOC, previous chapter, and next chapter.

---

## The Capstone Project: "Offline-First Syncable Habit Tracker"
To put these phases into practice, the user will build a Single Codebase mobile application incrementally:

- **Milestone 1 (Phase 2.3)**: UI shell with Bottom Tabs (Home, Habits, Settings) and Stacks (Habit Detail), fully typed with Expo Router.
- **Milestone 2 (Phase 3.3)**: Wire up fake networking with TanStack Query, separating UI layers from custom hooks (acting as ViewModels), injected via React Context.
- **Milestone 3 (Phase 4.3)**: Replace fake store with MMKV for settings/auth and SQLite + Drizzle for habits storage. Support offline updates (queueing mutations when offline and syncing back when online).
- **Milestone 4 (Phase 5.2)**: Request notification permissions and register a background fetch task that triggers a local notification reminder if the habits for the day haven't been completed.
- **Milestone 5 (Phase 7.2)**: Setup EAS configuration, build a dev client, install on emulator/device, publish a minor visual update via Expo OTA updates.

---

## Next Steps
Upon restart, we will:
1. Run a script or batch process to create all the note files with their respective content.
2. Review the resulting notes in the Obsidian Graph.
