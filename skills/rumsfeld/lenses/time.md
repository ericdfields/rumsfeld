# Lens: Time

**Core question:** stretch the system across time — what assumption holds for the demo but not for the deployed system months later?

## Ask what happens

```
five seconds later
overnight
two weeks later
six months later
after an upgrade
after thousands of accumulated objects
after institutional memory disappears
```

## Look for

- **Assumptions that only hold briefly.** Anything the spec assumes stays true "for now" (a schema, a provider's API shape, a team's staffing) — what's the plan for when it doesn't?
- **Accumulation effects.** Does anything in the spec behave differently once there are thousands of the things it was designed around, purely because of volume over time, not concurrent load (see the scale lens for concurrent load specifically)?
- **Institutional memory loss.** Does correct operation of this system depend on someone remembering a decision, a workaround, or a piece of context that isn't written down anywhere the spec produces? What happens when that person is no longer on the team?
- **Upgrade discontinuities.** When the system this spec describes is itself upgraded or replaced, what happens to data or processes that were mid-flight or created under the old version?
- **Silent drift.** Is there anything that's correct today because of an assumption about an external system, a config value, or a convention that could quietly become false without triggering any alert?

## Example

Bad: "Things change over time."

Good: "The spec's retry logic assumes the provider's error codes mean the same thing they mean today; if the provider changes what a given error code represents (as providers do), the spec's retry-vs-fail decision silently becomes wrong for that error, and nothing would flag it — it would just start retrying things that should fail, or vice versa."
