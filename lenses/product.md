# Lens: Product

**Core question:** once this capability exists, what will users naturally assume it can also do?

Most product review looks for consequences of failure. This lens looks for consequences of success — the requests, comparisons, and expectations that a working feature immediately creates, which the spec never anticipated because it was written before the feature existed.

## Look for

- **Adjacent capability assumed for free.** If you can create X, users will assume you can also list, filter, rename, duplicate, and delete X — even if the spec only described creating it.
- **Comparison and history.** The moment there's more than one of something (more than one version, more than one run, more than one result), users will want to compare them or see how they got here. Does the spec have a story for that, or does it only describe the singular case?
- **Undo and reversal.** Whatever action this spec adds, is there a natural "undo" a user will expect, even if the spec never promised one?
- **Sharing and visibility.** Once something exists, who else will want to see it, and does the spec's model support that, or does it implicitly assume single-user visibility?
- **Escalating usage.** If the described capability succeeds, what does a user who *loves* it try to do next week that the spec's model doesn't obviously support?

## How to use this lens

Read the spec's core capability, then finish this sentence from the user's point of view, several different ways: "Now that I can do this, I assumed I could also ___." Each plausible completion that the spec doesn't address is a candidate finding. Prioritize the completions that are genuinely natural, not exotic — this lens is about the very next thing a satisfied user reaches for, not the fifth thing.

## Example

Bad: "Users might want more features."

Good: "The spec lets a user regenerate a report, but the moment two reports exist, users will expect to see what changed between them — the spec has no concept of a report history or diff, only 'the current report.'"
