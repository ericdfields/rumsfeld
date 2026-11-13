# RUMSFELD.md template

Write the report using this section structure. Omit a section only if it's genuinely empty (say so in one line — "none found" — rather than deleting the heading, so a reader can tell the section was considered and not skipped). Keep section E short on purpose; that's where burying real signal under volume does the most damage.

```markdown
# Rumsfeld Report — <project/artifact name>

Date: <date>
Source artifact(s): <link or path>
Gate: <READY | READY WITH INVESTIGATIONS | NOT READY>

## A. Executive summary

<2-5 sentences: did this pass materially change understanding of the project,
or largely confirm the existing spec? Say which, plainly.>

## B. Reconstructed project model

<Actors, objects, relationships, states, transitions, boundaries — the Phase 1
model, written out so a reader can sanity-check it against their own mental
model of the project. A mismatch here is itself often a finding.>

## C. Hidden assumptions

<The material implicit assumptions from Phase 2, each with its classification
(explicit / implicit but supported / implicit and unverified / contradicted /
unknown) and a one-line note on why it matters.>

## D. Conceptual omissions

<Missing concepts that change how the system should be understood — not
missing fields, missing *ideas*. This is where contrarian-lens findings and
architecture-regret findings usually land.>

## E. High-impact discoveries

<The handful of findings that matter most. Use the finding schema from
SKILL.md. Deliberately short list — if everything is "high impact," nothing
is; push the rest down into F/G or into Accepted Unknowns/Monitoring.>

## F. Product and user discoveries

<Unexpected user behaviors, expectations, and consequences of success, from
the product / interaction / human-behavior lenses.>

## G. Technical discoveries

<Architecture, state, lifecycle, data, integration, and operational findings,
from the systems / lifecycle / data / integrations / operations / scale /
economics / time / reversibility / security lenses.>

## H. Two-week pre-mortem

<The discoveries generated in Phase 5, each with its 6-part breakdown
(discovery, why invisible, concealing assumption, invalidated work, exposing
question, plausibility verdict) — or a condensed version if the full
breakdown was already folded into E/F/G. Always keep the "exposing question"
for each, even condensed — it's the most reusable output of the whole pass.>

## I. Required PRD amendments

<Concrete, specific changes to the source spec. Write these as edits someone
could apply, not just as problems restated.>

## J. Investigations

<Questions that need an experiment, spike, prototype, benchmark, or user test
before they can be answered. State what evidence would resolve each one.>

## K. Accepted unknowns

<Real uncertainty the team is consciously choosing to proceed despite. State
why proceeding anyway is reasonable — this section is not a dumping ground
for unresolved worries, it's a record of deliberate risk acceptance.>

## L. Monitoring requirements

<Unknowns that can't be resolved pre-build but should be made observable
during implementation or production, so they surface fast if they occur
rather than being discovered by an angry user or a postmortem.>

## M. Rejected concerns

<Things that came up during the review and were deliberately excluded, with a
one-line reason each. Recording these prevents re-litigating them next time,
and shows the review wasn't just uncritically accepted.>

## N. Final disposition

<One paragraph: READY / READY WITH INVESTIGATIONS / NOT READY, and — if
INVESTIGATIONS — the specific decisions that must not be finalized until
which specific investigation lands.>
```

## Notes on filling this in

- Every item in E, F, G, and H should trace back to the finding schema in `SKILL.md` (impact, reversibility, confidence, disposition) even if you don't print the raw YAML in the report — the prose should make those attributes legible without needing the reader to decode a schema.
- If a Surprise Ledger (`SURPRISES.md`) existed and contributed context, say so explicitly in the executive summary or wherever the relevant finding appears — e.g. "this echoes the ownership-transfer surprise from `<related project>`."
- The report proposes; it doesn't decide. Don't silently promote every finding into scope — that's what dispositions are for.
