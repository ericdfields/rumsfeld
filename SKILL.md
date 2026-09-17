---
name: rumsfeld
description: Adversarial pre-build review that hunts for unknown unknowns in a PRD, spec, architecture proposal, or implementation plan — hidden assumptions, missing concepts, unmodeled state, lifecycle gaps, and category errors in the problem framing itself, not just missing fields or edge cases. Use this whenever a PRD, spec, architecture doc, or project plan looks "done" and someone is about to start implementation. Trigger on phrases like "run rumsfeld", "rumsfeld this", "rumsfeld the PRD", "is this ready to build", "pressure-test this spec", "what are we missing before we build this", "sanity check this plan before we start implementation", or a request to review a requirements doc for blind spots, hidden complexity, or wrong framing before implementation begins. Also trigger proactively when a user says a spec is "ready" or "final" and asks what to do next, since that psychological moment is exactly when this review adds the most value. Do not use this for routine PRD copyediting, line-level code review, or security scanning — those are different tools (see code-review, security-review). Do not use it on ideas that are still exploratory or half-formed; it needs something that already looks implementation-ready.
---

# rumsfeld

> "There are also unknown unknowns — the ones we don't know we don't know."
> — Donald Rumsfeld, DoD press briefing, February 12, 2002

A specification can be long, internally consistent, technically plausible, and still be built on the wrong model of the problem. Completeness inside the wrong model is not completeness. `rumsfeld` is an adversarial review that runs on a document that *looks* ready to build, and tries to find out before implementation starts, not two weeks into it.

The central question is not "did we specify every field, endpoint, and error state?" It is:

**What might we discover after beginning implementation that would show this was a different problem than we thought it was?**

## When to run this

Run it when the team believes the spec is done — the moment someone says "great, I think we're ready." Running it earlier just flags uncertainty everyone already knows about. If the artifact is still exploratory, tell the user it's too early and suggest firming it up first.

## Non-goals — read this before starting

This is the part most likely to go wrong, so front-load it. `rumsfeld` is not a generic checklist, not a security scanner, not an edge-case generator, not a device for making every spec bigger. An LLM's natural failure mode here is to produce 40 plausible-sounding concerns of roughly equal, low weight. That is not a good review — it's noise that trains the reader to stop reading.

**Ten deeply consequential discoveries beat two hundred speculative ones.** Optimize for that ratio, not for coverage. See "Stop condition and noise control" below — it is not optional housekeeping, it's the mechanism that keeps this tool useful instead of exhausting.

## Inputs

**Required:** at least one artifact believed to be implementation-ready — a PRD, spec, architecture proposal, project plan, or agent directive. The process must work from this alone.

**Optional, use if present in the repo or supplied by the user:** architecture docs, prototypes, schemas, API docs, prior specs, decision logs, source code, previous Rumsfeld reports, and — importantly — a Surprise Ledger (see below). Don't go looking for all of these exhaustively; a quick check of the repo root and any linked docs is enough.

**Surprise Ledger:** look for `SURPRISES.md` (or similar) in the project. If one exists, read it before Phase 3 and carry forward any entries whose `generalized_pattern` plausibly applies to this artifact (resumable jobs, ownership transfer, worker/agent migration, long-running or externally-dependent processes, etc.). Don't inject every past lesson reflexively — only the ones that are structurally similar. Say so explicitly in the report: "something structurally similar surprised us before — does that pattern exist here?"

## The seven phases

Work through these in order. Depth matters more than speed — this is meant to be a careful pass, not a fast one.

### Phase 1 — Reconstruct the project model

Before hunting for problems, figure out what model of the world the spec assumes. Extract the apparent actors, objects/entities, state, relationships, actions, transitions, lifecycles, boundaries, dependencies, invariants, sources of truth, success states, failure states, and ownership relationships. Write this down as a short model, even informally — e.g.:

```
Workstream
    owned by → Agent
    executed through → Provider
    operates on → Workspace
    has → Context
    produces → Actions
    persists → Checkpoint
```

This doesn't need to be a formal graph. It does need to exist explicitly, because the first adversarial question depends on it: **what noun, relationship, state, boundary, or transition should exist in this model but doesn't?** You cannot ask that question without first writing the model down.

### Phase 2 — Extract hidden assumptions

Identify what the spec relies on without ever stating. For each candidate assumption, classify it as `explicit`, `implicit but supported`, `implicit and unverified`, `contradicted`, or `unknown`. Examples of the shape these take:

- "A process will only have one owner."
- "External APIs accurately report whether an operation succeeded."
- "Users will finish the workflow in one sitting."
- "The UI and backend agree about current state."
- "Failures happen cleanly before or after an operation, never during it."

