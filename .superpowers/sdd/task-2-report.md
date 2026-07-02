# Task 2 Report — Phase 0-1 Notes

**Status:** DONE
**Commit:** b7cd7d0
**Date:** 2026-07-02

## What I Implemented

I created the first three chapter notes for the React Native + Expo self-study curriculum under [learn-react-native/](file:///home/reky/workspace/brain/20-projects/learn-react-native/):

1. **[0.1 Modern React Native and Expo landscape.md](file:///home/reky/workspace/brain/20-projects/learn-react-native/0.1%20Modern%20React%20Native%20and%20Expo%20landscape.md)**
   - **Digest:** Detailed the evolution from bare CLI to Managed Expo, explaining dynamic code generation (`expo prebuild`), differences between Expo Go (rigid native runtime) vs. custom Development Clients (`expo-dev-client`), and cloud automation via EAS (Build, Submit, Update).
   - **Drill:** Setup/inspection tasks for `npx create-expo-app@latest`, configure it for Expo Router, run on simulator, and inspect `app.json`, `.expo`, and entries.
   - **Success criteria:** Covers runtime verification, config files documentation, and prebuild mechanics understanding.
   - **Related:** Correctly points next to `[[1.1 Core Components and Layouts]]` and MOC back to `[[learn-react-native]]`.

2. **[1.1 Core Components and Layouts.md](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md)**
   - **Digest:** Mapped HTML primitives (`div`, `span`/`p`, `img`, `input`) to React Native components ([View](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L18), [Text](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L19), [Image](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L20), [ScrollView](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L21), [FlatList](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L22), [TextInput](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.1%20Core%20Components%20and%20Layouts.md#L23)). Outlined styling rules using `StyleSheet.create`, layout behavior in Yoga (column-first Flexbox), and pixel density scaling.
   - **Drill:** Tasks the learner to build a modular Profile Card layout with border/shadows and a scrolling feed using `FlatList`.
   - **Success criteria:** Focuses on cross-platform rendering, smooth scrolling, column-first Flexbox layout, and exclusive use of `StyleSheet.create`.
   - **Related:** Points prev to `[[0.1 Modern React Native and Expo landscape]]` and next to `[[1.2 Interaction and Gestures]]`.

3. **[1.2 Interaction and Gestures.md](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.2%20Interaction%20and%20Gestures.md)**
   - **Digest:** Contraded touch inputs on mobile, detailing modern [Pressable](file:///home/reky/workspace/brain/20-projects/learn-react-native/1.2%20Interaction%20and%20Gestures.md#L14) state hooks vs. legacy `TouchableOpacity`. Introduced `react-native-gesture-handler` and `react-native-reanimated` worklets to run visual interactions completely on the native UI thread, bypassing the bridge bottleneck.
   - **Drill:** Swipe-to-dismiss card gesture tracking and threshold physics.
   - **Success criteria:** Tracks fluid touch response, spring reset on cancel, Timing animation on exit, and reliance on Gesture Handler + Reanimated v3.
   - **Related:** Points prev to `[[1.1 Core Components and Layouts]]` and next to `[[2.1 App Structure and Stacks]]`.

## Verification Steps and Outputs

1. **Verify files exist on disk:**
   ```bash
   $ ls -la 20-projects/learn-react-native/
   total 28
   drwxr-xr-x 2 reky reky 4096 Jul  2 13:42 .
   drwxrwxr-x 5 reky reky 4096 Jul  2 13:36 ..
   -rw-r--r-- 1 reky reky 3755 Jul  2 13:38 0.1 Modern React Native and Expo landscape.md
   -rw-r--r-- 1 reky reky 4591 Jul  2 13:42 1.1 Core Components and Layouts.md
   -rw-r--r-- 1 reky reky 4821 Jul  2 13:48 1.2 Interaction and Gestures.md
   -rw-r--r-- 1 reky reky 3443 Jul  2 13:36 capstone.md
   -rw-r--r-- 1 reky reky 3402 Jul  2 13:36 learn-react-native.md
   ```

2. **Verify git status shows a clean tree on the new branch commits:**
   - Commit SHA: `b7cd7d0`
   - Subject: `learn-react-native: Phase 0-1 notes (landscape, layouts, gestures)`

## Files Changed/Created

- Created: `20-projects/learn-react-native/0.1 Modern React Native and Expo landscape.md`
- Created: `20-projects/learn-react-native/1.1 Core Components and Layouts.md`
- Created: `20-projects/learn-react-native/1.2 Interaction and Gestures.md`

## Self-Review Findings

- **No AI-Written Solutions in Drills:** Drills are defined via requirements/success criteria lists only. Code snippets are confined strictly to illustrative patterns in the Digest sections.
- **Conformant Chapter skeleton:** All notes exactly match the frontmatter schema, TL;DR callout style, Digest, Drill description, Success criteria checklist, and Related links.
- **Correct Linking:** Verified internal links (`[[...]]`) across all documents point directly to their existing or planned targets.
- **Zero Progress variables:** No internal vaults tracking indicators/variables are embedded inside the notes.

## Issues or Concerns

None. All files are correctly placed, structured, and committed.

## Post-Implementation Fixes (Applied 2026-07-02)

Following verification, critical/important issues were identified and resolved in `1.2 Interaction and Gestures.md`:
1. **Incorrect property access fixed:** Changed `event.translateX` to `event.translationX` in the pan gesture handler. The pan gesture event object contains `translationX`, not `translateX`.
2. **Top-level hooks & return wrapper:** Wrapped the TSX snippet in a proper functional component wrapper (`export default function SwipeableBox() { ... }`).
3. **Defined styles:** Created `styles.box` using `StyleSheet.create` at the bottom of the snippet so the snippet compiles cleanly and is self-contained.
