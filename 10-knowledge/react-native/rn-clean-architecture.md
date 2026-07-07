---
title: React Native Clean Architecture
aliases: [RN clean architecture, clean arch RN]
type: knowledge
project: global
status: evergreen
created: 2026-07-07
updated: 2026-07-07
tags: [react-native, architecture, clean-architecture, design-patterns]
source: community-consensus
---

# React Native Clean Architecture

> [!abstract] TL;DR
> Clean Architecture in React Native separates the app into concentric layers (Domain → Application → Infrastructure → Presentation) where dependencies point inward. The inner layers are pure TypeScript with no React/RN imports. Most production teams find this too ceremonial and use a pragmatic [[rn-scalable-paradigm]] instead, but the concepts still apply.

## The Layers

| Layer | Contains | Depends On |
|---|---|---|
| **Domain** | Entities, Repository interfaces, Value objects | Nothing (pure TS) |
| **Application** | Use cases, DTOs, Port interfaces | Domain only |
| **Infrastructure** | API clients, DB, Storage, Native modules | Domain + Application |
| **Presentation** | Screens, Components, Hooks, Navigation | Application (via hooks) |

## Folder Structure

```
src/
├── domain/                    # Pure business logic — NO React
│   ├── entities/User.ts
│   ├── repositories/IUserRepository.ts
│   └── value-objects/Email.ts
├── application/               # Use cases — orchestrate domain logic
│   ├── use-cases/LoginUseCase.ts
│   └── dtos/LoginDTO.ts
├── infrastructure/            # External world — implements interfaces
│   ├── api/AuthApiService.ts
│   ├── storage/SecureTokenStorage.ts
│   └── di/container.ts        # Dependency injection
├── presentation/              # React Native UI
│   ├── screens/LoginScreen.tsx
│   ├── hooks/useLogin.ts
│   └── providers/DIProvider.tsx
```

## Key Rules

1. **Domain and Application layers have zero React Native imports.** You should be able to copy them to a Node.js backend.
2. **Use cases receive interfaces via constructor** (dependency injection). They never import Axios, AsyncStorage, etc.
3. **Infrastructure implements domain interfaces** — e.g., `AuthApiService implements IAuthService`.
4. **DTO ↔ Entity mapping** happens at the infrastructure boundary.

## Pragmatic Tips

- Don't over-engineer small features — a simple CRUD screen doesn't need a use case class.
- Use cases = 1 action — `LoginUseCase`, not `UserUseCase` with 10 methods.
- Skip DI frameworks — a simple container object is enough for most RN apps.
- The golden rule: if you're importing `react` in domain/ or application/, something is wrong.

## Why Most Teams Don't Use Full Clean Architecture

React's compositional model (hooks calling hooks) already provides separation of concerns without OOP boilerplate. The [[rn-scalable-paradigm]] captures Clean Architecture concepts using React's native idioms.

## Related

- [[rn-scalable-paradigm]]
- [[rn-best-practices]]

## Sources

- Robert C. Martin, "Clean Architecture" (2017)
- Community adaptations for React Native
