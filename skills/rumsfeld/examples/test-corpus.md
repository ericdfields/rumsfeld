# Calibration test corpus

Not for normal use of the skill — this is for testing `rumsfeld` itself (e.g. via the skill-creator eval loop). Each scenario has a known-good direction of travel, used to check the skill neither under-reacts nor over-reacts.

## 1. Supervisor model switching

Give `rumsfeld` an early-stage PRD that frames provider/model switching primarily as a routing concern — "let an agent workstream move from one model/provider to another."

**A strong run should independently move toward:** durable state, ownership, interruption, handoff, partial transition, duplicate execution, recovery — without needing to be told those words. The contrarian lens (`lenses/contrarian.md`) should be the one that catches this if nothing else does; if the contrarian lens comes back empty on this scenario, that's a sign it's not being applied with enough force.

**Pass/fail shape:** it doesn't need to use these exact terms, but the report's Section D (conceptual omissions) or E (high-impact discoveries) needs to surface the shift from "routing" to "ownership transfer" in substance.

## 2. Simple CRUD application

Give `rumsfeld` an intentionally straightforward spec — a small, genuinely simple CRUD app with no distributed concerns, no long-running processes, no multi-agent anything.

**A good run should not manufacture a distributed-systems thesis.** This tests restraint, not thoroughness. If the report reads like it forced scale, integrations, or systems-lens findings that don't actually apply, that's a failure — check the "Non-goals" and "Stop condition and noise control" sections of `SKILL.md` are being honored, and that findings are named as concrete mechanisms rather than generic categories.

**Pass/fail shape:** the gate should plausibly land on READY or READY WITH INVESTIGATIONS with a short list of genuinely simple, concrete findings (e.g. missing soft-delete/undo, no audit trail) — not a padded report full of hypothetical distributed-systems risk.

## 3. UI-heavy product

Give `rumsfeld` a technically adequate PRD for a UI-heavy product that's missing the consequences-of-success the product and interaction lenses are built to catch.

**A good run should discover, where appropriate:** comparison, history, undo, branching, sharing, revisitation — the natural next things a satisfied user reaches for.

**Pass/fail shape:** Section F (product and user discoveries) should contain at least one or two of these, phrased as concrete mechanisms tied to the specific product, not a generic "add undo support" note.

## 4. Deliberately flawed abstraction

Construct a PRD whose fundamental entity model is subtly wrong — e.g. modeling something as a single mutable document when it actually needs to be a version history, or modeling something as a user preference when it actually needs to be shared team state.

**The contrarian pass should challenge it directly.** This is the sharpest test of `lenses/contrarian.md` — if this scenario doesn't produce a contrarian finding, the lens isn't doing its job.

**Pass/fail shape:** Section D (conceptual omissions) should name the actual structural mismatch, not just list surface-level missing fields.

## Using these as regression tests

As `rumsfeld` evolves, rerun these four and check the pass/fail shapes still hold — especially #2, since noise control tends to degrade first as a skill gets "improved" with more instructions. If #2 starts producing bloated reports, that's the earliest warning sign.
