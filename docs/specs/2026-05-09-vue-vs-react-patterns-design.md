# vue-vs-react-patterns — Design

**Date:** 2026-05-09
**Status:** Draft, awaiting user review
**Owner:** Denys Petryniak

## Why this exists

A side-by-side comparison repo for a Vue developer learning React. Each pattern is a thing that confused the author when crossing over — written down so it doesn't confuse the next person.

The repo's senior signal lives in the *honesty* of each pattern's README: not "Vue is better" or "React is better", but "here's where Vue's design wins, here's where React's design wins, and here's the tradeoff each language is making."

Eight half-finished patterns are worth less than one shipped one. The repo is built **incrementally, one pattern at a time**. Pattern #1 ships before #2 starts.

## Scope

**In scope (initial release — pattern #1 only):**
- Repo bootstrap with top-level README, MIT license, .gitignore, .editorconfig
- Pattern #1: Reactivity model — `ref`/`reactive`/`computed` vs `useState`/`useMemo`
- Two runnable Vite + TypeScript apps inside `patterns/01-reactivity/{vue,react}`
- Per-pattern README with a documented 4-section format
- Top-level README with a status table covering all 6 planned patterns

**Out of scope (initial release):**
- Patterns 02–06 — listed in the README index as planned, but no code yet
- Deployment of pattern apps (Netlify, GitHub Pages, etc.) — deferred until 2+ patterns exist
- Styling library, state management, routing, testing — anything that distracts from the core pattern story
- pnpm workspaces — each pattern stays an independent install

**Explicitly rejected:**
- A single "todo app" that ties patterns together. The 8 focused patterns ARE the artifact; a tying-it-together app dilutes focus and historically kills these projects.
- Pre-writing all 6 patterns before shipping any one of them.

## Pattern #1: Reactivity (live filtered list)

### Demo

A search input filters a small hardcoded list of objects in real time. ~8 items shaped `{ id: number; name: string; category: string }`. No API calls, no async, no styling library.

### Vue version (~25 lines)

```vue
<script setup lang="ts">
import { ref, computed } from 'vue'

const items = [
  { id: 1, name: 'Apple', category: 'fruit' },
  { id: 2, name: 'Carrot', category: 'vegetable' },
  // ~6 more
]
const query = ref('')
const filtered = computed(() =>
  items.filter(i => i.name.toLowerCase().includes(query.value.toLowerCase()))
)
</script>

<template>
  <input v-model="query" placeholder="Search..." />
  <ul>
    <li v-for="item in filtered" :key="item.id">{{ item.name }}</li>
  </ul>
</template>
```

### React version (~25 lines)

```tsx
import { useState, useMemo } from 'react'

const items = [/* same data as Vue side */]

export default function App() {
  const [query, setQuery] = useState('')
  const filtered = useMemo(
    () => items.filter(i => i.name.toLowerCase().includes(query.toLowerCase())),
    [query]
  )
  return (
    <>
      <input
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      <ul>
        {filtered.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </>
  )
}
```

### What the comparison teaches

- **Auto-tracking vs explicit deps.** Vue's `computed` knows when `query` changes because the proxy intercepts reads. React's `useMemo` requires you to list `[query]` explicitly. Forget the deps list and you get stale results silently.
- **Less ceremony vs more visibility.** Vue's version reads like the math you'd write on a whiteboard. React's version puts the data flow on the page where you can see it.
- **Both are deliberate design choices.** Vue traded explicitness for ergonomics. React traded ergonomics for predictability. Saying that explicitly is what makes the README useful.

### Pattern README structure

`patterns/01-reactivity/README.md` has four sections:

1. **The Pattern** — what derived state is; what each version is doing.
2. **Where Vue wins** — auto-tracking; less ceremony; computed reads like math.
3. **Where React wins** — explicit deps make data flow visible; easier to debug when something *doesn't* update; predictability over magic.
4. **What surprised me** — one personal sentence (the senior-signal hook).

## Repo structure

```
vue-vs-react-patterns/
├── README.md           ← top-level index (see below)
├── LICENSE             ← MIT
├── .gitignore          ← Node + Vite defaults
├── .editorconfig
└── patterns/
    └── 01-reactivity/
        ├── README.md   ← 4-section format
        ├── vue/        ← Vite + Vue 3 + TS, runnable
        └── react/      ← Vite + React + TS, runnable
```

Future patterns slot in as `patterns/02-side-effects/`, `patterns/03-two-way-binding/`, etc.

### Bootstrap commands

```bash
cd /Users/denys-petryniak/Developer/personal/projects
mkdir vue-vs-react-patterns && cd vue-vs-react-patterns
git init
mkdir -p patterns/01-reactivity
cd patterns/01-reactivity
pnpm create vite vue --template vue-ts
pnpm create vite react --template react-ts
```

## Stack

- **Build:** Vite (default scaffold for both frameworks)
- **Language:** TypeScript on both sides
- **Package manager:** pnpm
- **Styling:** none — `<style>` block in Vue, plain CSS file in React, only what's needed for legibility
- **Install strategy:** independent `pnpm install` per pattern × per framework. No workspace.

**Why no workspace:** each pattern stays portable (clone one folder, drop it into CodeSandbox, it runs). Tooling overhead from workspaces would have nothing to do with the learning goal. pnpm's content-addressable store dedupes packages on disk anyway.

## Top-level README

```markdown
# vue-vs-react-patterns

> Notes from a Vue dev learning React deeply. Each pattern is a
> thing that confused me — written down so it doesn't confuse
> the next person.

## How to use this repo

Each pattern is a self-contained sandbox. Pick one, `cd` into the
`vue/` or `react/` folder, run `pnpm install && pnpm dev`. The
README in each pattern explains what to look at and why.

## Patterns

| #  | Pattern                   | Vue side                    | React side               | Status        |
|----|---------------------------|-----------------------------|--------------------------|---------------|
| 01 | Reactivity                | ref / reactive / computed   | useState / useMemo       | 🚧 In progress |
| 02 | Side effects              | watch / watchEffect         | useEffect                | ⬜ Planned     |
| 03 | Two-way binding           | v-model                     | controlled inputs        | ⬜ Planned     |
| 04 | Lifecycle                 | onMounted / onUnmounted     | useEffect mount/cleanup  | ⬜ Planned     |
| 05 | Component communication   | provide / inject            | Context                  | ⬜ Planned     |
| 06 | Conditional & lists       | v-if / v-for                | JSX expressions          | ⬜ Planned     |

## Stack

Vite + TypeScript on both sides. No styling library, no state
management — each pattern stays as small as possible to keep the
contrast clean.

## License

MIT
```

## Definition of "Pattern #1 done"

All of the following true:

1. Repo exists at `/Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns` with the structure above.
2. Top-level README, LICENSE, .gitignore, .editorconfig committed.
3. `patterns/01-reactivity/vue/` and `patterns/01-reactivity/react/` both run `pnpm dev` cleanly and show the live filtered list.
4. `patterns/01-reactivity/README.md` is written with all four sections filled in (not stubs).
5. The two implementations behave identically (same items, same filter behavior, same UI structure).
6. Status in the top-level README is set to 🚧 (and flips to ✅ when this checklist is fully green and the work is committed).
7. Repo pushed to GitHub as a public repo named `vue-vs-react-patterns`.

## Open questions

None blocking. Decisions made above can be revisited when adding pattern #2 — particularly the install strategy (revisit if 16 node_modules becomes painful) and deployment (revisit when 2+ patterns are shipped).

## Future patterns (roadmap, not commitment)

| #  | Pattern | Notes |
|----|---------|-------|
| 02 | Side effects | The cleanup + deps-array story. Demo: window resize listener. |
| 03 | Two-way binding | Classic interview question. Demo: form input pair. |
| 04 | Lifecycle | StrictMode double-mount in React is a great gotcha. |
| 05 | Component communication | provide/inject vs Context. Demo: theme toggle. |
| 06 | Conditional & lists | v-if/v-for precedence vs JSX flexibility. Demo: filtered grid with empty state. |

These are sketches only. The actual demo for each gets re-brainstormed when its turn comes.
