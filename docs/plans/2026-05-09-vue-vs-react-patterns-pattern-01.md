# vue-vs-react-patterns — Pattern #01 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bootstrap the `vue-vs-react-patterns` repo and ship Pattern #01 (Reactivity model — live filtered list) end-to-end, including a public GitHub repo with a 4-section pattern README.

**Architecture:** Two independent Vite + TypeScript scaffolds (`vue/` and `react/`) inside `patterns/01-reactivity/`, each implementing the same live filtered list demo. No shared tooling, no workspace, no styling library. Top-level README is the index; per-pattern README is the actual learning artifact.

**Tech Stack:** pnpm, Vite, Vue 3, React 18+, TypeScript. Git. GitHub CLI (`gh`).

**Verification model:** No unit tests — the spec deliberately rejects test infrastructure as overhead that distracts from the learning goal. The "test" for each version is **manual**: run `pnpm dev`, type in the input, confirm the list filters in real time. Each task ends with this verification step before commit.

---

## File Structure (final state)

```
projects/vue-vs-react-patterns/
├── .editorconfig
├── .gitignore
├── LICENSE
├── README.md                                     ← top-level index
├── docs/
│   └── specs/
│       └── 2026-05-09-vue-vs-react-patterns-design.md  ← copy of the spec
└── patterns/
    └── 01-reactivity/
        ├── README.md                             ← 4-section learning artifact
        ├── vue/                                  ← Vite + Vue + TS scaffold
        │   └── src/App.vue                       ← MODIFIED to be the demo
        └── react/                                ← Vite + React + TS scaffold
            └── src/App.tsx                       ← MODIFIED to be the demo
```

**Files touched per task:**

| Task | Created                                                      | Modified                                  |
|------|--------------------------------------------------------------|-------------------------------------------|
| 1    | repo dir, `.gitignore`, `.editorconfig`, `LICENSE`, `README.md` | —                                       |
| 2    | `patterns/01-reactivity/vue/**` (Vite scaffold)              | —                                         |
| 3    | —                                                            | `patterns/01-reactivity/vue/src/App.vue`  |
| 4    | `patterns/01-reactivity/react/**` (Vite scaffold)            | —                                         |
| 5    | —                                                            | `patterns/01-reactivity/react/src/App.tsx`|
| 6    | `patterns/01-reactivity/README.md`                           | —                                         |
| 7    | `docs/specs/2026-05-09-vue-vs-react-patterns-design.md`      | —                                         |
| 8    | GitHub repo (remote)                                         | —                                         |
| 9    | —                                                            | `README.md` (status flip)                 |

---

## Task 1: Initialize repo and top-level files

**Files:**
- Create: `/Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns/`
- Create: `<repo>/.gitignore`
- Create: `<repo>/.editorconfig`
- Create: `<repo>/LICENSE`
- Create: `<repo>/README.md`

> Throughout this plan, `<repo>` means `/Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns`.

- [ ] **Step 1: Create the repo directory and initialize git**

```bash
mkdir -p /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git init -b main
```

Expected: `Initialized empty Git repository in .../vue-vs-react-patterns/.git/`

- [ ] **Step 2: Create `.gitignore`**

Path: `<repo>/.gitignore`

```gitignore
# Dependencies
node_modules
.pnpm-store

# Build output
dist
dist-ssr
*.local

# Editors
.DS_Store
.idea
.vscode/*
!.vscode/extensions.json

# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

# Env
.env
.env.local
.env.*.local
```

- [ ] **Step 3: Create `.editorconfig`**

Path: `<repo>/.editorconfig`

```editorconfig
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false
```

- [ ] **Step 4: Create `LICENSE` (MIT)**

Path: `<repo>/LICENSE`

