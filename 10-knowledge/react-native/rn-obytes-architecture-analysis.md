---
title: Obytes React Native Architecture Analysis
aliases: [Obytes template analysis, Obytes RN architecture]
type: knowledge
project: global
status: evergreen
created: 2026-07-07
updated: 2026-07-07
tags: [react-native, architecture, obytes, expo, analysis]
source: https://github.com/obytes/react-native-template-obytes
---

# Obytes React Native Architecture Analysis

> [!abstract] TL;DR
> Deep dissection of the [Obytes React Native Template](https://github.com/obytes/react-native-template-obytes) (2.3k+ stars). It uses a **4-zone architecture** (routes → features → components → lib) with strict one-way dependencies, Expo Router file-based routing, React Query for server state, Zustand for client state, and NativeWind for styling.

## The 4-Zone Model

The codebase splits into exactly 4 zones with strict dependency rules:

| Zone | Purpose | Can Import From |
|:---|:---|:---|
| `app/` | Routing shell — thin re-exports | features, components, lib |
| `features/` | Business logic — screens, hooks, feature API | components, lib |
| `components/ui/` | Design system — headless UI primitives | lib |
| `lib/` | Core infra — API client, auth, storage, i18n | Nothing internal |

**Critical rule**: Features never import from other features.

## Zone A: `app/` — The Routing Shell

Route files are **1-line re-exports** with zero logic:

```tsx
// app/(app)/index.tsx
export { FeedScreen as default } from '@/features/feed/feed-screen';
```

The `(app)/` directory is an Expo Router **route group** that creates an auth boundary. The layout uses Zustand selectors to guard:

```tsx
const status = useAuth.use.status();
if (status === 'signOut') return <Redirect href="/login" />;
```

## Zone B: `features/` — Self-Contained Vertical Slices

Each feature owns its screens, components, API hooks, and state:

```
features/auth/
├── login-screen.tsx
├── use-auth-store.tsx       # Zustand store
└── components/
    ├── login-form.tsx
    └── login-form.test.tsx   # Colocated test
```

Adding a new feature = create the folder + add a route file. No wiring elsewhere.

## Zone C: `components/ui/` — Design System

Uses **`tailwind-variants`** for type-safe, slot-based component variants:

```tsx
const button = tv({
  slots: {
    container: 'my-2 flex flex-row items-center justify-center rounded-md px-4',
    label: 'font-inter text-base font-semibold',
  },
  variants: {
    variant: {
      default: { container: 'bg-black dark:bg-white', label: 'text-white dark:text-black' },
      secondary: { container: 'bg-primary-600' },
      outline: { container: 'border border-neutral-400' },
      destructive: { container: 'bg-red-600' },
    },
  },
});
```

Feature developers use typed variants (`<Button variant="secondary" />`), never raw Tailwind.

## Zone D: `lib/` — Core Infrastructure

### Zustand Auth Store with `createSelectors`

```tsx
const _useAuthStore = create<AuthState>((set, get) => ({
  status: 'idle',
  token: null,
  signIn: (token) => { setToken(token); set({ status: 'signIn', token }); },
  signOut: () => { removeToken(); set({ status: 'signOut', token: null }); },
  hydrate: () => { /* load from storage */ },
}));

export const useAuthStore = createSelectors(_useAuthStore);
```

`createSelectors` auto-generates field-level selectors (`useAuthStore.use.status()`) so components only re-render when their specific field changes.

### Zod-Validated Environment

```tsx
const envSchema = z.object({
  EXPO_PUBLIC_APP_ENV: z.enum(['development', 'preview', 'production']),
  EXPO_PUBLIC_API_URL: z.string().url(),
});
```

Crashes at startup if env vars are wrong — not silently in production.

## 7 Scalability Patterns

1. **Feature isolation** — no cross-feature imports
2. **Thin route layer** — route files are 1-line re-exports
3. **Server/client state split** — React Query for API, Zustand for local only
4. **Atomic selectors** — `createSelectors()` prevents mass re-renders
5. **Zod-validated env** — fail fast at startup
6. **Design system as boundary** — feature devs use typed variants, not raw styles
7. **CI as enforcement** — lint, type-check, tests on every PR

## Identified Gaps

- No cross-feature communication pattern (when features need shared data)
- No dependency injection for the API client (harder to mock in tests)
- No offline-first capability (no `persistQueryClient`, no local DB)
- Auth uses string status enums instead of state machines (XState)
- No global error boundary beyond Expo Router's built-in

## Technology Stack

| Layer | Technology |
|---|---|
| Framework | Expo (managed) |
| Routing | Expo Router (file-based) |
| Server state | TanStack Query (React Query) |
| Client state | Zustand |
| Styling | NativeWind + tailwind-variants |
| Forms | react-hook-form + Zod |
| E2E testing | Maestro |
| Unit testing | Jest + React Native Testing Library |
| CI/CD | GitHub Actions + EAS Build |
| Package manager | pnpm |

## Related

- [[rn-scalable-paradigm]]
- [[rn-best-practices]]
- [[rn-architecture-repo-comparison]]

## Sources

- [Obytes GitHub repo](https://github.com/obytes/react-native-template-obytes)
- [Obytes official docs](https://starter.obytes.com)
