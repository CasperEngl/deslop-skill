# Slop Patterns

Use this file as a compact heuristic catalog during cleanup passes. Treat it as a guide for spotting low-signal complexity and choosing simpler replacements, not as a mandate to flatten every abstraction you see.

## AI-Slop Signals

Look closely at code with patterns like:

- pointless manager, helper, service, orchestrator, processor, or controller layers that add naming ceremony but no real boundary
- mirrored pass-through methods that only forward arguments and return the result unchanged
- fake extensibility hooks such as option bags, strategy objects, plugin points, or callback plumbing with only one concrete use
- duplicated helpers with tiny variations that should be merged or specialized
- broad config objects passed through one local call site
- vague names like `processDataHelper`, `executeOperation`, `handleManager`, or `serviceUtils`
- wrappers that exist only to rename a call without adding validation, translation, caching, retries, or boundary handling
- defensive branching for impossible or unsupported states with no evidence those states matter
- extracted functions so small or generic that they hide the control flow instead of clarifying it

## Good Simplification Moves

Prefer moves like:

- inline one-use wrappers
- delete dead code and stale compatibility scaffolding
- merge near-duplicate helpers
- rename generic identifiers to concrete domain terms
- flatten delegation chains
- specialize overly generic utilities to the actual use case
- narrow broad parameter objects into direct arguments when callers are local
- collapse classes or objects that only forward work

## False Positives

Keep complexity when it is carrying real value, including:

- cross-runtime boundaries such as edge vs. node or browser vs. server
- compatibility layers for old callers, serialized formats, or versioned protocols
- adapters around external APIs, SDKs, or unstable vendors
- real reuse across multiple call sites or packages
- performance-sensitive code where indirection is hiding a measured optimization
- test seams that materially improve isolation or readability
- domain-specific terminology that looks abstract but reflects the business model

## Red Flags Requiring User Confirmation

Stop and ask before cleanup would change:

- exported or public APIs
- persistence formats or stored data shapes
- network protocols, schemas, or request/response contracts
- concurrency behavior, ordering, locking, retries, or async semantics
- user-visible behavior where the intended outcome is not already clear