```
MIT License

Copyright (c) 2026 Denys Petryniak

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 5: Create top-level `README.md`**

Path: `<repo>/README.md`

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

- [ ] **Step 6: Verify and commit**

Run from `<repo>`:

```bash
ls -la
git status
git add .gitignore .editorconfig LICENSE README.md
git commit -m "chore: initial repo scaffold with README, license, gitignore"
```

Expected: clean commit, working tree clean.

---

## Task 2: Scaffold Vue side of Pattern #01

**Files:**
- Create: `<repo>/patterns/01-reactivity/vue/**` (full Vite + Vue + TS scaffold)

- [ ] **Step 1: Create the pattern directory**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
mkdir -p patterns/01-reactivity
cd patterns/01-reactivity
```

- [ ] **Step 2: Scaffold Vue + TS app via Vite**

```bash
pnpm create vite vue --template vue-ts
```

When prompted to install dependencies, accept. If not auto-prompted, run:

```bash
cd vue && pnpm install && cd ..
```

Expected: `vue/` directory created with Vite + Vue 3 + TS scaffold; `vue/node_modules/` populated.

- [ ] **Step 3: Verify the Vue app runs**

```bash
cd vue
pnpm dev
```

Expected: Vite prints `Local: http://localhost:5173/`. Open it. The default Vite + Vue welcome page renders. Stop the server with Ctrl+C.

- [ ] **Step 4: Commit the scaffold**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add patterns/01-reactivity/vue
git status
git commit -m "feat(01-reactivity): scaffold Vue side with Vite + TS"
```

Expected: commit succeeds; `node_modules/` is excluded by `.gitignore`.

---

## Task 3: Implement the Vue filtered list demo

**Files:**
- Modify: `<repo>/patterns/01-reactivity/vue/src/App.vue`

- [ ] **Step 1: Replace `App.vue` with the filtered list demo**

Path: `<repo>/patterns/01-reactivity/vue/src/App.vue`

```vue
<script setup lang="ts">
import { ref, computed } from 'vue'

type Item = { id: number; name: string; category: string }

const items: Item[] = [
  { id: 1, name: 'Apple', category: 'fruit' },
  { id: 2, name: 'Banana', category: 'fruit' },
  { id: 3, name: 'Carrot', category: 'vegetable' },
  { id: 4, name: 'Doughnut', category: 'snack' },
  { id: 5, name: 'Eggplant', category: 'vegetable' },
  { id: 6, name: 'Fig', category: 'fruit' },
  { id: 7, name: 'Grape', category: 'fruit' },
  { id: 8, name: 'Honeydew', category: 'fruit' },
]

const query = ref('')
const filtered = computed(() =>
  items.filter(i =>
    i.name.toLowerCase().includes(query.value.toLowerCase())
  )
)
</script>

<template>
  <main>
    <h1>Reactivity — Vue</h1>
    <input v-model="query" placeholder="Search..." />
    <ul v-if="filtered.length">
      <li v-for="item in filtered" :key="item.id">
        {{ item.name }} <small>({{ item.category }})</small>
      </li>
    </ul>
    <p v-else>No matches.</p>
  </main>
</template>

<style scoped>
main {
  font-family: system-ui, sans-serif;
  max-width: 32rem;
  margin: 2rem auto;
  padding: 0 1rem;
}
input {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  margin-bottom: 1rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  padding: 0.25rem 0;
}
small {
  color: #666;
}
</style>
```

- [ ] **Step 2: Manually verify the demo works**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns/patterns/01-reactivity/vue
pnpm dev
```

Open `http://localhost:5173`. Verify:
1. The page shows "Reactivity — Vue" heading, an input, and 8 items.
2. Typing `a` filters down to items containing "a" (case-insensitive).
3. Typing `xxx` shows "No matches."
4. Clearing the input restores all 8 items.

Stop the server with Ctrl+C.

- [ ] **Step 3: Commit**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add patterns/01-reactivity/vue/src/App.vue
git commit -m "feat(01-reactivity): implement Vue filtered list demo"
```

---

## Task 4: Scaffold React side of Pattern #01

**Files:**
- Create: `<repo>/patterns/01-reactivity/react/**` (full Vite + React + TS scaffold)

- [ ] **Step 1: Scaffold React + TS app via Vite**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns/patterns/01-reactivity
pnpm create vite react --template react-ts
```

When prompted to install dependencies, accept. If not auto-prompted, run:

```bash
cd react && pnpm install && cd ..
```

Expected: `react/` directory created with Vite + React + TS scaffold; `react/node_modules/` populated.

- [ ] **Step 2: Verify the React app runs**

```bash
cd react
pnpm dev
```

Expected: Vite prints a `Local: http://localhost:5173/` URL (or the next free port if 5173 is taken). The default Vite + React welcome page renders with the spinning React logo. Stop with Ctrl+C.

- [ ] **Step 3: Commit the scaffold**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add patterns/01-reactivity/react
git commit -m "feat(01-reactivity): scaffold React side with Vite + TS"
```

---

## Task 5: Implement the React filtered list demo

**Files:**
- Modify: `<repo>/patterns/01-reactivity/react/src/App.tsx`

- [ ] **Step 1: Replace `App.tsx` with the filtered list demo**

Path: `<repo>/patterns/01-reactivity/react/src/App.tsx`

```tsx
import { useState, useMemo } from 'react'

type Item = { id: number; name: string; category: string }

const items: Item[] = [
  { id: 1, name: 'Apple', category: 'fruit' },
  { id: 2, name: 'Banana', category: 'fruit' },
  { id: 3, name: 'Carrot', category: 'vegetable' },
  { id: 4, name: 'Doughnut', category: 'snack' },
  { id: 5, name: 'Eggplant', category: 'vegetable' },
  { id: 6, name: 'Fig', category: 'fruit' },
  { id: 7, name: 'Grape', category: 'fruit' },
  { id: 8, name: 'Honeydew', category: 'fruit' },
]

export default function App() {
  const [query, setQuery] = useState('')
  const filtered = useMemo(
    () =>
      items.filter(i =>
        i.name.toLowerCase().includes(query.toLowerCase())
      ),
    [query]
  )

  return (
    <main>
      <h1>Reactivity — React</h1>
      <input
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      {filtered.length ? (
        <ul>
          {filtered.map(item => (
            <li key={item.id}>
              {item.name} <small>({item.category})</small>
            </li>
          ))}
        </ul>
      ) : (
        <p>No matches.</p>
      )}
    </main>
  )
}
```

- [ ] **Step 2: Replace `src/App.css` with matching styles**

Path: `<repo>/patterns/01-reactivity/react/src/App.css`

```css
main {
  font-family: system-ui, sans-serif;
  max-width: 32rem;
  margin: 2rem auto;
  padding: 0 1rem;
}
input {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  margin-bottom: 1rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  padding: 0.25rem 0;
}
small {
  color: #666;
}
```

- [ ] **Step 3: Make sure `App.tsx` imports the CSS and `main.tsx` has no leftover boilerplate**

Path check: `<repo>/patterns/01-reactivity/react/src/App.tsx` — add the CSS import at the top:

```tsx
import './App.css'
import { useState, useMemo } from 'react'
// ... rest unchanged
```

Path check: `<repo>/patterns/01-reactivity/react/src/main.tsx` — leave it as scaffolded; default Vite scaffold imports `index.css` which is fine.

If `index.css` adds any default styling that conflicts (centered body, dark background), edit it down to a minimal reset:

Path: `<repo>/patterns/01-reactivity/react/src/index.css`

```css
* {
  box-sizing: border-box;
}
body {
  margin: 0;
}
```

- [ ] **Step 4: Manually verify the demo works**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns/patterns/01-reactivity/react
pnpm dev
```

Open the printed URL. Verify the same four behaviors as Task 3 Step 2:
1. "Reactivity — React" heading, input, 8 items rendered.
2. Typing `a` filters to items containing "a".
3. Typing `xxx` shows "No matches."
4. Clearing restores all 8.

The visual layout should match the Vue side closely. Stop with Ctrl+C.

- [ ] **Step 5: Commit**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add patterns/01-reactivity/react/src
git commit -m "feat(01-reactivity): implement React filtered list demo"
```

---

## Task 6: Write the pattern README

**Files:**
- Create: `<repo>/patterns/01-reactivity/README.md`

- [ ] **Step 1: Write `patterns/01-reactivity/README.md` with all four sections**

Path: `<repo>/patterns/01-reactivity/README.md`

```markdown
# 01 — Reactivity

`ref` / `reactive` / `computed` &nbsp;vs&nbsp; `useState` / `useMemo`

## Run it

```bash
# Vue side
cd vue && pnpm install && pnpm dev

# React side
cd react && pnpm install && pnpm dev
```

Type into the search input. The list filters in real time on both sides.

## The Pattern

Both apps render the same list of 8 items and let you filter them by typing
into a search box. The filtered list is **derived state** — it isn't stored
anywhere; it's computed from the source array and the current query.

The interesting question is: *how does the framework know to recompute the
filtered list when the query changes?*

- **Vue** uses **auto-tracking**. When `computed(() => ...)` runs, Vue
  records every reactive value the callback reads. When any of those values
  change, the computed re-runs. You don't tell it what to depend on — it
  figures it out from the code itself.
- **React** uses **explicit deps**. `useMemo(fn, [query])` only recomputes
  when an entry in the deps array changes. The list is your contract with
  React: "these are the inputs to my computation."

## Where Vue wins

- **Less ceremony.** The Vue version reads like the math you'd write on a
  whiteboard. No deps array, no closures-over-state to think about.
- **No stale-closure traps.** Forgetting to update a deps list is one of
  the most common React bugs. Vue's auto-tracking can't go stale because
  the dependencies are inferred from the actual reads at runtime.
- **Refactoring is safer.** Adding a new reactive dependency to a
  `computed` is just adding the read. In React you must remember to also
  add it to the deps array.

## Where React wins

- **Data flow is on the page.** The deps list makes "what triggers a
  recompute" explicit and grep-able. In Vue you have to *trust* the
  proxy — and read the implementation to be sure.
- **Easier to debug a stale value.** When something doesn't update in
  React, you check the deps array. When something doesn't update in Vue,
  you have to reason about whether the value was reactive in the first
  place (a common gotcha: destructuring a `reactive` object loses
  reactivity).
- **Predictable performance.** `useMemo` runs only when deps change, by
  spec. Vue's `computed` is also lazy and cached, but the auto-tracking
  layer adds machinery you can't see.

## What surprised me

[Personal one-sentence note — fill in after building both sides. The honest
hook is the senior signal of this repo.]
```

> **Note on the "What surprised me" section:** This is intentionally a
> placeholder for the implementer (the user) to fill in based on their
> actual experience writing both versions. The plan deliberately does not
> generate this — it's the author's voice, not Claude's.

- [ ] **Step 2: Fill in the "What surprised me" section**

Open `patterns/01-reactivity/README.md` and replace the placeholder with one or two honest sentences from your experience implementing both sides. Examples of the right tone:

- "Writing the React version made me realize how much I take Vue's auto-tracking for granted — I forgot the deps array on my first pass and the filter went stale silently."
- "I expected React to feel heavier, but `useMemo` made the data flow more explicit than I'd have written naturally in Vue."

This is the senior-signal hook. It must be your own words; do not skip.

- [ ] **Step 3: Commit**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add patterns/01-reactivity/README.md
git commit -m "docs(01-reactivity): add 4-section pattern README"
```

---

## Task 7: Copy the design spec into the repo

**Files:**
- Create: `<repo>/docs/specs/2026-05-09-vue-vs-react-patterns-design.md`

- [ ] **Step 1: Copy the spec into the repo**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
mkdir -p docs/specs
cp /Users/denys-petryniak/Developer/personal/docs/superpowers/specs/2026-05-09-vue-vs-react-patterns-design.md \
   docs/specs/
ls docs/specs/
```

Expected: `2026-05-09-vue-vs-react-patterns-design.md` listed.

- [ ] **Step 2: Commit**

```bash
git add docs/specs
git commit -m "docs: import design spec for pattern #01"
```

---

## Task 8: Create GitHub repo and push

**Files:**
- Remote: `https://github.com/denys-petryniak/vue-vs-react-patterns`

- [ ] **Step 1: Verify `gh` is authenticated**

```bash
gh auth status
```

Expected: `Logged in to github.com account denys-petryniak`. If not, run `gh auth login` interactively.

- [ ] **Step 2: Create the GitHub repo and push**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
gh repo create denys-petryniak/vue-vs-react-patterns \
  --public \
  --source=. \
  --description="Side-by-side comparison patterns for a Vue dev learning React. Each pattern is a thing that confused me — written down so it doesn't confuse the next person." \
  --push
```

Expected: repo created on GitHub, current branch pushed, remote `origin` configured.

- [ ] **Step 3: Verify the repo is live**

```bash
gh repo view denys-petryniak/vue-vs-react-patterns --web
```

Expected: browser opens to the repo. Confirm:
1. README renders with the table.
2. `patterns/01-reactivity/README.md` exists with all four sections.
3. Both `vue/` and `react/` folders are visible.

---

## Task 9: Flip status to ✅ and finalize

**Files:**
- Modify: `<repo>/README.md` (status column for row 01)

- [ ] **Step 1: Update top-level README status**

Open `<repo>/README.md` and change the row for #01:

From:
```markdown
| 01 | Reactivity                | ref / reactive / computed   | useState / useMemo       | 🚧 In progress |
```

To:
```markdown
| 01 | Reactivity                | ref / reactive / computed   | useState / useMemo       | ✅ Done        |
```

- [ ] **Step 2: Final manual verification — both apps still run**

```bash
# Vue
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns/patterns/01-reactivity/vue
pnpm dev
# Visit http://localhost:5173, type into input, confirm filter works, Ctrl+C.

# React
cd ../react
pnpm dev
# Visit the printed URL, type into input, confirm filter works, Ctrl+C.
```

Both must work cleanly. If either is broken, **do not** mark done — go back and fix.

- [ ] **Step 3: Commit and push**

```bash
cd /Users/denys-petryniak/Developer/personal/projects/vue-vs-react-patterns
git add README.md
git commit -m "docs: mark pattern 01 (reactivity) as done"
git push
```

- [ ] **Step 4: Cross-check Definition of Done from the spec**

All seven items in the spec's "Definition of Pattern #1 done" section must be true:

1. ✅ Repo exists at the expected path.
2. ✅ Top-level README, LICENSE, .gitignore, .editorconfig committed.
3. ✅ Both `vue/` and `react/` run `pnpm dev` cleanly and show the live filtered list.
4. ✅ `patterns/01-reactivity/README.md` has all four sections filled in (not stubs — including the personal "What surprised me" sentence).
5. ✅ The two implementations behave identically.
6. ✅ Top-level status is ✅ Done.
7. ✅ Repo is public on GitHub at `denys-petryniak/vue-vs-react-patterns`.

If all seven are green, Pattern #01 is shipped.

---

## Self-review notes

- **Spec coverage:** Every requirement from the spec maps to a task — bootstrap (Task 1), Vue scaffold (Task 2), Vue impl (Task 3), React scaffold (Task 4), React impl (Task 5), pattern README with all 4 sections (Task 6), spec import (Task 7), GitHub publish (Task 8), DoD check (Task 9).
- **No placeholders in the plan** itself — every code block is concrete. The one deliberate placeholder (the "What surprised me" sentence in the pattern README) is called out explicitly with reasoning: it must be the user's voice.
- **Type consistency:** `Item` type is identical between Vue and React versions. Item data is identical (8 items, same names and categories, same ids). `query` is the input state name on both sides. `filtered` is the derived value name on both sides.
- **Verification model is honest:** manual verification only. The spec deliberately rejects test infrastructure as overhead. The plan respects that and provides explicit, repeatable manual verification steps with concrete expected behaviors at each step.
