# vue-vs-react-patterns

> Notes from a Vue dev learning React deeply. Each pattern is a
> thing that confused me — written down so it doesn't confuse
> the next person.

## How it's organized

This is a **pnpm workspace monorepo** — one repository, many small projects.
The root coordinates them; each pattern's Vue and React versions live
as separate workspace packages.

For a deeper walk-through of the workspace setup, see
[`docs/architecture.md`](./docs/architecture.md).

```
vue-vs-react-patterns/
├── package.json              ← root: orchestration scripts
├── pnpm-workspace.yaml       ← declares which folders are packages
├── pnpm-lock.yaml            ← single lockfile for the whole repo
└── patterns/
    └── 01-reactivity/
        ├── README.md         ← the learning artifact for this pattern
        ├── vue/              ← package: 01-reactivity-vue
        └── react/            ← package: 01-reactivity-react
```

## Getting started

One install at the root covers every pattern:

```bash
pnpm install
```

Then run a pattern's Vue and React sides **side by side**:

```bash
pnpm dev:01-reactivity
```

That starts both dev servers in parallel:

- React → http://localhost:5173
- Vue   → http://localhost:5174

To run just one side:

```bash
pnpm --filter 01-reactivity-vue dev
pnpm --filter 01-reactivity-react dev
```

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

Vite + TypeScript on both sides. pnpm workspace at the root. No styling
library, no state management — each pattern stays as small as possible
to keep the contrast clean.

## License

MIT
