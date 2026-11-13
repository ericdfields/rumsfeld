# Lens: Systems

**Core question:** what machinery must secretly exist for the described behavior to actually work?

Specs describe outcomes ("the workstream moves to a new provider") far more often than they describe the machinery underneath ("moves" requires state transfer, ownership handoff, and fencing against double execution). This lens exists to make that machinery explicit before someone discovers it mid-implementation.

Look for:

- **Hidden coordination** — does this behavior require two or more components to agree on something? How do they agree, and what happens if they briefly disagree?
- **State transfer** — does anything move from one owner, process, or machine to another? What exactly constitutes "the state" that moves?
- **Synchronization** — are there operations that must happen in a specific order across components? What enforces that order?
- **Ownership** — for anything long-running or stateful, who owns it at each point in time? Is ownership ever ambiguous or dual?
- **Persistence** — what needs to survive a crash, restart, or deploy that the spec doesn't mention persisting?
- **Ordering** — could two described operations race? What does the spec assume about their relative timing?
- **Idempotency** — if a described operation runs twice (retry, duplicate event, replay), is that safe? Does the spec say?
- **Consistency** — are there two places that could each believe they hold the current truth?
- **Recovery** — when something in this system fails partway, what brings it back to a known-good state, and who/what triggers that?

## Example

Bad (topic, not a finding): "There could be race conditions."

Good (mechanism): "The spec describes 'switching providers' as a single step, but nothing says whether the old provider's in-flight request is cancelled, allowed to finish, or racing against the new provider's first request — all three are plausible readings of the current text."

## How to use this lens

Walk through each major verb in the spec (start, switch, resume, cancel, sync, merge, complete) and ask what machinery that verb is quietly assuming. If the machinery isn't named anywhere in the spec, that's a candidate finding — not because machinery is missing (it's always missing at the PRD level) but because the *decision about* that machinery is missing.
