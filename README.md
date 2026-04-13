# Deslop

`deslop` helps an agent clean up AI-slop, junk code, and overengineered internals with a targeted refactor pass.

It is designed to:

- identify low-signal abstractions, dead wrappers, defensive noise, and duplicated logic
- prefer deletion, inlining, renaming, flattening, and merging over adding new structure
- preserve behavior by default while allowing strong local or internal simplification
- protect public APIs unless the user explicitly widens scope
- verify changes with the smallest useful checks

## Install

Install with the Skills CLI:

```bash
npx skills add https://github.com/CasperEngl/deslop-skill --skill deslop
```

For global installation without prompts:

```bash
npx skills add https://github.com/CasperEngl/deslop-skill --skill deslop -g -y
```

Browse skills and docs:

- https://skills.sh/
- https://skills.sh/docs

## When to use it

Use `deslop` when code feels bloated, generic, defensive, duplicated, or obviously AI-generated, and the goal is to make it smaller, sharper, and easier to read without drifting into a broad redesign.

## What it does

The skill helps the agent:

- read the target code, nearby callers, tests, and relevant interfaces before changing anything
- classify cleanup opportunities as `fix now`, `defer`, or `keep`
- simplify code by inlining wrappers, merging duplicates, flattening delegation, and deleting stale scaffolding
- keep justified complexity when it reflects real boundaries, reuse, runtime constraints, or domain language
- ask only when cleanup would change public APIs, behavior is unclear, or risk is not well covered by tests

## Notes

- See [references/slop-patterns.md](references/slop-patterns.md) for the compact heuristic catalog.
- `deslop` is for focused cleanup and refactor passes, not broad architectural redesign.
