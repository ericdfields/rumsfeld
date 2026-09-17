# Lens: Security and trust

**Core question:** what changes if the actors involved aren't all well-intentioned, well-informed, and permanently authorized?

This lens is not a full security audit (see the separate `security-review` skill for that). It's specifically about trust assumptions baked into the *product model* that the spec never states out loud.

## Ask what changes when

- **Actors are malicious.** Someone using this system's normal, documented interface tries to get an outcome the spec didn't intend. Where does the spec rely on users simply not trying that?
- **Actors make honest mistakes.** Not malice — just a wrong click, a copy-paste error, a misunderstanding. Does the spec's model make mistakes cheap and visible, or silent and hard to undo?
- **Credentials or grants expire.** Anything in the spec that depends on a token, session, permission, or connected account — what happens the moment that access lapses mid-operation, not before it starts?
- **Boundaries change.** If this spec has any notion of ownership, team, or workspace, what happens when an actor's relationship to that boundary changes (they're removed, their role changes, ownership transfers) while something of theirs is still in flight?
- **Sensitive information crosses systems.** Anywhere data moves between components, services, or external providers in this spec — does it carry anything that shouldn't cross that particular boundary, and does the spec say who's responsible for that?
- **Permissions evolve.** The spec grants some capability now. Six months from now the permission model is refined — does anything created under the old, coarser model become inconsistent with the new, finer one?

## Example

Bad: "We should think about security."

Good: "The spec lets an agent hold a long-running credential to an external provider for the life of a workstream, but says nothing about what happens to in-flight work if that credential is revoked mid-execution — the workstream has no defined state for 'authorization lost, work in progress.'"
