# SURPRISES.md entry template

Use this when something learned during implementation or use turns out to be something the Rumsfeld pass (or ordinary spec review) failed to anticipate. Append entries to `SURPRISES.md` at the project root, creating the file if it doesn't exist yet.

A surprise is worth recording when it's a genuine conceptual miss — something that changed the model of the problem, not an ordinary bug. If it's the kind of thing that would have been caught by a unit test, it's not a Rumsfeld surprise; it's just a bug, and belongs in the issue tracker instead.

```markdown
## <short title>

date: <date discovered>
project: <project/artifact name>
related_rumsfeld_finding: <link to the RUMSFELD.md finding this echoes or
  missed, or "none — escaped the review entirely">

**Discovery:** <what was actually learned, stated plainly>

**Original assumption:** <what the spec or the team believed instead>

**Consequence:** <what this actually cost — rework, delay, data migration,
  a redesign, an incident>

**Cost of discovery:** <rough size — hours, days, a sprint, a redesign>

**When discovered:** <planning / early build / late build / after ship / incident>

**Question that might have exposed it:** <the specific question that, if
  asked during spec review, would plausibly have surfaced this in advance>

**Generalized pattern:** <the reusable lesson, stripped of this project's
  specifics, phrased so it's recognizable in an unrelated future project —
  e.g. "long-running processes that migrate between executors require
  explicit state and ownership-transfer semantics.">
```

## Why the generalized pattern matters most

The `generalized_pattern` field is what makes this ledger useful to *future* projects, not just a post-mortem log for this one. A future `rumsfeld` run should be able to scan existing `SURPRISES.md` entries' `generalized_pattern` fields and ask, for a new spec: does this pattern plausibly apply here? Write it so that question is answerable without knowing anything about the original project — no project-specific nouns, just the shape of the mistake.

## Example

```markdown
## Provider switching required durable ownership transfer, not just routing

date: 2026-08-03
project: agent-workstream-supervisor
related_rumsfeld_finding: RUMSFELD.md#contrarian — "is this a routing problem"

**Discovery:** Provider switching requires transfer of durable execution
ownership, not merely changing the model used for the next inference call.

**Original assumption:** Model selection is essentially a routing concern —
pick a different provider for the next call.

**Consequence:** Had to redesign the workstream state machine mid-build to
add explicit ownership and handoff states; two weeks of already-built routing
logic was reworked into a handoff protocol instead.

**Cost of discovery:** ~2 weeks of rework.

**When discovered:** early build.

**Question that might have exposed it:** "When ownership of a long-running
process moves between executors, what state needs to survive that move, and
who owns the process during the move itself?"

**Generalized pattern:** Long-running processes that migrate between
executors require explicit state and ownership-transfer semantics — treating
the migration as a simple routing decision hides this until implementation.
```
