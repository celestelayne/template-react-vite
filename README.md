# template-react-vite

A design-engineering starter for small, local-only React apps, built one vertical slice at a time, with AI doing most of the typing.

## What ships
- `AGENTS.md` — the project contract. Read it once; the AI reads it every time.
- `CLAUDE.md` — a one-line pointer to `AGENTS.md` for Claude Code.
- `.claude/skills/new-slice/` — the skill and template that drafts a slice contract.
- `src/index.css` — a 12-token `@theme` block (type, color, shape, motion) with a `prefers-color-scheme: dark` override.
- `src/App.tsx` — a token specimen landing page. Replace it with real UI when the first slice ships.
- Vite + React 19 + TypeScript + Tailwind 4 + Vitest + ESLint.

No feature code. No data. The first slice decides the shape of both.

## Project use cases
Intended for a local-only, read-only app that browses a list of things with filterable categories, sortable numbers or dates, and searchable text. Examples:

| Project         | Properties | Why it fits |
| :-- | :-- | :-- |
| Reading list   | Title, author, genre, status (want / reading / read), rating, year. | The classic — everyone understands it instantly. |
| Film watchlist | Same shape plus runtime, which makes a nice "under 2 hours" filter. | What the current `AGENTS.md` is pre-written for. |
| Exhibition log | Artist, venue, medium, city, dates, status (want to go / went). | Good for date-based filters like "on now" or "closing soon". |

## First-time setup
1. "Use this template" on GitHub → clone → `npm install` → `npm run dev`.
2. Confirm the token specimen loads at the dev URL. Light/dark should auto-switch with your OS theme.
3. Update the project name in `package.json` and the page title in `index.html`.
4. Rewrite the "About" section of `AGENTS.md` for your project (2–3 sentences: what it is, who it's for, what it is NOT).
5. Adjust token values in `src/index.css` for your palette. Don't add new names yet — tokens earn their way in.

## The slice loop
Every feature is a slice. Build one at a time, in this order:

1. **Draft the contract.** Ask the AI to run the `new-slice` skill. It reads `AGENTS.md`, scans existing slices, and writes three files under `docs/slices/<name>/`: `intent.md`, `spec.md`, `plan.md`. No code is written in this step.
2. **Review the contract.** Answer every "Open question" bullet. Argue with the plan, flag shared-file coupling, split the slice if it got too big. Fix it here, not in code.
3. **Build.** Hand the signed-off contract back and ask the AI to implement in the sequence `plan.md` lays out. Thin first; a running UI before a correct one.
4. **Verify.** Before "Done":
   - `npm run typecheck && npm run test && npm run lint` — all green.
   - Walk the spec's "Done means" checklist, item by item.
   - Walk the Interface quality standards in `AGENTS.md` (contrast, focus, hit targets, reduced motion, empty/loading/error states, 320px, non-colour signals).
5. **Replace the specimen.** Once the first slice renders real UI, delete the token specimen from `App.tsx`.

## Suggested slice order
These are the slices the "About" line implies, in a sensible build order. Each is small enough to spec cleanly and extends the previous one's pattern. (Reword freely for a reading list or exhibition log — the shape is the same.)

1. **`films-list`** — render all films as a list. Creates the `Film` type and starter data in `src/features/films/films.ts`, and replaces the token specimen in `App.tsx`. Nothing to search against until this exists.
2. **`search`** — text filter over title. State in the URL (`?q=`). Introduces `view-rules.ts` and the `matches(q)` predicate.
3. **`filter`** — by genre (and maybe runtime). State in the URL (`?genre=`). Extends `view-rules.ts`.
4. **`sort`** — by title, year, runtime. State in the URL (`?sort=`). Completes the `search → filter → sort` pipeline from guardrail #2.
5. **`grid-list-toggle`** — switch between views. State in browser storage per guardrail #3.
6. **`select`** — film detail. State in the component per guardrail #3. Modal or drawer; not a route.

Each slice follows the same shape: a *choice* lives in URL / storage / component, a *pure rule* in `view-rules.ts` consumes it, and the visible list is derived — never stored.

## Commands
- `npm run dev` — local dev server
- `npm run build` — production bundle
- `npm run typecheck` — type check
- `npm run test` — run tests (passes cleanly on an empty suite)
- `npm run lint` — lint

## Updating the template
New projects are one-time copies. Improvements to `AGENTS.md`, the `new-slice` skill, or the token file don't flow back to older projects — copy them by hand, and only bring a change back into this template after it has earned its keep in a real project.
