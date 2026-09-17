# Lens: Operations

**Core question:** once this is running in production, who keeps it running, and what did the spec assume would never need attention?

Specs describe the thing working. This lens is about everything that has to happen around the thing so that it keeps working, and so that someone other than the original author can operate it.

## Look for

- **Debugging.** When this breaks in production, what does the person debugging it actually have to look at? Does the spec produce that information anywhere, or does it only exist transiently in memory?
- **Observability.** Are the states and transitions from the lifecycle lens (see `lifecycle.md`) actually visible from outside — logged, queryable, or dashboarded — or only inferable by reading code?
- **Support.** When a user reports "it didn't work," what does support need to look up to figure out what happened to that specific user's case? Does the spec's data model retain enough history to answer that after the fact?
- **Ownership.** If this breaks at 2am, whose problem is it? Does the spec draw a boundary between systems in a way that makes ownership unambiguous, or does it straddle a boundary no team clearly owns?
- **Escalation.** Is there a defined path from "something's wrong" to "the right person is looking at it," or does the spec assume someone will just notice?
- **Upgrades and migration.** When this spec's model changes in six months (new field, new state, new version), what happens to data or in-flight processes created under the old model? Does the spec assume a clean cutover that never actually happens?
- **Cleanup.** Does anything created by this spec need to be deleted, archived, or expired eventually? Who or what does that, and did the spec say so?
- **Incident recovery.** If this component needs to be manually intervened on (killed, rolled back, force-completed), does the spec's data model make that safe, or does manual intervention risk corrupting state the spec assumed was only ever touched by the system itself?

## Example

Bad: "We'll need monitoring."

Good: "The spec has no state field visible outside the process itself for 'handoff in progress' — if a handoff hangs, on-call has no query that distinguishes a hung handoff from a slow one, and no documented way to safely force it to a terminal state without risking duplicate execution."
