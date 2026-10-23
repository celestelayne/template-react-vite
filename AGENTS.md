# About

A read-only list of films to watch. One person browses: search, filter, sort, select, grid or list. No accounts, no server.

## Slices (build in this order, one at a time)

Each slice lives in `docs/slices/<name>/` as `intent.md`, `spec.md`, `plan.md`. 
New slices are drafted with the `new-slice` skill, which uses `.claude/skills/new-slice/contract-template.md`.
Read the slice's spec before working on it. If code and spec disagree, stop and ask. Never build ahead of the current slice.

## Commands

```bash
npm run lint
npm run typecheck
npm run test
npm run dev
```
If any script does not exist, report that fact. Do not silently add a new tool or dependency in order to make it exist.

## Where things live

```text
template-react-vite/
├── AGENTS.md
├── CLAUDE.md
├── docs/
│   └── slices/
│       └── <name>/             # intent, spec, plan for each slice
│           ├── intent.md
│           ├── spec.md
│           └── plan.md
├── .claude/                    # Claude settings and skills
│   ├── settings.json           # permissions and config (commit)
│   └── skills/
│       └── new-slice/
│           └── contract-template.md
└── src/
    ├── main.tsx
    ├── App.tsx                 # near-empty shell
    ├── index.css               # theme tokens (@theme block)
    └── features/               # created by the first slice
        └── films/
            └── view-rules.ts   # pure search/filter/sort (born with the first slice that needs it)
```

## Architecture guardrails
1. The visible list is always calculated from the full list plus the current choices. Never store a filtered copy.
2. View rules are pure, in one file, never inside components. Order: search → filter → sort.
3. Search, filter, sort live in the web address. Grid/list lives in browser storage. Selection lives in the component.
4. Check everything coming in (starter data, web address, browser storage). Bad values fall back to defaults; they never crash the app.
5. No abstractions until something repeats.
6. Use theme tokens only. No loose values.
7. Keyboard works, focus is visible, result count is announced.

## Interface quality
Every slice meets these before "Done":
1. Contrast is at least WCAG AA (4.5:1 body text, 3:1 UI and large text).
2. Every interactive element is reachable by keyboard with a visible focus ring.
3. Hit targets are at least 44×44 CSS pixels on touch.
4. Motion respects `prefers-reduced-motion` and defaults to a single duration + easing.
5. Empty, loading, and error states are designed, not accidents.
6. Layout works from 320px up. No horizontal scroll on mobile.
7. Color is never the only signal (pair with icon, text, or shape).

## Verifying your work
Before declaring that a task is done:
1. Run typecheck, test and lint. Paste the output.
2. Open the app and check the changed screen against the spec, in every view that exists so far.
3. Walk through every item in the spec's "Done means" and say how you checked each one.

## Never
- Read `.env` files
- Edit a test to make it pass
- Add a library without asking
- Add accounts, a server, or ways to change films
- Change another slice's behavior without saying so first

## When Claude gets something wrong twice
When the same correction comes up twice in review, add it here as `YYYY-MM-DD — rule`. The guardrails above are for things that are already true; this section is where new rules earn their way in.
