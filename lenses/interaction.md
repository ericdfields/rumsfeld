# Lens: Interaction / UX

**Core question:** what does a user need to see, understand, compare, change, undo, recover, inspect, explain, navigate, or remember — that the spec doesn't mention showing them?

This lens is about the gap between "the system does the right thing" and "the user can tell the system did the right thing, and can act on that."

## Simulate four users

Run the spec's core workflow through each of these, and note where each one would get stuck or confused:

- **First-time user** — has no prior mental model. What does the spec assume they already know?
- **Returning user** — left mid-task and came back. Does the spec preserve enough state and surface enough context for them to reorient?
- **Confused user** — something didn't go the way they expected. What does the spec give them to figure out what happened?
- **Expert/power user** — knows the system well and wants to move fast. Does the spec's interaction model scale down to fewer clicks, or does it only work at first-time-user pace?

## Look for

- **Silent state.** Anything the system tracks internally that the user needs to see to understand what's happening (progress, ownership, why a decision was made) but that the spec doesn't surface anywhere.
- **No path back.** Actions the spec describes going forward but not in reverse — can the user undo it, cancel it mid-flight, or correct a mistake without starting over?
- **Comparison surfaces.** If there's ever more than one of something (see the product lens), does the UI let the user actually compare them, or only view one at a time?
- **Explainability gaps.** When the system does something surprising (auto-picks a default, silently retries, falls back to a different path), what does the user see that tells them this happened and why?
- **Navigation and memory.** Once a user leaves this flow, can they find their way back to exactly where they were, or does the spec implicitly assume one continuous session?

## Example

Bad: "The UI needs more polish."

Good: "The spec has the system silently retry a failed step up to three times before surfacing an error, but nothing in the interaction model tells the user a retry happened — a user who sees the operation eventually succeed after 40 seconds of delay has no way to know why, and no way to tell a genuinely slow operation from a retried one."
