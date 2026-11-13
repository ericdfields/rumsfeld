# Lens: Human behavior

**Core question:** what completely understandable action will a user take that the design never considered?

This lens assumes users are reasonable, not adversarial and not careless — but reasonable people deviate from the happy path constantly, for ordinary reasons: they get interrupted, they misremember, they do things in a different order than the designer imagined, they have a goal slightly different from the one the feature assumes.

## Look for

- **Reordering.** The spec assumes steps happen in a specific sequence. What happens if a reasonable user does them in a different order, or does step 3 before step 2 because the UI didn't stop them?
- **Interruption mid-flow.** The user starts the workflow, gets pulled away (phone call, closed the tab, lost connection), and comes back an hour, a day, or a week later. What does the spec assume about session continuity that won't hold?
- **Parallel intent.** A user opens the same workflow in two tabs, or starts a second one before finishing the first, not out of malice but because they didn't realize the first was still running. What does the spec assume about single-instance usage?
- **Reasonable misuse.** Using a feature for a purpose adjacent to, but not exactly, what it was designed for — because it's the closest tool available. Does the spec's model break in a confusing way, or degrade gracefully?
- **Forgetting.** The user did something relevant days ago and doesn't remember doing it. Does the system's behavior now depend on state the user isn't aware exists?

## How to use this lens

For each major user action in the spec, ask: what's the most boring, plausible way a real person deviates from the assumed sequence — not a stress test, not an edge case, just an ordinary Tuesday. If the spec's behavior in that case is undefined or surprising, that's a candidate finding.

## Example

Bad: "Users might misuse the feature."

Good: "The spec assumes a user completes the multi-step import in one sitting, but the steps are spread across a wizard with no save-and-resume — a user who gets pulled into a meeting after step 2 will come back to a blank wizard with no indication their partial work existed."
