# Lens: Scale

**Core question:** what breaks *semantically*, not just performance-wise, as cardinality changes?

This lens is not about premature optimization or capacity planning. It's about assumptions that are true at low cardinality and silently become false at higher cardinality — usually because a "the" in the spec should have been "a."

## Change the count

For the main entities and actors in the spec, ask what changes qualitatively (not just "gets slower") at each of:

```
1
10
100
10,000
```

## Invert the assumed cardinality

The spec was probably written with one implicit cardinality in mind for each of these. Ask what happens at the opposite:

```
one user      → many users acting on the same thing at once
one process   → concurrent processes
one workspace → many workspaces
one provider  → many providers
one machine   → distributed across machines
```

## Look for

- **Singular language hiding a plural reality.** Any place the spec says "the X" where X could reasonably be more than one (the current version, the active session, the owner) — is that guaranteed to stay singular, or is it just singular in the example the author had in mind?
- **Implicit global state.** Anything the spec treats as a single shared value (a counter, a lock, a "current" pointer) — does it stay correct with more than one writer?
- **Fan-out the author didn't picture.** If one action can plausibly trigger many downstream effects (notifications, webhooks, child processes), does the spec's model hold up when that fan-out is large?

## Example

Bad: "This might not scale."

Good: "The spec models 'the active workstream' as a singular concept per user, but nothing in the spec actually prevents a user from starting a second one before the first finishes — at scale, that's not an edge case, it's a routine occurrence, and the spec has no described behavior for two active workstreams under one user."
