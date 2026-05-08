# Architecture: pnpm Workspace Monorepo

A walk-through of how this repo is organized and why. Written for a
developer who's used Vite-based apps before but is new to monorepos.

## Table of contents

- [The big idea](#the-big-idea)
- [The three files that make it a workspace](#the-three-files-that-make-it-a-workspace)
- [How the pieces fit](#how-the-pieces-fit)
- [How to think about packages](#how-to-think-about-packages)
- [Day-to-day workflow](#day-to-day-workflow)
- [What this gives you when adding a new pattern](#what-this-gives-you-when-adding-a-new-pattern)
- [When NOT to use a monorepo](#when-not-to-use-a-monorepo)

## The big idea

A **monorepo** is one git repository that contains multiple related
projects. A **workspace** is a tooling layer (here: pnpm) that knows
about those projects and treats them as a coordinated whole.

You already use this idea in Vite — when you build a Vue app, Vite knows
about your `src/`, `node_modules/`, and `dist/`. A workspace just zooms
out one level: instead of one app with internal pieces, you have one
*root* with multiple apps as pieces.

In this repo:

- **The repo** holds patterns comparing Vue and React.
- **Each pattern** has two apps — a Vue version and a React version.
- **Each app** is a separate workspace package with its own
  `package.json`, dependencies, and scripts.
- **The root** orchestrates them via pnpm.

## The three files that make it a workspace

### 1. `pnpm-workspace.yaml` — "what's in the workspace"

```yaml
packages:
  - 'patterns/*/vue'
  - 'patterns/*/react'
```

This is the **declaration**. It tells pnpm: any folder matching these
globs is a package — go find its `package.json` and treat it as a
workspace member. When you add `patterns/02-side-effects/vue/`, pnpm
picks it up automatically because it matches `patterns/*/vue`.

### 2. Root `package.json` — "orchestration"

```json
{
  "name": "vue-vs-react-patterns",
  "private": true,
  "scripts": {
    "dev:01-reactivity": "pnpm --parallel --filter './patterns/01-reactivity/*' run dev"
  }
}
```

This is the **conductor**. It doesn't ship code — `"private": true`
means it'll never be published to npm. Its only job is the `scripts`
block, which runs commands across the workspace.

The `dev:01-reactivity` script reads as:

> Run `dev` in every package matching `./patterns/01-reactivity/*`,
> all in parallel.

That's why `pnpm dev:01-reactivity` boots both Vue and React at once.

### 3. `pnpm-lock.yaml` (root) — "one truth for all dependencies"

Without a workspace, each package would have its own lockfile — Vue
would lock its versions independently from React, and you'd risk
`vite@8.0.10` in one and `vite@8.0.11` in another.

With a workspace, **one lockfile at the root** locks everything
together. Reproducible across the whole repo. If you ever clone fresh
and run `pnpm install`, every package gets exactly the versions in this
lockfile — no drift.

## How the pieces fit

```
                     pnpm install (run at root)
                              │
                              ▼
                    reads pnpm-workspace.yaml
                              │
                ┌─────────────┴─────────────┐
                ▼                           ▼
    patterns/01-reactivity/vue       patterns/01-reactivity/react
       (01-reactivity-vue)              (01-reactivity-react)
                │                           │
                ▼                           ▼
     installs Vite, Vue,            installs Vite, React,
     TypeScript, etc.               TypeScript, etc.
                │                           │
                └─────────────┬─────────────┘
                              ▼
              ONE root pnpm-lock.yaml
              (deduplicated via pnpm's content-addressable store)
```

**Why pnpm specifically:** pnpm stores every package version exactly
once on your disk in `~/.pnpm-store` and symlinks them into each
project's `node_modules`. Even with 16 future workspace packages, you
don't get 16 copies of `vite` taking up disk — you get 16 symlinks
pointing to one real copy.

## How to think about packages

Each workspace package has its **own** `package.json` with its own
dependencies and scripts. They don't share code automatically — they're
isolated apps that happen to live in one repo.

| Question                                  | Answer                                           |
| ----------------------------------------- | ------------------------------------------------ |
| Does the Vue package use React?           | No — separate `package.json`s.                   |
| Could they share code?                    | Yes, by importing one as a dep, but we don't.    |
| Does each have its own `vite.config.ts`?  | Yes — fully independent.                         |
| One install or many?                      | **One** at the root. Pnpm wires the rest.        |

## Day-to-day workflow

```bash
# Once after cloning or pulling new patterns:
pnpm install

# Most common — work on a pattern (both sides at once):
pnpm dev:01-reactivity

# Just one side (e.g., focused on Vue):
pnpm --filter 01-reactivity-vue dev

# Build everything (later, when we add prod builds):
pnpm -r build

# Run a script in every package that has it:
pnpm -r <script-name>
```

The `--filter` flag is your main tool. It accepts:

- A package name: `--filter 01-reactivity-vue`
- A glob path:    `--filter './patterns/01-reactivity/*'`
- A name pattern: `--filter '*-vue'` matches all Vue packages

## What this gives you when adding a new pattern

**Without workspace:** scaffold Vue, `pnpm install`, scaffold React,
`pnpm install`, repeat per pattern. 12+ installs total over the life
of the repo.

**With workspace:** scaffold Vue, scaffold React, run `pnpm install`
once at the root. The new packages are picked up automatically because
their paths match `patterns/*/vue` and `patterns/*/react`. Add a
`dev:02-side-effects` script in the root `package.json`. Done.

That's the trade. You pay ~10 minutes upfront in workspace setup; you
save that back every time you add a pattern.

## When NOT to use a monorepo

Monorepos are wrong when packages are unrelated. Two completely
different products in one repo creates friction (CI, releases,
ownership boundaries blur, dependency upgrades become political).

The right test: *do these things change together?* For this repo —
patterns that share docs, share tooling, and exist specifically to be
compared — yes, they belong together.

For separate side projects with no overlap, give each its own repo.