The `implicit and unverified` ones are the strongest candidates to carry into Phase 3.

### Phase 3 — Run the lens reviews

Thirteen adversarial lenses live in `lenses/`. Each file is short (~20-40 lines) and self-contained — read a lens file right before applying it, not all thirteen up front. The lenses are:

`systems`, `lifecycle`, `product`, `interaction`, `human-behavior`, `operations`, `data`, `integrations`, `security`, `scale`, `economics`, `time`, `reversibility`, `contrarian`.

(That's 13 files under `lenses/` — see the reference table at the end of this file for the exact filenames.)

**Orchestration:** if you have access to an Agent/subagent tool, spawn one reviewer per lens in parallel, each given the artifact plus its one lens file, and instruct each not to see the others' output — independent framing is the point, it's what keeps everyone from converging on the first plausible interpretation. If you don't have subagents available, work through the lenses sequentially yourself, one at a time, actually reading each lens file before applying it rather than relying on memory of this list.

Give the **contrarian** lens the strongest mandate of the thirteen. It asks whether the fundamental abstraction in the spec is wrong — a category error, not an implementation gap. One contrarian finding can outweigh everything else combined; don't let it get averaged down by consensus from the other twelve.

### Phase 4 — Unknown-unknown generators

Beyond the lenses, apply the standard probes in `generators/probes.md` (atomicity challenge, hidden-state challenge, boundary walk, interruption test, double-execution test, concurrency test, partial-success test, false-success/false-failure tests, dependency-removal test, role inversion, success consequence, power-user projection, explainability test, data-model regret, architecture regret). These are designed to actively transform the problem rather than just inspect it — read that file before running them.

### Phase 5 — The two-week pre-mortem (mandatory)

This step is not optional. Follow the procedure in `generators/premortem.md`: imagine it's two weeks into implementation and the team just realized "this isn't the problem we thought it was" — generate plausible versions of that discovery, and for each one work through what was discovered, why the spec didn't surface it, what assumption concealed it, what would already be invalidated, what question could have exposed it, and whether it's worth investigating now. Favor conceptual surprises over ordinary defects — "the API might 500" is not a pre-mortem finding, "we assumed a job has one execution owner, but failover means ownership must be modeled durably" is.

### Phase 6 — Recursive drilling

Don't stop at the first-order finding for anything that looks consequential. Keep asking "why" and "what does that require" until you hit something concrete and either resolvable or worth flagging. Example chain: agent handoff requires state transfer → what counts as agent state? → conversation, workspace, tool state, pending side effects, execution position → can those transfer independently? → yes, so partial handoff exists → who owns the task during a partial handoff? → ownership needs explicit semantics → can both agents resume? → duplicate execution needs fencing. Prefer this kind of depth on a handful of findings over broad, shallow speculation across many.

### Phase 7 — Synthesize

Pull together everything from Phases 2-6: deduplicate, combine related findings, reject weak speculation, trace causal chains, separate symptoms from root causes, surface contradictions between lenses, and rank by impact and reversibility. A finding's consensus count (how many lenses independently raised it) can be noted, but consensus does not equal truth — a single contrarian finding can matter more than five lenses agreeing on something minor.

## Finding schema

Every finding that survives synthesis gets this structure:

```yaml
finding:
  title:
  summary:
  discovered_by:            # which lens(es) or generator
  underlying_assumption:
  why_it_matters:
  evidence:
  impact: low | medium | high | foundational
  reversibility: easy | moderate | difficult | extremely difficult
  confidence: speculative | plausible | likely | demonstrated
  affected_areas:
  follow_up_questions:
  disposition:               # see below
```

A finding that is both high-impact and difficult to reverse deserves real attention even at `plausible` confidence — don't wait for `demonstrated` on things where being wrong is expensive to undo.

## Dispositions

Every retained finding must end in exactly one of these. Discovering an issue does not automatically add scope — that distinction is the whole point of having dispositions instead of a flat list of worries.

- **PRD amendment** — something is missing from the spec; propose the concrete addition.
- **Architecture decision** — the requirement is understood, but the architecture must explicitly address it.
- **Investigation** — evidence is needed (prototype, spike, benchmark, API experiment, user test) before a decision can be made.
- **Design exploration** — an interaction or user-behavior question needs more design work.
- **Accepted unknown** — the uncertainty is real and the team is consciously proceeding despite it.
- **Monitor** — can't reasonably be resolved pre-build, but implementation should make the behavior observable so it's caught early if it happens.
- **Rejected** — irrelevant, too implausible, intentionally out of scope, already handled, or not worth its cost. Record it anyway; a rejected concern with a stated reason is more useful than silence.

## Stop condition and noise control

An LLM can generate concerns indefinitely — that is a failure mode of this exact skill, not a feature. Stop when:

1. new passes mostly produce duplicates of what's already found;
2. new findings are predominantly low-impact;
3. foundational findings have already been recursively drilled (Phase 6);
4. no unresolved item combines high impact with difficult reversibility and lacks a disposition;
5. the hidden assumptions surfaced are sufficient to support a deliberate decision;
6. reviewers stop introducing genuinely new concepts into the project model from Phase 1.

The goal is diminishing marginal discovery, not exhaustive imagination. Actively reject findings that are categories rather than mechanisms — "concurrency could cause issues," "the external API might change," "security is important," "scale might become a problem" are not findings, they're topics. A retained finding names a concrete mechanism: not "concurrency could cause issues" but "the spec lets users launch multiple revisions simultaneously but models `current_version` as a single mutable pointer with no conflict semantics." If you can't name the mechanism, don't retain it.

If every run of this skill ends in NOT READY, the skill is being run too conservatively — recalibrate. NOT READY should be uncommon.

## Producing the report

Write the output to `RUMSFELD.md` in the project (adapt the filename to repo convention if one is obvious — e.g. `docs/RUMSFELD.md`). Use `templates/report.md` for the exact section structure (executive summary, reconstructed model, hidden assumptions, conceptual omissions, high-impact discoveries, product/user discoveries, technical discoveries, the pre-mortem, required PRD amendments, investigations, accepted unknowns, monitoring requirements, rejected concerns, final disposition).

Keep section E (high-impact discoveries) intentionally short — that's where burying signal under volume does the most damage.

The report proposes changes; it does not silently become the new spec. State clearly which items are findings versus which (if any) you're recommending be auto-accepted, and let the user or project policy decide. Don't rewrite the source PRD yourself unless the user asks you to apply the amendments.

### Final disposition — pick exactly one

- **READY** — no unresolved discovery currently suggests a likely foundational reversal; remaining uncertainty is understood and acceptable.
- **READY WITH INVESTIGATIONS** — implementation can start, but name the specific investigations that must land before specified downstream decisions become irreversible (e.g. "don't finalize the durable execution-state schema until the provider-handoff spike is done").
- **NOT READY** — a foundational assumption or missing concept has been exposed that materially changes what should be built. Should be rare; see the stop-condition note above.

## After implementation — closing the loop

This skill is more valuable the second time a project uses it. If the user mentions they're a couple of weeks into implementation on something this skill previously reviewed, or asks for a calibration pass, compare the report to what actually happened: what surprised the team, which findings were useful, which were noise, what escaped the review entirely and why, and what question would have caught it. Turn genuine escapes into new entries using `templates/surprise.md`, appended to (or creating) `SURPRISES.md`. That ledger is what future `rumsfeld` runs on related projects should draw on — see "Surprise Ledger" above.

## Reference files

| File | When to read it |
|---|---|
| `lenses/systems.md` | Phase 3 — hidden coordination, state, ordering, recovery machinery |
| `lenses/lifecycle.md` | Phase 3 — walk every entity through its full lifecycle |
| `lenses/product.md` | Phase 3 — consequences of success, not just failure |
| `lenses/interaction.md` | Phase 3 — what users need to see/undo/compare/explain |
| `lenses/human-behavior.md` | Phase 3 — reasonable-but-unconsidered user actions |
| `lenses/operations.md` | Phase 3 — debugging, support, escalation, upgrades, cleanup |
| `lenses/data.md` | Phase 3 — data lifecycle and hidden sources of truth |
| `lenses/integrations.md` | Phase 3 — external dependencies as partially unreliable |
| `lenses/security.md` | Phase 3 — malicious/mistaken actors, expiring trust boundaries |
| `lenses/scale.md` | Phase 3 — cardinality changes and inverted assumptions |
| `lenses/economics.md` | Phase 3 — small operations that get expensive at volume |
| `lenses/time.md` | Phase 3 — the system stretched across minutes to years |
| `lenses/reversibility.md` | Phase 3 — cheap-now, expensive-to-reverse-later decisions |
| `lenses/contrarian.md` | Phase 3 — is the fundamental abstraction wrong |
| `generators/probes.md` | Phase 4 — atomicity, hidden-state, interruption, double-execution, etc. |
| `generators/premortem.md` | Phase 5 — the mandatory two-week pre-mortem procedure |
| `templates/report.md` | When writing `RUMSFELD.md` |
| `templates/surprise.md` | When adding an entry to `SURPRISES.md` after implementation |
| `examples/test-corpus.md` | Calibration scenarios for testing this skill itself, not for normal use |
