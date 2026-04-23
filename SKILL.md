---
name: deslop
description: Clean up slop, junk code, and overengineered internals with a targeted refactor pass. Use when code feels bloated, generic, defensive, duplicated, or obviously AI-generated; when the user wants to simplify AI-generated code; remove fake abstractions; collapse wrappers, helpers, and managers that add no value; reduce unnecessary code indirection; make code smaller, sharper, and easier to read; or refactor internals without changing behavior. Prefer focused cleanup over broad redesign.
---

# Deslop

## Purpose

Use this skill for direct cleanup passes on code that feels bloated, generic, defensive, duplicated, or obviously AI-generated.

Read [references/slop-patterns.md](references/slop-patterns.md) when you need a compact catalog of common AI-slop signals, simplification moves, false positives, or change types that require user confirmation.

## Operating Stance

- Preserve behavior by default.
- Prefer deletion and simplification over abstraction.
- Allow internal and local API cleanup when usages are close and verification is strong.
- Avoid public API changes unless the user explicitly asks.
- Avoid speculative architecture work.
- Do not "improve" code by adding new layers, config objects, managers, factories, or generic helpers.

## Workflow

1. Ground in scope before changing anything.
   - Read the target files, nearby callers, tests, and relevant types or interfaces.
   - Identify which behavior must stay stable and which interfaces are internal, local, or public.

2. Identify slop candidates.
   - Call out low-signal abstractions, dead code, duplicated logic, defensive noise, generic naming, one-off helpers, unnecessary code indirection, and future-proofing with no current need.

3. Classify each candidate.
   - Use `fix now`, `defer`, or `keep`.

4. Apply the smallest set of edits that materially improve clarity.
   - Prefer inlining, collapsing, deleting, renaming, and merging over introducing new structure.

5. Verify with the smallest useful check.
   - Run targeted tests, typecheck, build, or focused validation based on the touched area.

6. Report the result.
   - State what was simplified, what was intentionally left alone, and any risks.

## Classification Rules

### `Fix now`

Choose `fix now` for:

- clear noise or duplication
- fake flexibility with no real second use case
- dead wrappers or useless indirection
- unreadable or generic naming
- low-risk internal simplification

### `Defer`

Choose `defer` for:

- cleanup that widens scope
- changes that require broad API movement
- cases that depend on product intent or unclear behavior

### `Keep`

Choose `keep` for:

- abstractions justified by boundaries or runtime constraints
- reuse that is real, not speculative
- testability seams with clear value
- domain language that looks indirect but carries meaning

## Cleanup Heuristics

Make cleanup changes like these:

- inline one-use wrappers
- inline pass-through helpers that only hide the real work
- merge near-duplicate helpers
- delete dead branches or stale TODO scaffolding
- replace option bags with direct parameters when the call graph is local
- remove just-in-case guards that obscure the real flow
- rename vague identifiers to concrete ones
- flatten needless delegation chains
- collapse classes or objects that only forward calls
- remove generic comments that only restate the code

Do not:

- chase stylistic nits
- rewrite stable architecture for taste alone
- expand scope into unrelated cleanup
- delete complexity that is actually carrying domain constraints

## Asking Rules

Ask the user only when:

- cleanup would change public APIs
- behavior is ambiguous
- there are competing simplification directions with real tradeoffs
- tests are missing and the risk is non-trivial

Otherwise proceed.

## Output Contract

Return:

- what you simplified
- what patterns you removed
- what you deferred and why
- what verification you ran
- residual risks or unverified areas
