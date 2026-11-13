# Lens: Lifecycle

**Core question:** for each important entity in the project model, what states does it pass through — and which ones did the spec skip?

Specs are usually written around the happy-path lifecycle: created, used, finished. The missing states are almost always the interesting ones.

## Procedure

Take every important entity from the Phase 1 project model (not just the "main" one — secondary entities like sessions, attachments, or subscriptions often have the most neglected lifecycles). For each, walk it through as many of these as apply:

```
creation
initialization
active use
interruption
pause
resume
retry
duplication
migration
completion
failure
abandonment
deletion
restoration
```

For each stage, ask two things:

1. Does the spec say what happens here, even implicitly?
2. If this entity got stuck at this stage indefinitely, would anything notice?

## What to look for

- **Missing intermediate states.** Specs often jump straight from "created" to "active" or from "active" to "done," skipping states like "abandoned," "partially migrated," or "pending confirmation" that will exist in practice whether or not they're named.
- **States with no exit.** If an entity can enter a state but the spec never says what gets it out, that's a candidate finding — e.g. "paused" with no described resume path.
- **States that silently merge.** Sometimes the spec treats two lifecycle stages as one ("cancelled" and "failed" both just mean "gone"), but they need different handling (a cancelled job might need cleanup a failed one doesn't, or vice versa).
- **Orphans.** What happens to an entity when the thing that owns it (a user, session, parent object) is itself deleted or expires mid-lifecycle?

## Example

Bad: "What if creation fails?"

Good: "The spec describes a workstream as created, active, or complete, but doesn't have a state for 'handoff initiated, not yet confirmed by the receiving agent' — which is exactly the window where duplicate execution or silent abandonment would happen."
