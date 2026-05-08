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
