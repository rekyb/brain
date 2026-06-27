---
title: Compound Components
aliases: [Compound Component Pattern, Composable Components]
tags: [ux, design-systems, components, react, reference]
created: 2026-06-27
status: evergreen
source: "React docs + Kent C. Dodds"
---

# 🧬 Compound Components

> [!abstract] TL;DR
> A **compound component** is a set of components designed to work *together* as one unit, sharing implicit state — like `<Select>` + `<Select.Option>`. It gives a flexible, expressive API where the *parent* manages state and the *children* handle layout. Modern systems pair this with [[Atomic Design]] and [[Design Tokens]] to handle the **cross-cutting behavior** the strict atomic hierarchy struggles with.

Part of the [[Design Systems]] cluster. Referenced as a "modern take" in the [[Atomic Design]] pitfalls.

## The problem it solves

A single mega-component with 20 props ("prop explosion") becomes rigid and hard to read:

```jsx
// ❌ rigid: every option is a prop
<Tabs tabs={[{label:'A', content:..., disabled:true}, ...]} />
```

Compound components flip control to the consumer via composition:

```jsx
// ✅ flexible: structure is expressed in JSX
<Tabs defaultValue="a">
  <Tabs.List>
    <Tabs.Trigger value="a">Account</Tabs.Trigger>
    <Tabs.Trigger value="b">Billing</Tabs.Trigger>
  </Tabs.List>
  <Tabs.Panel value="a">…</Tabs.Panel>
  <Tabs.Panel value="b">…</Tabs.Panel>
</Tabs>
```

The parent `Tabs` holds the active state (often via React Context); children read it implicitly. The consumer controls **order, layout, and which parts to include** without new props.

## Why it fits design systems

- **Composability over configuration** — mirrors the atomic idea of combining small parts.
- **A home for cross-cutting behavior** — shared state (open/closed, selected) lives in the parent, not scattered.
- **Cleaner public API** — components read like the UI they produce.

## When to use vs. not

- **Use** for components with *related sub-parts + shared state*: tabs, accordions, menus, selects, dialogs, forms.
- **Skip** for truly atomic parts (a `Button`, an `Icon`) — composition adds nothing there.

> [!tip] Trade-off
> More flexibility = more ways to misuse it. Pair with sensible defaults and good docs/[[Patterns]] so consumers assemble the parts correctly.

## Related

- Extends → [[Component Library]] · complements → [[Atomic Design]] · [[Patterns]]
- Styled by → [[Design Tokens]] · part of → [[Design Systems]]

## Sources

- React docs — [Passing data deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)
- Kent C. Dodds — [The Compound Components pattern](https://kentcdodds.com/blog/compound-components-with-react-hooks)
