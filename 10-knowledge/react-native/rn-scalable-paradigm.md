---
title: React Native Scalable Paradigm
aliases: [RN scalable paradigm, feature-sliced hooks]
type: knowledge
project: global
status: evergreen
created: 2026-07-07
updated: 2026-07-07
tags: [react-native, architecture, hooks, state-management]
source: community-consensus
---

# React Native Scalable Paradigm

> [!abstract] TL;DR
> The dominant pattern in production React Native apps is a **feature-based architecture** driven by **custom hooks as the orchestration layer**, with React Query for server state and Zustand for client state. Not full Clean Architecture — a pragmatic 3-layer model.

## The Core Idea: 3 Layers, Not 5

| Layer | Responsibility | Tool |
|---|---|---|
| **Screen** | Render UI, capture user intent | React Native components |
| **Hook** | Orchestrate logic, transform data | Custom hooks |
| **Data** | Fetch, cache, persist | React Query + Zustand |

Custom hooks ARE your use cases. Features ARE your boundaries.

## Folder Structure

```
src/
├── features/           # Each feature is a self-contained vertical slice
│   ├── auth/
│   │   ├── api/        # React Query queries/mutations
│   │   ├── hooks/      # Business logic lives HERE
│   │   ├── components/ # UI specific to this feature
│   │   ├── screens/    # Screen components
│   │   ├── types.ts
│   │   └── index.ts    # Public API
│   ├── profile/
│   └── settings/
├── shared/             # Truly shared, cross-feature code
│   ├── api/            # API client setup
│   ├── components/     # Design system
│   ├── hooks/          # Generic hooks
│   └── utils/
├── navigation/
├── providers/
└── App.tsx
```

## State Management Split

- **Server state** → React Query (caching, refetching, pagination, optimistic updates)
- **Client global state** → Zustand (auth token, theme, onboarding flag)
- **Local UI state** → `useState` / `useReducer`

**Never duplicate server data into Zustand/Redux.** React Query's cache IS your global server state.

## Hooks Compose Hooks

Complex workflows chain simpler hooks:

```typescript
function useCheckout() {
  const cart = useCart();
  const payment = usePayment();
  const order = useCreateOrder();
  
  const checkout = async () => {
    const paymentResult = await payment.process(cart.total);
    await order.mutateAsync({ cartId: cart.id, paymentId: paymentResult.id });
  };
  
  return { checkout, loading: payment.isPending || order.isPending };
}
```

## Scaling Rules

1. **Features don't import from other features directly** — lift shared code to `shared/` or expose through the feature's `index.ts`.
2. **Colocation over separation** — keep tests, types, components, and hooks together inside the feature folder.
3. **Delete a feature folder → zero dead code remains.**

## Companies Using This Pattern

| App / Company | Architecture |
|---|---|
| Shopify (Shop app) | Feature-sliced + React Query |
| Coinbase | Feature modules + Zustand |
| Discord | Feature folders + custom hooks |
| Meta (Instagram) | Feature-based with Relay (GraphQL) |

## Related

- [rn-best-practices](rn-best-practices.md)
- [rn-clean-architecture](rn-clean-architecture.md)
- [rn-obytes-architecture-analysis](rn-obytes-architecture-analysis.md)

## Sources

- React Native community patterns (2024-2025)
- Bulletproof React architecture by alan2207
