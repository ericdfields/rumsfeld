# The two-week pre-mortem (mandatory)

This is Phase 5 of the review, and it is not optional — run it even if the lens reviews and probes already feel thorough. It catches things the lens-by-lens approach structurally misses, because it starts from the discovery and works backward instead of starting from the spec and working forward.

## The scenario

Put yourself two weeks into substantive implementation of this spec. The team has just discovered something important enough that everyone says, out loud: **"Oh. This isn't actually the problem we thought it was."**

Generate several plausible versions of that moment — genuinely plausible ones, grounded in what Phase 1-4 already surfaced, not invented from nothing. Three to six well-reasoned discoveries beat a dozen generic ones.

## For each candidate discovery, answer all six

1. **What was discovered?** State it as the team would have said it out loud, in plain language.
2. **Why wasn't it visible in the original spec?** What was the spec talking about instead, that made this easy to miss?
3. **What assumption concealed it?** Name the specific assumption (should trace back to something from Phase 2 or a lens finding if possible).
4. **What implementation has already become invalid?** Be concrete — which component, which data model decision, which piece of already-written code would need to be redone?
5. **What question could have exposed it earlier?** Write the actual question. This is often the most useful output of the whole exercise — it can go straight into the PRD as something to explicitly answer before building.
6. **Is the scenario plausible enough to investigate now?** Not every generated scenario clears this bar — say so honestly. A scenario that fails this test still gets recorded (as a rejected concern, per the dispositions in `SKILL.md`), just not acted on.

## Calibrate against this pair

**Bad pre-mortem finding** (a defect, not a conceptual surprise):
> "The API might return a 500."

**Good pre-mortem finding** (a conceptual surprise that reframes the problem):
> "We assumed a job had a single execution owner, but failover means ownership itself must be modeled durably — 'who is running this right now' turns out to be a piece of state that needs to survive a crash, not just an in-memory fact."

The difference: the bad example is a thing that could go wrong within the existing model. The good example is a thing that changes what the model needs to contain in the first place. Aim for the second kind — if a candidate discovery reads like an ordinary bug report, it's probably not a pre-mortem finding, and belongs in ordinary QA instead.

## Feed forward

Discoveries that pass the plausibility bar in step 6 should be written up as full findings using the schema in `SKILL.md` and carried into Phase 6 (recursive drilling) and Phase 7 (synthesis) like any other finding — the pre-mortem is a generation technique, not a separate output track.
